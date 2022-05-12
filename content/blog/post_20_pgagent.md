---
title: آموزش نحوه نصب و کار با PgAgent
image: images/post_20_pgagent/postgresql-logo.png
description: در واقع PgAgent یک سرویس زمانبندی task/job است که اولا قابل نصب روی سیستم عامل های هم لینوکس هم مک و هم ویندوز می باشد و هم دو مدل task را هندل و بر نحوه اجرای اونا نظارت می کند که عبارتند از :تسک های sql ی تسک های قابل اجرا در shell سیستم عامل این سرویس در واقع یک سرویس زمانبند task مختص دیتابیس Postgresql است ولی چون قابلیت اجرای فایل های bat / sh دارد بنابراین می شود گفت یک سرویس زمانبند task همه منظوره است .
date: 2018-09-13T18:23:33+04:30
author: solmaz_oskouie
tags:
- server
categories:
- نرم افزار
---

## **فهرست :**

#### **معرفی PgAgent**

#### **نصب PgAgent**

#### **نوشتن job/task**

چند وقت پیش بخاطر کار روی پروژه ای مجبور شدم برای انجام کارهای زمانبندی شده و دوره ای (در سطح دیتابیس) سراغ یک job scheduling agent بگردم و چون نیازم در حوزه دیتابیس بود دنبال ابزاری توی این بخش می گشتم. نوع دیتا بیس پروژه ای که رویش کار می کردم Postgresql بود و بالطبع دنبال افزونه ای بودم که متعلق به این خانواده باشد و در نهایت بین PgAgent و pg_cron من PgAgent را انتخاب کردم . هرچند چیزی که باعث تعجبم شد این بود که انگار تعداد کمی از اهالی فن از این ابزار استفاده می کنند! (تعداد مستندات و صفحات رفع اشکال کم و محدودی برای این ابزار پیدا کردم ). شاید ابزارهایی که خود سیستم عامل ها ارایه می دهند بهتر و کاراتر از این ابزار انتخابی من باشد. به هر حال من فعلا PgAgent را برای نیازم انتخاب کردم و از دوستانی که تجربه بیشتری دارند خواهش می کنم اونارو با من هم در میون بگذارند .

### **معرفی :**

در واقع PgAgent یک سرویس زمانبندی task/job است که اولا قابل نصب روی سیستم عامل های هم لینوکس هم مک و هم ویندوز می باشد و هم دو مدل task را هندل و بر نحوه اجرای اونا نظارت می کند که عبارتند از :

**تسک های sql ی**

**تسک های قابل اجرا در shell سیستم عامل**

این سرویس در واقع یک سرویس زمانبند task مختص دیتابیس Postgresql است ولی چون قابلیت اجرای فایل های bat / sh دارد بنابراین می شود گفت یک سرویس زمانبند task همه منظوره است **.**

### **نصب PgAgent :**

اگر پلتفرم شما ویندوزیست نصب PgAgent مثل آب خوردن است اگر موقع دانلود و نصب postgresql از مواهب StackBuilder بهره برده باشید .

در‌واقع StackBuilder یک ابزار کمکی برای دانلود و نصب پکیج ها و add-on های دیتابیس postgresql است .

اگر postgresql را روی ویندوز خود نصب شده دارید لطفاً این کلیدواژه ها رو سرچ کنید :

**application stackBuilder**

**stack builder Plus**

اگر StackBuilder را نصب شده داشته باشید بایدکادری مانند شکل زیر براتون باز بشود :

