---
title: ساخت بلاکچین با نگاهی به ساختار بین کوین - قسمت اول (نمونه اولیه)
image: images/blockchain/xtmjm4iukjis.png
description: سخن مترجم:درسال های اخیر این بهترین مقاله ای هست که در رابطه با بلاکچین خواندم.نسخه اصلی آن در سال ۲۰۱۷ در قالب ۷ قسمت توسط آقای Ivan Kuzne…
date: 2022-06-13
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

>   
> سخن مترجم:درسال های اخیر این بهترین مقاله ای هست که در رابطه با بلاکچین خواندم.نسخه اصلی آن در سال ۲۰۱۷ در قالب ۷ قسمت توسط آقای [Ivan Kuznetsov](https://jeiwan.net/) نوشته شده است.برگردان این مجموعه به زبان های مختلفی از جمله چینی نیز ترجمه شده است.

_نکته: تمامی نمونه کد ها با زبان Go (Golang) پیاده سازی شده است._

#### مقدمه

بلاکچین یکی از انقلابی‌ترین فناوری‌های قرن بیست و یکم است که هنوز در حال بلوغ است و پتانسیل آن هنوز به‌طور کامل شناخته نشده است. در اصل، بلاکچین فقط یک پایگاه داده توزیع شده از سوابق است. اما چیزی که آن را منحصر به فرد می کند این است که یک پایگاه داده خصوصی نیست، بلکه عمومی است، یعنی هرکسی که از آن استفاده می کند یک نسخه کامل یا جزئی از آن را دارد. و یک رکورد جدید فقط با رضایت سایر نگهبانان پایگاه داده می تواند اضافه شود. همچنین، این بلاکچین است که ارزهای دیجیتال و قراردادهای هوشمند را امکان پذیر کرد.

در این سری از مقالات، یک ارز دیجیتال ساده‌سازی شده را می‌سازیم که مبتنی بر پیاده‌سازی ساده بلاکچین است.

#### بلوک (Block):

بیایید با بخش «بلاک» «بلاکچین» شروع کنیم. در بلاکچین، بلوک هایی هستند که اطلاعات ارزشمند را ذخیره می کنند. به عنوان مثال، بلوک های بلاکچین در بیت کوین از تراکنش های این رمز ارز نگهداری میکند. علاوه بر این، یک بلوک حاوی برخی اطلاعات فنی مانند نسخه آن، زمان (timestamp) و هش بلوک قبلی است. در این مقاله قصد نداریم بلاک را همانطور که در مشخصات بلاکچین یا بیت کوین توضیح داده شده پیاده سازی کنیم، در عوض از یک نسخه ساده شده آن استفاده خواهیم کرد که فقط حاوی اطلاعات قابل توجهی است. 

کد زیر نمونه چیزی است که از یک بلوک میخواهیم توسعه دهیم.

```go
type Block struct {
	Timestamp     int64
	Data          []byte
	PrevBlockHash []byte
	Hash          []byte
}
```

این _**Timestamp**_ زمان فعلی است (زمانی که بلوک ایجاد می‌شود)، _**Data**_ اطلاعات ارزشمند واقعی موجود در بلوک است، _**PrevBlockHash**_ هش بلوک قبلی را ذخیره می‌کند و _**Hash**_ هش بلوک فعلی است. در مشخصات بیت کوین _**Timestamp**_، _**PrevBlockHash**_ و _**Hash**_ هدرهای بلوکی هستند که یک ساختار داده جداگانه را تشکیل می دهند و تراکنش ها (در مورد ما داده ها یا _**Data**_) یک ساختار داده جداگانه است. بنابراین ما آنها را در اینجا برای سادگی با هم مخلوط می کنیم.

خُب چگونه هش ها را محاسبه کنیم؟ نحوه محاسبه هش ها ویژگی بسیار مهم بلاکچین است و این ویژگی است که بلاکچین را ایمن می کند. مسئله این است که محاسبه هش از نظر محاسباتی یک عملیات دشوار است، حتی در رایانه‌های سریع هم زمان می‌برد (به همین دلیل است که مردم برای استخراج بیت‌کوین پردازنده‌های گرافیکی قدرتمندی می‌خرند). این یک طراحی معماری عمدی است که افزودن بلوک‌های جدید را دشوار می‌کند، بنابراین از تغییر آن‌ها پس از اضافه شدن جلوگیری می‌کند. ما در مقاله آینده درباره پیاده سازی این مکانیسم بحث خواهیم کرد.

در حال حاضر، ما فقط فیلدهای بلوک را می گیریم، آنها را به هم متصل می کنیم و یک هش SHA-256 را روی این داده های متصل شده محاسبه می کنیم. 

بیایید این کار را در متد _**SetHash**_ انجام دهیم:

```go
func (b *Block) SetHash() {
	timestamp := []byte(strconv.FormatInt(b.Timestamp, 10))
	headers := bytes.Join([][]byte{b.PrevBlockHash, b.Data, timestamp}, []byte{})
	hash := sha256.Sum256(headers)

	b.Hash = hash[:]
}
```

در مرحله بعد، از طریق کد زیر یک تابعی را اجرا می کنیم که ایجاد یک بلوک را ساده می کند:

```go
func NewBlock(data string, prevBlockHash []byte) *Block {
	block := &Block{time.Now().Unix(), []byte(data), prevBlockHash, []byte{}}
	block.SetHash()
	return block
}
```

و برای ساخت بلوک همین کافی است.

## بلاکچین (Blockchain) :

حالا بیایید یک بلاکچین را پیاده سازی کنیم. در اصل بلاکچین فقط یک پایگاه داده با ساختار مشخص است: 

این یک لیست ترتیب گذاری شده و لینک شده با عناصر قبلی موجود در خود است. 

این بدان معناست که بلوک ها به ترتیب درج و ذخیره می شوند و هر بلوک به بلوک قبلی پیوند داده می شود. این ساختار امکان دریافت سریع آخرین بلوک در یک زنجیره و دریافت (به طور کارآمد) بلوک توسط هش آن را فراهم می کند.

در Golang می‌توان این ساختار را با استفاده از یک آرایه و یا یک map پیاده‌سازی کرد: آرایه هش‌های مرتب را نگه می‌دارد (آرایه‌ها در Go مرتب می‌شوند)، و نقشه جفت‌های _**hash → block**_ را نگه می‌دارد (map ها در گولنگ نامرتب هستند). اما برای نمونه اولیه بلاکچین ما فقط از یک آرایه استفاده می کنیم، زیرا در حال حاضر نیازی به دریافت بلوک ها توسط هش آنها نداریم.

```go
type Blockchain struct {
	blocks []*Block
}
```

این اولین بلاکچین ماست! هیچوقت فکر نمیکردم به این راحتی باشه😉

حالا بیایید امکان اضافه کردن بلوک به آن را فراهم کنیم:

```go
func (bc *Blockchain) AddBlock(data string) {
	prevBlock := bc.blocks[len(bc.blocks)-1]
	newBlock := NewBlock(data, prevBlock.Hash)
	bc.blocks = append(bc.blocks, newBlock)
}
```

ساده بود یا نه؟..

برای افزودن یک بلوک جدید نیاز به یک بلوک داریم که از اول موجود باشد، اما بلوکی در زنجیره بلوکی ما وجود ندارد! بنابراین، در هر بلاکچین، حداقل باید یک بلوک وجود داشته باشد، و چنین بلوکی، اولین بلوک در زنجیره، **_بلوک پیدایش یا_ genesis block** نامیده می شود. بیایید روشی را پیاده سازی کنیم که چنین بلوکی ایجاد می کند:

```go
func NewGenesisBlock() *Block {
	return NewBlock("Genesis Block", []byte{})
}
```

اکنون، می‌توانیم تابعی را پیاده‌سازی کنیم که با بلاک پیدایش، یک زنجیره بلوکی ایجاد می‌کند:

```go
func NewBlockchain() *Blockchain {
	return &Blockchain{[]*Block{NewGenesisBlock()}}
}
```

بیایید بررسی کنیم که بلاکچین به درستی کار می کند:

```go
func main() {
	bc := NewBlockchain()

	bc.AddBlock("Send 1 BTC to Ivan")
	bc.AddBlock("Send 2 more BTC to Ivan")

	for _, block := range bc.blocks {
		fmt.Printf("Prev. hash: %x\n", block.PrevBlockHash)
		fmt.Printf("Data: %s\n", block.Data)
		fmt.Printf("Hash: %x\n", block.Hash)
		fmt.Println()
	}
}
```

خروجی:

```txt
Prev. hash:
Data: Genesis Block
Hash: aff955a50dc6cd2abfe81b8849eab15f99ed1dc333d38487024223b5fe0f1168

Prev. hash: aff955a50dc6cd2abfe81b8849eab15f99ed1dc333d38487024223b5fe0f1168
Data: Send 1 BTC to Ivan
Hash: d75ce22a840abb9b4e8fc3b60767c4ba3f46a0432d3ea15b71aef9fde6a314e1

Prev. hash: d75ce22a840abb9b4e8fc3b60767c4ba3f46a0432d3ea15b71aef9fde6a314e1
Data: Send 2 more BTC to Ivan
Hash: 561237522bb7fcfbccbc6fe0e98bbbde7427ffe01c6fb223f7562288ca2295d1
```

و تمام.

#### نتیجه گیری:

ما یک نمونه اولیه بلاکچین بسیار ساده ساختیم. این فقط مجموعه‌ای از بلوک‌ها است که هر بلوک به بلوک قبلی متصل است. با این حال، بلاکچین واقعی بسیار پیچیده تر است. در بلاکچین ما، افزودن بلوک‌های جدید آسان و سریع است، اما در بلاکچین واقعی، افزودن بلوک‌های جدید به مقداری کار نیاز دارد: قبل از دریافت مجوز برای افزودن بلوک، باید محاسبات سنگینی انجام داد (این مکانیسم _**اثبات کار یا Proof-of-Work**_ نامیده می‌شود). همچنین، بلاکچین یک پایگاه داده توزیع شده است که هیچ تصمیم گیرنده واحدی ندارد. بنابراین، یک بلوک جدید باید توسط سایر شرکت کنندگان شبکه قبول و تأیید شود (این مکانیسم _**اجماع یا consensus**_ نامیده می شود). و هنوز هیچ تراکنشی در بلاکچین ما انجام نشده است!

در مقالات آینده به هر یک از این ویژگی ها خواهیم پرداخت.

_**پایان قسمت اول**_

_**[قسمت دوم](/blog/post_41_bc2/)**_

_**[لینک منبع ](https://jeiwan.net/posts/building-blockchain-in-go-part-1/)**_

_**باقی قسمت ها نسخه اصلی**_

* [Building Blockchain in Go. Part 1: Basic Prototype](https://jeiwan.net/posts/building-blockchain-in-go-part-1/)
* [Building Blockchain in Go. Part 2: Proof-of-Work](https://jeiwan.net/posts/building-blockchain-in-go-part-2/)
* [Building Blockchain in Go. Part 3: Persistence and CLI](https://jeiwan.net/posts/building-blockchain-in-go-part-3/)
* [Building Blockchain in Go. Part 4: Transactions 1](https://jeiwan.net/posts/building-blockchain-in-go-part-4/)
* [Building Blockchain in Go. Part 5: Addresses](https://jeiwan.net/posts/building-blockchain-in-go-part-5/)
* [Building Blockchain in Go. Part 6: Transactions 2](https://jeiwan.net/posts/building-blockchain-in-go-part-6/)
* [Building Blockchain in Go. Part 7: Network](https://jeiwan.net/posts/building-blockchain-in-go-part-7/)