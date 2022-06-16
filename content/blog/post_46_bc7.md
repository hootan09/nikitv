---
title: ساخت بلاکچین با نگاهی به ساختار بین کوین - قسمت هفتم (شبکه)
image: images/blockchain/3hxahdamqiu1.jpeg
description: مقدمه تا کنون، ما یک بلاکچین ساخته‌ایم که همه ویژگی‌های کلیدی را دارد آدرس‌های ناشناس، امن و به‌طور تصادفی تولید شده. ذخیره سازی داده های ب…
date: 2022-06-16T11:01:34+04:30
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

تا کنون، ما یک بلاکچین ساخته‌ایم که همه ویژگی‌های کلیدی را دارد: آدرس‌های ناشناس، امن و به‌طور تصادفی تولید شده. ذخیره سازی داده های بلاکچین؛ سیستم اثبات کار؛ روشی قابل اعتماد برای ذخیره تراکنش ها در حالی که این ویژگی ها بسیار مهم هستند، کافی نیست. چیزی که باعث می‌شود این ویژگی‌ها واقعاً بدرخشند و رمزارزها را ممکن می‌سازد، شبکه است. اجرای چنین پیاده‌سازی بلاکچین فقط روی یک رایانه چه فایده ای دارد؟ وقتی فقط یک کاربر وجود دارد، این ویژگی‌های مبتنی بر رمزنگاری چه کاربردی دارند؟ این شبکه است که باعث می شود همه این مکانیسم ها کار کنند و مفید باشند.

شما می توانید آن ویژگی های بلاکچین را به عنوان قوانینی در نظر بگیرید، شبیه به قوانینی که افراد زمانی که می خواهند با هم زندگی کنند و پیشرفت کنند، وضع می کنند. نوعی ترتیبات اجتماعی. شبکه بلاکچین جامعه ای از برنامه هاست که از قوانین مشابهی پیروی می کنند و این پیروی از قوانین است که شبکه را زنده می کند. به همین ترتیب، وقتی مردم ایده‌های یکسانی دارند، قوی‌تر می‌شوند و می‌توانند با هم زندگی بهتری بسازند. اگر افرادی وجود داشته باشند که از مجموعه قوانین متفاوتی پیروی کنند، در یک جامعه جداگانه (ایالت، مزرعه اشتراکی و غیره) زندگی خواهند کرد. به طور مشابه، اگر گره های بلاکچینی وجود داشته باشند که از قوانین متفاوتی پیروی می کنند، یک شبکه جداگانه تشکیل می دهند.

_**این بسیار مهم است:**_ بدون شبکه و بدون اکثریت گره هایی که قوانین یکسانی را به اشتراک می گذارند، این قوانین بی فایده هستند!

> _**سلب مسئولیت**_: متأسفانه، من زمان کافی برای پیاده سازی یک نمونه اولیه شبکه P2P واقعی را نداشتم. در این مقاله من رایج ترین سناریو را نشان خواهم داد که شامل گره هایی از انواع مختلف است. بهبود این سناریو و تبدیل آن به یک شبکه P2P می تواند چالش و تمرین خوبی برای شما باشد! همچنین من نمی توانم تضمین کنم که سناریوهای دیگر به جز آنچه در این مقاله اجرا شده است، کار می کنند. متاسفم!

