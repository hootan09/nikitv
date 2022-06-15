---
title: ساخت بلاکچین با نگاهی به ساختار بین کوین - قسمت سوم (ماندگاری داده و رابط خط فرمان یا CLI)
image: images/blockchain/pxdksot8munu.png
description: مقدمه تا کنون، ما یک بلاک…
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

_**[تا کنون](/blog/post_41_bc2)**_، ما یک بلاکچین با سیستم اثبات کار (proof-of-work) ساخته‌ایم که استخراج را ممکن می‌کند. پیاده‌سازی ما در حال نزدیک‌تر شدن به یک بلاکچین کاملاً کاربردی است، اما همچنان فاقد برخی ویژگی‌های مهم است. امروز شروع به ذخیره یک بلاکچین در یک پایگاه داده می کنیم و پس از آن یک رابط خط فرمان ساده برای انجام عملیات با بلاکچین ایجاد می کنیم. در اصل، بلاکچین یک پایگاه داده توزیع شده است. فعلاً قسمت «توزیع‌شده» را حذف می‌کنیم و روی قسمت «پایگاه داده» تمرکز می‌کنیم.

#### انتخاب پایگاه داده

در حال حاضر، هیچ پایگاه داده ای در پیاده سازی ما وجود ندارد. در عوض، هر بار که برنامه را اجرا می کنیم بلوک هایی ایجاد می کنیم و آنها را در حافظه مموری ذخیره می کنیم. ما نمی‌توانیم از یک بلاکچین دوباره استفاده کنیم، نمی‌توانیم آن را با دیگران به اشتراک بگذاریم، بنابراین باید آن را روی دیسک ذخیره کنیم.

