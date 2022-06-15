---
title: ساخت بلاکچین با نگاهی به ساختار بین کوین - قسمت چهارم (تراکنش ها-۱)
image: images/blockchain/y4ilhv4wagyi.webp
description: مقدمه تراکنش ها قلب بیت کوین هستند و تنها هدف بلاکچین ذخیره تراکنش ها به روشی ایمن و قابل اعتماد است، بنابراین هیچ کس نمی تواند آنها را پس ا…
date: 2022-06-15
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

تراکنش ها قلب بیت کوین هستند و تنها هدف بلاکچین ذخیره تراکنش ها به روشی ایمن و قابل اعتماد است، بنابراین هیچ کس نمی تواند آنها را پس از ایجاد تغییر دهد. امروز ما اجرای تراکنش ها را آغاز می کنیم. اما از آنجایی که این یک موضوع کاملاً بزرگ است، آن را به دو بخش تقسیم می‌کنم: در این بخش، مکانیسم کلی تراکنش‌ها را پیاده‌سازی می‌کنیم و در قسمت دوم، جزئیات را بررسی می‌کنیم.

همچنین، از آنجایی که تغییرات کد بسیار زیاد است، توصیف همه آنها در اینجا بی معنی است. شما می توانید تمام تغییرات را _**[اینجا](https://github.com/Jeiwan/blockchain_go/compare/part_3...part_4#files_bucket)**_ ببینید.

### قاشقی وجود ندارد

اگر تا به حال یک برنامه وب ایجاد کرده اید، به منظور اجرای پرداخت ها، احتمالاً این جداول را در یک DB ایجاد می کنید: حساب ها (`accounts`) و تراکنش ها (`transactions`). یک حساب اطلاعات مربوط به یک کاربر، از جمله اطلاعات شخصی و موجودی او را ذخیره می کند، و یک تراکنش اطلاعات مربوط به انتقال پول از یک حساب به حساب دیگر را ذخیره می کند. در بیت کوین، پرداخت ها به روشی کاملا متفاوت انجام می شود. آنها موارد زیر هستند :

۱- بدون حساب.

۲- بدون بالانس یا موجودی

۳- بدون آدرس

۴- بدون سکه

۵- بدون فرستنده و گیرنده

از آنجایی که بلاکچین یک پایگاه داده عمومی و باز است، ما نمی‌خواهیم اطلاعات حساس در مورد صاحبان کیف پول را ذخیره کنیم. سکه ها در حساب ها جمع آوری نمی شوند. تراکنش ها پولی را از یک آدرس به آدرس دیگر منتقل نمی کنند. هیچ فیلد یا مشخصه ای وجود ندارد که موجودی حساب را نگه دارد. فقط معاملات وجود دارد. اما درون یک تراکنش چیست؟

#### تراکنش بیت کوین

تراکنش ترکیبی از ورودی و خروجی است:

```go
type Transaction struct {
	ID   []byte
	Vin  []TXInput
	Vout []TXOutput
}
```

ورودی‌های یک تراکنش جدید، خروجی‌های یک تراکنش قبلی است (البته یک استثنا وجود دارد که بعداً درباره آن صحبت خواهیم کرد). خروجی ها جایی هستند که سکه ها در واقع ذخیره می شوند. نمودار زیر ارتباط متقابل تراکنش ها را نشان می دهد:

{{< image src="/images/blockchain/vjocfdj115fw.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

توجه کنید که:

۱- خروجی هایی هستند که به ورودی ها مرتبط نیستند.

۲- در یک تراکنش، ورودی ها می توانند به خروجی های چند تراکنش اشاره کنند.

۳- یک ورودی باید به یک خروجی اشاره کند.

در طول این مقاله، ما از کلماتی مانند "پول" (money)، "سکه"(coins)، "خرج کردن"(spend)، "ارسال"(send)، "حساب"(account) و غیره استفاده خواهیم کرد. اما چنین مفاهیمی در بیت کوین وجود ندارد. تراکنش‌ها فقط مقادیر را با یک اسکریپت قفل می‌کنند، که فقط توسط کسی که آنها را قفل کرده است می‌تواند باز شود.

#### خروجی های تراکنش یا Transaction Outputs

ابتدا با خروجی ها شروع می کنیم:

```go
type TXOutput struct {
	Value        int
	ScriptPubKey string
}
```

در واقع، این خروجی‌ها هستند که «سکه‌ها» را ذخیره می‌کنند (به قسمت Value در بالا توجه کنید) و ذخیره به معنای قفل کردن آنها با یک پازل است که در _**ScriptPubKey**_ ذخیره می شود. در داخل، بیت کوین از یک زبان برنامه نویسی به نام _Script_ استفاده می کند که برای تعریف منطق قفل و باز کردن خروجی ها استفاده می شود. این زبان کاملاً ابتدایی است (این به طور عمدی ساخته شده است تا از هک ها و سوء استفاده های احتمالی جلوگیری شود)، اما ما در مورد آن با جزئیات صحبت نمی کنیم. در _**[اینجا](https://en.bitcoin.it/wiki/Script)**_ می توانید توضیح دقیقی در مورد آن پیدا کنید.

> در بیت کوین، فیلد Value تعداد ساتوشی ها را ذخیره می کند، نه تعداد بیت کوین. ساتوشی صد میلیونیم بیت کوین (0.00000001 BTC) است، بنابراین این کوچکترین واحد ارز در بیت کوین (مانند یک سنت) است.

از آنجایی که ما آدرس‌ها را پیاده‌سازی نکرده ایم، فعلاً از کل منطق مربوط به اسکریپت‌نویسی اجتناب می‌کنیم. _**ScriptPubKey**_ یک رشته دلخواه (آدرس کیف پول تعریف شده توسط کاربر) را ذخیره می کند.

> به هر حال، داشتن چنین زبان برنامه نویسی به این معنی است که بیت کوین می تواند به عنوان یک پلتفرم قرارداد هوشمند (smart-contract) نیز مورد استفاده قرار گیرد.

یک چیز مهم در مورد خروجی ها این است که آنها تقسیم ناپذیر (**indivisible**) هستند، به این معنی که شما نمی توانید بخشی از مقدار آن را ارجاع دهید. هنگامی که یک خروجی در یک تراکنش جدید ارجاع داده می شود، به عنوان یک کل مصرف می شود. و اگر مقدار آن بیشتر از مقدار مورد نیاز باشد، تغییر ایجاد شده و به فرستنده بازگردانده می شود. این شبیه شرایط دنیای واقعی است که شما مثلاً یک اسکناس 5 دلاری را برای چیزی که 1 دلار قیمت دارد پرداخت می کنید و 4 دلار برگردانده می شود.

#### ورودی های تراکنش یا Transaction Inputs

و این ورودی است:

```go
type TXInput struct {
	Txid      []byte
	Vout      int
	ScriptSig string
}
```

همانطور که قبلا ذکر شد، یک ورودی به خروجی قبلی اشاره می کند: _**Txid**_ شناسه چنین تراکنش را ذخیره می کند و _**Vout**_ اندیس یک خروجی را در تراکنش ذخیره می کند. _**ScriptSig**_ یک اسکریپت است که داده هایی را برای استفاده در _**ScriptPubKey**_ یک خروجی فراهم می کند.

اگر داده ها درست باشند، خروجی را می توان باز کرد و از مقدار آن برای تولید خروجی های جدید استفاده کرد. اگر درست نباشد، خروجی را نمی توان در ورودی ارجاع داد. این مکانیزمی است که تضمین می کند کاربران نمی توانند سکه های متعلق به افراد دیگر را خرج کنند.

باز هم، از آنجایی که ما هنوز آدرسی را پیاده سازی نکرده ایم، _**ScriptSig**_ فقط یک آدرس کیف پول دلخواه تعریف شده توسط کاربر را ذخیره می کند. در مقاله بعدی بررسی کلیدهای عمومی و امضاها را اجرا خواهیم کرد.

بیایید آن را خلاصه کنیم. خروجی ها جایی هستند که "سکه ها" ذخیره می شوند. هر خروجی دارای یک اسکریپت باز کردن قفل است که منطق باز کردن قفل خروجی را تعیین می کند. هر تراکنش جدید باید حداقل یک ورودی و خروجی داشته باشد. یک ورودی به خروجی یک تراکنش قبلی اشاره می کند و داده هایی (فیلد _**ScriptSig**_) را ارائه می دهد که در اسکریپت باز کردن قفل خروجی برای باز کردن قفل آن و استفاده از مقدار آن برای ایجاد خروجی های جدید استفاده می شود.

اما چه چیزی اول شد: ورودی یا خروجی؟

#### اول تخم مرغ بوده. (اشاره به اول مرغ بوده یا تخم مرغ)

در بیت کوین، این تخم مرغ است که قبل از مرغ آمده است. منطق ورودی-ارجاع-خروجی ها وضعیت کلاسیک "مرغ یا تخم مرغ" است. ورودی ها خروجی ها را تولید می کنند و خروجی ها ورودی ها را ممکن می کنند. و در بیت کوین، خروجی ها قبل از ورودی ها قرار می گیرند.

هنگامی که یک ماینر شروع به استخراج یک بلوک می کند، یک _**تراکنش کوین بیس یا**_ **coinbase transaction** به آن اضافه می کند. تراکنش کوین بیس نوع خاصی از تراکنش است که به خروجی های قبلی نیاز ندارد. خروجی‌ها (یعنی «سکه‌ها») را از هیچ‌جا ایجاد می‌کند. تخم مرغ بدون مرغ این پاداشی است که ماینرها برای استخراج بلوک های جدید دریافت می کنند.

همانطور که می دانید، بلوک پیدایش (genesis block) در ابتدای زنجیره بلوکی وجود دارد. این بلوک است که اولین خروجی را در بلاک چین تولید می کند. و هیچ خروجی قبلی لازم نیست زیرا هیچ تراکنش قبلی و چنین خروجی وجود ندارد.

بیایید یک تراکنش کوین بیس ایجاد کنیم:

```go
func NewCoinbaseTX(to, data string) *Transaction {
	if data == "" {
		data = fmt.Sprintf("Reward to '%s'", to)
	}

	txin := TXInput{[]byte{}, -1, data}
	txout := TXOutput{subsidy, to}
	tx := Transaction{nil, []TXInput{txin}, []TXOutput{txout}}
	tx.SetID()

	return &tx
}
```

یک تراکنش کوین بیس تنها یک ورودی دارد. در اجرای ما _**Txid**_ آن خالی است و _**Vout**_ برابر با **1-** است. همچنین، یک تراکنش coinbase یک اسکریپت را در _**ScriptSig**_ ذخیره نمی کند. در عوض، داده های دلخواه در آنجا ذخیره می شود.

> در بیت کوین، اولین تراکنش کوین بیس حاوی پیام زیر است: "The Times 03/Jan/2009 Chancellor on brink of second bailout for banks”". _**[خودت میتونی ببینیش](https://blockchain.info/tx/4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b?show_adv=true)**_

این `subsidy` مقدار پاداش است. در بیت کوین، این عدد در هیچ کجا ذخیره نمی شود و فقط بر اساس تعداد کل بلاک ها محاسبه می شود: تعداد بلاک ها بر 210000 تقسیم می شود. استخراج بلوک پیدایش 50 BTC تولید می کند و هر 210000 بلوک مقدار پاداش نصف می شود. در اجرای خود، پاداش را به عنوان یک ثابت ذخیره می کنیم (حداقل در حال حاضر 😉).

#### ذخیره تراکنش ها در بلاک چین

از این پس، هر بلوک باید حداقل یک تراکنش را ذخیره کند و دیگر امکان استخراج بلوک بدون تراکنش وجود ندارد. این بدان معنی است که ما باید فیلد Data را از Block حذف کنیم و به جای آن تراکنش ها را ذخیره کنیم:  

```go
type Block struct {
	Timestamp     int64
	Transactions  []*Transaction
	PrevBlockHash []byte
	Hash          []byte
	Nonce         int
}
```

دو تابع NewBlock و NewGenesisBlock نیز باید بر این اساس تغییر کنند:

```go
func NewBlock(transactions []*Transaction, prevBlockHash []byte) *Block {
	block := &Block{time.Now().Unix(), transactions, prevBlockHash, []byte{}, 0}
	...
}

func NewGenesisBlock(coinbase *Transaction) *Block {
	return NewBlock([]*Transaction{coinbase}, []byte{})
}
```

چیزی که باید تغییر کند ایجاد یک بلاکچین جدید است:

```go
func CreateBlockchain(address string) *Blockchain {
	...
	err = db.Update(func(tx *bolt.Tx) error {
		cbtx := NewCoinbaseTX(address, genesisCoinbaseData)
		genesis := NewGenesisBlock(cbtx)

		b, err := tx.CreateBucket([]byte(blocksBucket))
		err = b.Put(genesis.Hash, genesis.Serialize())
		...
	})
	...
}
```

اکنون، تابع آدرسی می گیرد که پاداش استخراج بلوک پیدایش را دریافت می کند.

#### اصلاح الگوریتم Proof-of-Work

الگوریتم Proof-of-Work باید تراکنش های ذخیره شده در یک بلوک را در نظر بگیرد تا ثبات و قابلیت اطمینان بلاکچین را به عنوان ذخیره تراکنش تضمین کند. بنابراین اکنون باید روش _**ProofOfWork.prepareData**_ را اصلاح کنیم:

```go
func (pow *ProofOfWork) prepareData(nonce int) []byte {
	data := bytes.Join(
		[][]byte{
			pow.block.PrevBlockHash,
			pow.block.HashTransactions(), // This line was changed
			IntToHex(pow.block.Timestamp),
			IntToHex(int64(targetBits)),
			IntToHex(int64(nonce)),
		},
		[]byte{},
	)

	return data
}
```

به جای _**pow.block.Data**_، اکنون از تابع  _**pow.block.HashTransactions**_ استفاده می کنیم که عبارت است از:

```go
func (b *Block) HashTransactions() []byte {
	var txHashes [][]byte
	var txHash [32]byte

	for _, tx := range b.Transactions {
		txHashes = append(txHashes, tx.ID)
	}
	txHash = sha256.Sum256(bytes.Join(txHashes, []byte{}))

	return txHash[:]
}
```

باز هم، ما از هش به عنوان مکانیزمی برای ارائه نمایش منحصر به فرد داده ها استفاده می کنیم. ما می‌خواهیم همه تراکنش‌های یک بلوک به‌طور منحصربه‌فرد توسط یک هش شناسایی شوند. برای رسیدن به این هدف، هش های هر تراکنش را دریافت می کنیم، آنها را به هم متصل می کنیم و یک هش از ترکیب الحاقی را دریافت می کنیم.

> بیت کوین از تکنیک پیچیده تری استفاده می کند: تمام تراکنش های موجود در یک بلوک را به عنوان درخت مرکل یا [Merkle tree](https://en.wikipedia.org/wiki/Merkle_tree) نشان می دهد و از ریشه هش درخت در سیستم اثبات کار استفاده می کند. این رویکرد به شما امکان می دهد تا به سرعت بررسی کنید که آیا یک بلوک حاوی تراکنش خاصی است، فقط با داشتن هش ریشه و بدون دانلود همه تراکنش ها.

بیایید بررسی کنیم که همه چیز تا اینجا درست است:

```txt
$ blockchain_go createblockchain -address Ivan
00000093450837f8b52b78c25f8163bb6137caf43ff4d9a01d1b731fa8ddcc8a
Done!
```

خب! ما اولین جایزه استخراج را دریافت کردیم. اما چگونه موجودی یک کاربر یا بالانس را بررسی کنیم؟

#### خروجی های تراکنش خرج نشده یا Unspent Transaction Outputs

ما باید تمام خروجی های تراکنش مصرف نشده (UTXO) را پیدا کنیم. مصرف نشده به این معنی است که این خروجی ها در هیچ ورودی ارجاع نشده اند. در نمودار بالا، این موارد عبارتند از:

```txt
1.  tx0, output 1;
2.  tx1, output 0;
3.  tx3, output 0;
4.  tx4, output 0.
```

البته، وقتی بالانس یا موجودی را بررسی می‌کنیم، به همه آن‌ها نیاز نداریم، بلکه فقط به آن‌هایی نیاز داریم که می‌توان با کلیدی که در اختیار ماست، قفل آن را باز کرد (در حال حاضر کلیدها پیاده‌سازی نشده اند و به جای آن از آدرس‌های تعریف‌شده کاربر استفاده می‌کنیم). ابتدا، بیایید روش های قفل-باز کردن قفل (locking-unlocking) را در ورودی ها و خروجی ها تعریف کنیم:

در اینجا ما فقط فیلدهای اسکریپت را با unlockingData مقایسه می کنیم. پس از پیاده سازی آدرس ها بر اساس کلیدهای خصوصی، این قطعات در مقاله آینده بهبود خواهند یافت.

```go
func (in *TXInput) CanUnlockOutputWith(unlockingData string) bool {
	return in.ScriptSig == unlockingData
}

func (out *TXOutput) CanBeUnlockedWith(unlockingData string) bool {
	return out.ScriptPubKey == unlockingData
}
```

مرحله بعدی - یافتن تراکنش های حاوی خروجی های خرج نشده - این مورد بسیار دشوار است:

```go
func (bc *Blockchain) FindUnspentTransactions(address string) []Transaction {
  var unspentTXs []Transaction
  spentTXOs := make(map[string][]int)
  bci := bc.Iterator()

  for {
    block := bci.Next()

    for _, tx := range block.Transactions {
      txID := hex.EncodeToString(tx.ID)

    Outputs:
      for outIdx, out := range tx.Vout {
        // Was the output spent?
        if spentTXOs[txID] != nil {
          for _, spentOut := range spentTXOs[txID] {
            if spentOut == outIdx {
              continue Outputs
            }
          }
        }

        if out.CanBeUnlockedWith(address) {
          unspentTXs = append(unspentTXs, *tx)
        }
      }

      if tx.IsCoinbase() == false {
        for _, in := range tx.Vin {
          if in.CanUnlockOutputWith(address) {
            inTxID := hex.EncodeToString(in.Txid)
            spentTXOs[inTxID] = append(spentTXOs[inTxID], in.Vout)
          }
        }
      }
    }

    if len(block.PrevBlockHash) == 0 {
      break
    }
  }

  return unspentTXs
}
```

از آنجایی که تراکنش ها در بلوک ها ذخیره می شوند، ما باید هر بلوک را در یک زنجیره بلوکی بررسی کنیم. با خروجی ها شروع می کنیم:

```go
if out.CanBeUnlockedWith(address) {
	unspentTXs = append(unspentTXs, tx)
}
```

اگر خروجی با همان آدرسی که ما در حال جستجوی خروجی های تراکنش خرج نشده آن هستیم قفل شده باشد، این همان خروجی است که می خواهیم. اما قبل از گرفتن آن، باید بررسی کنیم که آیا یک خروجی قبلاً در یک ورودی ارجاع شده است یا خیر:

```go
if spentTXOs[txID] != nil {
	for _, spentOut := range spentTXOs[txID] {
		if spentOut == outIdx {
			continue Outputs
		}
	}
}
```

ما از مواردی که در ورودی ها به آنها اشاره شده بود صرفنظر می کنیم (مقادیر آنها به خروجی های دیگر منتقل شده است، بنابراین نمی توانیم آنها را بشماریم). پس از بررسی خروجی‌ها، همه ورودی‌هایی را جمع‌آوری می‌کنیم که می‌توانند قفل خروجی‌ها را با آدرس ارائه‌شده باز کنند (این برای تراکنش‌های coinbase صدق نمی‌کند، زیرا آنها قفل خروجی‌ها را باز نمی‌کنند):

```go
if tx.IsCoinbase() == false {
    for _, in := range tx.Vin {
        if in.CanUnlockOutputWith(address) {
            inTxID := hex.EncodeToString(in.Txid)
            spentTXOs[inTxID] = append(spentTXOs[inTxID], in.Vout)
        }
    }
}
```

این تابع فهرستی از تراکنش های حاوی خروجی های خرج نشده را برمی گرداند. برای محاسبه بالانس یا موجودی به یک تابع دیگر نیاز داریم که تراکنش ها را می گیرد و فقط خروجی ها را برمی گرداند:

```go
func (bc *Blockchain) FindUTXO(address string) []TXOutput {
       var UTXOs []TXOutput
       unspentTransactions := bc.FindUnspentTransactions(address)

       for _, tx := range unspentTransactions {
               for _, out := range tx.Vout {
                       if out.CanBeUnlockedWith(address) {
                               UTXOs = append(UTXOs, out)
                       }
               }
       }

       return UTXOs
}
```

خودشه! اکنون می توانیم دستور _**getbalance**_ را پیاده سازی کنیم:

```go
func (cli *CLI) getBalance(address string) {
	bc := NewBlockchain(address)
	defer bc.db.Close()

	balance := 0
	UTXOs := bc.FindUTXO(address)

	for _, out := range UTXOs {
		balance += out.Value
	}

	fmt.Printf("Balance of '%s': %d\n", address, balance)
}
```

مانده حساب مجموع مقادیر تمام خروجی های تراکنش خرج نشده است که توسط آدرس حساب قفل شده اند.

بیایید بالانس خود را پس از استخراج بلوک پیدایش بررسی کنیم:

```txt
$ blockchain_go getbalance -address Ivan
Balance of 'Ivan': 10
```

این اولین پول ماست!

### ارسال سکه Sending Coins

اکنون می خواهیم چند سکه برای شخص دیگری ارسال کنیم. برای این کار، باید یک تراکنش جدید ایجاد کنیم، آن را در یک بلوک قرار دهیم و بلوک را استخراج کنیم. تا به حال فقط تراکنش coinbase (که نوع خاصی از تراکنش ها است) را پیاده سازی کرده بودیم، اکنون به یک تراکنش عمومی نیاز داریم:

```go
func NewUTXOTransaction(from, to string, amount int, bc *Blockchain) *Transaction {
	var inputs []TXInput
	var outputs []TXOutput

	acc, validOutputs := bc.FindSpendableOutputs(from, amount)

	if acc < amount {
		log.Panic("ERROR: Not enough funds")
	}

	// Build a list of inputs
	for txid, outs := range validOutputs {
		txID, err := hex.DecodeString(txid)

		for _, out := range outs {
			input := TXInput{txID, out, from}
			inputs = append(inputs, input)
		}
	}

	// Build a list of outputs
	outputs = append(outputs, TXOutput{amount, to})
	if acc > amount {
		outputs = append(outputs, TXOutput{acc - amount, from}) // a change
	}

	tx := Transaction{nil, inputs, outputs}
	tx.SetID()

	return &tx
}
```

قبل از ایجاد خروجی های جدید، ابتدا باید تمام خروجی های مصرف نشده را پیدا کرده و مطمئن شویم که ارزش کافی را ذخیره می کنند. این کاری است که روش _**FindSpendableOutputs**_ انجام می دهد. پس از آن، برای هر خروجی یافت شده یک مرجع ورودی ایجاد می شود. سپس دو خروجی ایجاد می کنیم:

۱- یکی که با آدرس گیرنده قفل شده است. این انتقال واقعی سکه ها به آدرس دیگری است.

۲- یکی که با آدرس فرستنده قفل شده است. این یک تغییر است. فقط زمانی ایجاد می شود که خروجی های خرج نشده ارزش بیشتری نسبت به تراکنش جدید داشته باشند. به یاد داشته باشید: خروجی ها تقسیم ناپذیر یا **indivisible** هستند.

متد _**FindSpendableOutputs**_ مبتنی بر روش _**FindUnspentTransactions**_ است که قبلا تعریف کردیم:

```go
func (bc *Blockchain) FindSpendableOutputs(address string, amount int) (int, map[string][]int) {
	unspentOutputs := make(map[string][]int)
	unspentTXs := bc.FindUnspentTransactions(address)
	accumulated := 0

Work:
	for _, tx := range unspentTXs {
		txID := hex.EncodeToString(tx.ID)

		for outIdx, out := range tx.Vout {
			if out.CanBeUnlockedWith(address) && accumulated < amount {
				accumulated += out.Value
				unspentOutputs[txID] = append(unspentOutputs[txID], outIdx)

				if accumulated >= amount {
					break Work
				}
			}
		}
	}

	return accumulated, unspentOutputs
}
```

این روش بر روی تمام تراکنش های خرج نشده تکرار می شود و مقادیر آنها را جمع می کند. هنگامی که مقدار انباشته بیشتر یا برابر با مقداری است که می‌خواهیم انتقال دهیم، متوقف می‌شود و مقدار انباشته و شاخص‌های خروجی گروه‌بندی شده بر اساس شناسه‌های تراکنش را برمی‌گرداند. ما نمی‌خواهیم بیشتر از چیزی که قرار است خرج کنیم، بگیریم.

اکنون می توانیم روش _**Blockchain.MineBlock**_ را اصلاح کنیم:

```go
func (bc *Blockchain) MineBlock(transactions []*Transaction) {
	...
	newBlock := NewBlock(transactions, lastHash)
	...
}
```

در نهایت، اجازه دهید دستور ارسال یا _**send**_ را پیاده سازی کنیم:

```go
func (cli *CLI) send(from, to string, amount int) {
	bc := NewBlockchain(from)
	defer bc.db.Close()

	tx := NewUTXOTransaction(from, to, amount, bc)
	bc.MineBlock([]*Transaction{tx})
	fmt.Println("Success!")
}
```

ارسال سکه به معنای ایجاد یک تراکنش و افزودن آن به بلاک چین از طریق استخراج یک بلوک است. اما بیت کوین فورا این کار را انجام نمی دهد (مانند ما). در عوض، تمام تراکنش‌های جدید را در مموری و حافظه (یا mempool) قرار می‌دهد و زمانی که ماینر آماده استخراج یک بلوک است، تمام تراکنش‌ها را از mempool می‌گیرد و یک بلوک کاندید ایجاد می‌کند. تراکنش‌ها تنها زمانی تایید می‌شوند که بلوکی حاوی آن‌ها استخراج شده و به بلاک چین اضافه شود.

بیایید بررسی کنیم که ارسال سکه کار می کند:

```txt
$ blockchain_go send -from Ivan -to Pedro -amount 6
00000001b56d60f86f72ab2a59fadb197d767b97d4873732be505e0a65cc1e37
Success!

$ blockchain_go getbalance -address Ivan
Balance of 'Ivan': 4

$ blockchain_go getbalance -address Pedro
Balance of 'Pedro': 6
```
بسیار عالی! اکنون، بیایید تراکنش‌های بیشتری ایجاد کنیم و اطمینان حاصل کنیم که ارسال از چندین خروجی به خوبی انجام می‌شود:

```txt
$ blockchain_go send -from Pedro -to Helen -amount 2
00000099938725eb2c7730844b3cd40209d46bce2c2af9d87c2b7611fe9d5bdf
Success!

$ blockchain_go send -from Ivan -to Helen -amount 2
000000a2edf94334b1d94f98d22d7e4c973261660397dc7340464f7959a7a9aa
Success!
```

اکنون سکه های هلن در دو خروجی قفل شده اند: یکی از پدرو و دیگری از ایوان. بیایید آنها را برای شخص دیگری بفرستیم:

```txt
$ blockchain_go send -from Helen -to Rachel -amount 3
000000c58136cffa669e767b8f881d16e2ede3974d71df43058baaf8c069f1a0
Success!

$ blockchain_go getbalance -address Ivan
Balance of 'Ivan': 2

$ blockchain_go getbalance -address Pedro
Balance of 'Pedro': 4

$ blockchain_go getbalance -address Helen
Balance of 'Helen': 1

$ blockchain_go getbalance -address Rachel
Balance of 'Rachel': 3
```

عالی به نظر می رسد! حالا بیایید یک شکست را آزمایش کنیم:

```txt
$ blockchain_go send -from Pedro -to Ivan -amount 5
panic: ERROR: Not enough funds

$ blockchain_go getbalance -address Pedro
Balance of 'Pedro': 4

$ blockchain_go getbalance -address Ivan
Balance of 'Ivan': 2
```

#### نتیجه گیری

اوه! آسان نبود، اما ما اکنون معاملات داریم! اگرچه، برخی از ویژگی های کلیدی یک ارز دیجیتال مشابه بیت کوین وجود ندارد:

۱- آدرس ها. ما هنوز آدرس‌های مبتنی بر کلید خصوصی واقعی نداریم.

۲- پاداش. استخراج بلوک مطلقاً سودآور نیست!

۳- مجموعه UTXO. به دست آوردن بالانس نیاز به اسکن کل زنجیره بلوکی دارد، که در صورت وجود بلوک های بسیار و زیاد، ممکن است زمان بسیار زیادی طول بکشد. همچنین، اگر بخواهیم تراکنش‌های بعدی را تأیید کنیم، ممکن است زمان زیادی طول بکشد. مجموعه UTXO برای حل این مشکلات و انجام سریع عملیات با تراکنش ها در نظر گرفته شده است.

۴- ممپول (Mempool). این جایی است که تراکنش ها قبل از بسته بندی در یک بلوک ذخیره می شوند. در اجرای فعلی ما، یک بلوک فقط شامل یک تراکنش است و این کاملاً ناکارآمد است.

پایان قسمت چهارم

_**[قسمت پنجم](/blog/post_44_bc5)**_

_**[لینک منبع](https://jeiwan.net/posts/building-blockchain-in-go-part-4/)**_

_**باقی قسمت ها نسخه اصلی**_

* [Building Blockchain in Go. Part 1: Basic Prototype](https://jeiwan.net/posts/building-blockchain-in-go-part-1/)
* [Building Blockchain in Go. Part 2: Proof-of-Work](https://jeiwan.net/posts/building-blockchain-in-go-part-2/)
* [Building Blockchain in Go. Part 3: Persistence and CLI](https://jeiwan.net/posts/building-blockchain-in-go-part-3/)
* [Building Blockchain in Go. Part 4: Transactions 1](https://jeiwan.net/posts/building-blockchain-in-go-part-4/)
* [Building Blockchain in Go. Part 5: Addresses](https://jeiwan.net/posts/building-blockchain-in-go-part-5/)
* [Building Blockchain in Go. Part 6: Transactions 2](https://jeiwan.net/posts/building-blockchain-in-go-part-6/)
* [Building Blockchain in Go. Part 7: Network](https://jeiwan.net/posts/building-blockchain-in-go-part-7/)