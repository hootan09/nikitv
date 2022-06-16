---
title: ساخت بلاکچین با نگاهی به ساختار بین کوین - قسمت ششم (تراکنش ها-۲)
image: images/blockchain/ipevzgcgw8tu.png
description: مقدمه در همان قسمت اول این مجموعه گفتم که بلاکچین یک پایگاه داده توزیع شده است. در آن زمان…
date: 2022-06-16T10:44:13+04:30
author: mam_niki
tags:
- رمزارز
- کریپتو
- برنامه نویسی
- بلاک‌چین
- ارز دیجیتال
categories:
- برنامه نویسی
---

#### مقدمه

در همان _**[قسمت اول](/blog/post_40_bc1)**_ این مجموعه گفتم که بلاکچین یک پایگاه داده توزیع شده است. در آن زمان، تصمیم گرفتیم از بخش «توزیع شده» صرف نظر کنیم و روی بخش «پایگاه داده» تمرکز کنیم. تا به حال، ما تقریباً تمام مواردی را که یک پایگاه داده بلاکچین را می سازد، پیاده سازی کرده ایم. در این پست، مکانیسم‌هایی را که در قسمت‌های قبلی نادیده گرفته شده‌اند، پوشش می‌دهیم و در قسمت بعدی کار بر روی ماهیت توزیع‌شده بلاکچین را آغاز می‌کنیم.