به کدام پایگاه داده نیاز داریم؟ در واقع، هر پایگاه داده ای می تواند باشد. **_[در مقاله اصلی بیت کوین](https://bitcoin.org/bitcoin.pdf)_**، چیزی در مورد استفاده از یک پایگاه داده خاص گفته نشده است، بنابراین این به توسعه دهنده بستگی دارد که از چه DB استفاده کند.**_[ Bitcoin Core](https://github.com/bitcoin/bitcoin)_** که در ابتدا توسط ساتوشی ناکاموتو منتشر شد و در حال حاضر یک پیاده سازی مرجع بیت کوین است، از **_[LevelDB](https://github.com/google/leveldb)_** استفاده می کند (اگرچه تنها در سال 2012 به مشتری معرفی شد). و ما استفاده خواهیم کرد از …

#### _پایگاه داده_ BoltDB

زیرا

۱- ساده و مینیمالیست.

۲- در Go پیاده سازی شده است.

۳- نیازی به اجرای سرور ندارد.

۴- این اجازه می دهد تا ساختار داده ای را که می خواهیم بسازیم.

میتوانید نگاهی به[ README BoltDB در Github](https://github.com/boltdb/bolt) بیاندازید.

این Bolt یک پایگاه داده به صورت key/value hsj که تماما با زبان Go پیاده سازی شده است که از پروژه LMDB هاوارد چو الهام گرفته شده است. هدف این پروژه ارائه یک پایگاه داده ساده، سریع و قابل اعتماد برای پروژه هایی است که به سرور پایگاه داده کامل مانند Postgres یا MySQL نیاز ندارند.

از آنجایی که Bolt قرار است به عنوان یک عملکرد سطح پایین مورد استفاده قرار گیرد، سادگی امری کلیدی است. تعداد و پیاده سازی API ها کوچک خواهد بود و فقط روی دریافت مقادیر و تنظیم مقادیر تمرکز دارد. همین.

برای نیازهای ما عالی به نظر می رسد! بیایید یک دقیقه آن را مرور کنیم.

این BoltDB یک ذخیره‌سازی key/value است، به این معنی که هیچ جدولی مانند SQL RDBMS (MySQL، PostgreSQL، و غیره)، هیچ ردیف و ستونی وجود ندارد. در عوض، داده ها به صورت جفت کلید-مقدار ذخیره می شوند (مانند map در Golang). جفت‌های کلید-مقدار در قالب سطل‌هایی (buckets) ذخیره می‌شوند که برای گروه‌بندی جفت‌های مشابه در نظر گرفته شده‌اند (این شبیه به جداول در RDBMS است). بنابراین، برای به دست آوردن یک مقدار، باید یک سطل (bucket) و یک کلید را بشناسید.

### ساختار پایگاه داده

قبل از شروع اجرای منطق ماندگاری داده، ابتدا باید تصمیم بگیریم که چگونه داده ها را در DB ذخیره کنیم و برای این، به روشی که Bitcoin Core این کار را انجام می دهد اشاره خواهیم کرد.

به عبارت ساده، Bitcoin Core از دو سطل (buckets) برای ذخیره داده ها استفاده می کند:

1- بلوک ها (Blocks) که متادیتا را ذخیره می کند که تمام بلوک های یک زنجیره را توصیف کند.

2- وضعیت یک زنجیره (ChainState) را ذخیره می‌کند، که تمام خروجی‌های تراکنش مصرف‌نشده در حال حاضر و برخی متادیتاها هستند.

همچنین بلوک ها به صورت فایل های جداگانه روی دیسک ذخیره می شوند. این برای یک هدف عملکردی انجام می شود. خواندن یک بلوک واحد نیازی به بارگیری همه (یا برخی) از آنها در حافظه ندارد. ما این را اجرا نخواهیم کرد.

در _**بلوک**_ ها، جفت های _**کلید -> مقدار**_ عبارتند از:

```txt
1- 'b' + 32-byte block hash -> block index record
2- 'f' + 4-byte file number -> file information record
3- 'l' -> 4-byte file number: the last block file number used
4- 'R' -> 1-byte boolean: whether we're in the process of reindexing
5- 'F' + 1-byte flag name length + flag name string -> 1 byte boolean: various flags that can be on or off
6- 't' + 32-byte transaction hash -> transaction index record
```

در ChainState ها ، جفت های _**کلید -> مقدار**_ عبارتند از:

```txt
1- 'c' + 32-byte transaction hash -> unspent transaction output record for that transaction
2- 'B' -> 32-byte block hash: the block hash up to which the database represents the unspent transaction outputs
```
(توضیحات مفصل را می توانید در _**[اینجا](https://en.bitcoin.it/wiki/Bitcoin_Core_0.11_(ch_2):_Data_Storage)**_ بیابید)

از آنجایی که ما هنوز تراکنش نداریم، فقط سطل بلوک یا Blocks خواهیم داشت. همچنین، همانطور که در بالا گفته شد، کل DB را به صورت یک فایل واحد ذخیره می کنیم، بدون اینکه بلوک ها را در فایل های جداگانه ذخیره کنیم. بنابراین ما به هیچ چیز مرتبط با شماره فایل نیاز نخواهیم داشت.

بنابراین اینها جفت های کلیدی -> ارزشی هستند که از آنها استفاده خواهیم کرد:

```txt
1.  32-byte block-hash -> Block structure (serialized)
2.  'l' -> the hash of the last block in a chain
```

این تمام چیزی است که برای شروع اجرای مکانیسم پایداری باید بدانیم.

#### سریال سازی یا Serialization

همانطور که قبلا گفته شد، مقادیر در BoltDB فقط می توانند از نوع []byte باشند، و ما می خواهیم ساختارهای Block را در DB ذخیره کنیم. ما از[ encoding/gob](https://golang.org/pkg/encoding/gob/) برای سریال سازی ساختارها استفاده خواهیم کرد.

بیایید روش Serialize از Block را پیاده سازی کنیم (پردازش خطاها برای اختصار حذف شده است):

```go
func (b *Block) Serialize() []byte {
	var result bytes.Buffer
	encoder := gob.NewEncoder(&result)

	err := encoder.Encode(b)

	return result.Bytes()
}
```

این قطعه کد ساده است. در ابتدا، بافری را اعلام می کنیم که داده های سریالی را ذخیره می کند. سپس یک انکدر _**gob**_ را مقداردهی اولیه می کنیم و بلوک را رمزگذاری می کنیم. نتیجه به عنوان یک آرایه بایت برگردانده می شود.

در مرحله بعد، ما به یک تابع deserializing جهت خارج کردن از حالت سریال سازی نیاز داریم که یک آرایه بایت را به عنوان ورودی دریافت کند و یک Block را برگرداند. این یک متد نیست بلکه یک تابع مستقل خواهد بود.

```go
func DeserializeBlock(d []byte) *Block {
	var block Block

	decoder := gob.NewDecoder(bytes.NewReader(d))
	err := decoder.Decode(&block)

	return &block
}
```

و همین برای سریال سازی کافی است!

#### ماندگاری یا Persistence

بیایید با عملکرد NewBlockchain شروع کنیم. در حال حاضر، یک نمونه جدید از بلاکچین ایجاد می کند و بلوک پیدایش (genesis block) را به آن اضافه می کند. کاری که ما می خواهیم انجام دهد این است که:

۱- یک فایل DB را باز کنید.

۲- بررسی کنید که آیا بلاکچین در آن ذخیره شده است یا خیر.

۳- اگر بلاک چین وجود دارد:

۳-۱ - یک نمونه بلاکچین جدید ایجاد کنید.

۳-۲ - نوک (tip) نمونه Blockchain را روی آخرین هش بلاک ذخیره شده در DB قرار دهید.

۴- اگر بلاکچین موجود وجود نداشته باشد:

۴-۱ - بلوک پیدایش را ایجاد کنید.

۴-۲ - در DB ذخیره کنید.

۴-۳ - هش بلوک پیدایش (genesis block’s) را به عنوان آخرین هش بلوک ذخیره کنید.

۴-۴ - یک نمونه Blockchain جدید با نوک آن به سمت بلوک پیدایش ایجاد کنید.

نمونه کد، به شکل زیر است

```go
func NewBlockchain() *Blockchain {
	var tip []byte
	db, err := bolt.Open(dbFile, 0600, nil)

	err = db.Update(func(tx *bolt.Tx) error {
		b := tx.Bucket([]byte(blocksBucket))

		if b == nil {
			genesis := NewGenesisBlock()
			b, err := tx.CreateBucket([]byte(blocksBucket))
			err = b.Put(genesis.Hash, genesis.Serialize())
			err = b.Put([]byte("l"), genesis.Hash)
			tip = genesis.Hash
		} else {
			tip = b.Get([]byte("l"))
		}

		return nil
	})

	bc := Blockchain{tip, db}

	return &bc
}
```

بیایید این قطعه کد را مرور کنیم.

```go
db, err := bolt.Open(dbFile, 0600, nil)
```

این یک روش استاندارد برای باز کردن یک فایل BoltDB است. توجه داشته باشید که اگر چنین فایلی وجود نداشته باشد، خطایی را بر نمی گرداند.

```go
err = db.Update(func(tx *bolt.Tx) error {
...
})
```

در BoltDB، عملیات با پایگاه داده در یک تراکنش اجرا می شود. و دو نوع تراکنش وجود دارد: **فقط خواندنی** و **خواندنی-نوشتنی**. در اینجا، ما یک تراکنش خواندن-نوشتن را باز می کنیم زیرا انتظار داریم بلوک پیدایش را در DB قرار دهیم.

```go
b := tx.Bucket([]byte(blocksBucket))

if b == nil {
	genesis := NewGenesisBlock()
	b, err := tx.CreateBucket([]byte(blocksBucket))
	err = b.Put(genesis.Hash, genesis.Serialize())
	err = b.Put([]byte("l"), genesis.Hash)
	tip = genesis.Hash
} else {
	tip = b.Get([]byte("l"))
}
```

این هسته فانکشن است. در اینجا، سطلی (bucket) را به دست می آوریم که بلوک های ما را ذخیره می کند. اگر وجود داشته باشد، کلید **l** (L کوچک) را از آن می خوانیم. اگر وجود نداشته باشد، بلوک پیدایش را تولید می‌کنیم، سطل را ایجاد می‌کنیم، بلوک را در آن ذخیره می‌کنیم و کلید **l** (L کوچک) را به‌روزرسانی می‌کنیم که آخرین هش بلاک زنجیره را ذخیره می‌کند.

همچنین به روش جدید ایجاد یک بلاکچین توجه کنید:

```go
bc := Blockchain{tip, db}
```

ما دیگر همه بلوک ها را در آن ذخیره نمی کنیم، در عوض فقط نوک زنجیره ذخیره می شود. همچنین، ما یک اتصال DB را ذخیره می کنیم، زیرا می خواهیم یک بار آن را باز کنیم و در حین اجرای برنامه باز نگه داریم. بنابراین، ساختار بلاکچین اکنون به شکل زیر است:

```go
type Blockchain struct {
	tip []byte
	db  *bolt.DB
}
```

مورد بعدی که می‌خواهیم به‌روزرسانی کنیم، روش _**AddBlock**_ است. اکنون اضافه کردن بلوک‌ها به یک زنجیره به آسانی افزودن یک عنصر به یک آرایه نیست. از این به بعد بلوک ها را در DB ذخیره می کنیم:

```go
func (bc *Blockchain) AddBlock(data string) {
	var lastHash []byte

	err := bc.db.View(func(tx *bolt.Tx) error {
		b := tx.Bucket([]byte(blocksBucket))
		lastHash = b.Get([]byte("l"))

		return nil
	})

	newBlock := NewBlock(data, lastHash)

	err = bc.db.Update(func(tx *bolt.Tx) error {
		b := tx.Bucket([]byte(blocksBucket))
		err := b.Put(newBlock.Hash, newBlock.Serialize())
		err = b.Put([]byte("l"), newBlock.Hash)
		bc.tip = newBlock.Hash

		return nil
	})
}
```

بیایید این قطعه کد را مرور کنیم.

```go
err := bc.db.View(func(tx *bolt.Tx) error {
	b := tx.Bucket([]byte(blocksBucket))
	lastHash = b.Get([]byte("l"))

	return nil
})
```

این نوع دیگر (فقط خواندنی) تراکنش های BoltDB است. در اینجا آخرین هش بلاک را از DB دریافت می کنیم تا از آن برای استخراج هش بلاک جدید استفاده کنیم.

```go
newBlock := NewBlock(data, lastHash)
b := tx.Bucket([]byte(blocksBucket))
err := b.Put(newBlock.Hash, newBlock.Serialize())
err = b.Put([]byte("l"), newBlock.Hash)
bc.tip = newBlock.Hash
```

پس از استخراج یک بلوک جدید، نسخه سریال شده آن را در DB ذخیره می کنیم و کلید l را به روز می کنیم، که اکنون هش بلوک جدید را ذخیره می کند.

انجام شد! سخت نبود، نه؟

#### بررسی بلاک چین

همه بلوک‌های جدید اکنون در یک پایگاه داده ذخیره می‌شوند، بنابراین می‌توانیم یک بلاکچین را دوباره باز کنیم و یک بلوک جدید به آن اضافه کنیم. اما پس از اجرای این، یک ویژگی خوب را از دست دادیم. دیگر نمی‌توانیم بلوک‌های بلاکچین را چاپ کنیم زیرا دیگر بلوک‌ها را در یک آرایه ذخیره نمی‌کنیم. بیایید این نقص را برطرف کنیم!

این BoltDB اجازه می دهد تا روی همه کلیدها در یک سطل (bucket) تکرار شود، اما کلیدها به ترتیب بایتی ذخیره می شوند و ما می خواهیم بلوک ها به ترتیبی که در یک زنجیره بلوکی می گیرند چاپ شوند. همچنین، چون نمی‌خواهیم همه بلوک‌ها را در حافظه بارگذاری کنیم (DB بلاکچین ما می‌تواند بزرگ باشد!... یا بیایید وانمود کنیم که می‌تواند)، آنها را یکی یکی می‌خوانیم. برای این منظور، به یک تکرارکننده بلاکچین نیاز داریم:

```go
type BlockchainIterator struct {
	currentHash []byte
	db          *bolt.DB
}
```

هر بار که بخواهیم روی بلوک‌های بلاکچین لوپ بزنیم، یک تکرار کننده ایجاد می‌شود و هش بلاک ازچرخه فعلی و یک اتصال DB را ذخیره می‌کند. به دلیل که بعدا خواهیم دید، یک تکرار کننده به طور منطقی به یک بلاکچین متصل می شود (این یک نمونه (instance) از بلاکچین است که یک اتصال DB را ذخیره می کند) و بنابراین این کار یک متد برای ایجاد بلاکچین است.

```go
func (bc *Blockchain) Iterator() *BlockchainIterator {
	bci := &BlockchainIterator{bc.tip, bc.db}

	return bci
}
```

توجه داشته باشید که یک تکرار کننده در ابتدا به نوک یک بلاک چین اشاره می کند، بنابراین بلوک ها از بالا به پایین، از جدیدترین به قدیمی ترین به دست می آیند. در واقع، _**انتخاب یک نوک به معنای «رای دادن» به یک بلاکچین است.**_ یک بلاکچین می‌تواند چندین انشعاب داشته باشد و طولانی‌ترین آن‌ به عنوان انشعاب اصلی در نظر گرفته می‌شود. پس از گرفتن نوک (این می تواند هر بلوکی در بلاکچین باشد) می توانیم کل بلاکچین را بازسازی کنیم و طول آن و کار مورد نیاز برای ساخت آن را پیدا کنیم. این واقعیت همچنین به این معنی است که یک نوک نوعی شناسه یک بلاکچین است.

این _**BlockchainIterator**_ تنها یک کار را انجام می دهد بلوک بعدی را از یک بلاکچین برمی گرداند.

```go
func (i *BlockchainIterator) Next() *Block {
	var block *Block

	err := i.db.View(func(tx *bolt.Tx) error {
		b := tx.Bucket([]byte(blocksBucket))
		encodedBlock := b.Get(i.currentHash)
		block = DeserializeBlock(encodedBlock)

		return nil
	})

	i.currentHash = block.PrevBlockHash

	return block
}
```

همین. بخش DB تمام است!

#### رابط خط فرمان یا CLI

تا کنون پیاده سازی ما هیچ رابطی برای تعامل با برنامه ارائه نکرده است. ما به سادگی _**NewBlockchain**_، _**bc.AddBlock**_ را در تابع _**main**_ اجرا کرده ایم. زمان بهبود این امر است! ما می خواهیم این دستورات را داشته باشیم:

```txt
blockchain_go addblock "Pay 0.031337 for a coffee"
blockchain_go printchain
```

تمام عملیات مربوط به خط فرمان توسط ساختار CLI پردازش می شود:

```go
type CLI struct {
	bc *Blockchain
}
```

"نقطه ورودی" آن تابع Run است:

```go
func (cli *CLI) Run() {
	cli.validateArgs()

	addBlockCmd := flag.NewFlagSet("addblock", flag.ExitOnError)
	printChainCmd := flag.NewFlagSet("printchain", flag.ExitOnError)

	addBlockData := addBlockCmd.String("data", "", "Block data")

	switch os.Args[1] {
	case "addblock":
		err := addBlockCmd.Parse(os.Args[2:])
	case "printchain":
		err := printChainCmd.Parse(os.Args[2:])
	default:
		cli.printUsage()
		os.Exit(1)
	}

	if addBlockCmd.Parsed() {
		if *addBlockData == "" {
			addBlockCmd.Usage()
			os.Exit(1)
		}
		cli.addBlock(*addBlockData)
	}

	if printChainCmd.Parsed() {
		cli.printChain()
	}
}
```

ما از پکیج استاندارد [flag](https://golang.org/pkg/flag/) در زبان Go برای تجزیه آرگومان های خط فرمان استفاده می کنیم.

```go
addBlockCmd := flag.NewFlagSet("addblock", flag.ExitOnError)
printChainCmd := flag.NewFlagSet("printchain", flag.ExitOnError)
addBlockData := addBlockCmd.String("data", "", "Block data")
```

ابتدا دو دستور فرعی _**addblock**_ و _**printchain**_ ایجاد می کنیم و سپس پرچم یا فلگ data- را به اولی اضافه می کنیم. _**`printchain`**_ هیچ پرچمی نخواهد داشت.

```go
switch os.Args[1] {
case "addblock":
	err := addBlockCmd.Parse(os.Args[2:])
case "printchain":
	err := printChainCmd.Parse(os.Args[2:])
default:
	cli.printUsage()
	os.Exit(1)
}
```

سپس دستور ارائه شده توسط کاربر را بررسی می کنیم و زیرفرمان (subcommand) پرچم مرتبط را تجزیه می کنیم.

```go
if addBlockCmd.Parsed() {
	if *addBlockData == "" {
		addBlockCmd.Usage()
		os.Exit(1)
	}
	cli.addBlock(*addBlockData)
}

if printChainCmd.Parsed() {
	cli.printChain()
}
```

سپس بررسی می کنیم که کدام یک از دستورات فرعی تجزیه شده و توابع مرتبط را اجرا می کنیم.

```go
func (cli *CLI) addBlock(data string) {
	cli.bc.AddBlock(data)
	fmt.Println("Success!")
}

func (cli *CLI) printChain() {
	bci := cli.bc.Iterator()

	for {
		block := bci.Next()

		fmt.Printf("Prev. hash: %x\n", block.PrevBlockHash)
		fmt.Printf("Data: %s\n", block.Data)
		fmt.Printf("Hash: %x\n", block.Hash)
		pow := NewProofOfWork(block)
		fmt.Printf("PoW: %s\n", strconv.FormatBool(pow.Validate()))
		fmt.Println()

		if len(block.PrevBlockHash) == 0 {
			break
		}
	}
}
```

این قطعه بسیار شبیه به قطعه ای است که قبلا داشتیم. تنها تفاوت این است که ما اکنون از BlockchainIterator برای تکرار بر روی بلوک های یک بلاکچین استفاده می کنیم.

همچنین فراموش نکنید که تابع main را بر این اساس تغییر دهید:

```go
func main() {
	bc := NewBlockchain()
	defer bc.db.Close()

	cli := CLI{bc}
	cli.Run()
}
```

توجه داشته باشید که یک بلاکچین جدید بدون توجه به اینکه چه آرگومان های خط فرمان ارائه می شود ایجاد می شود.

و تمام! بیایید بررسی کنیم که همه چیز همانطور که انتظار می رود کار می کند:

```txt
$ blockchain_go printchain
No existing blockchain found. Creating a new one...
Mining the block containing "Genesis Block"
000000edc4a82659cebf087adee1ea353bd57fcd59927662cd5ff1c4f618109b

Prev. hash:
Data: Genesis Block
Hash: 000000edc4a82659cebf087adee1ea353bd57fcd59927662cd5ff1c4f618109b
PoW: true

$ blockchain_go addblock -data "Send 1 BTC to Ivan"
Mining the block containing "Send 1 BTC to Ivan"
000000d7b0c76e1001cdc1fc866b95a481d23f3027d86901eaeb77ae6d002b13

Success!

$ blockchain_go addblock -data "Pay 0.31337 BTC for a coffee"
Mining the block containing "Pay 0.31337 BTC for a coffee"
000000aa0748da7367dec6b9de5027f4fae0963df89ff39d8f20fd7299307148

Success!

$ blockchain_go printchain
Prev. hash: 000000d7b0c76e1001cdc1fc866b95a481d23f3027d86901eaeb77ae6d002b13
Data: Pay 0.31337 BTC for a coffee
Hash: 000000aa0748da7367dec6b9de5027f4fae0963df89ff39d8f20fd7299307148
PoW: true

Prev. hash: 000000edc4a82659cebf087adee1ea353bd57fcd59927662cd5ff1c4f618109b
Data: Send 1 BTC to Ivan
Hash: 000000d7b0c76e1001cdc1fc866b95a481d23f3027d86901eaeb77ae6d002b13
PoW: true

Prev. hash:
Data: Genesis Block
Hash: 000000edc4a82659cebf087adee1ea353bd57fcd59927662cd5ff1c4f618109b
PoW: true
```

_(صدای باز شدن قوطی آبجو "بدول الکل")_

#### _نتیجه گیری_

_دفعه بعد آدرس ها، کیف پول ها و (احتمالا) تراکنش ها را اجرا خواهیم کرد. پس با ما همراه باشید!_

_پایان قسمت سوم._

_**[قسمت چهارم](/blog/post_43_bc4)**_

_**[لینک منبع](https://jeiwan.net/posts/building-blockchain-in-go-part-3/)**_

_**باقی قسمت ها نسخه اصلی**_

* [Building Blockchain in Go. Part 1: Basic Prototype](https://jeiwan.net/posts/building-blockchain-in-go-part-1/)
* [Building Blockchain in Go. Part 2: Proof-of-Work](https://jeiwan.net/posts/building-blockchain-in-go-part-2/)
* [Building Blockchain in Go. Part 3: Persistence and CLI](https://jeiwan.net/posts/building-blockchain-in-go-part-3/)
* [Building Blockchain in Go. Part 4: Transactions 1](https://jeiwan.net/posts/building-blockchain-in-go-part-4/)
* [Building Blockchain in Go. Part 5: Addresses](https://jeiwan.net/posts/building-blockchain-in-go-part-5/)
* [Building Blockchain in Go. Part 6: Transactions 2](https://jeiwan.net/posts/building-blockchain-in-go-part-6/)
* [Building Blockchain in Go. Part 7: Network](https://jeiwan.net/posts/building-blockchain-in-go-part-7/)