> این بخش تغییرات قابل توجهی را در کد ایجاد می کند، بنابراین توضیح همه آنها در اینجا بی معنی است. لطفاً برای مشاهده تمام تغییرات از آخرین مقاله به_**[ این صفحه](https://github.com/Jeiwan/blockchain_go/compare/part_6...part_7#files_bucket)**_ مراجعه کنید.

#### شبکه بلاکچین

شبکه بلاکچین غیرمتمرکز است، به این معنی که هیچ سروری وجود ندارد که کارها را انجام دهد و همین طور کلاینت هایی که از سرورها برای دریافت یا پردازش داده ها استفاده می کنند. در شبکه بلاکچین گره هایی وجود دارد و هر گره عضوی کامل از شبکه است. یک گره همه چیز است: هم مشتری و هم سرور است. این بسیار مهم است که به خاطر داشته باشید، زیرا با برنامه های وب معمولی بسیار متفاوت است.

شبکه بلاکچین یک شبکه P2P (Peer-to-Peer) است، به این معنی که گره ها مستقیماً به یکدیگر متصل می شوند. توپولوژی آن مسطح (flat) است، زیرا هیچ سلسله مراتبی در نقش های گره وجود ندارد. در اینجا نمایش شماتیک آن:

{{< image src="/images/blockchain/b28wjzjk9gj0.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

گره ها در چنین شبکه ای برای پیاده سازی دشوارتر هستند، زیرا آنها باید عملیات زیادی را انجام دهند. هر گره باید با چندین گره دیگر تعامل داشته باشد، باید وضعیت گره دیگر را درخواست کند، آن را با وضعیت خود مقایسه کند و زمانی که قدیمی است وضعیت خود را به روز کند.

#### نقش های گره

گره های بلاکچین علیرغم کامل بودن، می توانند نقش های متفاوتی را در شبکه ایفا کنند. اینجا اند:

_**۱- معدن کار.**_

چنین گره هایی بر روی سخت افزار قدرتمند یا تخصصی (مانند ASIC) اجرا می شوند و تنها هدف آنها استخراج بلوک های جدید در سریع ترین زمان ممکن است. ماینرها فقط در بلاکچین هایی که از Proof-of-Work استفاده می کنند امکان پذیر است، زیرا ماینینگ در واقع به معنای حل پازل های PoW است. برای مثال، در بلاکچین های Proof-of-Stake، ماینینگ وجود ندارد.

_**۲- گره کامل**_

این گره ها بلوک های استخراج شده توسط ماینرها را تایید می کنند و تراکنش ها را تایید می کنند. برای انجام این کار، آنها باید کل نسخه بلاکچین را داشته باشند. همچنین، چنین گره هایی چنین عملیات مسیریابی را انجام می دهند، مانند کمک به سایر گره ها برای کشف یکدیگر.

برای شبکه بسیار مهم است که تعداد زیادی گره کامل داشته باشد، زیرا این گره ها هستند که تصمیم می گیرند: آنها تصمیم می گیرند که آیا یک بلوک یا تراکنش معتبر است یا خیر.

_**۳- گره SPV.**_

این SPV مخفف عبارت Simplified Payment Verification است. این گره‌ها یک کپی کامل از بلاکچین را ذخیره نمی‌کنند، اما همچنان می‌توانند تراکنش‌ها را تأیید کنند (نه همه آن‌ها، بلکه یک زیرمجموعه، برای مثال، آنهایی که به آدرس خاصی ارسال شده‌اند). یک گره SPV به یک گره کامل برای دریافت داده بستگی دارد، و ممکن است گره های SPV زیادی به یک گره کامل متصل شوند. SPV برنامه های کیف پول را ممکن می کند: نیازی به دانلود کامل بلاکچین نیست، اما همچنان می تواند تراکنش های خود را تأیید کند.

#### ساده سازی شبکه

برای پیاده سازی شبکه در بلاکچین، باید مواردی را ساده کنیم. مشکل این است که ما کامپیوترهای زیادی برای شبیه سازی یک شبکه با چندین گره نداریم. ما می‌توانستیم از ماشین‌های مجازی یا داکر برای حل این مشکل استفاده کنیم، اما می‌تواند همه چیز را دشوارتر کند: شما باید مشکلات احتمالی ماشین مجازی یا داکر را حل کنید، در حالی که هدف من تمرکز بر پیاده‌سازی بلاکچین است. بنابراین، ما می خواهیم چندین گره بلاک چین را روی یک ماشین اجرا کنیم و در همان زمان می خواهیم آدرس های متفاوتی داشته باشند. برای رسیدن به این هدف، به جای آدرس های IP، از پورت ها به عنوان شناسه گره ها استفاده می کنیم. به عنوان مثال، گره هایی با آدرس های: 127.0.0.1:3000، 127.0.0.1:3001، 127.0.0.1:3002 و غیره وجود خواهند داشت. ما شناسه گره پورت را فراخوانی می کنیم و از متغیر محیطی NODE_ID برای تنظیم آنها استفاده می کنیم. بنابراین، می توانید چندین پنجره ترمینال را باز کنید، NODE_ID های مختلف را تنظیم کنید و گره های مختلفی در حال اجرا داشته باشید.

این رویکرد همچنین مستلزم داشتن بلاکچین ها و فایل های کیف پول متفاوت است. آنها اکنون باید به شناسه گره وابسته باشند و مانند blockchain_3000.db، blockchain_30001.db و wallet_3000.db، wallet_30001.db، و غیره نامگذاری شوند.

#### پیاده سازی

بنابراین، چه اتفاقی می افتد که مثلاً بیت کوین Core را دانلود کرده و برای اولین بار اجرا می کنید؟ برای دانلود آخرین وضعیت بلاکچین، باید به یک گره متصل شود. با توجه به اینکه رایانه شما از همه یا برخی از گره های بیت کوین آگاه نیست، این گره چیست؟

هاردکد کردن آدرس گره در بیت کوین Core یک اشتباه است: گره ممکن است مورد حمله قرار گیرد یا بسته شود، که می تواند منجر به عدم امکان پیوستن گره های جدید به شبکه شود. در عوض، در Bitcoin Core، دانه های _**[DNS](https://bitcoin.org/en/glossary/dns-seed)**_ هاردکد شده وجود دارد. اینها گره نیستند، بلکه سرورهای DNS هستند که آدرس برخی از گره ها را می دانند. هنگامی که یک هسته بیت کوین تمیز را راه اندازی می کنید، به یکی از دانه ها متصل می شود و لیستی از گره های کامل را دریافت می کند که سپس بلاکچین را از آن دانلود می کند.

در اجرای ما، متمرکز سازی (centralization) وجود خواهد داشت. ما سه گره خواهیم داشت:

۱- گره مرکزی. این همان گرهی است که تمام گره های دیگر به آن متصل می شوند و این گره ای است که داده ها را بین گره های دیگر ارسال می کند.

۲- یک گره ماینر. این گره تراکنش های جدید را در mempool ذخیره می کند و زمانی که تراکنش های کافی وجود داشته باشد، یک بلوک جدید استخراج می کند.

۳- یک گره کیف پول این گره برای ارسال سکه بین کیف پول ها استفاده خواهد شد. برخلاف گره‌های SPV، یک نسخه کامل از بلاک چین را ذخیره می‌کند.

#### سناریو

هدف این مقاله اجرای سناریوی زیر است:

۱- گره مرکزی یک بلاکچین ایجاد می کند.

۲- گره دیگر (کیف پول) به آن متصل شده و بلاک چین را دانلود می کند.

۳- یک گره دیگر (ماینر) به گره مرکزی متصل می شود و بلاک چین را دانلود می کند.

۴- گره کیف پول یک تراکنش ایجاد می کند.

۵- گره های ماینر تراکنش را دریافت کرده و آن را در حوضه حافظه خود نگه می دارند.

۶- هنگامی که تراکنش های کافی در استخر حافظه وجود دارد، ماینر شروع به استخراج یک بلوک جدید می کند.

۷- هنگامی که یک بلوک جدید استخراج می شود، به گره مرکزی ارسال می شود.

۸- گره کیف پول با گره مرکزی همگام می شود.

۹- کاربر گره کیف پول چک می کند که پرداخت آنها موفقیت آمیز بوده است.

این چیزی است که در بیت کوین به نظر می رسد. حتی اگر قرار نیست یک شبکه P2P واقعی بسازیم، یک مورد استفاده واقعی و اصلی و مهم بیت کوین را پیاده سازی خواهیم کرد.

#### نسخه یا version

گره ها از طریق پیام ها ارتباط برقرار می کنند. هنگامی که یک گره جدید اجرا می شود، چندین گره از یک دانه DNS دریافت می کند و پیام نسخه یا ورژن را برای آنها ارسال می کند که در پیاده سازی ما به شکل زیر خواهد بود:

```go
type version struct {
    Version    int
    BestHeight int
    AddrFrom   string
}
```

ما فقط یک نسخه بلاک چین داریم، بنابراین قسمت Version هیچ اطلاعات مهمی را حفظ نخواهد کرد. BestHeight طول بلاکچین گره را ذخیره می کند. AddFrom آدرس فرستنده را ذخیره می کند.

گره ای که پیام نسخه یا ورژن را دریافت می کند چه کاری باید انجام دهد؟ با پیام نسخه خودش پاسخ خواهد داد. این یک نوع دست دادن است: هیچ تعامل دیگری بدون احوالپرسی قبلی امکان پذیر نیست. اما این فقط ادب نیست: نسخه برای یافتن یک بلاک چین طولانی تر استفاده می شود. هنگامی که یک گره یک پیام نسخه دریافت می کند، بررسی می کند که آیا بلاک چین گره از مقدار BestHeight طولانی تر است یا خیر. اگر اینطور نباشد، گره بلوک های گمشده را درخواست و دانلود می کند.

برای دریافت پیام به سرور نیاز داریم:

```go
var nodeAddress string
var knownNodes = []string{"localhost:3000"}

func StartServer(nodeID, minerAddress string) {
    nodeAddress = fmt.Sprintf("localhost:%s", nodeID)
    miningAddress = minerAddress
    ln, err := net.Listen(protocol, nodeAddress)
    defer ln.Close()

    bc := NewBlockchain(nodeID)

    if nodeAddress != knownNodes[0] {
        sendVersion(knownNodes[0], bc)
    }

    for {
        conn, err := ln.Accept()
        go handleConnection(conn, bc)
    }
}
```

ابتدا آدرس گره مرکزی را هاردکد می کنیم: هر گره باید ابتدا بداند که به کجا متصل شود. آرگومان minerAddress آدرسی را برای دریافت پاداش استخراج مشخص می کند. این قطعه:

```go
if nodeAddress != knownNodes[0] {
    sendVersion(knownNodes[0], bc)
}
```

به این معنی که اگر گره فعلی، گره مرکزی نیست، باید پیام نسخه را به گره مرکزی ارسال کند تا متوجه شود که آیا بلاک چین قدیمی است یا خیر.

```go
func sendVersion(addr string, bc *Blockchain) {
    bestHeight := bc.GetBestHeight()
    payload := gobEncode(version{nodeVersion, bestHeight, nodeAddress})

    request := append(commandToBytes("version"), payload...)

    sendData(addr, request)
}
```

پیام های ما، در سطح پایین تر، دنباله ای از بایت هستند. 12 بایت اول نام فرمان ("نسخه" در این مورد) را مشخص می کند، و بایت های دوم حاوی ساختار پیام با gob-encoded هستند. commandToBytes به شکل زیر است:

```go
func commandToBytes(command string) []byte {
    var bytes [commandLength]byte

    for i, c := range command {
        bytes[i] = byte(c)
    }

    return bytes[:]
}
```

یک بافر 12 بایتی ایجاد می کند و آن را با نام فرمان پر می کند و باقی بایت های را خالی می گذارد. یک تابع مخالف وجود دارد:

```go
func bytesToCommand(bytes []byte) string {
    var command []byte

    for _, b := range bytes {
        if b != 0x0 {
            command = append(command, b)
        }
    }

    return fmt.Sprintf("%s", command)
}
```

هنگامی که یک گره دستوری را دریافت می کند، bytesToCommand را برای استخراج نام فرمان اجرا می کند و بدنه فرمان را با کنترل کننده صحیح پردازش می کند:

```go
func handleConnection(conn net.Conn, bc *Blockchain) {
    request, err := ioutil.ReadAll(conn)
    command := bytesToCommand(request[:commandLength])
    fmt.Printf("Received %s command\n", command)

    switch command {
    ...
    case "version":
        handleVersion(request, bc)
    default:
        fmt.Println("Unknown command!")
    }

    conn.Close()
}
```

بسیار خوب، این چیزی است که کنترل کننده فرمان نسخه یا ورژن به نظر می رسد:

```go
func handleVersion(request []byte, bc *Blockchain) {
    var buff bytes.Buffer
    var payload verzion

    buff.Write(request[commandLength:])
    dec := gob.NewDecoder(&buff)
    err := dec.Decode(&payload)

    myBestHeight := bc.GetBestHeight()
    foreignerBestHeight := payload.BestHeight

    if myBestHeight < foreignerBestHeight {
        sendGetBlocks(payload.AddrFrom)
    } else if myBestHeight > foreignerBestHeight {
        sendVersion(payload.AddrFrom, bc)
    }

    if !nodeIsKnown(payload.AddrFrom) {
        knownNodes = append(knownNodes, payload.AddrFrom)
    }
}
```

ابتدا باید درخواست را رمزگشایی کرده و payload را استخراج کنیم. این شبیه به همه کنترل‌کننده‌ها است، بنابراین من این قطعه را در کدهای آتی حذف خواهم کرد.

سپس یک گره BestHeight خود را با یکی از پیام مقایسه می کند. اگر زنجیره بلاک گره طولانی تر باشد، با پیام نسخه پاسخ می دهد. در غیر این صورت، پیام getblock ارسال می کند.

#### ساختار getblocks

```go
type getblocks struct {
    AddrFrom string
}
```

این getblocks به معنای «به من نشان دهید چه بلاک هایی دارید» (در بیت کوین، پیچیده تر است). توجه کنید، نمی‌گوید «همه بلوک‌هایتان را به من بدهید»، در عوض فهرستی از هش‌های بلاک را درخواست می‌کند. این کار برای کاهش بار شبکه انجام می شود، زیرا بلوک ها را می توان از گره های مختلف دانلود کرد و ما نمی خواهیم ده ها گیگابایت از یک گره دانلود کنیم.

مدیریت دستور به آسانی:

```go
func handleGetBlocks(request []byte, bc *Blockchain) {
    ...
    blocks := bc.GetBlockHashes()
    sendInv(payload.AddrFrom, "block", blocks)
}
```

در پیاده سازی ساده ما، همه هش های بلوک را برمی گرداند.

#### ساختارinv

```go
type inv struct {
    AddrFrom string
    Type     string
    Items    [][]byte
}
```

بیت کوین از inv استفاده می کند تا به سایر گره ها نشان دهد که گره فعلی چه بلوک ها یا تراکنش هایی دارد. باز هم، شامل بلوک‌ها و تراکنش‌های کامل نیست، فقط هش‌های آنها را شامل می‌شود. فیلد Type می گوید که آیا اینها بلوک هستند یا تراکنش.

مدیریت inv دشوارتر است:

```go
func handleInv(request []byte, bc *Blockchain) {
    ...
    fmt.Printf("Recevied inventory with %d %s\n", len(payload.Items), payload.Type)

    if payload.Type == "block" {
        blocksInTransit = payload.Items

        blockHash := payload.Items[0]
        sendGetData(payload.AddrFrom, "block", blockHash)

        newInTransit := [][]byte{}
        for _, b := range blocksInTransit {
            if bytes.Compare(b, blockHash) != 0 {
                newInTransit = append(newInTransit, b)
            }
        }
        blocksInTransit = newInTransit
    }

    if payload.Type == "tx" {
        txID := payload.Items[0]

        if mempool[hex.EncodeToString(txID)].ID == nil {
            sendGetData(payload.AddrFrom, "tx", txID)
        }
    }
}
```

اگر هش بلوک ها منتقل شوند، می خواهیم آنها را در متغیر blocksInTransit ذخیره کنیم تا بلوک های دانلود شده را ردیابی کنیم. این به ما امکان می دهد بلوک ها را از گره های مختلف دانلود کنیم. بلافاصله پس از قرار دادن بلوک ها در حالت ترانزیت، دستور getdata را به فرستنده پیام inv ارسال می کنیم و blocksInTransit را به روز می کنیم. در یک شبکه P2P واقعی، ما می خواهیم بلوک ها را از گره های مختلف منتقل کنیم.

در اجرای خود، هرگز inv را با هش های متعدد ارسال نمی کنیم. 

به همین دلیل است که وقتی payload.Type == "tx" فقط اولین هش گرفته می شود. سپس بررسی می کنیم که آیا از قبل هش را در mempool خود داریم یا خیر، و اگر نه، پیام getdata ارسال می شود.

#### ساختار getdata

```go
type getdata struct {
    AddrFrom string
    Type     string
    ID       []byte
}
```

این getdata یک درخواست برای بلوک یا تراکنش خاص است و می تواند فقط یک شناسه بلوک/تراکنش داشته باشد.

```go
func handleGetData(request []byte, bc *Blockchain) {
    ...
    if payload.Type == "block" {
        block, err := bc.GetBlock([]byte(payload.ID))

        sendBlock(payload.AddrFrom, &block)
    }

    if payload.Type == "tx" {
        txID := hex.EncodeToString(payload.ID)
        tx := mempool[txID]

        sendTx(payload.AddrFrom, &tx)
    }
}
```

کنترل کننده ساده است: اگر آنها درخواست بلوک کردند، بلوک را برگردانید. اگر آنها درخواست معامله کردند، تراکنش را برگردانید. توجه داشته باشید که ما بررسی نمی کنیم که آیا واقعاً این بلوک یا تراکنش را داریم یا خیر. این یک عیب است :)

#### بلوک و tx

```go
type block struct {
    AddrFrom string
    Block    []byte
}

type tx struct {
    AddFrom     string
    Transaction []byte
}
```

این پیام ها هستند که در واقع داده ها را منتقل می کنند.

مدیریت پیام بلوک آسان است:

```go
func handleBlock(request []byte, bc *Blockchain) {
    ...

    blockData := payload.Block
    block := DeserializeBlock(blockData)

    fmt.Println("Recevied a new block!")
    bc.AddBlock(block)

    fmt.Printf("Added block %x\n", block.Hash)

    if len(blocksInTransit) > 0 {
        blockHash := blocksInTransit[0]
        sendGetData(payload.AddrFrom, "block", blockHash)

        blocksInTransit = blocksInTransit[1:]
    } else {
        UTXOSet := UTXOSet{bc}
        UTXOSet.Reindex()
    }
}
```

وقتی یک بلوک جدید دریافت کردیم، آن را در بلاکچین خود قرار می دهیم. اگر بلوک های بیشتری برای دانلود وجود دارد، آنها را از همان گره ای که بلوک قبلی دانلود کرده بودیم درخواست می کنیم. هنگامی که ما در نهایت همه بلوک ها را دانلود کردیم، مجموعه UTXO مجددا ایندکس می شود.

> یک TODO: به جای اعتماد بی قید و شرط، باید هر بلوک ورودی را قبل از اضافه کردن آن به بلاک چین اعتبارسنجی کنیم.

> یک TODOدیگر: به جای اجرای UTXOSet.Reindex()، باید از UTXOSet.Update(block) استفاده شود، زیرا اگر بلاک چین بزرگ باشد، فهرست مجدد کل مجموعه UTXO به زمان زیادی نیاز دارد.

مدیریت پیام های tx سخت ترین بخش است:

```go
func handleTx(request []byte, bc *Blockchain) {
    ...
    txData := payload.Transaction
    tx := DeserializeTransaction(txData)
    mempool[hex.EncodeToString(tx.ID)] = tx

    if nodeAddress == knownNodes[0] {
        for _, node := range knownNodes {
            if node != nodeAddress && node != payload.AddFrom {
                sendInv(node, "tx", [][]byte{tx.ID})
            }
        }
    } else {
        if len(mempool) >= 2 && len(miningAddress) > 0 {
        MineTransactions:
            var txs []*Transaction

            for id := range mempool {
                tx := mempool[id]
                if bc.VerifyTransaction(&tx) {
                    txs = append(txs, &tx)
                }
            }

            if len(txs) == 0 {
                fmt.Println("All transactions are invalid! Waiting for new ones...")
                return
            }

            cbTx := NewCoinbaseTX(miningAddress, "")
            txs = append(txs, cbTx)

            newBlock := bc.MineBlock(txs)
            UTXOSet := UTXOSet{bc}
            UTXOSet.Reindex()

            fmt.Println("New block is mined!")

            for _, tx := range txs {
                txID := hex.EncodeToString(tx.ID)
                delete(mempool, txID)
            }

            for _, node := range knownNodes {
                if node != nodeAddress {
                    sendInv(node, "block", [][]byte{newBlock.Hash})
                }
            }

            if len(mempool) > 0 {
                goto MineTransactions
            }
        }
    }
}
```

اولین کاری که باید انجام دهید این است که تراکنش جدید را در mempool قرار دهید (باز هم، تراکنش ها باید قبل از قرار گرفتن در mempool تأیید شوند). قطعه بعدی:

```go
if nodeAddress == knownNodes[0] {
    for _, node := range knownNodes {
        if node != nodeAddress && node != payload.AddFrom {
            sendInv(node, "tx", [][]byte{tx.ID})
        }
    }
}
```

بررسی می کند که آیا گره فعلی گره مرکزی است یا خیر. در پیاده سازی ما، گره مرکزی بلوک ها را استخراج نمی کند. در عوض، تراکنش‌های جدید را به گره‌های دیگر شبکه ارسال می‌کند.

قطعه بزرگ بعدی فقط برای گره های ماینر است. بیایید آن را به قطعات کوچکتر تقسیم کنیم:

```go
if len(mempool) >= 2 && len(miningAddress) > 0 {
```

این miningAddress فقط روی گره‌های ماینر تنظیم می‌شود. هنگامی که 2 یا بیشتر تراکنش در ممپول گره فعلی (ماینر) وجود دارد، استخراج شروع می شود.

```go
for id := range mempool {
    tx := mempool[id]
    if bc.VerifyTransaction(&tx) {
        txs = append(txs, &tx)
    }
}

if len(txs) == 0 {
    fmt.Println("All transactions are invalid! Waiting for new ones...")
    return
}
```

ابتدا، تمام تراکنش‌های موجود در mempool تأیید می‌شوند. تراکنش های نامعتبر نادیده گرفته می شوند و در صورت عدم وجود تراکنش های معتبر، استخراج قطع می شود.

```go
cbTx := NewCoinbaseTX(miningAddress, "")
txs = append(txs, cbTx)

newBlock := bc.MineBlock(txs)
UTXOSet := UTXOSet{bc}
UTXOSet.Reindex()

fmt.Println("New block is mined!")
```

تراکنش های تایید شده در یک بلوک و همچنین یک تراکنش کوین بیس همراه با پاداش قرار می گیرند. پس از استخراج بلوک، مجموعه UTXO دوباره ایندکس می شود.

> یک TODO: دوباره باید از UTXOSet.Update به جای UTXOSet.Reindex استفاده شود.

```go
for _, tx := range txs {
    txID := hex.EncodeToString(tx.ID)
    delete(mempool, txID)
}

for _, node := range knownNodes {
    if node != nodeAddress {
        sendInv(node, "block", [][]byte{newBlock.Hash})
    }
}

if len(mempool) > 0 {
    goto MineTransactions
}
```

پس از استخراج تراکنش، از mempool حذف می شود. هر گره دیگری که گره فعلی از آن آگاه است، پیام inv را با هش بلوک جدید دریافت می کند. آنها می توانند پس از رسیدگی به پیام، بلوک را درخواست کنند.

#### نتیجه

بیایید سناریویی را که قبلا تعریف کرده بودیم بازی کنیم.

ابتدا NODE_ID را روی 3000 تنظیم کنید (export NODE_ID=3000) در اولین پنجره ترمینال. قبل از پاراگراف‌های بعدی از نشان‌هایی مانند NODE 3000 یا NODE 3001 استفاده خواهم کرد تا بدانید روی چه گره‌ای باید اقدامات انجام دهید.

نود 3000

یک کیف پول و یک بلاک چین جدید بسازید:

```txt
$ blockchain_go createblockchain -address CENTREAL_NODE
```

(برای وضوح و اختصار از آدرس های جعلی استفاده خواهم کرد)

پس از آن، زنجیره بلوکی حاوی بلوک تک تک پیدایش خواهد بود. ما باید بلوک را ذخیره کنیم و از آن در گره های دیگر استفاده کنیم. بلوک‌های Genesis به عنوان شناسه‌های زنجیره‌های بلوکی عمل می‌کنند (در بیت‌کوین کور، بلوک پیدایش کدگذاری شده است).

```txt
$ cp blockchain_3000.db blockchain_genesis.db
```

NODE 3001

بعد، یک پنجره ترمینال جدید باز کنید و شناسه گره را روی 3001 تنظیم کنید. این یک گره کیف پول خواهد بود. برخی از آدرس‌ها را با بلاکچین_گو کیف پول ایجاد کنید، این آدرس‌ها را WALLET_1، WALLET_2، WALLET_3 می‌نامیم.

نود 3000

چند سکه به آدرس های کیف پول ارسال کنید:

```txt
$ blockchain_go send -from CENTREAL_NODE -to WALLET_1 -amount 10 -mine
$ blockchain_go send -from CENTREAL_NODE -to WALLET_2 -amount 10 -mine
```


پرچم mine- به این معنی است که بلوک بلافاصله توسط همان گره استخراج می شود. ما باید این پرچم را داشته باشیم زیرا در ابتدا هیچ گره ماینری در شبکه وجود ندارد.

گره را شروع کنید:

```txt
$ blockchain_go startnode
```

گره باید تا پایان سناریو در حال اجرا باشد.

گره 3001

بلاک چین گره را با بلوک پیدایش ذخیره شده در بالا شروع کنید:

```txt
$ cp blockchain_genesis.db blockchain_3001.db
```

گره را اجرا کنید

```txt
$ blockchain_go startnode
```

تمام بلوک ها را از گره مرکزی دانلود می کند. برای بررسی اینکه همه چیز درست است، گره را متوقف کنید و تعادل را بررسی کنید:

```txt
$ blockchain_go getbalance -address WALLET_1  
Balance of 'WALLET_1': 10  

$ blockchain_go getbalance -address WALLET_2  
Balance of 'WALLET_2': 10
```

همچنین، می توانید تعادل آدرس CENTRAL_NODE را بررسی کنید، زیرا گره 3001 اکنون زنجیره بلوکی خود را دارد:

```txt
$ blockchain_go getbalance -address CENTRAL_NODE  
Balance of 'CENTRAL_NODE': 10
```

گره 3002

یک پنجره ترمینال جدید باز کنید و شناسه آن را روی 3002 تنظیم کنید و یک کیف پول ایجاد کنید. این یک گره ماینر خواهد بود. بلاک چین را راه اندازی کنید:

```txt
$ cp blockchain_genesis.db blockchain_3002.db
```

و گره را شروع کنید:

```txt
$ blockchain_go startnode -miner MINER_WALLET
```

گره 3001

ارسال چند سکه:

```txt
$ blockchain_go send -from WALLET_1 -to WALLET_3 -amount 1  
$ blockchain_go send -from WALLET_2 -to WALLET_4 -amount 1
```

گره 3002

به سرعت! به گره ماینر بروید و آن را در حال استخراج یک بلوک جدید ببینید! همچنین خروجی گره مرکزی را بررسی کنید.

گره 3001

به گره کیف پول بروید و آن را شروع کنید:

```txt
$ blockchain_go startnode
```

بلوک تازه استخراج شده را دانلود می کند!

آن را متوقف کنید و تعادل را بررسی کنید:

```txt
$ blockchain_go getbalance -address WALLET_1  
Balance of 'WALLET_1': 9  

$ blockchain_go getbalance -address WALLET_2  
Balance of 'WALLET_2': 9  

$ blockchain_go getbalance -address WALLET_3  
Balance of 'WALLET_3': 1  

$ blockchain_go getbalance -address WALLET_4  
Balance of 'WALLET_4': 1  

$ blockchain_go getbalance -address MINER_WALLET  
Balance of 'MINER_WALLET': 10
```

و تمام

#### نتیجه گیری

این قسمت پایانی سریال بود. من می‌توانستم چند پست دیگر در مورد اجرای یک نمونه اولیه واقعی از یک شبکه P2P منتشر کنم، اما برای این کار وقت ندارم. امیدوارم این مقاله به برخی از سؤالات شما در مورد فناوری بیت کوین پاسخ دهد و سؤالات جدیدی را مطرح کند، که می توانید پاسخ آنها را خودتان بیابید. چیزهای جالب تری در فناوری بیت کوین پنهان شده است! موفق باشید!

پینوشت : همانطور که در پروتکل شبکه بیت کوین توضیح داده شده است، می توانید با پیاده سازی پیام addr، بهبود شبکه را شروع کنید (لینک در زیر). این یک پیام بسیار مهم است، زیرا به گره ها اجازه می دهد یکدیگر را کشف کنند. من شروع به اجرای آن کردم، اما تمام نشده است!

1.  [Source codes](https://github.com/Jeiwan/blockchain_go/tree/part_7)
2.  [Bitcoin protocol documentation](https://en.bitcoin.it/wiki/Protocol_documentation)
3.  [Bitcoin network](https://en.bitcoin.it/wiki/Network)

پایان قسمت هفتم

_**باقی قسمت ها نسخه اصلی**_

* [Building Blockchain in Go. Part 1: Basic Prototype](https://jeiwan.net/posts/building-blockchain-in-go-part-1/)
* [Building Blockchain in Go. Part 2: Proof-of-Work](https://jeiwan.net/posts/building-blockchain-in-go-part-2/)
* [Building Blockchain in Go. Part 3: Persistence and CLI](https://jeiwan.net/posts/building-blockchain-in-go-part-3/)
* [Building Blockchain in Go. Part 4: Transactions 1](https://jeiwan.net/posts/building-blockchain-in-go-part-4/)
* [Building Blockchain in Go. Part 5: Addresses](https://jeiwan.net/posts/building-blockchain-in-go-part-5/)
* [Building Blockchain in Go. Part 6: Transactions 2](https://jeiwan.net/posts/building-blockchain-in-go-part-6/)
* [Building Blockchain in Go. Part 7: Network](https://jeiwan.net/posts/building-blockchain-in-go-part-7/)