> این بخش تغییرات قابل توجهی را در کد ایجاد می کند، بنابراین توضیح همه آنها در اینجا بی معنی است. لطفاً برای مشاهده تمام تغییرات از آخرین مقاله به _**[این صفحه](https://github.com/Jeiwan/blockchain_go/compare/part_5...part_6#files_bucket)**_ مراجعه کنید.

#### پاداش یا Reward

یکی از چیزهای کوچکی که در مقاله قبلی نادیده گرفتیم، پاداش برای استخراج است. و ما در حال حاضر همه چیز برای اجرای آن داریم.

این پاداش فقط یک تراکنش coinbase است. هنگامی که یک گره ماینینگ شروع به استخراج یک بلوک جدید می کند، تراکنش ها را از صف می گیرد و یک تراکنش کوین بیس را به آنها اضافه می کند. تنها خروجی تراکنش کوین بیس حاوی هش کلید عمومی ماینر است.

پیاده‌سازی پاداش‌ها به آسانی به‌روزرسانی دستور ارسال است:

```go
func (cli *CLI) send(from, to string, amount int) {
    ...
    bc := NewBlockchain()
    UTXOSet := UTXOSet{bc}
    defer bc.db.Close()

    tx := NewUTXOTransaction(from, to, amount, &UTXOSet)
    cbTx := NewCoinbaseTX(from, "")
    txs := []*Transaction{cbTx, tx}

    newBlock := bc.MineBlock(txs)
    fmt.Println("Success!")
}
```

در اجرای ما، کسی که یک تراکنش ایجاد می‌کند، بلوک جدید را استخراج می‌کند و به این ترتیب، یک پاداش دریافت می‌کند.

#### مجموعه UTXO 

در **_[قسمت سوم (ماندگاری داده و رابط خط فرمان یا CLI)](/blog/post_42_bc3)_**ما روشی را که بیت کوین Core بلاک ها را در یک پایگاه داده ذخیره می کند مورد مطالعه قرار دادیم. گفته شد که بلوک ها در پایگاه داده بلوک ها و خروجی های تراکنش در پایگاه داده زنجیره ای ذخیره می شوند. اجازه دهید به شما یادآوری کنم که ساختار **_chainstate_** چیست:

```txt
1.  'c' + 32-byte transaction hash -> unspent transaction output record for that transaction

رکورد خروجی تراکنش خرج نشده برای آن تراکنش

1.  'B' -> 32-byte block hash: the block hash up to which the database represents the unspent transaction outputs

هش بلوک که پایگاه داده خروجی های تراکنش خرج نشده را نشان می دهد
```

از آن مقاله، ما قبلاً تراکنش‌ها را اجرا کرده‌ایم، اما از _**Chainstate**_ برای ذخیره خروجی‌های آن‌ها استفاده نکرده‌ایم. بنابراین، این همان کاری است که اکنون می خواهیم انجام دهیم.

این `chainstate` تراکنش ها را ذخیره نمی کند. در عوض، آنچه را که مجموعه UTXO یا مجموعه خروجی های تراکنش خرج نشده نامیده می شود، ذخیره می کند. علاوه بر این، «هش بلوک را که پایگاه داده خروجی‌های تراکنش مصرف‌نشده را نشان می‌دهد» را ذخیره می‌کند، که فعلاً آن را حذف می‌کنیم زیرا از ارتفاع بلوک استفاده نمی‌کنیم (اما آنها را در مقالات بعدی پیاده‌سازی خواهیم کرد).

بنابراین، چرا می خواهیم مجموعه UTXO را داشته باشیم؟

روش Blockchain.FindUnspentTransactions را که قبلاً پیاده سازی کرده بودیم در نظر بگیرید

```go
func (bc *Blockchain) FindUnspentTransactions(pubKeyHash []byte) []Transaction {
    ...
    bci := bc.Iterator()

    for {
        block := bci.Next()

        for _, tx := range block.Transactions {
            ...
        }

        if len(block.PrevBlockHash) == 0 {
            break
        }
    }
    ...
}
```

این تابع تراکنش هایی را با خروجی های خرج نشده پیدا می کند. از آنجایی که تراکنش‌ها در بلوک‌ها ذخیره می‌شوند، روی هر بلوک در بلاکچین تکرار می‌شود و هر تراکنش موجود در آن را بررسی می‌کند. از 18 سپتامبر 2017، 485,860 بلاک در بیت کوین وجود دارد و کل پایگاه داده 140+ گیگابایت فضای دیسک را اشغال می کند. این به این معنی است که برای تأیید تراکنش ها باید یک گره کامل اجرا شود. علاوه بر این، اعتبارسنجی تراکنش‌ها نیاز به تکرار در بسیاری از بلوک‌ها دارد.

راه حل مشکل این است که شاخصی داشته باشیم که فقط خروجی های مصرف نشده را ذخیره کند، و این همان کاری است که مجموعه UTXO انجام می دهد: این یک حافظه پنهان است که از تمام تراکنش های بلاک چین ساخته شده است (بله، با تکرار روی بلوک ها، اما این کار فقط یک بار انجام می شود. ) و بعداً برای محاسبه مانده و اعتبارسنجی معاملات جدید استفاده می شود. مجموعه UTXO تا سپتامبر 2017 حدود 2.7 گیگابایت است.

خوب، بیایید فکر کنیم برای اجرای مجموعه UTXO چه چیزی را باید تغییر دهیم. در حال حاضر از روش های زیر برای یافتن تراکنش ها استفاده می شود:

۱- این Blockchain.FindUnspentTransactions – تابع اصلی که تراکنش ها را با خروجی های خرج نشده پیدا می کند. این تابعی است که در آن تکرار همه بلوک ها اتفاق می افتد.

۲- این Blockchain.FindSpendableOutputs – این تابع زمانی که تراکنش جدیدی ایجاد می شود استفاده می شود. اگر به تعداد کافی خروجی که مقدار مورد نیاز را در خود نگه می دارند، بیابد. از Blockchain.FindUnspentTransactions استفاده می کند.

۳- این Blockchain.FindUTXO - خروجی‌های خرج نشده برای هش کلید عمومی را که برای بدست آوردن تعادل استفاده می‌شود، پیدا می‌کند. از Blockchain.FindUnspentTransactions استفاده می کند.

۴- این Blockchain.FindTransaction – یک تراکنش در بلاکچین را با شناسه آن پیدا می کند. روی همه بلوک ها تکرار می شود تا زمانی که آن را پیدا کند.

همانطور که می بینید، همه روش ها روی بلوک های پایگاه داده تکرار می شوند. اما فعلا نمی‌توانیم همه آنها را بهبود ببخشیم، زیرا مجموعه UTXO همه تراکنش‌ها را ذخیره نمی‌کند، بلکه فقط آنهایی را که خروجی‌های خرج نشده دارند ذخیره می‌کند. بنابراین، نمی توان از آن در Blockchain.FindTransaction استفاده کرد.

بنابراین، ما روش های زیر را می خواهیم:

۱- این Blockchain.FindUTXO - تمام خروجی های خرج نشده را با تکرار روی بلوک ها پیدا می کند.

۲- این UTXOSet.Reindex — از FindUTXO برای یافتن خروجی های خرج نشده استفاده می کند و آنها را در پایگاه داده ذخیره می کند. اینجا جایی است که کش اتفاق می افتد.

۳- این UTXOSet.FindSpendableOutputs – آنالوگ Blockchain.FindSpendableOutputs، اما از مجموعه UTXO استفاده می کند.

۴- این UTXOSet.FindUTXO - آنالوگ Blockchain.FindUTXO، اما از مجموعه UTXO استفاده می کند.

۵- این Blockchain.FindTransaction ثابت باقی می ماند.

بنابراین، دو تابع پرکاربرد از هم اکنون از حافظه پنهان (cache) استفاده خواهند کرد! بیایید کدنویسی را شروع کنیم.

```go
type UTXOSet struct {
    Blockchain *Blockchain
}
```

ما از یک پایگاه داده استفاده می کنیم، اما مجموعه UTXO را در یک سطل (bucket) دیگر ذخیره می کنیم. بنابراین، UTXOSet با بلاکچین همراه است.

```go
func (u UTXOSet) Reindex() {
    db := u.Blockchain.db
    bucketName := []byte(utxoBucket)

    err := db.Update(func(tx *bolt.Tx) error {
        err := tx.DeleteBucket(bucketName)
        _, err = tx.CreateBucket(bucketName)
    })

    UTXO := u.Blockchain.FindUTXO()

    err = db.Update(func(tx *bolt.Tx) error {
        b := tx.Bucket(bucketName)

        for txID, outs := range UTXO {
            key, err := hex.DecodeString(txID)
            err = b.Put(key, outs.Serialize())
        }
    })
}
```

این متد در ابتدا مجموعه UTXO را ایجاد می کند. ابتدا سطل (bucket) را در صورت وجود حذف می کند، سپس تمام خروجی های مصرف نشده را از بلاکچین دریافت می کند و در نهایت خروجی ها را در سطل ذخیره می کند.

این Blockchain.FindUTXO تقریباً مشابه Blockchain.FindUnspentTransactions است، اما اکنون map ای از جفت TransactionID → TransactionOutputs را برمی گرداند.

اکنون می توان از مجموعه UTXO برای ارسال سکه استفاده کرد:

```go
func (u UTXOSet) FindSpendableOutputs(pubkeyHash []byte, amount int) (int, map[string][]int) {
    unspentOutputs := make(map[string][]int)
    accumulated := 0
    db := u.Blockchain.db

    err := db.View(func(tx *bolt.Tx) error {
        b := tx.Bucket([]byte(utxoBucket))
        c := b.Cursor()

        for k, v := c.First(); k != nil; k, v = c.Next() {
            txID := hex.EncodeToString(k)
            outs := DeserializeOutputs(v)

            for outIdx, out := range outs.Outputs {
                if out.IsLockedWithKey(pubkeyHash) && accumulated < amount {
                    accumulated += out.Value
                    unspentOutputs[txID] = append(unspentOutputs[txID], outIdx)
                }
            }
        }
    })

    return accumulated, unspentOutputs
}
```

یا بالانس را بررسی کنید:

```go
func (u UTXOSet) FindUTXO(pubKeyHash []byte) []TXOutput {
    var UTXOs []TXOutput
    db := u.Blockchain.db

    err := db.View(func(tx *bolt.Tx) error {
        b := tx.Bucket([]byte(utxoBucket))
        c := b.Cursor()

        for k, v := c.First(); k != nil; k, v = c.Next() {
            outs := DeserializeOutputs(v)

            for _, out := range outs.Outputs {
                if out.IsLockedWithKey(pubKeyHash) {
                    UTXOs = append(UTXOs, out)
                }
            }
        }

        return nil
    })

    return UTXOs
}
```

اینها نسخه های کمی تغییر یافته از روش های بلاکچین مربوطه هستند. آن روش های بلاک چین دیگر مورد نیاز نیست.

داشتن مجموعه UTXO به این معنی است که داده ها (معاملات) ما اکنون به ذخیره سازی تقسیم می شوند: تراکنش های واقعی در بلاک چین ذخیره می شوند و خروجی های مصرف نشده در مجموعه UTXO ذخیره می شوند. چنین جداسازی به مکانیزم همگام سازی مستحکم نیاز دارد زیرا ما می خواهیم مجموعه UTXO همیشه به روز شود و خروجی های تراکنش های اخیر ذخیره شود. اما ما نمی خواهیم هر بار که یک بلوک جدید استخراج می شود دوباره ایندکس کنیم زیرا این اسکن های مکرر بلاکچین است که می خواهیم از آن اجتناب کنیم. بنابراین، ما به مکانیزمی برای به روز رسانی مجموعه UTXO نیاز داریم:

```go
func (u UTXOSet) Update(block *Block) {
    db := u.Blockchain.db

    err := db.Update(func(tx *bolt.Tx) error {
        b := tx.Bucket([]byte(utxoBucket))

        for _, tx := range block.Transactions {
            if tx.IsCoinbase() == false {
                for _, vin := range tx.Vin {
                    updatedOuts := TXOutputs{}
                    outsBytes := b.Get(vin.Txid)
                    outs := DeserializeOutputs(outsBytes)

                    for outIdx, out := range outs.Outputs {
                        if outIdx != vin.Vout {
                            updatedOuts.Outputs = append(updatedOuts.Outputs, out)
                        }
                    }

                    if len(updatedOuts.Outputs) == 0 {
                        err := b.Delete(vin.Txid)
                    } else {
                        err := b.Put(vin.Txid, updatedOuts.Serialize())
                    }

                }
            }

            newOutputs := TXOutputs{}
            for _, out := range tx.Vout {
                newOutputs.Outputs = append(newOutputs.Outputs, out)
            }

            err := b.Put(tx.ID, newOutputs.Serialize())
        }
    })
}
```

این روش بزرگ به نظر می رسد، اما کاری که انجام می دهد کاملاً ساده است. هنگامی که یک بلوک جدید استخراج می شود، مجموعه UTXO باید به روز شود. به روز رسانی به معنای حذف خروجی های صرف شده و افزودن خروجی های خرج نشده از تراکنش های تازه استخراج شده است. اگر تراکنشی که خروجی آن حذف شده است، خروجی دیگری نداشته باشد، آن نیز حذف می شود. خیلی ساده!

بیایید اکنون از مجموعه UTXO در جایی که لازم است استفاده کنیم:

```go
func (cli *CLI) createBlockchain(address string) {
    ...
    bc := CreateBlockchain(address)
    defer bc.db.Close()

    UTXOSet := UTXOSet{bc}
    UTXOSet.Reindex()
    ...
}
```

ایندکس مجدد درست پس از ایجاد یک بلاکچین جدید انجام می شود. در حال حاضر، این تنها جایی است که از Reindex استفاده می‌شود، حتی اگر در اینجا بیش از حد به نظر می‌رسد، زیرا در ابتدای یک بلاک چین تنها یک بلوک با یک تراکنش وجود دارد و به‌جای آن می‌توان از Update استفاده کرد. اما ممکن است در آینده به مکانیسم نمایه سازی مجدد نیاز داشته باشیم.

```go
func (cli *CLI) send(from, to string, amount int) {
    ...
    newBlock := bc.MineBlock(txs)
    UTXOSet.Update(newBlock)
}
```

و مجموعه UTXO پس از استخراج یک بلوک جدید به روز می شود.

بیایید بررسی کنیم که کار می کند

```txt
$ blockchain_go createblockchain -address 1JnMDSqVoHi4TEFXNw5wJ8skPsPf4LHkQ1
00000086a725e18ed7e9e06f1051651a4fc46a315a9d298e59e57aeacbe0bf73
Done!

$ blockchain_go send -from 1JnMDSqVoHi4TEFXNw5wJ8skPsPf4LHkQ1 -to 12DkLzLQ4B3gnQt62EPRJGZ38n3zF4Hzt5 -amount 6
0000001f75cb3a5033aeecbf6a8d378e15b25d026fb0a665c7721a5bb0faa21b
Success!

$ blockchain_go send -from 1JnMDSqVoHi4TEFXNw5wJ8skPsPf4LHkQ1 -to 12ncZhA5mFTTnTmHq1aTPYBri4jAK8TacL -amount 4  
000000cc51e665d53c78af5e65774a72fc7b864140a8224bf4e7709d8e0fa433
Success!

$ blockchain_go getbalance -address 1JnMDSqVoHi4TEFXNw5wJ8skPsPf4LHkQ1
Balance of '1F4MbuqjcuJGymjcuYQMUVYB37AWKkSLif': 20

$ blockchain_go getbalance -address 12DkLzLQ4B3gnQt62EPRJGZ38n3zF4Hzt5  
Balance of '1XWu6nitBWe6J6v6MXmd5rhdP7dZsExbx': 6

$ blockchain_go getbalance -address 12ncZhA5mFTTnTmHq1aTPYBri4jAK8TacL
Balance of '13UASQpCR8Nr41PojH8Bz4K6cmTCqweskL': 4
```


خب! آدرس 1JnMDSqVoHi4TEFXNw5wJ8skPsPf4LHkQ1 سه بار جایزه دریافت کرد:

۱- یک بار برای استخراج بلوک های پیدایش.

۲- یک بار برای استخراج بلوک 0000001f75cb3a5033aeecbf6a8d378e15b25d026fb0a665c7721a5bb0faa21b.

۳- و یک بار برای ماینینگ بلوک 000000cc51e665d53c78af5e65774a72fc7b864140a8224bf4e7709d8e0fa433

#### درخت مرکل یا Merkle Tree

یک مکانیسم بهینه سازی دیگر وجود دارد که می خواهم در این پست در مورد آن صحبت کنم.

همانطور که در بالا گفته شد، پایگاه داده کامل بیت کوین (یعنی بلاکچین) بیش از 140 گیگابایت فضای دیسک را اشغال می کند. به دلیل ماهیت غیرمتمرکز بیت کوین، هر گره در شبکه باید مستقل و خودکفا باشد، یعنی هر گره باید یک نسخه کامل از بلاکچین را ذخیره کند. با توجه به اینکه بسیاری از مردم شروع به استفاده از بیت کوین کرده اند، پیروی از این قانون دشوارتر می شود: بعید است که همه یک گره کامل را اجرا کنند. همچنین، از آنجایی که گره‌ها مشارکت‌کنندگان کامل شبکه هستند، مسئولیت‌هایی دارند: باید تراکنش‌ها و بلوک‌ها را تأیید کنند. همچنین، برای تعامل با گره‌های دیگر و دانلود بلوک‌های جدید، ترافیک اینترنتی خاصی مورد نیاز است.

_**[در مقاله اولیه بیت کوین](https://bitcoin.org/bitcoin.pdf)**_ منتشر شده توسط ساتوشی ناکاموتو، راه حلی برای این مشکل وجود داشت: تأیید پرداخت ساده یا Simplified Payment Verification به اختصار (SPV). SPV یک گره سبک بیت کوین است که کل بلاکچین را دانلود نمی کند و _**بلوک ها و تراکنش ها را تأیید نمی کند.**_ در عوض، تراکنش‌ها را در بلوک‌ها (برای تأیید پرداخت‌ها) پیدا می‌کند و به یک گره کامل متصل می‌شود تا فقط داده‌های ضروری را بازیابی کند. این مکانیسم امکان داشتن چندین گره کیف پول سبک را با اجرای تنها یک گره کامل فراهم می کند.

برای اینکه SPV امکان پذیر باشد، باید راهی برای بررسی اینکه آیا یک بلوک حاوی تراکنش خاصی است بدون دانلود کل بلوک وجود دارد. و اینجاست که درخت مرکل وارد بازی می شود.

درختان مرکل توسط بیت کوین برای به دست آوردن هش تراکنش ها استفاده می شود، که سپس در هدر بلوک ذخیره می شود و توسط سیستم اثبات کار در نظر گرفته می شود. تا به حال، ما فقط هش های هر تراکنش را در یک بلوک به هم متصل می کردیم و SHA-256 را روی آنها اعمال می کردیم. این نیز یک راه خوب برای دریافت یک نمایش منحصر به فرد از تراکنش های بلوکی است، اما مزایای درختان مرکل را ندارد.

بیایید به درخت مرکل نگاه کنیم:

{{< image src="/images/blockchain/jcq56ahem3aa.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}


یک درخت مرکل برای هر بلوک ساخته می‌شود و با برگ‌ها (پایین درخت) شروع می‌شود، جایی که یک برگ یک هش تراکنش است (بیت‌کوین از هش دوگانه SHA256 استفاده می‌کند). تعداد برگ‌ها باید زوج باشد، اما هر بلوک دارای تعداد تراکنش زوج نیست. در صورت وجود تعداد فرد از تراکنش ها، آخرین تراکنش کپی می شود (در درخت مرکل، نه در بلوک!).

با حرکت از پایین به بالا، برگ‌ها به صورت جفت گروه‌بندی می‌شوند، هش‌های آن‌ها به هم متصل می‌شوند و یک هش جدید از هش‌های پیوسته به دست می‌آید. هش های جدید گره های درختی جدید را تشکیل می دهند. این فرآیند تا زمانی تکرار می شود که فقط یک گره وجود داشته باشد که ریشه درخت نامیده می شود. سپس هش ریشه به عنوان نمایش منحصر به فرد تراکنش ها استفاده می شود، در هدرهای بلوک ذخیره می شود و در سیستم اثبات کار استفاده می شود.

مزیت درختان مرکل این است که یک گره می تواند عضویت تراکنش خاصی را بدون دانلود کل بلوک تأیید کند. فقط یک هش تراکنش، یک هش ریشه درخت Merkle و یک مسیر Merkle برای این مورد نیاز است.

در آخر بیایید کد بنویسیم:

```go
type MerkleTree struct {
    RootNode *MerkleNode
}

type MerkleNode struct {
    Left  *MerkleNode
    Right *MerkleNode
    Data  []byte
}
```

ما با ساختارها شروع می کنیم. هر MerkleNode داده ها و پیوندهایی را به شاخه های خود نگه می دارد. MerkleTree در واقع گره ریشه است که به گره های بعدی پیوند می خورد که به نوبه خود به گره های بعدی و غیره مرتبط می شوند.

بیایید ابتدا یک گره جدید ایجاد کنیم:

```go
func NewMerkleNode(left, right *MerkleNode, data []byte) *MerkleNode {
    mNode := MerkleNode{}

    if left == nil && right == nil {
        hash := sha256.Sum256(data)
        mNode.Data = hash[:]
    } else {
        prevHashes := append(left.Data, right.Data...)
        hash := sha256.Sum256(prevHashes)
        mNode.Data = hash[:]
    }

    mNode.Left = left
    mNode.Right = right

    return &mNode
}
```

هر گره حاوی مقداری داده است. هنگامی که یک گره یک برگ است، داده ها از خارج منتقل می شوند (در مورد ما یک تراکنش سریالی). هنگامی که یک گره به گره های دیگر پیوند داده می شود، داده های آنها را می گیرد و آنها را به هم متصل می کند و هش می کند.

```go
func NewMerkleTree(data [][]byte) *MerkleTree {
    var nodes []MerkleNode

    if len(data)%2 != 0 {
        data = append(data, data[len(data)-1])
    }

    for _, datum := range data {
        node := NewMerkleNode(nil, nil, datum)
        nodes = append(nodes, *node)
    }

    for i := 0; i < len(data)/2; i++ {
        var newLevel []MerkleNode

        for j := 0; j < len(nodes); j += 2 {
            node := NewMerkleNode(&nodes[j], &nodes[j+1], nil)
            newLevel = append(newLevel, *node)
        }

        nodes = newLevel
    }

    mTree := MerkleTree{&nodes[0]}

    return &mTree
}
```

هنگامی که یک درخت جدید ایجاد می شود، اولین چیزی که باید اطمینان حاصل کرد این است که تعداد برگ های زوج وجود داشته باشد. پس از آن، داده ها (که آرایه ای از تراکنش های سریالی است) به برگ درخت تبدیل می شود و درختی از این برگ ها رشد می کند.

اکنون، اجازه دهید Block.HashTransactions را تغییر دهیم، که در سیستم اثبات کار برای به دست آوردن هش تراکنش ها استفاده می شود:

```go
func (b *Block) HashTransactions() []byte {
    var transactions [][]byte

    for _, tx := range b.Transactions {
        transactions = append(transactions, tx.Serialize())
    }
    mTree := NewMerkleTree(transactions)

    return mTree.RootNode.Data
}
```

ابتدا، تراکنش ها سریالی می شوند (با استفاده از `encoding/gob`)، و سپس از آنها برای ساخت درخت مرکل استفاده می شود. ریشه درخت به عنوان شناسه منحصر به فرد تراکنش های بلوک عمل می کند.

#### پرداخت به هش کلید عمومی یا Pay to Public Key Hash یا به اختصار P2PKH

یک چیز دیگر وجود دارد که می خواهم با جزئیات بیشتر در مورد آن صحبت کنم.

همانطور که به یاد دارید، در بیت کوین زبان برنامه نویسی Script وجود دارد که برای قفل کردن خروجی های تراکنش استفاده می شود. و ورودی های تراکنش داده هایی را برای باز کردن قفل خروجی ها فراهم می کنند. زبان ساده است و کد در این زبان فقط دنباله ای از داده ها و عملگرها است. این مثال را در نظر بگیرید:

`5 2 OP_ADD 7 OP_EQUAL`

مقادیر 5، 2 و 7 داده ها هستند. OP_ADD و OP_EQUAL عملگرها هستند. کد اسکریپت از چپ به راست اجرا می شود: هر قطعه داده در پشته قرار می گیرد و عملگر بعدی به عناصر پشته بالایی اعمال می شود. پشته اسکریپت فقط یک حافظه ساده FILO (اولین ورودی آخرین خروجی) است: اولین عنصر در پشته آخرین عنصری است که باید برداشته شود، با هر عنصر دیگری که روی عنصر قبلی قرار می‌گیرد.

بیایید اجرای اسکریپت بالا را به مراحل تقسیم کنیم:

```txt
1.  Stack: empty. Script: 5 2 OP_ADD 7 OP_EQUAL.
2.  Stack: 5. Script: 2 OP_ADD 7 OP_EQUAL.
3.  Stack: 5 2. Script: OP_ADD 7 OP_EQUAL.
4.  Stack: 7. Script: 7 OP_EQUAL.
5.  Stack: 7 7. Script: OP_EQUAL.
6.  Stack: true. Script: empty.
```

این OP_ADD دو عنصر را از پشته می گیرد، آنها را خلاصه می کند و مجموع را به پشته پوش می کند. OP_EQUAL دو عنصر را از پشته می گیرد و آنها را با هم مقایسه می کند: اگر برابر باشند، به پشته true پوش می کند. در غیر این صورت false را پوش می کند. نتیجه اجرای یک اسکریپت مقدار عنصر پشته بالایی است: در مورد ما، درست است، به این معنی که اسکریپت با موفقیت به پایان رسید.

حال بیایید به اسکریپتی که در بیت کوین برای انجام پرداخت ها استفاده می شود نگاه کنیم:

```txt
<signature> <pubKey> OP_DUP OP_HASH160 <pubKeyHash> OP_EQUALVERIFY OP_CHECKSIG
```

این اسکریپت Pay to Public Key Hash (P2PKH) نامیده می شود و این اسکریپت رایج ترین اسکریپت مورد استفاده در بیت کوین است. به معنای واقعی کلمه به یک هش کلید عمومی پرداخت می کند، یعنی سکه ها را با یک کلید عمومی خاص قفل می کند. این قلب پرداخت بیت کوین است: هیچ حسابی وجود ندارد، هیچ وجهی بین آنها انتقال نمی یابد. فقط یک اسکریپت وجود دارد که بررسی می کند امضای ارائه شده و کلید عمومی صحیح است.

اسکریپت در واقع در دو قسمت ذخیره می شود:

۱- اولین قطعه، <signature> <pubKey>، در فیلد ScriptSig ورودی ذخیره می شود.

۲- قطعه دوم، OP_DUP OP_HASH160 <pubKeyHash> OP_EQUALVERIFY OP_CHECKSIG در ScriptPubKey خروجی ذخیره می شود.

بنابراین، این خروجی‌ها هستند که منطق باز کردن قفل را تعریف می‌کنند، و این ورودی‌ها هستند که داده‌هایی را برای باز کردن خروجی‌ها فراهم می‌کنند. بیایید اسکریپت را اجرا کنیم:

```txt
1.  Stack: empty  
    Script: <signature> <pubKey> OP_DUP OP_HASH160 <pubKeyHash> OP_EQUALVERIFY OP_CHECKSIG
2.  Stack: <signature>
    Script: <pubKey> OP_DUP OP_HASH160 <pubKeyHash> OP_EQUALVERIFY OP_CHECKSIG
3.  Stack: <signature> <pubKey>
    Script: OP_DUP OP_HASH160 <pubKeyHash> OP_EQUALVERIFY OP_CHECKSIG
4.  Stack: <signature> <pubKey> <pubKey>
    Script: OP_HASH160 <pubKeyHash> OP_EQUALVERIFY OP_CHECKSIG
5.  Stack: <signature> <pubKey> <pubKeyHash>
    Script: <pubKeyHash> OP_EQUALVERIFY OP_CHECKSIG
6.  Stack: <signature> <pubKey> <pubKeyHash> <pubKeyHash>
    Script: OP_EQUALVERIFY OP_CHECKSIG
7.  Stack: <signature> <pubKey>
    Script: OP_CHECKSIG
8.  Stack: true or false. Script: empty.
```

این OP_DUP عنصر پشته بالایی را کپی می کند. OP_HASH160 عنصر بالای پشته را می گیرد و آن را با RIPEMD160 هش می کند. نتیجه به پشته وارد می شود. OP_EQUALVERIFY دو عنصر پشته بالایی را با هم مقایسه می‌کند و اگر برابر نباشند، اجرای اسکریپت را قطع می‌کند. OP_CHECKSIG امضای یک تراکنش را با هش کردن تراکنش و استفاده از <signature> و <pubKey> تأیید می کند. اپراتور دوم بسیار پیچیده است: یک کپی کوتاه شده از تراکنش ایجاد می کند، آن را هش می کند (زیرا هش تراکنش امضا شده است)، و درستی امضا را با استفاده از <signature> و <pubKey> بررسی می کند.

داشتن چنین زبان برنامه نویسی به بیت کوین اجازه می دهد تا یک پلت فرم قرارداد هوشمند نیز باشد: این زبان علاوه بر انتقال به یک کلید، طرح های پرداخت دیگری را نیز ممکن می سازد.

#### نتیجه گیری

و بس! ما تقریباً تمام ویژگی های کلیدی یک ارز دیجیتال مبتنی بر بلاکچین را پیاده سازی کرده ایم. ما بلاکچین، آدرس، استخراج و تراکنش داریم. اما یک چیز دیگر وجود دارد که به همه این مکانیسم ها جان می دهد و بیت کوین را به یک سیستم جهانی تبدیل می کند: اجماع. در مقاله بعدی، اجرای بخش «غیرمتمرکز» بلاکچین را آغاز خواهیم کرد. گوش به زنگ باشید!

پایان قسمت ششم

_**[قسمت هفتم](/blog/post_46_bc7)**_

_**باقی قسمت ها نسخه اصلی**_

* [Building Blockchain in Go. Part 1: Basic Prototype](https://jeiwan.net/posts/building-blockchain-in-go-part-1/)
* [Building Blockchain in Go. Part 2: Proof-of-Work](https://jeiwan.net/posts/building-blockchain-in-go-part-2/)
* [Building Blockchain in Go. Part 3: Persistence and CLI](https://jeiwan.net/posts/building-blockchain-in-go-part-3/)
* [Building Blockchain in Go. Part 4: Transactions 1](https://jeiwan.net/posts/building-blockchain-in-go-part-4/)
* [Building Blockchain in Go. Part 5: Addresses](https://jeiwan.net/posts/building-blockchain-in-go-part-5/)
* [Building Blockchain in Go. Part 6: Transactions 2](https://jeiwan.net/posts/building-blockchain-in-go-part-6/)
* [Building Blockchain in Go. Part 7: Network](https://jeiwan.net/posts/building-blockchain-in-go-part-7/)