{{< image src="images/post_20_pgagent/v9n7znvama4s.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

با انتخاب pgAgent از بخش add-ons ها می توانید آن را به راحتی نصب کنید ( شکل زیر )

{{< image src="images/post_20_pgagent/up3jhk4ibtj1.gif" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

به این راحتی pgAgent براتون نصب و البته راه اندازی میشه و به صورت یک سرویس در محیط سیستم عامل ویندوزیتون راه اندازی میشه ( می تونید به بخش لیست سرویس ها بروید و وضعیت اجرایی اونو چک کنید تا مطئن بشید واقعا در حال running است یا نه ).

اما اگه پلتفرمتون لینوکسی و مکی باشه باید بی خیال StackBuilder بشیم و باید سراغ[ این صفحه وب](https://wiki.postgresql.org/wiki/Apt) رفته و طبق راهنمایی های اون اقدام به نصب pgAgent روی پلتفرم لینوکسی (دبیان )کنیم .

در‌واقع postgresql تمامی پکیج ها و add-on های دیبان و ابونتو خود را در داخل repo زیر قرار می دهد :

https://apt.postgresql.org/pub/repos/apt

روشی که در ادامه می‌بینید در‌واقع مراحل نصب خود دیتابیس Postgresql و ابزار گرافیکی مدیریت داده های آن یعنی Pgadmin4 روی پلتفرم لینوکس ( اوبونتو ) را نشان می دهد. ( اگر شما قبلاً دیتابیس را نصب کردید فقط کافیست pgadmin4 را نصب کنید ).

مراحل کار به صورت زیر است :

 ابتدا باید کلید repository را وارد کنید ( دستور زیر ) :

```sh
$ sudo apt-get install wget ca-certificates
$ wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc|sudo apt-key add -
```

 در مرحله دوم ،یک فایل در مسیر زیر ایجاد می‌کنیم :

```sh
$ sudo vim /etc/apt/sources.list.d/pgdg.list
```

محتوای فایل باید به صورت زیر باشد **:**

```sh
$ deb http://apt.postgresql.org/pub/repos/apt/ stretch-pgdg main
```

باید بجای واژه **stretch** نام به اصطلاح **codename** مر بوط به **distribution** سیستم عامل لینوکسی خود را بگذارید برای اینکه بدانید نام **codename** شما چی هست می‌توانید دستور زیر را در ترمینال لینوکسی **(**دبیانی**)** خود بزنید **:**

```sh
$ lsb_release -c
```


یا برای راحتی کلاً خط زیر را در ترمینال بنویسید تا هم فایل را ایجاد و هم محتوای آن را پر کند **:**

```sh
$ sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" &gt; /etc/apt/sources.list.d/pgdg.list'
```

مثلاً برای سیستم لینوکسی من **(ubuntu 16.04 )** محتوا این فایل به صورت زیر است **:**

{{< image src="images/post_20_pgagent/odwhgmtrdwkq.gif" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

 مرحله سوم باید دستورات زیر را اجرا کنید **:**

```sh
$ sudo apt-get update</textarea>
$ sudo apt-get upgrade
$ sudo apt-get install postgresql-10 pgadmin4
```

همون طوری که گفتم اگر قبلاً دیتابیس **postgres** را نصب کردید و ورژن آن برابر یا بالاتر از ۹**.**۱ است کافیست دستور زیر را اجرا کنید:

```sh
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get install pgadmin4
```

مثلاً نسخه سیستم من 9.6 است و تنها pgadmin4 را نصب کردم اگر نسخه نصب شده در سیستم شما ۹.۱ و پایین‌تر است بهتر است pgadmin3 را نصب کنید .

نکته دیگری هم که لازم به ذکر است این است اگر pgadmin3 را قبلاً نصب شده در سیستم خود دارید بهتر است آن را پاک کنید.

جهت پاک کردن فقط خود pgadmin3 :

```sh
$ sudo apt-get remove pgadmin3
```

جهت پاک کردن pgadmin3 به همراه dependency هایش :

```sh
$ sudo apt-get remove --auto-remove pgadmin3
```

بانصبpgadmin4 & نصب pgagent هم انجام می‌شود ( از کجا مطمئن بشیم ؟ بروید به مسیر زیر یا دستور whereis pgagent را در ترمینال اجرا کنید ).

خروجی حاصل از اجرای دستور whereis pgagent در سیستم من به صورت زیر است :

```sh
$ pgagent: /usr/bin/pgagent   /usr/share/man/man1/pgagent.1.gz
```

در‌واقع با نصب بسته نرم افزاری pgadmin4 خود pgagent هم به صورت دستوری در مسیر /usr/bin ایجاد می‌شود .

با اجرای دستوری با فرمت زیر:

```sh
$ /path/to/pgagent [options] <connect-string>
```

در‌واقع یک daemon [( پروسس هایی که برایشان معادل فایلی در سیستم لینوکس ایجاد نمی‌شود تحت کنترل مستقیم کاربر نیست ولی در پشت صحنه در حال اجرا و انجام وظایف خود است ).](https://askubuntu.com/questions/192058/what-is-technical-difference-between-daemon-service-and-process)

ایجاد می‌شود که بهش گفته می‌شود به دیتابیسی در روی فلان ماشین وصل شو و task/job که برایت تعیین شده و مشخصاتش آنجا تعریف شده ( معمولاً هم به صورت یک فایل اسکریپتی تحویل pgagent داده می‌شود ) سرزمانها و تاریخ های مشخص اجرا کن .

فعلاً کار ما اینجا تمام شد برویم سراغ pgadmin4 و برای پیکربندی pgagent لازم است کارهای زیرا به ترتیب انجام می‌دهیم :


 روی دیتابیسی که قصد ایجادjob /task برایش دارید کلیک کنید از منوی tools گزینه Query Tool رو انتخاب کنید و دستورات زیر را در محیط query Tool اجرا کنید:

```sql
CREATE EXTENSION pgagent;
```

با اجرای این دستور اتفاق زیر رخ می‌دهد ( در محیط pgadmin4 روی دیتابیسی که کلیک کردید به شاخه Extensions بروید می‌بینید که دو زیر شاخه بهش اضافه شده ( در نسخه های postgresql بالاتر از ۹.۱ زیر شاخه plpgsql از قبل وجود دارد ولی زیر شاخه pgagent بهش با اجرای دستور بالایی اضافه می‌شود ) شکل زیر :

{{< image src="images/post_20_pgagent/cwa8mavq4d7o.gif" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

در‌واقع plpgsql یک زبان procedure است که باید نصب بشود ( در نسخه های پایین‌تر از ۹.۱ این زبان نصب نیست و باید با دستور زیر اونو نصب کرد)

```sql
CREATE LANGUAGE plpgsql;
```

با اینکه ما pgagent را نصب کردیم با اینکه سمت pgadmin4 تنظیمات مربوطه را انجام دادیم ولی هنوز pgagent ها به عنوان یک daemon فعالیتش را شروع نکرده!

به سراغ این فرمت دستوری که پیشتر نوشتیم می‌رویم یعنی این فرمت دستور :

```sh
$ /path/to/pgagent [options] <connect-string>
```

با اجرای دستوری با فرمت بالایی باید pgagent را بالا آورده و به دیتا بیس خودوصلش کنیم تا job/task مورد نظر را اجرا کند یک نمونه از دستورات می‌تواند به صورت زیر باشد:

```sh
$ pgagent -l hostaddr=127.0.0.1 dbname=test2233 user=postgres password=1
```

این دستور را باید در مسیر pgagent اجرا کنیم مثلاً در سیستم ابنتو ۱۶.۰۴ من این در مسیر /usr/bin است

ممکن است با اجرای این دستور با چنین خطایی مواجه شوید :

```txt
Tue Sep 11 15:45:44 2018 WARNING: Couldn't create the primary connection [Attempt \#1]
```

جوابی که برای این خطا من پیدا کردم و کمکم کرد آن را بر طرف کنم در لینک زیر آمده :

[https://www.postgresql.org/message-id/20160510090828.GA7473%40msg.df7cb.de](https://www.postgresql.org/message-id/20160510090828.GA7473%40msg.df7cb.de)

با تغییر user ام به postgres این خطا برطرف شد

یعنی با :

```sh
$ sudo -i -u postgresq
```
خطای دیگری که ممکن است شما هم مثل من بهش برخورد کنید می‌توانید چنین پیام خطایی در خروجی باشد :

```txt
ERROR: Unsupported schema version: 3. Version 4 is required - please run pgagent_upgrade.sql.
```

این خطا رو من به این صورت حل کردم ( تا جایی که توی وب گشتم راه حل مستقیم و راحتی برایش ارایه نشده بود اگر شما جواب سرراست و شفاف و کاملی برایش داشتید خوشحال میشم منم اونو بدونم ) .

من رفتم روی نود Extensions روی شاخه اصلی pgadmin4 و روی pgagent راست کلیک کردم در بخش Definition ورژن رو از ۳ به 4 تغییر دادم

شکل زیر :

{{< image src="images/post_20_pgagent/bifrl5femcjg.gif" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}


{{< image src="images/post_20_pgagent/ry04v6wo8c9z.gif" caption="تغییر ورژن از ۳ به ۴ برای pgadmin در قسمت Extesions از محیط pgadmin4" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}


* ### **نحوه ایجاد یک job در محیط pgadmin4 جهت استفاده توسط pgAgent**

درست کردن یک job/task از طریق pgadmin4 خیلی ساده است روی درخت pgadmin گره ای بنام jobs پیدا کنید

اگر اون رو پیدا نمی‌کنید بدین صورت عمل کنید در ریشه راست کلیک کرده ویک سرور جدید تعریف و ایجاد کنید اصطلاحاً بهش maintain server ‌گفته می‌شود ( زیاد مطمئن نیستم دقیقاً اسمش این باشد ) با اینکار می‌توانید این گره jobs رو روی سرور جدید پیدا کنید .

روی اون گره jobs راست کلیک کرده تا job جدیدی تعریف کنیم

مراحل کار و تنظیمات اولیه با فرض اینکه pgagent و postgresql روی یک ماشین هستند به صورت زیر است :

{{< image src="images/post_20_pgagent/9w5y5whih4jk.gif" caption="ایجاد / تعریف یک job جدید از طریق محیط pgadmin4 با راست کلیک کردن روی گره jobs" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

{{< image src="images/post_20_pgagent/wlexnf5oyuil.gif" caption="انتخاب یک نام برای job مورد نظر و نوع job " command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

{{< image src="images/post_20_pgagent/obwowxxqcidr.gif" caption="تعریف خود job در سربرگ step و مراحل کار و تعریف connection String و نیز در سربرگ code نوشتن کدهای job" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}


درشکل بالایی اگر نوع job/task ما از نوع sql ای باشد( در همین سربرگ مشخص می‌کنیم ) باید در قسمت سربرگ code ( به شکل بالایی توجه کنید ) اون کد sql ی خود را اینجا قرار دهیم ولی اگر از نوع یک batch file باشد باید مسیر اون فایل اسکریپتی را بهش بدهیم ( در مقاله بعدی مراحل ایجاد یک job/ task ی که عمل backup گیری را انجام میدهد آموزش می‌دهیم ) .

بعد مشخص کردن نوع task و نوشتن کد به سراغ تعریف زمانبند می‌رویم تا به pgagent بگوییم در چه بازه هایی از زمان / تاریخ باید job ما را اجرا و تکرار کند .

{{< image src="images/post_20_pgagent/re7e559r4xon.gif" caption="انتخاب تاریخ و زمان شروع و پایان job در سربرگ schedules" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

{{< image src="images/post_20_pgagent/pyijpag7tklf.gif" caption="مشخص کردن زمانهای تکرار در نقطه های زمانی ماه- هفته- روز- ساعت- دقیقه" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}


وقتی تمامی مراحل را طی کردیم روی دکمه save زده تا تنظیمات برای job موردنظر ذخیره و اعمال شود .

**_نکته_ : تمامی مراحل گفته شده برای ایجاد job جدید روی هر دو پلتفرم ویندوز و لینوکس مشابه هم هستند.**

برای اینکه مطئن بشویم این job ما سر زمانهای تعیین شده اجرا می‌شود روی job راست کلیک کرده و گزینه run now را می‌زنیم اگر همه چیز اوکی باشد باید وقتی روی زیر شاخه step کلیک بکنیم در پنل سمت راستی گزارش و نتیجه اجرای job را بتوانیم ببینیم ( در قسمت سربرگ statistics ) :

{{< image src="images/post_20_pgagent/0bdvpkwe09cq.gif" caption="گزارش نتایج اجرای job تعریف شده " command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

{{< image src="images/post_20_pgagent/xwksvnsihtsi.gif" caption="با کلیک روی زیر شاخه steps برروی گره jobs در محیط pgadmin4 می توانیم نتایج اجرای کد منتسب شده به job را ببینیم." command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

#### **منابع :**

[http://installion.co.uk/ubuntu/xenial/universe/p/pgadmin3/uninstall/index.html](http://installion.co.uk/ubuntu/xenial/universe/p/pgadmin3/uninstall/index.html)

https://www.pgadmin.org/docs/pgadmin3/1.22/pgagent-install.html

https://www.pgadmin.org/docs/pgadmin4/dev/pgagent_install.html

https://www.postgresql.org/message-id/20160510090828.GA7473%40msg.df7cb.de