---
title: ساخت بلاکچین با نگاهی به ساختار بین کوین - قسمت پنجم (آدرس ها)
image: images/blockchain/4w6c6njlk6xv.webp
description: مقدمه در مقاله قبل پیاده سازی تراکنش ها را آغاز کردیم. همچنین با ماهیت غیرشخصی تر…
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

در_**[ مقاله قبل](/blog/post_43_bc4)**_ پیاده سازی تراکنش ها را آغاز کردیم. همچنین با ماهیت غیرشخصی تراکنش ها آشنا شدید: هیچ حساب کاربری وجود ندارد، اطلاعات شخصی شما (به عنوان مثال، نام، شماره پاسپورت یا SSN) مورد نیاز نیست و در هیچ کجای بیت کوین ذخیره نمی شود. اما هنوز باید چیزی وجود داشته باشد که شما را به عنوان صاحب خروجی های تراکنش شناسایی کند (یعنی صاحب سکه هایی که روی این خروجی ها قفل شده اند). و این همان چیزی است که آدرس های بیت کوین برای آن مورد نیاز است. تاکنون از رشته های تعریف شده توسط کاربر دلخواه به عنوان آدرس استفاده کرده ایم، و زمان پیاده سازی آدرس های واقعی فرا رسیده است، همانطور که در بیت کوین پیاده سازی می شوند.

