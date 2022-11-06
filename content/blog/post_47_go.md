---
title: چگونه به پولدارترین فرد کره زمین تبدیل شویم (و یادگیری کمی گولنگ در مسیر)
image: images/post_47_go/qzzzewwdc2qc.jpg
description: پیاده سازی یک نمونه شبیه سازی شده از روند توزیع ثروت میان فقیر و غنی در جامعه
date: 2022-10-17T11:01:34+04:30
author: mam_niki
tags:
- پول
- ثروت 
- برنامه نویسی
- گولنگ
- شبیه سازی 
categories:
- برنامه نویسی
---

خُب صادقانه بگویم این نوشته بیشتر درباره قسمت دوم عنوان **(یعنی گولنگ)** خواهد بود.

ما یک شبیه سازی بازار با حداقل کد Go خواهیم ساخت و نشان خواهیم داد که **چگونه ثروتمندان حتی زمانی که اصلا حریص نیستند ثروتمندتر می شوند.**


#### نابرابری ثروت

در حالی که چندی پیش، با مقاله ای در Spektrum der Wissenschaft، نسخه آلمانی Scientific American مواجه شدم (**[اینجا](https://www.scientificamerican.com/article/is-inequality-inevitable/)** پیوندی به مقاله اصلی در SA است، جایی که نویسنده، پروفسور بروس ام. بوقوسیان، مدل اقتصادی توزیع ثروت را توضیح می دهد. این مدل یک شبیه‌سازی نسبتاً ساده از معاملات بین عوامل اقتصادی است **و به نظر نمی‌رسد مکانیزم مدل به هیچ وجه به نفع عوامل ثروتمندتر باشد**، در واقع قوانین به نفع عامل فقیرتر از این دو عامل (یعنی فقیر و ثروتمند) ساخته شده است. با این حال، در این مدل، پول به سمت اشخاص کمتر و کمتری جریان می‌یابد که ثروتمندتر و ثروتمندتر می‌شوند. در پایان، بیشتر ثروت متعلق به یک نماینده واحد یا تعداد بسیار کمی از نمایندگان است و بقیه اشخاص تقریباً مالک هیچ چیز نیستند.

چگونه می تواند این باشد؟

#### وارد شبیه سازی های بازار شوید: مدل Yard Sale (یا حراج خانگی)

{{< image src="/images/post_47_go/omtd4zy1wxel.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

توضیح اولیه مدل فروش حیاط است که توسط Anirban Chakraborti توسعه یافته است **[(به توزیع پول در بازارهای مدل اقتصاد مراجعه کنید)](https://arxiv.org/format/cond-mat/0205221/)**. فرض اصلی این است که یک خریدار هرگز دقیقاً به اندازه ارزش کالا پرداخت نمی کند. گاهی اوقات، خریدار بیشتر و گاهی کمتر از ارزش واقعی کالای معامله شده پرداخت می کند.

به عنوان مثال، تصور کنید که از یک حیاط فروشی بازدید می کنید و یک Yokutlix زیبا را کشف می کنید. همانطور که همه می دانند **[Yokutlixe](https://www.fantasynamegenerators.com/lovecraftian-names.php)** ها دارای ارزش اقتصادی 147 دلار هستند. پس از چانه زنی زیاد، فروشنده موافقت می کند که آن را تنها با 122 دلار به شما بدهد.

پس از معامله، ثروت شخصی شما 147 تا 122 دلار یا 25 دلار افزایش یافت، در حالی که ثروت فروشنده به همان میزان کاهش یافت.

در فروش بعدی، **[یک ویجت القاء چمدان رباتیک](https://www.fantasynamegenerators.com/invention-names.php)** پیدا می‌کنید که به نظر می‌رسد نسبتاً استفاده شده است، اما همچنان کار می‌کند، و از آنجایی که از کودکی می‌خواستید یک ویجت محرک چمدان رباتیک داشته باشید، فوراً آن را با قیمت 67 دلار خریداری می‌کنید. (و اعتراف کنید: شما حتی تا 97.50 دلار برای این کار پرداخت می کردید، اینطور نیست؟) اکنون ارزش اقتصادی واقعی این ویجت نه چندان جدید ویجت القاء چمدان رباتیک فقط 50 دلار است، بنابراین این بار ثروت شما کاهش می یابد. 17 دلار، و ثروت فروشنده بر این اساس افزایش می یابد.



بیایید با آن روبرو شویم، شما مهارت استثنایی در چانه زنی ندارید و با هر یک از معاملاتتان، به نظر می رسد ثروت به طور تصادفی به سمت شما یا از شما شناور می شود. اما شما از قبل این را می دانستید، درست است؟

اگر همه مردم یک بازار مثل شما باشند چه؟ اگر همه افراد در یک بازار مهارت های معاملاتی یکسانی داشته باشند، ثروت باید کمابیش به طور مساوی توزیع شود، شاید مانند توزیع نویز سفید.

**با این حال، در شبیه‌سازی Yard Sale، حتی اگر همه شرکت‌کنندگان احتمال یکسانی برای به دست آوردن یا از دست دادن ثروت در یک معامله دارند، ثروت به ناچار به سمت چند نفر (یا حتی یک نفر واحد) جریان می‌یابد و بقیه را در فقر می‌گذارد.**



دوباره - چگونه این اتفاق می افتد؟


#### مدل کازینو

برای پاسخ به این، بیایید نگاهی دقیق تر به مدل Bruce M. Boghosian بیندازیم. من آن را **«مدل کازینو»** می‌نامم زیرا بوقوسیان از استعاره کازینو برای توصیف ماهیت یک معامله استفاده می‌کند: یک روی سکه تصمیم می‌گیرد که آیا یک نماینده در یک معامله ثروت به دست آورد یا از دست بدهد. تنظیم در واقع بسیار ساده است، چند قانون برای توصیف مدل کافی است.

**(توجه: اینها دقیقاً همان قوانینی نیستند که در مقاله ای که من به آن اشاره کردم. چند مدل مشابه در اطراف وجود دارد، و من برای کدنویسی ساده تر چند جنبه را تطبیق دادم، همانطور که بعداً خواهیم دید.)**

1- شبیه سازی شامل یک بازار با تعداد ثابت عامل و مقدار ثابت پول است.

2- همه نمایندگان با یک مقدار پول شروع می کنند.

3- شبیه سازی در دور (گردش) پیشرفت می کند. در هر دور، دو عامل به طور تصادفی انتخاب شده وارد یک تراکنش می شوند.

4- در هر تراکنش، از آنجایی که پول پرداخت شده برای یک کالا هرگز با ارزش واقعی کالا یکسان نیست، ثروت به طور تصادفی از یک عامل به نماینده دیگر جریان می یابد. (مثلاً در شرط‌بندی کازینو، با چرخاندن سکه مشخص می‌شود.)

5- جابجایی ثروت همیشه تنها کسری از ثروت فقیرتر از این دو عامل (فقیر و ثروتمند) است.

6- اگر ثروت از فقیرتر به عامل ثروتمندتر سرازیر شود، f باید کوچکتر از حالت مخالف باشد، یعنی زمانی که ثروت به سمت عامل فقیرتر سرازیر شود.

7- هیچ بدهی مجاز نیست. از این رو نمایندگان نمی توانند بیش از آنچه که دارند ضرر کنند.



قانون 6 به خصوص جالب است. این قانون بدیهی است که به نماینده فقیرتر مزیتی می دهد. برای توضیح این موضوع، اگر عامل فقیرتر ثروت را به دست آورد، اجازه دهید f را بر روی 20٪ و اگر عامل فقیرتر ثروت را از دست بدهد، 17٪ تعیین کنیم. به نظر یک مزیت واقعی برای مامور فقیرتر است، درست است؟

همانطور که خواهیم دید، حتی این مزیت نیز مانع از نابرابری ثروت نمی شود.

#### کد برنامه

من سعی کردم با کمترین کد برای پیاده سازی این شبیه سازی کنار بیایم.

عوامل بازار فقط تکه ای از شناورها هستند که نشان دهنده ثروت هر نماینده است. من عمداً سعی نکردم چند مدل بازیگر مستقل فانتزی با ساختار و روش بسازم. و ممکن است تعجب کنید که آیا این انتخاب خوبی است یا خیر. به هر حال، این یک مدل شبیه‌سازی است، و بنابراین طبیعی به نظر می‌رسد که بازیگران در این شبیه‌سازی به‌عنوان موجودیت‌های مستقل با رفتاری کاملاً تعریف‌شده و وضعیت درونی طراحی شوند. اما فقط برش ها؟!

بله، فقط برش ها. من فکر می کنم مهم است که از دام طراحی بیش از حد یا معماری بیش از حد یک راه حل جلوگیری کنیم. به KISS فکر کنید **(Keep It Simple, Stupid)** - آن را ساده نگه دارید، احمقانه.

و، به‌ویژه وقتی وسوسه می‌شوید که لایه‌هایی از انتزاع بسازید، زیرا فکر می‌کنید باید راه‌حل خود را تعمیم دهید تا بتوانید از آن برای مشکلات مشابه در آینده استفاده مجدد کنید، این احتمال وجود دارد که تو به آن نیاز نخواهی داشت.

اینجا چه نیازی داریم؟

ما به یک حلقه شبیه‌سازی نیاز داریم که به دو عامل اجازه می‌دهد معامله کنند و مقداری ثروت را ببرند یا از دست بدهند. این حلقه را می توان به دو بخش بیشتر تقسیم کرد.

ابتدا باید دو عامل را به صورت تصادفی انتخاب کنیم. سپس، دو عامل وارد معامله می شوند و یکی از آنها مقداری ارزش به دست می آورد و دیگری همان مقدار را از دست می دهد. کدام یک از این دو برنده و کدام یک بازنده کاملا تصادفی است. همانطور که در مقاله ساینتیفیک امریکن توضیح داده شد، می‌توانیم یک سکه را برای آن برگردانیم. اما صبر کنید، ما قبلاً این دو نماینده را به صورت تصادفی انتخاب کردیم. بنابراین می توانیم به سادگی تعریف کنیم که اولین انتخاب شده کسی است که ثروت خود را در تجارت از دست می دهد، در حالی که دیگری کسی است که ثروت به دست می آورد. یک قدم کمتر برای مراقبت.

سپس، معامله انجام می شود. ما باید مشخص کنیم که کدام یک از این دو عامل فقیرتر از این دو است، زیرا ثروتی که بین نمایندگان منتقل می شود به ثروت عامل فقیرتر (قانون شماره 5) و به مزیتی که به عامل فقیرتر اعطا می شود (قانون شماره 6) بستگی دارد.

این تمام چیزی است که نیاز دارد. با این تفاوت که هنوز باید نتیجه را تجسم کنیم! هیچ شبیه سازی خوبی بدون تجسم وجود ندارد.

من می توانم به دنبال یک کتابخانه گراف مانند مثل کتابخانه نمایش نمودار gonum بروم و یک هیستوگرام توزیع ثروت را در پایان شبیه سازی ترسیم کنم. اما من واقعاً ترجیح می دهم در حین اجرای شبیه سازی مقداری خروجی را زنده ببینم. و من بسته‌ای می‌خواهم که پیاده‌سازی آن فوق‌العاده آسان باشد و کد را بیهوده افزایش ندهد. یاد مقاله ای در مورد **[رابط های کاربری مبتنی بر متن](https://appliedgo.net/tui/)** افتادم که چندی پیش نوشتم و از بسته های TUI که در آن زمان تست کردم، termui را انتخاب کردم. این یک ویجت نمودار میله ای را ارائه می دهد و می تواند با چند خط کد تنظیم شود.


{{< image src="/images/post_47_go/9va0z6f1vs7r.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

اما با این حال، یک پیچیدگی جزئی وجود دارد که باید به آن پرداخت. termui یک پوشش موقت روی ترمینال ایجاد می کند، مشابه دستور less در سیستم های لینوکسی . هنگامی که برنامه به پایان می رسد، رابط کاربری ناپدید می شود و ترمینال به محتویات قبلی خود برمی گردد. با این حال، من می خواهم نمودار میله ای را پس از پایان حلقه شبیه سازی قابل مشاهده نگه دارم. termui کنترل رویدادهای صفحه کلید را در دست می گیرد، بنابراین من نمی توانم از رویکرد استاندارد os.Signal برای منتظر ماندن Ctrl-C استفاده کنم. در عوض، من از تابع ```termui’s PollEvents```برای خواندن رویدادهای صفحه کلید و خروج از هر کلیدی استفاده می‌کنم. با فرستادن کانال انجام شده ```(done channel)``` نیز به حلقه شبیه سازی، می توانم شبیه سازی را با فشردن کلید در صورتی که برای مدت طولانی اجرا شود قطع کنم.

```go
package main

import (
	"fmt"
	"log"
	"math/rand"
	"time"

	ui "github.com/gizak/termui/v3"
	"github.com/gizak/termui/v3/widgets"
)

const (
	// Number of agents in the market
	numOfAgents = 10
	// Initial amount of money that each agent owns
	initialWealth = 100.0
	// How many trades to simulate
	rounds = 10000
	// If the poorer agent gains wealth it is this percentage of their total wealth.
	percentGain = 0.20
	// If the poorer agent loses wealth, it is this percentage of their total wealth.
	percentLoss = 0.17
)

// Agents are defined by the amount of money, or wealth, they have.
type agents []float64

// pickTwoRandomAgents generates two random numbers `sender` and `receiver` between 0 and numOfAgents-1
// and ensures that `sender` and `receiver` are not equal. (After all, agents would not trade with themselves.)
// Note the use of named return values that saves an extra declaration of `receiver` outside the loop
// (to avoid that `receiver` exists only in the scope of the loop).
func pickTwoRandomAgents() (sender, receiver int) {
	sender = rand.Intn(numOfAgents)
	receiver = sender

	// Generate a random`receiver`. Repeat until `receiver` != `sender`
	for receiver == sender {
		receiver = rand.Intn(numOfAgents)
	}
	return sender, receiver
}

// The trading formula assumes that agents sometimes pay either more or less than the traded good is worth.
// Because of this, wealth flows from one agent to another.
// As both agents `sender`, `receiver` were already chosen randomly, we can decide at this point that agent `sender` always loses
// wealth, and agent `receiver` always gains wealth in this transaction.
// Note: the agents
func trade(a agents, sender, receiver int) {
	// Wealth flows from sender to `receiver` in this transaction.
	// The amount that flows from sender to `receiver` is always a given percentage of the poorer agent.

	// If`receiver` is the poorer agent, the gain is `percentGain` of `receiver`'s total wealth.
	transfer := a[receiver] * percentGain

	// If `sender` is the poorer agent, the loss is `percentLoss` of `sender`'s total wealth.
	if a[sender] < a[receiver] {
		transfer = a[sender] * percentLoss
	}
	// It's a deal!
	a[sender] -= transfer
	a[receiver] += transfer
}

// Draw a bar chart of the current wealth of all agents
func drawChart(a agents, bc *widgets.BarChart) {
	bc.Data = a
	// Scale the bar chart dynamically, to better see
	// the distribution when the current maximum wealth is
	// much smaller than the maximum possible wealth.
	maxPossibleWealth := initialWealth * numOfAgents
	currentMaxWealth, _ := ui.GetMaxFloat64FromSlice(a)
	bc.MaxVal = currentMaxWealth + (maxPossibleWealth-currentMaxWealth)*0.05
	ui.Render(bc)
}

// Run the simulation
func run(a agents, bc *widgets.BarChart, done <-chan struct{}) {
	for n := 0; n < rounds; n++ {
		// Pick two different agents.
		sender, receiver := pickTwoRandomAgents()
		// Have them do a trade.
		trade(a, sender, receiver)
		// Update the chart
		drawChart(a, bc)
		// Try to read a value from channel `done`.
		// The read shall not block, hence it is enclosed in a
		// select block with a default clause.
		select {
		case <-done:
			// At this point, the done channel has unblocked and emitted a zero value. Leave the simulation loop.
			return
		default:
		}
	}
}

func main() {
	// Setup

	// Pre-allocate the slice, to avoid allocations during the simulation
	a := make(agents, numOfAgents)

	// Set a random seed
	rand.Seed(time.Now().UnixNano())

	for i := range a {
		// All agents start with the same amount of money.
		a[i] = initialWealth
	}

	// UI setup. `gizak/termui` makes rendering a bar chart in a terminal super easy.
	err := ui.Init()
	if err != nil {
		log.Fatalln(err)
	}
	defer ui.Close()
	bc := widgets.NewBarChart()
	bc.Title = "Agents' Wealth"
	bc.BarWidth = 5
	bc.SetRect(5, 5, 10+(bc.BarWidth+1)*numOfAgents, 25)
	bc.LabelStyles = []ui.Style{ui.NewStyle(ui.ColorBlue)}
	bc.NumStyles = []ui.Style{ui.NewStyle(ui.ColorBlack)}
	bc.NumFormatter = func(n float64) string {
		return fmt.Sprintf("%3.1f", n)
	}
	// Start rendering.
	ui.Render(bc)

	// `termui` has its own event polling.
	// We use this here to watch for a key press
	// to end the simulation
	done := make(chan struct{})
	go func(done chan<- struct{}) {
		for e := range ui.PollEvents() {
			if e.Type == ui.KeyboardEvent {
				// Unblock the channel by closing it
				// After closing the channel, it emits zero values upon reading.
				close(done)
				return
			}
		}
	}(done)

	// Start the simulation!
	run(a, bc, done)

	// After the simulation, wait for a key press
	// so that the final chart remains visible.
	<-done
}
```

#### نحوه دریافت و اجرای کد

```sh
go install github.com/appliedgo/rich@latest
```

این دستور پروژه را در قسمت کش ماژول دانلود می کند، آن را کامپایل می کند و یک باینری به نام rich را در

```txt
$(go env GOBIN)
```

یا

```txt
$(go env GOPATH)/bin
```

اگر GOBIN تنظیم نشده است قرار می دهد. می توانید با فراخوانی rich در اعلان شل (shell)، باینری را اجرا کنید.

با این حال، بازی با کد سرگرم کننده تر است. برای انجام این کار، پروژه را روی دیسک خود کلون (clone) کنید و آن را به صورت محلی اجرا کنید:

سپس در سورس کد منبع قرار گیرید (با دستور cd)، وابستگی ها را دریافت کنید، کد را به دلخواه تغییر دهید و آن را اجرا کنید:

```sh
cd rich 
go run rich.go
```

(اگر خطاهایی در مورد عدم وابستگی دریافت کردید، مطمئن شوید که محیط Go شما در حالت Go Modules است و go mod tidy یا go mod download را اجرا کنید.)

پارامترهایی مانند درصد برد یا باخت، تعداد نمایندگان یا ثروت اولیه را تغییر دهید و ببینید نتایج چگونه تغییر می کند.

به عنوان یک چالش اضافی، نمودار میله ای را تغییر دهید تا توزیع ثروت را در سطل به جای نمایندگان فردی نشان دهد. سپس شبیه سازی را با مثلاً 1000 عامل در 100000 دور اجرا کنید.

توجه داشته باشید که به دلیل الزامات بسته termui، کد در Go Playground اجرا نمی شود.

#### درس های آموخته شده

{{< image src="/images/post_47_go/7ebdqalifxps.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

#### برای سیاستمداران، اقتصاددانان و همه کسانی که با فقر مبارزه می کنند:

با چند خط کد، نشان داده‌ایم که ثروت همیشه از فقرا به ثروتمندان سرازیر می‌شود، مهم نیست که همه مهارت‌های معاملاتی یکسانی داشته باشند. به نظر می رسد برخی افراد خوش شانس هستند و در ابتدا مقداری ثروت به دست می آورند، که پس از آن جمع آوری ثروت بیشتر را آسان تر می کند. مطمئنا، دنیای واقعی بسیار پیچیده تر و متنوع تر است. اما این مدل ساده است، اما نشان می دهد که نابرابری شدید ثروت نمی تواند ناشی از چیزی جز مکانیسم های اساسی بازار آزاد باشد، حتی در غیاب بازیگران حریص و بد اندیش.

#### برای Gopher ها:

یک حلقه کوچک و یک بسته تجسمی تمام چیزی است که برای نوشتن شبیه سازی نیاز دارید. دفعه بعد که با یک مدل تکراری واقعیت مواجه شدید، یک بسته UI/graph/plot بگیرید، یک حلقه بنویسید و فرضیه پشت مدل را تأیید کنید. **Happy coding!**

**[منبع](https://appliedgo.net/rich/)**