> این بخش تغییرات قابل توجهی را در کد ایجاد می کند، بنابراین توضیح همه آنها در اینجا بی معنی است. لطفاً برای مشاهده تمام تغییرات از آخرین مقاله به_**[ این صفحه](https://github.com/Jeiwan/blockchain_go/compare/part_4...part_5#files_bucket)**_ مراجعه کنید.

#### آدرس های بیت کوین

در اینجا نمونه ای از آدرس بیت کوین آورده شده است:

_**[ 1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa. ](https://blockchain.info/address/1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa)**_

این اولین آدرس بیت کوین است که ظاهراً _**متعلق به ساتوشی ناکاموتو**_ است. آدرس های بیت کوین عمومی هستند. اگر می خواهید برای شخصی سکه بفرستید، باید آدرس او را بدانید. اما آدرس ها (با وجود منحصر به فرد بودن) چیزی نیستند که شما را به عنوان صاحب «کیف پول» معرفی کنند. در واقع، چنین آدرس هایی نمایشی از کلیدهای عمومی قابل خواندن توسط انسان هستند. در بیت کوین، هویت شما یک جفت کلید خصوصی و عمومی است که در رایانه شما ذخیره شده است (یا در مکان دیگری که به آن دسترسی دارید ذخیره می شود). بیت کوین برای ایجاد این کلیدها به ترکیبی از الگوریتم های رمزنگاری متکی است و تضمین می کند که هیچ کس دیگری در جهان نمی تواند بدون دسترسی فیزیکی به کلیدهای شما به سکه های شما دسترسی داشته باشد. بیایید بحث کنیم که این الگوریتم ها چیست.

#### رمزنگاری کلید عمومی (Public-key Cryptography)

الگوریتم های رمزنگاری برای تولید کلید عمومی از جفت کلید استفاده می کنند: کلیدهای عمومی و کلیدهای خصوصی. کلیدهای عمومی حساس نیستند و می توانند در اختیار هر کسی قرار گیرند. در مقابل، کلیدهای خصوصی نباید فاش شوند: هیچ کس جز مالک نباید به آنها دسترسی داشته باشد زیرا این کلیدهای خصوصی هستند که به عنوان شناسه مالک عمل می کنند. شما کلیدهای خصوصی خود هستید (البته در دنیای ارزهای دیجیتال).

در اصل، کیف پول بیت کوین فقط یک جفت از این کلیدها است. هنگامی که یک برنامه کیف پول نصب می کنید یا از یک مشتری بیت کوین برای ایجاد یک آدرس جدید استفاده می کنید، یک جفت کلید برای شما ایجاد می شود. کسی که کلید خصوصی را کنترل می کند، تمام سکه های ارسال شده به این کلید را در بیت کوین کنترل می کند.

کلیدهای خصوصی و عمومی فقط دنباله های تصادفی بایت هستند، بنابراین نمی توانند روی صفحه چاپ شوند و توسط انسان خوانده شوند. به همین دلیل است که بیت کوین از یک الگوریتم برای تبدیل کلیدهای عمومی به یک رشته قابل خواندن توسط انسان استفاده می کند.

> اگر تا به حال از یک برنامه کیف پول بیت کوین استفاده کرده اید، به احتمال زیاد یک عبارت عبور قابل حفظ کردن برای شما ایجاد شده است. چنین عباراتی به جای کلیدهای خصوصی استفاده می شود و می توان از آنها برای تولید کلید استفاده کرد. این مکانیزم در_**[ BIP-039](https://bip-039./)**_ پیاده سازی شده است.

خب، ما اکنون می دانیم که چه چیزی کاربران را در بیت کوین شناسایی می کند. اما بیت کوین چگونه مالکیت خروجی های تراکنش (و سکه های ذخیره شده روی آنها) را بررسی می کند؟

#### امضاهای دیجیتال یا Digital Signatures

در ریاضیات و رمزنگاری، مفهوم امضای دیجیتال وجود دارد - الگوریتم هایی که تضمین می کنند:

۱- که داده ها در حین انتقال از فرستنده به گیرنده تغییر نکرده اند.

۲- که داده ها توسط یک فرستنده خاص ایجاد شده است.

۳- که فرستنده نمی تواند ارسال داده ها را انکار کند.

با اعمال یک الگوریتم امضا کننده برای داده ها (یعنی امضای داده ها)، شخص امضایی دریافت می کند که بعداً می تواند تأیید شود. امضای دیجیتال با استفاده از یک کلید خصوصی اتفاق می‌افتد و تأیید به یک کلید عمومی نیاز دارد.

برای امضای داده ها به موارد زیر نیاز داریم:

۱- داده برای امضا؛

۲- کلید خصوصی

عملیات امضاء یک امضا تولید می کند که در ورودی های تراکنش ذخیره می شود. برای تأیید امضا، موارد زیر مورد نیاز است:

۱- داده هایی که امضا شد؛

۲- امضا؛

۳- کلید عمومی.

به زبان ساده، فرآیند تأیید را می توان اینگونه توصیف کرد: بررسی کنید که این امضا از این داده ها با یک کلید خصوصی که برای تولید کلید عمومی استفاده می شود، به دست آمده است.

> امضای دیجیتال رمزگذاری نیست، شما نمی توانید داده ها را از یک امضا بازسازی کنید. این شبیه هش است: شما داده ها را از طریق یک الگوریتم هش اجرا می کنید و یک نمایش منحصر به فرد از داده ها دریافت می کنید. تفاوت بین امضا و هش جفت کلید است: آنها تأیید امضا را امکان پذیر می کنند.

> اما از جفت‌های کلید نیز می‌توان برای رمزگذاری داده‌ها استفاده کرد: یک کلید خصوصی برای رمزگذاری و یک کلید عمومی برای رمزگشایی داده‌ها استفاده می‌شود. اگرچه بیت کوین از الگوریتم های رمزگذاری استفاده نمی کند.

هر ورودی تراکنش در بیت کوین توسط شخصی که تراکنش را ایجاد کرده است امضا می شود. هر تراکنش در بیت کوین باید قبل از قرار گرفتن در یک بلوک تأیید شود. ابزار تأیید (علاوه بر سایر رویه ها):

۱- بررسی اینکه ورودی‌ها مجوز استفاده از خروجی‌های تراکنش‌های قبلی را دارند.

۲- بررسی صحت امضای معامله

از نظر شماتیک، فرآیند امضای داده ها و تأیید امضا به این صورت است:


{{< image src="/images/blockchain/o8wnsmbxi0yx.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

بیایید اکنون چرخه عمر کامل یک تراکنش را مرور کنیم:

۱- در ابتدا، بلوک پیدایش (genesis block) وجود دارد که حاوی یک تراکنش کوین بیس است. هیچ ورودی واقعی در معاملات coinbase وجود ندارد، بنابراین امضا لازم نیست. خروجی تراکنش coinbase شامل یک کلید عمومی هش شده است (الگوریتم های _**RIPEMD16(SHA256(PubKey))**_ استفاده می شود).

۲- وقتی کسی سکه می فرستد، یک تراکنش ایجاد می شود. ورودی های تراکنش به خروجی های تراکنش(های) قبلی اشاره خواهد کرد. هر ورودی یک کلید عمومی (نه هش شده) و امضای کل تراکنش را ذخیره می کند.

۳- گره های دیگر در شبکه بیت کوین که تراکنش را دریافت می کنند، آن را تأیید می کنند. علاوه بر موارد دیگر، آنها بررسی خواهند کرد که: هش کلید عمومی در ورودی با هش خروجی ارجاع شده مطابقت داشته باشد (این امر تضمین می کند که فرستنده فقط سکه های متعلق به آنها را خرج می کند). امضا صحیح است (این تضمین می کند که معامله توسط صاحب واقعی سکه ها ایجاد شده است).

۴- هنگامی که یک گره ماینر آماده استخراج یک بلوک جدید است، تراکنش را در یک بلوک قرار می دهد و شروع به استخراج آن می کند.

۵- هنگامی که بلوک استخراج می شود، هر گره دیگری در شبکه پیامی مبنی بر استخراج بلوک دریافت می کند و بلوک را به بلاکچین اضافه می کند.

۶- پس از اضافه شدن یک بلوک به بلاکچین، تراکنش کامل شد، خروجی های آن می توانند در تراکنش های جدید ارجاع داده شوند.

#### رمزنگاری منحنی بیضوی یا Elliptic Curve Cryptography

همانطور که در بالا توضیح داده شد، کلیدهای عمومی و خصوصی دنباله ای از بایت های تصادفی هستند. از آنجایی که این کلیدهای خصوصی هستند که برای شناسایی صاحبان سکه ها استفاده می شوند، یک شرط لازم وجود دارد: الگوریتم تصادفی باید بایت های واقعا تصادفی تولید کند. ما نمی خواهیم تصادفاً یک کلید خصوصی که متعلق به شخص دیگری است ایجاد کنیم.

بیت کوین از منحنی های بیضوی (elliptic curves) برای تولید کلیدهای خصوصی استفاده می کند. منحنی های بیضوی یک مفهوم پیچیده ریاضی است که ما در اینجا قصد نداریم جزئیات آن را توضیح دهیم (اگر کنجکاو هستید، _**[این مقدمه ملایم برای منحنی های بیضوی را بررسی کنید](http://andrea.corbellini.name/2015/05/17/elliptic-curve-cryptography-a-gentle-introduction/)**_ هشدار: فرمول های ریاضی!). چیزی که ما باید بدانیم این است که از این منحنی ها می توان برای تولید اعداد واقعا بزرگ و تصادفی استفاده کرد. منحنی استفاده شده توسط بیت کوین می تواند به طور تصادفی عددی بین 0 تا _**۲ به توان ۲۵۶**_ (که تقریباً معادل ۱۰ _**به توان ۷۷**_ است، این درحالی است که بین _**۱۰ به توان ۷۸**_ تا  _**۱۰ به توان ۸۲**_ اتم در جهان مرئی وجود دارد) انتخاب کند. چنین محدودیت بالایی به این معنی است که تقریباً غیرممکن است که یک کلید خصوصی را دو بار تولید کنید.

همچنین، بیت کوین از الگوریتم ECDSA (الگوریتم امضای دیجیتال منحنی بیضوی یا Elliptic Curve Digital Signature Algorithm) برای امضای تراکنش ها استفاده می کند و ما نیز خواهیم کرد.

#### مبنای ۵۸ یا Base58

حالا بیایید به آدرس بیت کوین ذکر شده در بالا برگردیم:

1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa. 

اکنون می دانیم که این نمایش یک کلید عمومی برای انسان قابل خواندن است. و اگر آن را رمزگشایی کنیم، در اینجا کلید عمومی به نظر می رسد (به عنوان دنباله ای از بایت های نوشته شده در سیستم هگزادسیمال):

```txt
0062E907B15CBF27D5425399EBF6F0FB50EBB88F18C29B7D93
```

بیت کوین از الگوریتم Base58 برای تبدیل کلیدهای عمومی به فرمت قابل خواندن توسط انسان استفاده می کند. این الگوریتم بسیار شبیه به Base64 معروف است، اما از الفبای کوتاه تری استفاده می کند: برخی از حروف از الفبا حذف شدند تا از حملاتی که از شباهت حروف استفاده می کنند جلوگیری شود. بنابراین، این نمادها وجود ندارد: 0 (صفر)، O ( o بزرگ)، I ( i بزرگ)، l (حروف کوچک L)، زیرا شبیه به هم هستند. همچنین هیچ علامت + و / وجود ندارد.

بیایید روند دریافت آدرس از یک کلید عمومی را به صورت شماتیک تجسم کنیم:


{{< image src="/images/blockchain/vdlvowte9w1y.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}


بنابراین، کلید عمومی رمزگشایی شده فوق از سه بخش تشکیل شده است:

```txt
Version Public key hash Checksum
00 62E907B15CBF27D5425399EBF6F0FB50EBB88F18 C29B7D93
```

از آنجایی که توابع هش یک راه هستند (یعنی نمی توان آنها را معکوس کرد)، نمی توان کلید عمومی را از هش استخراج کرد. اما می‌توانیم با اجرای آن از طریق توابع هش ذخیره و مقایسه هش‌ها بررسی کنیم که آیا از کلید عمومی برای دریافت هش استفاده شده است یا خیر.

خب، حالا که همه قطعات را داریم، بیایید یک کد بنویسیم. برخی از مفاهیم زمانی که به صورت کد نوشته می شوند باید واضح تر باشند.

#### آدرس های پیاده سازی

ما با ساختار Wallet شروع می کنیم:

```go
type Wallet struct {
	PrivateKey ecdsa.PrivateKey
	PublicKey  []byte
}

type Wallets struct {
	Wallets map[string]*Wallet
}

func NewWallet() *Wallet {
	private, public := newKeyPair()
	wallet := Wallet{private, public}

	return &wallet
}

func newKeyPair() (ecdsa.PrivateKey, []byte) {
	curve := elliptic.P256()
	private, err := ecdsa.GenerateKey(curve, rand.Reader)
	pubKey := append(private.PublicKey.X.Bytes(), private.PublicKey.Y.Bytes()...)

	return *private, pubKey
}
```

کیف پول چیزی جز یک جفت کلید نیست. همچنین برای نگهداری مجموعه ای از کیف پول ها، ذخیره آنها در یک فایل و بارگیری آنها از آن، به نوع _**Wallets**_ نیاز داریم. در عملکرد ساخت کیف پول یک جفت کلید جدید تولید می شود. تابع newKeyPair ساده است. این ECDSA بر اساس منحنی های بیضوی است، بنابراین ما به یک منحنی نیاز داریم. سپس یک کلید خصوصی با استفاده از منحنی و یک کلید عمومی از کلید خصوصی تولید می شود. یک نکته قابل توجه است در الگوریتم های مبتنی بر منحنی بیضوی، کلیدهای عمومی نقاطی روی یک منحنی هستند. بنابراین، یک کلید عمومی ترکیبی از مختصات X، Y است. در بیت کوین، این مختصات به هم پیوسته و یک کلید عمومی را تشکیل می دهند.

حالا بیایید یک آدرس تولید کنیم:

```go
func (w Wallet) GetAddress() []byte {
	pubKeyHash := HashPubKey(w.PublicKey)

	versionedPayload := append([]byte{version}, pubKeyHash...)
	checksum := checksum(versionedPayload)

	fullPayload := append(versionedPayload, checksum...)
	address := Base58Encode(fullPayload)

	return address
}

func HashPubKey(pubKey []byte) []byte {
	publicSHA256 := sha256.Sum256(pubKey)

	RIPEMD160Hasher := ripemd160.New()
	_, err := RIPEMD160Hasher.Write(publicSHA256[:])
	publicRIPEMD160 := RIPEMD160Hasher.Sum(nil)

	return publicRIPEMD160
}

func checksum(payload []byte) []byte {
	firstSHA := sha256.Sum256(payload)
	secondSHA := sha256.Sum256(firstSHA[:])

	return secondSHA[:addressChecksumLen]
}
```

در اینجا مراحل تبدیل کلید عمومی به آدرس Base58 آمده است.

۱- کلید عمومی را بردارید و با الگوریتم های هش _**RIPEMD160(SHA256(PubKey))**_ دوبار آن را هش کنید.

۲- نسخه (version) الگوریتم تولید آدرس را در هش آماده کنید.

۳- با هش کردن نتیجه مرحله 2 با _**SHA256(SHA256(Payload))**_ مبلغ چکسام را محاسبه کنید. چکسام از چهار بایت اول هش به دست آمده است.

۴- چکسام سوم را به ترکیب version+PubKeyHash اضافه کنید.

5- ترکیب انکد شده version+PubKeyHash+checksum را با Base58 رمزگذاری کنید.  

در نتیجه، یک آدرس بیت کوین واقعی دریافت خواهید کرد، حتی می توانید موجودی آن را در **_[blockchain.info](https://blockchain.info/)_** بررسی کنید. اما می توانم به شما اطمینان دهم که هر چند بار یک آدرس جدید ایجاد کنید و موجودی آن را بررسی کنید، موجودی 0 است. به همین دلیل است که انتخاب الگوریتم رمزنگاری کلید عمومی مناسب بسیار مهم است: با توجه به اینکه کلیدهای خصوصی اعداد تصادفی هستند، شانس تولید همان عدد باید تا حد امکان کم باشد. در حالت ایده آل، باید به اندازه «هرگز» پایین باشد.

همچنین توجه داشته باشید که برای دریافت آدرس نیازی به اتصال به گره بیت کوین ندارید. الگوریتم تولید آدرس از ترکیبی از الگوریتم های باز استفاده می کند که در بسیاری از زبان های برنامه نویسی و کتابخانه ها پیاده سازی شده اند.

اکنون باید ورودی ها و خروجی ها را تغییر دهیم تا بتوانند از آدرس ها استفاده کنند:

```go
type TXInput struct {
	Txid      []byte
	Vout      int
	Signature []byte
	PubKey    []byte
}

func (in *TXInput) UsesKey(pubKeyHash []byte) bool {
	lockingHash := HashPubKey(in.PubKey)

	return bytes.Compare(lockingHash, pubKeyHash) == 0
}

type TXOutput struct {
	Value      int
	PubKeyHash []byte
}

func (out *TXOutput) Lock(address []byte) {
	pubKeyHash := Base58Decode(address)
	pubKeyHash = pubKeyHash[1 : len(pubKeyHash)-4]
	out.PubKeyHash = pubKeyHash
}

func (out *TXOutput) IsLockedWithKey(pubKeyHash []byte) bool {
	return bytes.Compare(out.PubKeyHash, pubKeyHash) == 0
}
```

توجه داشته باشید که ما دیگر از فیلدهای _**ScriptPubKey**_ و _**ScriptSig**_ استفاده نمی کنیم، زیرا قرار نیست یک زبان برنامه نویسی را پیاده سازی کنیم. در عوض، _**ScriptSig**_ به فیلدهای _**Signature**_ و _**PubKey**_ تقسیم می شود و _**ScriptPubKey**_ به _**PubKeyHash**_ تغییر نام می دهد. ما همان منطق قفل/باز کردن قفل خروجی و امضای ورودی‌ها را مانند بیت کوین پیاده‌سازی می‌کنیم، اما در عوض این کار را در متدها انجام خواهیم داد.

روش _**UsesKey**_ بررسی می کند که یک ورودی از یک کلید خاص برای باز کردن قفل خروجی استفاده می کند. توجه داشته باشید که ورودی‌ها کلیدهای عمومی خام را ذخیره می‌کنند (یعنی هش نشده)، اما تابع یک هش می‌گیرد. _**IsLockedWithKey**_ بررسی می کند که آیا از هش کلید عمومی ارائه شده برای قفل کردن خروجی استفاده شده است. این یک تابع مکمل _**UsesKey**_ است و هر دو در _**FindUnspentTransactions**_ برای ایجاد ارتباط بین تراکنش ها استفاده می شوند.

این _**LOCK**_ به سادگی یک خروجی را قفل می کند. وقتی برای شخصی سکه می فرستیم، فقط آدرس او را می دانیم، بنابراین تابع یک آدرس را به عنوان تنها آرگومان می گیرد. سپس آدرس رمزگشایی می شود و هش کلید عمومی از آن استخراج می شود و در قسمت _**PubKeyHash**_ ذخیره می شود.

اکنون، بیایید بررسی کنیم که همه چیز به درستی کار می کند:

```txt
$ blockchain_go createwallet
Your new address: 13Uu7B1vDP4ViXqHFsWtbraM3EfQ3UkWXt

$ blockchain_go createwallet
Your new address: 15pUhCbtrGh3JUx5iHnXjfpyHyTgawvG5h

$ blockchain_go createwallet
Your new address: 1Lhqun1E9zZZhodiTqxfPQBcwr1CVDV2sy

$ blockchain_go createblockchain -address 13Uu7B1vDP4ViXqHFsWtbraM3EfQ3UkWXt
0000005420fbfdafa00c093f56e033903ba43599fa7cd9df40458e373eee724d
Done!`  

$ blockchain_go getbalance -address 13Uu7B1vDP4ViXqHFsWtbraM3EfQ3UkWXt
Balance of '13Uu7B1vDP4ViXqHFsWtbraM3EfQ3UkWXt': 10

$ blockchain_go send -from 15pUhCbtrGh3JUx5iHnXjfpyHyTgawvG5h -to 13Uu7B1vDP4ViXqHFsWtbraM3EfQ3UkWXt -amount 5
2017/09/12 13:08:56 ERROR: Not enough funds

$ blockchain_go send -from 13Uu7B1vDP4ViXqHFsWtbraM3EfQ3UkWXt -to 15pUhCbtrGh3JUx5iHnXjfpyHyTgawvG5h -amount 6
00000019afa909094193f64ca06e9039849709f5948fbac56cae7b1b8f0ff162
Success!

$ blockchain_go getbalance -address 13Uu7B1vDP4ViXqHFsWtbraM3EfQ3UkWXt
Balance of '13Uu7B1vDP4ViXqHFsWtbraM3EfQ3UkWXt': 4

$ blockchain_go getbalance -address 15pUhCbtrGh3JUx5iHnXjfpyHyTgawvG5h
Balance of '15pUhCbtrGh3JUx5iHnXjfpyHyTgawvG5h': 6

$ blockchain_go getbalance -address 1Lhqun1E9zZZhodiTqxfPQBcwr1CVDV2sy
Balance of '1Lhqun1E9zZZhodiTqxfPQBcwr1CVDV2sy': 0
```

خب! حالا بیایید امضاهای تراکنش را پیاده سازی کنیم.

#### پیاده سازی امضاها یا Signatures

تراکنش ها باید امضا شوند زیرا این تنها راهی است که در بیت کوین تضمین می کند که شخص نمی تواند سکه های متعلق به شخص دیگری را خرج کند. اگر امضای نامعتبر باشد، تراکنش نیز نامعتبر تلقی می شود و بنابراین نمی توان آن را به بلاکچین اضافه کرد.

ما همه قطعات را برای اجرای امضای تراکنش ها داریم، به جز یک چیز: داده هایی که باید امضا شوند. چه بخش هایی از یک معامله در واقع امضا می شود؟ یا یک معامله به طور کلی امضا شده است؟ انتخاب داده برای امضا بسیار مهم است. موضوع این است که داده هایی که باید امضا شوند باید حاوی اطلاعاتی باشند که داده ها را به روشی منحصر به فرد شناسایی کند. به عنوان مثال، امضا کردن فقط مقادیر خروجی معنی ندارد زیرا چنین امضایی فرستنده و گیرنده را در نظر نمی گیرد.

با توجه به اینکه تراکنش‌ها خروجی‌های قبلی را باز می‌کنند، مقادیر آنها را دوباره توزیع می‌کنند و خروجی‌های جدید را قفل می‌کنند، داده‌های زیر باید امضا شوند:

۱- هش های کلید عمومی که در خروجی های قفل نشده ذخیره می شوند. این "فرستنده" تراکنش را مشخص می کند.

۲- هش های کلید عمومی که در خروجی های جدید قفل شده ذخیره می شوند. این "گیرنده" تراکنش را مشخص می کند.

۳- مقدار خروجی های جدید

> در بیت کوین، منطق قفل/باز کردن قفل در اسکریپت هایی ذخیره می شود که به ترتیب در فیلدهای ورودی و خروجی _**ScriptSig**_ و _**ScriptPubKey**_ ذخیره می شوند. از آنجایی که بیت کوین انواع مختلفی از این اسکریپت ها را مجاز می کند، کل محتوای _**ScriptPubKey**_ را امضا می کند.

همانطور که می بینید، ما نیازی به امضای کلیدهای عمومی ذخیره شده در ورودی ها نداریم. به همین دلیل، در بیت کوین، این یک تراکنش امضا شده نیست، بلکه کپی کوتاه شده آن با ورودی هایی است که _**ScriptPubKey**_ را از خروجی های ارجاع داده شده ذخیره می کند.


> روند دقیق دریافت یک کپی تراکنش کوتاه شده در اینجا شرح داده شده است. احتمالاً منسوخ شده است، اما من نتوانستم منبع اطلاعاتی قابل اعتمادتری پیدا کنم.


خب، پیچیده به نظر می رسد، بنابراین اجازه دهید شروع به کدنویسی کنیم. ما با روش Sign شروع می کنیم:

```go
func (tx *Transaction) Sign(privKey ecdsa.PrivateKey, prevTXs map[string]Transaction) {
	if tx.IsCoinbase() {
		return
	}

	txCopy := tx.TrimmedCopy()

	for inID, vin := range txCopy.Vin {
		prevTx := prevTXs[hex.EncodeToString(vin.Txid)]
		txCopy.Vin[inID].Signature = nil
		txCopy.Vin[inID].PubKey = prevTx.Vout[vin.Vout].PubKeyHash
		txCopy.ID = txCopy.Hash()
		txCopy.Vin[inID].PubKey = nil

		r, s, err := ecdsa.Sign(rand.Reader, &privKey, txCopy.ID)
		signature := append(r.Bytes(), s.Bytes()...)

		tx.Vin[inID].Signature = signature
	}
}
```

این متد یک کلید خصوصی و map ای از تراکنش های قبلی را می گیرد. همانطور که در بالا ذکر شد، برای امضای یک تراکنش، باید به خروجی های اشاره شده در ورودی های تراکنش دسترسی داشته باشیم، بنابراین به تراکنش هایی نیاز داریم که این خروجی ها را ذخیره می کنند.

بیایید این روش را مرحله به مرحله مرور کنیم:

```go
if tx.IsCoinbase() {
	return
}
```

تراکنش های کوین بیس امضا نمی شوند زیرا هیچ ورودی واقعی در آنها وجود ندارد.

```go
txCopy := tx.TrimmedCopy()
```

یک نسخه بریده شده امضا خواهد شد، نه یک تراکنش کامل:

```go
func (tx *Transaction) TrimmedCopy() Transaction {
	var inputs []TXInput
	var outputs []TXOutput

	for _, vin := range tx.Vin {
		inputs = append(inputs, TXInput{vin.Txid, vin.Vout, nil, nil})
	}

	for _, vout := range tx.Vout {
		outputs = append(outputs, TXOutput{vout.Value, vout.PubKeyHash})
	}

	txCopy := Transaction{tx.ID, inputs, outputs}

	return txCopy
}
```

کپی شامل تمام ورودی‌ها و خروجی‌ها می‌شود، اما _**TXInput.Signature**_ و _**TXInput.PubKey**_ روی nil تنظیم شده‌اند.

بعد، روی هر ورودی در کپی تکرار می کنیم:

```go
for inID, vin := range txCopy.Vin {
	prevTx := prevTXs[hex.EncodeToString(vin.Txid)]
	txCopy.Vin[inID].Signature = nil
	txCopy.Vin[inID].PubKey = prevTx.Vout[vin.Vout].PubKeyHash
```


در هر ورودی، Signature روی nil (فقط یک بررسی مجدد) و PubKey روی PubKeyHash خروجی ارجاع شده تنظیم می شود. در این لحظه، همه تراکنش‌ها به جز تراکنش فعلی «خالی» هستند، یعنی فیلدهای Signature و PubKey آنها روی nil تنظیم شده‌اند. بنابراین، _**ورودی ها به طور جداگانه امضا می شوند**_، اگرچه این برای برنامه ما ضروری نیست، اما بیت کوین اجازه می دهد تا تراکنش ها حاوی ورودی هایی باشند که به آدرس های مختلف اشاره می کنند.

```go
txCopy.ID = txCopy.Hash()
	txCopy.Vin[inID].PubKey = nil
```

متد _**Hash**_ تراکنش را سریالی می کند و آن را با الگوریتم SHA-256 هش می کند. هش حاصل، داده‌ای است که ما امضا می‌کنیم. پس از دریافت هش، باید فیلد _**PubKey**_ را بازنشانی کنیم تا بر تکرارهای بعدی تأثیری نگذارد.

حال، قطعه مرکزی:

```go
r, s, err := ecdsa.Sign(rand.Reader, &privKey, txCopy.ID)
	signature := append(r.Bytes(), s.Bytes()...)

	tx.Vin[inID].Signature = signature
```

ما _**txCopy.ID**_ را با _**privKey**_ امضا می کنیم. امضای ECDSA یک جفت اعداد است که آنها را به هم متصل کرده و در قسمت امضای ورودی ذخیره می کنیم.

حالا تابع تایید:

```go
func (tx *Transaction) Verify(prevTXs map[string]Transaction) bool {
	txCopy := tx.TrimmedCopy()
	curve := elliptic.P256()

	for inID, vin := range tx.Vin {
		prevTx := prevTXs[hex.EncodeToString(vin.Txid)]
		txCopy.Vin[inID].Signature = nil
		txCopy.Vin[inID].PubKey = prevTx.Vout[vin.Vout].PubKeyHash
		txCopy.ID = txCopy.Hash()
		txCopy.Vin[inID].PubKey = nil

		r := big.Int{}
		s := big.Int{}
		sigLen := len(vin.Signature)
		r.SetBytes(vin.Signature[:(sigLen / 2)])
		s.SetBytes(vin.Signature[(sigLen / 2):])

		x := big.Int{}
		y := big.Int{}
		keyLen := len(vin.PubKey)
		x.SetBytes(vin.PubKey[:(keyLen / 2)])
		y.SetBytes(vin.PubKey[(keyLen / 2):])

		rawPubKey := ecdsa.PublicKey{curve, &x, &y}
		if ecdsa.Verify(&rawPubKey, txCopy.ID, &r, &s) == false {
			return false
		}
	}

	return true
}
```

متد کاملاً ساده است. ابتدا به همان نسخه تراکنش نیاز داریم:

```go
txCopy := tx.TrimmedCopy()
```

در مرحله بعد، به همان منحنی که برای تولید جفت کلید استفاده می شود نیاز داریم:

```go
curve := elliptic.P256()
```

سپس، امضا را در هر ورودی بررسی می کنیم:

```go
for inID, vin := range tx.Vin {
	prevTx := prevTXs[hex.EncodeToString(vin.Txid)]
	txCopy.Vin[inID].Signature = nil
	txCopy.Vin[inID].PubKey = prevTx.Vout[vin.Vout].PubKeyHash
	txCopy.ID = txCopy.Hash()
	txCopy.Vin[inID].PubKey = nil
```

این قطعه مشابه موردی است که در روش _**Sign**_ وجود دارد، زیرا در حین تأیید به همان داده هایی نیاز داریم که امضا شده است.

```go
    r := big.Int{}
	s := big.Int{}
	sigLen := len(vin.Signature)
	r.SetBytes(vin.Signature[:(sigLen / 2)])
	s.SetBytes(vin.Signature[(sigLen / 2):])

	x := big.Int{}
	y := big.Int{}
	keyLen := len(vin.PubKey)
	x.SetBytes(vin.PubKey[:(keyLen / 2)])
	y.SetBytes(vin.PubKey[(keyLen / 2):])
```


در اینجا ما مقادیر ذخیره شده در TXInput.Signature و TXInput.PubKey را باز می کنیم، زیرا یک امضا یک جفت عدد و یک کلید عمومی یک جفت مختصات است. ما قبلاً آنها را برای ذخیره سازی به هم متصل کردیم، و اکنون باید آنها را باز کنیم تا در توابع`crypto/ecdsa` استفاده کنیم.

```go
rawPubKey := ecdsa.PublicKey{curve, &x, &y}
	if ecdsa.Verify(&rawPubKey, txCopy.ID, &r, &s) == false {
		return false
	}
}

return true
```

اینجاست: ما یک ecdsa.PublicKey را با استفاده از کلید عمومی استخراج شده از ورودی ایجاد می کنیم و `ecdsa.Verify` را اجرا می کنیم. امضای استخراج شده از ورودی را تأیید می کنیم. اگر همه ورودی‌ها تأیید شوند، true را برگردانید. اگر حداقل یک ورودی تأیید نشد، false را برگردانید.

اکنون برای به دست آوردن تراکنش های قبلی به یک تابع نیاز داریم. از آنجایی که این امر مستلزم تعامل با بلاکچین است، آن را به روشی از بلاکچین تبدیل خواهیم کرد:

```go
func (bc *Blockchain) FindTransaction(ID []byte) (Transaction, error) {
	bci := bc.Iterator()

	for {
		block := bci.Next()

		for _, tx := range block.Transactions {
			if bytes.Compare(tx.ID, ID) == 0 {
				return *tx, nil
			}
		}

		if len(block.PrevBlockHash) == 0 {
			break
		}
	}

	return Transaction{}, errors.New("Transaction is not found")
}

func (bc *Blockchain) SignTransaction(tx *Transaction, privKey ecdsa.PrivateKey) {
	prevTXs := make(map[string]Transaction)

	for _, vin := range tx.Vin {
		prevTX, err := bc.FindTransaction(vin.Txid)
		prevTXs[hex.EncodeToString(prevTX.ID)] = prevTX
	}

	tx.Sign(privKey, prevTXs)
}

func (bc *Blockchain) VerifyTransaction(tx *Transaction) bool {
	prevTXs := make(map[string]Transaction)

	for _, vin := range tx.Vin {
		prevTX, err := bc.FindTransaction(vin.Txid)
		prevTXs[hex.EncodeToString(prevTX.ID)] = prevTX
	}

	return tx.Verify(prevTXs)
}
```

این توابع ساده هستند: FindTransaction یک تراکنش را با شناسه پیدا می کند (این کار مستلزم تکرار بر روی تمام بلوک های زنجیره بلاک است). SignTransaction یک تراکنش را می گیرد، تراکنش هایی را که به آن ارجاع می دهد پیدا می کند و آن را امضا می کند. VerifyTransaction هم همین کار را می کند، اما در عوض تراکنش را تأیید می کند.

اکنون، ما باید در واقع معاملات را امضا و تأیید کنیم. امضا در NewUTXOTtransaction انجام می شود:

```go
func NewUTXOTransaction(from, to string, amount int, bc *Blockchain) *Transaction {
	...

	tx := Transaction{nil, inputs, outputs}
	tx.ID = tx.Hash()
	bc.SignTransaction(&tx, wallet.PrivateKey)

	return &tx
}
```

تأیید قبل از قرار دادن یک تراکنش در یک بلوک انجام می شود:

```go
func (bc *Blockchain) MineBlock(transactions []*Transaction) {
	var lastHash []byte

	for _, tx := range transactions {
		if bc.VerifyTransaction(tx) != true {
			log.Panic("ERROR: Invalid transaction")
		}
	}
	...
}
```

و بس! بیایید یک بار دیگر همه چیز را بررسی کنیم:

```txt
$ blockchain_go createwallet
Your new address: 1AmVdDvvQ977oVCpUqz7zAPUEiXKrX5avR

$ blockchain_go createwallet
Your new address: 1NE86r4Esjf53EL7fR86CsfTZpNN42Sfab

$ blockchain_go createblockchain -address 1AmVdDvvQ977oVCpUqz7zAPUEiXKrX5avR
000000122348da06c19e5c513710340f4c307d884385da948a205655c6a9d008
Done!

$ blockchain_go send -from 1AmVdDvvQ977oVCpUqz7zAPUEiXKrX5avR -to 1NE86r4Esjf53EL7fR86CsfTZpNN42Sfab -amount 6
0000000f3dbb0ab6d56c4e4b9f7479afe8d5a5dad4d2a8823345a1a16cf3347b
Success!

$ blockchain_go getbalance -address 1AmVdDvvQ977oVCpUqz7zAPUEiXKrX5avR
Balance of '1AmVdDvvQ977oVCpUqz7zAPUEiXKrX5avR': 4

$ blockchain_go getbalance -address 1NE86r4Esjf53EL7fR86CsfTZpNN42Sfab
Balance of '1NE86r4Esjf53EL7fR86CsfTZpNN42Sfab': 6
```

هیچ چیز خراب نیست. عالی!

بیایید همچنین در مورد فراخوان bc.SignTransaction (&tx، wallet.PrivateKey) در NewUTXOTransaction نظر بدهیم تا اطمینان حاصل شود که تراکنش‌های بدون امضا قابل استخراج نیستند:

```go
func NewUTXOTransaction(from, to string, amount int, bc *Blockchain) *Transaction {
   ...
	tx := Transaction{nil, inputs, outputs}
	tx.ID = tx.Hash()
	// bc.SignTransaction(&tx, wallet.PrivateKey)

	return &tx
}
```

```txt
$ go install

$ blockchain_go send -from 1AmVdDvvQ977oVCpUqz7zAPUEiXKrX5avR -to 1NE86r4Esjf53EL7fR86CsfTZpNN42Sfab -amount 1
2017/09/12 16:28:15 ERROR: Invalid transaction
```

#### نتیجه گیری

این واقعاً عالی است که ما تا اینجای کار انجام داده ایم و بسیاری از ویژگی های کلیدی بیت کوین را اجرا کرده ایم! ما تقریباً همه چیز را در خارج از شبکه پیاده سازی کرده ایم، و در قسمت بعدی، تراکنش ها را به پایان خواهیم رساند.

پایان قسمت پنجم

_**[قسمت ششم](/blog/post_45_bc6)**_

_**باقی قسمت ها نسخه اصلی**_

* [Building Blockchain in Go. Part 1: Basic Prototype](https://jeiwan.net/posts/building-blockchain-in-go-part-1/)
* [Building Blockchain in Go. Part 2: Proof-of-Work](https://jeiwan.net/posts/building-blockchain-in-go-part-2/)
* [Building Blockchain in Go. Part 3: Persistence and CLI](https://jeiwan.net/posts/building-blockchain-in-go-part-3/)
* [Building Blockchain in Go. Part 4: Transactions 1](https://jeiwan.net/posts/building-blockchain-in-go-part-4/)
* [Building Blockchain in Go. Part 5: Addresses](https://jeiwan.net/posts/building-blockchain-in-go-part-5/)
* [Building Blockchain in Go. Part 6: Transactions 2](https://jeiwan.net/posts/building-blockchain-in-go-part-6/)
* [Building Blockchain in Go. Part 7: Network](https://jeiwan.net/posts/building-blockchain-in-go-part-7/)