---
title: پیاده سازی پایپ لاین CI/CD برای برنامه های Node.js با Jenkins
image: images/virgoolPosts/upanvkgiwbb9.png
description: آموزش ساده اتوماتیک سازی تست و Deployment برنامه های Node.js
date: 2021-12-03
author: mam_niki
tags:
- nodejs
- server
categories:
- نرم افزار
- برنامه نویسی
---

**تعریف پایپ لاین در مهندسی نرم افزار:** به توالی خطی ای از ماژول های مشخص می گویند که در جهتی مشخص در حال حرکت هستند- [منبع](https://virgool.io/apieco/%D9%85%D9%81%D8%A7%D9%87%DB%8C%D9%85-cicd-%D9%88-pipeline-%D8%A7%D8%B2-%D8%A7%D8%A8%D8%AA%D8%AF%D8%A7-%D8%AA%D8%A7-%D9%BE%DB%8C%D8%B4%D8%B1%D9%81%D8%AA%D9%87-gq0uabguiu7n)

#### تعریف CI/CD:

**CI: Continuous Integration**

**CD: Continuous Deployment/Delivery**

ادغام مستمر (**Continuous Integration**) و پیاده سازی مستمر یا تحویل مستمر (**Continuous Deployment**) دو روش مدرن توسعه نرم افزار هستند.

ادغام مستمر (**CI**) فرآیندی است که هر بار که یکی از اعضای تیم تغییراتی را در کنترل نسخه ای کد (**Version Control**) انجام می دهد، سپس فرآیند ساخت (**Build**) و آزمایش (**Test**) کد به طور خودکار انجام می شود.

تحویل مستمر (**CD**) را می توان به عنوان توسعه یکپارچه سازی مداوم در نظر گرفت و فرآیندی است که به طور خودکار یک برنامه کاربردی را پس از موفقیت آمیز بودن CI به کار می گیرد.

هدف مینیمم کردن مدیریت زمان است. بازه زمانی ای که بین توسعه و نوشتن یک خط کد جدید و استفاده از این کد توسط کاربران فعال در نرم افزار تولید شده سپری میشود(فرآیند بروز رسانی نرم افزار تولید شده).

#### چرا CI/CD:

مزایای زیادی برای شیوه های CI/CD وجود دارد.من قصد ندارم در مورد هر مزیتی با جزئیات صحبت کنم، اما می‌خواهم چند مورد از مهمترین آنها را در اینجا بیان کنم:

**مزایای ادغام مستمر (CI):**

* باگ کمتر
* تغییرات زمینه ای (context switching) کمتر ، به عنوان توسعه دهندگان به محض شکسته شدن روند ساخت (Build) ، هشدار داده می شود
* هزینه تست نرم افزار و کد کاهش میابد
* تیم تضمین کیفیت QA یا (**Quality Assurance**) شما زمان کمتری را صرف آزمایش و امتحان می کند 

**مزایای تحویل مستمر (CD):**

* _انتشار نسخه ها ریسک کمتری دارند_
* _فرآیند انتشار ساده تر میشود_
* _مشتریان یک جریان مداوم از پیشرفت ها را مشاهده می کنند_
* _توسعه را تسریع می کند زیرا نیازی به توقف توسعه برای انتشار نیست_

#### _در این آموزش ما چه چیزی خواهیم ساخت:_

در این آموزش ما یک برنامه ساده Node.js ای خواهیم ساخت که روی سرورهای دیجیتال اوشن میزبانی می شود.

علاوه بر این ما قصد داریم یک سرور اتوماتیک سازی به نام جنکینز (**Jenkins**) را پیکربندی کنیم و در یک سرور جداگانه از دیجیتال اوشن میزبانی کنیم.

این Jenkins به ما کمک می کند که فرآیندهای CI/CD اتوماتیک شود.در هر بار تغییر کدهای موجود در گیت ریپازیتوری برای برنامه Node.js ای ما ، Jenkins مطلع می شود و تغییرات را در سرور Jenkins (**مرحله 1**) دریافت می کند ، وابستگی ها را نصب می کند (**مرحله 2**) و تستِ ادغام (**مرحله 3**) را اجرا می کند.

اگر همه تست ها درست انجام شود، جنکینز برنامه را در سرور Node.js ای ما مستقر می کند (**مرحله 4**). در صورت عدم موفقیت، به توسعه دهنده اطلاع داده می شود.

{{< image src="/images/virgoolPosts/upanvkgiwbb9.png" caption="شمای کلی روش انجام (شکل 1)" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

#### _ساخت یک برنامه Node.js:_

قبل از نوشتن پایپ لاین های CI/CD ، ما اول نیاز به یک برنامه داریم تا قابل تست کردن و پیاده سازی باشد.

ما قصد داریم یک برنامه ساده تحت وب Node.js ای بسازیم که متن "hello world" را به عنوال ریسپانس برگرداند.

اول بیایید تا باهم مراحل ساخت یک ریپازیتوری گیت هابی را برای این پروژه انجام دهیم.

#### _ساخت ریپازیتوری گیت هاب:_

* ابتدا یک ریپازیتوری جدید در حساب کاربری GitHub خود ایجاد کنید و نام آن را **node-app** بگذارید.
* _شما می توانید نوع ریپوی خود را **Public** یا **Private** انتخاب کنید_
* _در قسمت چک باکس تیک گزینه_ **Initialize this repository with a README** را بزنید
* در منوی کشویی در قسمت **Add .gitignore** گزینه **Node** را انتخاب کنید
* بر روی دکمه **Create repository** کلیک کنید

{{< image src="/images/virgoolPosts/sfty8grcxuiu.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

اکنون برنامه node-app مارا از ریپوی زیر به کامپیوتر خود کُلن کنید و به پوشه آن بروید.

```sh
git clone git@github.com:<github username>/node-app.git
cd node-app
```

#### برنامه Node.js موجود:

اولین گام هنگام ساختن یک برنامه Node.js ای ، ایجاد فایل **package.json** است. در این فایل وابستگی های اپلیکیشن را لیست می کنیم. یک فایل جدید در ریشه پروژه خود به نام **package.json** ایجاد کنید و محتوای زیر را در آن کپی کنید:

```js
{
     “name”: “node-app”,
     “description”: “hello jenkins test app”,
     “version”: “0.0.1”,
     “private”: true,
     “dependencies”: {
           “express”: “3.12.0”
      },
      “devDependencies”: {
             “mocha”: “1.20.1”,
             “supertest”: “0.13.0”
      }
}
```
نیازمندی های موجود در این برنامه عبارتند از:

* فریم ورک **Express**
* فریم ورک **Mocha**  جهت تست برنامه های نود ****(شما می توانید هر کدام از فریم ورک های تست دیگر نظیر Jasmin , Jest , Tape و... را استفاده کنید)
* ماژول **Supertest** که یک انتزاع سطح بالا برای تست برنامه های مبتنی بر **HTTP** ارائه می دهد

پس از اینکه وابستگی های خود را در فایل package.json تعریف کردیم، آماده نصب آنها هستیم:

```sh
$ npm install
```

بسیار عالی! برای نوشتن کد آماده هستید؟ یک فایل جدید در ریشه پروژه به نام index.js ایجاد کنید و کد زیر را کپی کنید:

```js
//importing node framework
const express = require(‘express’);

const app = express();

//Respond with "hello world" for requests that hit our root "/"
app.get(‘/’, function (req, res) {
     res.send(‘hello world’);
    });

//listen to port 3000 by default
app.listen(process.env.PORT || 3000);

module.exports = app;
```

هنگامی که درخواست‌هایی که به URL ریشه ما (“/“) در مرورگر وارد می شود، برنامه ما با "hello world" پاسخ می‌دهد.

و این شد برنامه ما.در زیر ساختار پوشه آن را می بینید.

{{< image src="/images/virgoolPosts/yblbl5d2gcbc.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

اکنون ما آماده ایم تا برنامه خود را اجرا کنیم.

```sh
$ node index.js
```

شما می توانید برنامه خود را در مرورگر مشاهده کنید و به آدرس زیر بروید:

_http://localhost:3000_

#### _نوشتن تست برای برنامه:_

_ما آماده ایم تا اولین تست خود را بنویسیم.تست ما به آدرس ریشه سایت ("/") می رود و تأیید می کند که صفحه با متن "_hello world_" پاسخ می دهد یا خیر._

_یک پوشه جدید به اسم **test** ایجاد کنید و سپس در داخل آن فایل **test.js** را ایجاد کنید.کدهای زیر را در این فایل کپی کنید._

```js
const request = require(‘supertest’);
const app = require(‘../index.js’);

describe(‘GET /’, function() {
     it(‘respond with hello world’, function(done) { 

     //navigate to root and check the the response is "hello world"
      request(app).get(‘/’).expect(‘hello world’, done);
      });
});
```
ما قصد داریم که از **Mocha** برای اجرای تست های خود استفاده کنیم.ما Mocha را به عنوان بخشی از devDependencies خود در فایل package.json نصب کردیم.برای اجرای تست باید فایل test/test.js/ خود را به عنوان آرگومان Mocha ارسال کنیم.

```sh
$ ./node_modules/.bin/mocha ./test/test.js
```

اگر این دستور را از ریشه پروژه خود اجرا کنید، اجرای تست و قبولی را خواهید دید.

{{< image src="/images/virgoolPosts/lrk97tppvxqu.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

با نگاهی به **شکل 1** مرحله **شماره 3**، ما می خواهیم Jenkins تست ادغام ما را پس از ساخت (Build) آن اجرا کند. برای رسیدن به آن، باید یک شل اسکریپت (shell script) در پروژه خود ایجاد کنیم که آزمایش ما را آغاز کند.

یک پوشه جدید با نام **script** ایجاد کنید و در آن یک فایل با نام **test (بدون فرمت و پسوند)** ایجاد کرده و سپس کدهای زیر را در آن کپی کنید.

```sh
#!/bin/sh
./node_modules/.bin/mocha ./test/test.js
```

جهت اعطای مجوزهای اجرایی به این فایل دستور زیر را اجرا کنید:

```sh
$ chmod +x script/test
```

و آن را با اجرای این شل اسکریپت از ریشه پروژه تست کنید:

```sh
$ ./script/test
```

بوم! تست ادغام آماده است و اکنون ما آماده هستیم تا کد خود را به GitHub پوش (push) کنیم:

```sh
$ git add .
$ git commit -m ‘simple node app with test’
$ git push origin master
```

#### پیاده سازی سرور نود در دیجیتال اوشن:

ما قصد داریم برنامه نود خود را روی یک سرور میزبانی کنیم تا تمام جهان بتوانند شاهکار ما را ببینند. ما از DigitalOcean به عنوان ارائه دهنده میزبانی خود استفاده خواهیم کرد. DigitalOcean یک راه آسان برای پیکربندی سرورها و اجرای نمونه های جدید ارائه می دهد.

**_ساخت یک دراپلت (Droplet) نود :_**

ابتدا ثبت نام کنید و به حساب DigitalOcean خود وارد شوید.

* روی دکمه **Create new droplet** کلیک کنید.
* **انتخاب یک ایمیج:** روی تب **one click app** کلیک کنید و **node JS** را از لیست انتخاب کنید
* **اندازه مقدار رم را انتخاب کنید:** 1 گیگابایت (ارزان ترین)
* **انتخاب منطقه دیتاسنتر:** نزدیکترین منطقه به خود را انتخاب کنید. من منطقه 3 نیویورک را انتخاب می کنم
* **کلیدهای SSH خود را اضافه کنید:** کلید SSH دستگاه محلی خود را اضافه کنید. اگر کلید SSH ندارید [این ](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)را دنبال کنید تا یکی بسازید. این دستور کلید عمومی SSH شما را کپی کرده و در قسمت متن قرار می دهد

```sh
$ pbcopy < ~/.ssh/id_rsa.pub
```

* نام میزبان را انتخاب کنید: نام آن را "nodejs-app" بگذارید
* روی دکمه ایجاد کلیک کنید

{{< image src="/images/virgoolPosts/wuw0vnjo8xnq.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

## تنضیمات سرور **Nodejs-app:**

بیایید کلاه DevOps را روی سر بگذاریم و سرور نود خود را راه اندازی کنیم

ترمینال را در کامپیوتر خود خود باز کنید و به عنوان کاربر اصلی (root) وارد سرور nodejs-app خود شوید:

```sh
$ ssh root@NODE.SERVER.IP
```

اکنون شما به عنوان کاربر root که یک کاربر فوق العاده قدرتمند است، وارد شده اید. و "**با قدرت بزرگ، مسئولیت های بزرگی به وجود می آید**".

{{< image src="/images/virgoolPosts/komyqr1qpxsu.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

از آنجایی که ما مسئولیت‌ها را دوست نداریم، بیایید یک کاربر جدید برای انجام تنظیمات سرور ایجاد کنیم و آن را با نام خانوادگی (last name) شما نامگذاری کنیم:

```sh
$ adduser <lastname>
```

رمز عبور کاربر را وارد کنید و دستورات را دنبال کنید. قبل از اینکه به اکانت جدید خود سوییچ کنیم، باید به او امتیازات sudo بدهیم:

```sh
$ usermod -a -G sudo <username>
```

اکنون می توانید به اکانت جدید خود سوییچ کنید:

```sh
$ su — username
```

#### پیاده سازی برنامه نود در سرور:

سرور DigitalOcean ما با Node ارائه می شود اما Git روی آن نصب نیست. اجازه می دهیم git را با استفاده از app-get نصب کنیم:

```sh
$ sudo apt-get install git
```

برنامه نود خود را از ریپو به سرور کُلن کنید:

```sh
$ git clone https://github.com/<username>/node-app.git
```

به پوشه آن بروید و نیازمندی ها را با فرمان زیر نصب کنید:

```sh
$ cd node-app
$ npm install — production
```

قبل از اینکه بتوانیم به برنامه خود در مرورگر دسترسی پیدا کنیم، باید یک مرحله اضافی را تکمیل کنیم. همانطور که به یاد دارید ما برنامه خود را به طور پیش فرض روی پورت 3000 اجرا می کنیم. فایروال DigitalOcean دسترسی مشتریان به هر پورت به جز 80 پورت را مسدود می کند. خوشبختانه اوبونتو دارای ابزار پیکربندی فایروال **UFW** است که با دستور زیر قوانین (rule) فایروال را برای رفع انسداد پورت های خاص اضافه می کند.

```sh
$ sudo ufw allow 3000
$ node index.js
```

اکنون می توانید با اضافه کردن PORT به آدرس IP سرور به برنامه نود خود دسترسی پیدا کنید:

http://NODE.SERVER.IP:3000

#### اجرای همیشگی برنامه نود: 

شروع برنامه نود با دستور بالا برای اهداف توسعه خوب است اما برای پروداکشن خیر. در صورت خرابی نمونه نود ما به فرآیند و برنامه ای نیاز داریم که راه اندازی مجدد خودکار را انجام دهد. ما قصد داریم از ماژول **PM2** برای کمک به این کار استفاده کنیم. **PM2** یک مدیر فرآیند (**Proccess manager**) برای اهداف کلی و یک **Runtime** برای برنامه‌های Node.js با قابلیت **Load Balancer** داخلی است. بیایید **PM2** را نصب کنیم و نمونه نود خود را راه اندازی کنیم:

```sh
$ sudo npm install pm2@latest -g
$ pm2 start index.js
```

اکنون برنامه نود ما تنظیم و اجرا شده است.  

#### تنظیم سرور Jenkins :

**_ساخت یک دراپلت (Droplet) Jenkins:_**

بیایید با ایجاد دومین دراپلت DigitalOcean شروع کنیم که برنامه جنکینز ما را میزبانی می کند. دستورالعمل های زیر را در بخش **_ساخت یک دراپلت (Droplet) نود_** در بالا دنبال کنید و "**jenkins-app**" را به عنوان نام میزبان خود انتخاب کنید. در نهایت با 2 دراپلت مواجه خواهید شد:

{{< image src="/images/virgoolPosts/pfhc7lwkdkkb.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

_**ساخت کاربر جدید:**_

به عنوان کاربر روت به دراپلت جدید SSH بزنید.یک کاربر جدید بسازید و به آن امتیازات sudo بدهید و به کاربر تازه اضافه شده بروید:

```sh
$ ssh root@JENKINS.SERVER.IP
$ adduser <username>
$ usermod -a -G sudo <username>
$ su — <username>
```

جنکینز باید بتواند تغییرات را از ریپوی برنامه نود دریافت کند، بنابراین، باید git را روی این سرور نیز نصب کنیم:

```sh
$ sudo apt-get install git
```

#### نصب Jenkins:

**_دریافت Jenkins:_**

```sh
//add the repository key to the system
$ wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
//append the Debian package repository address to the server's echo
$ deb https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list
//update
$ sudo apt-get update
```

**_نصب Jenkins:_**

```sh
$ sudo apt-get install jenkins
```

**_اجرای Jenkins:_**

```sh
$ sudo systemctl start jenkins
```

جنکینز روی پورت **8080** اجرا می شود. فایروال را به خاطر دارید؟ بیایید پورت را باز کنیم:

```sh
$ sudo ufw allow 8080
```

و اکنون می توانیم از طریق وب و مرورگر به Jenkins برویم:

http://JENKINS.SERVER.IP:8080

#### تنظیمات Jenkins:

هنگامی که به صفحه اصلی جنکینز می روید، احتمالاً متوجه مرحله دیگری شده اید که باید انجام دهید. شما باید قفل جنکینز را باز کنید

{{< image src="/images/virgoolPosts/vbmxz1w9zlgh.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

با استفاده از مسیر زیر رمز عبور جنکینز اجرا شده در سرور جنکینز خود را کپی کنید

```sh
$ sudo vim /var/lib/jenkins/secrets/initialAdminPassword
```

رمز عبور را در قسمت باکس متن در صفحه وب قرار دهید. شما آماده راه اندازی جنکینز هستید. ابتدا می خواهیم **افزونه GitHub** را اضافه کنیم. از منوی سمت چپ **manage Jenkins** را انتخاب کنید و به **manage plugins** بروید. در صفحه پلاگین ها، تب **available** را انتخاب کنید و به دنبال **GitHub plugin** بگردید، چک باکس آن را انتخاب کنید و روی دکمه **Download now and install after restart** کلیک کنید.

{{< image src="/images/virgoolPosts/do1fy7a3zm2x.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

پس از اتمام نصب، صفحه را به پایین اسکرول کنید و گزینه **Restart Jenkins when installation is complete** را انتخاب کنید. با این کار جنکینز مجددا راه اندازی می شود و نصب پلاگین کامل می شود.

#### تغییر پسورد ادمین Jenkins:

من در این مرحله پیشنهاد می کنم رمز عبور کاربر ادمین جنکینز خود را تغییر دهید. از منوی سمت چپ گزینه **Manage Jenkins** را انتخاب کرده و روی **Manage Users** کلیک کنید. کاربر ادمین را انتخاب کنید و یک رمز عبور جدید انتخاب کنید. در آینده هنگام ورود به Jenkins از رمز عبور جدید استفاده خواهید کرد.

## ساخت **Jenkins Job:**

ما می‌خواهیم جاب Jenkins خود را ایجاد کنیم که مسئول برداشتن تغییرات کد از ریپوی گیت node-app ، نصب وابستگی‌ها، اجرای تست و استقرار برنامه ای است که هر بار که یک توسعه‌دهنده به شاخه اصلی ریپو (**Main Branch**) node-app تغییراتی در کد می‌دهد، جاب را اجرا کنیم.

روی دکمه **New Item** کلیک کنید، نام مورد را **node-app** بگذارید و گزینه **Build a free-style software project** را انتخاب کنید و دکمه **OK** را بزنید.

## تنظیمات **Jenkins Job:**

در قسمت **Source Code Management** دکمه رادیویی **git** را انتخاب کنید و لینک **github** https را به قسمت **Repository URL** وارد کنید:

```sh
https://github.com/<username>/node-app.git
```

در قسمت **Build Triggers** گزینه **GitHub hook trigger for GITScm polling** را انتخاب کنید.این کار جاب جنکینز مارا با هر بار پوش کردن تغییرات در برنچ Main گیت هاب اجرا می کند.

در قسمت **Build** روی دکمه **Add Build Step** کلیک کنید و گزینه **Execute Shell** را انتخاب کنید.

دستورات زیر را درقسمت مربوط به **Command** وارد کنید

```sh
npm install
./script/test
```

در این مرحله از بیلد، ما قصد داریم وابستگی ها را نصب کنیم و سپس شل اسکریپت تست خود را اجرا کنیم.

{{< image src="/images/virgoolPosts/hgcf41s90oxi.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

## اضافه کردن Git Webhook:

ما قصد داریم Git Webhook را اضافه کنیم تا هر بار که توسعه‌دهنده کد جدیدی را به برنچ Main در گیت هاب ارسال می‌کند، به Jenkins اطلاع دهیم.

به ریپوی خود Node-app در گیت هاب بروید، روی تب **Settings** کلیک کنید، **Webhooks** را از منوی سمت چپ انتخاب کنید و روی دکمه **Add Webhooks** کلیک کنید. آدرس webhook Jenkins خود را در **Payload URL** وارد کنید:

http://JENKINS.SERVER.IP:8080/github-webhook/

و گزینه **Just the Push Event** را انتخاب کنید در نهایت دکمه **Add webhook** را کلیک کنید.

{{< image src="/images/virgoolPosts/ivkzt3o9mvoa.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

بیایید آنچه را که تا کنون انجام داده ایم ،امتحان کنیم. به پروژه node-app در کامپیوتر خود بروید و نسخه موجود در **package.json** را به **0.0.2** تغییر دهید. این تغییر را کامیت و به GitHub پوش کنید. پس از پوش کردن ، به جاب (Job) جنکینز خود در مرورگر بروید و مشاهده کنید که جاب جنکینز با موفقیت شروع و تکمیل می شود.

#### پیاده سازی و Deployment:

این قسمت ، آخرین قطعه از پازل پیاده سازی برنامه نود ما در سرور node-app است، زمانی که تست ما اجرا و قبول خواهد شد.

#### احراز هویت SSH:

برای انجام این کار، سرور جنکینز باید بتواند به سرور node-app وارد شود، تغییرات جدید در مخزن را پول (pull) کند، وابستگی ها را نصب کند و سرور را مجددا راه اندازی کند. اجازه دهید ابتدا دسترسی ssh به جنکینز را تنظیم کنیم.

وقتی جنکینز را نصب می کنیم، به طور خودکار یوزر جنکینز را ایجاد می شود. با SSH به سرور جنکینز به عنوان کاربر روت وارد میشویم و مراحل زیر را انجام می دهیم:

```sh
$ ssh root@JENKINS.SERVER.IP
```

سوییچ به کاربر جنکینز:

```sh
$ su — jenkins
```

ایجاد کلید SSH:

```sh
$ ssh-keygen -t rsa
```

سپس کلید تولید شده را در مسیر _**var/lib/jenkins/.ssh/id_rsa/**_ ذخیره میکنیم

```sh
cat ~/.ssh/id_rsa.pub
```

و خروجی را در کلیپ بورد خود کپی کنید. اکنون آماده هستیم تا کلید عمومی را روی سرور nodejs-app قرار دهیم تا احراز هویت بین سرور Jenkins و سرور nodejs-app تکمیل شود.

از طریق SSH به عنوان روت وارد سرور nodejs-app شده و به کاربر خود سوئیچ کنید:

```sh
$ ssh root@NODE.SERVER.IP
$ su - <username>
```

فایلی را که در آن کلیدهای مجاز ذخیره شده است باز کنید:

```sh
$ vim ~/.ssh/authorized_keys
```

و کلید عمومی (public key) جنکینز را که ایجاد کردیم در آن فایل کپی کنید. با فشار دادن دکمه esc روی صفحه کلید خود آن را ذخیره کنید، x: را تایپ کرده و enter را فشار دهید.

مجوز (permission) صحیح را در پوشه ssh. تنظیم کنید:

```sh
chmod 700 ~/.ssh
chmod 600 ~/.ssh/*
```

قبل از اینکه ادامه دهیم ، اجازه می‌دهیم راه‌اندازی SSH خود را امتحان کنیم. اگر تنظیم درست باشد، باید بتوانیم از سرور جنکینز (JENKINS.SERVER.IP) به عنوان کاربر jenkins به username>@NODE.SERVER.IP> بدون وارد کردن رمز عبور SSH کنیم.

```sh
$ ssh root@JENKINS.SERVER.IP
$  su - jenkins
$ ssh <username>@NODE.SERVER.IP
```

#### اتوماتیک سازی Deployment:

ما قصد داریم شل اسکریپت دیگری ایجاد کنیم که مسئول استقرار و پیاده سازی است. یک فایل دیگر در پوشه **script** به نام **deploy** ایجاد کنید و اسکریپت زیر را به آن اضافه کنید:

```sh
#!/bin/sh
ssh ezderman@NODE.SERVER.IP <<EOF
   cd ~/node-app
   git pull
   npm install — production
   pm2 restart all
   exit
EOF
```

این اسکریپت به سرور نود SSH می زند، تغییرات را از GitHub پول (pull) می کند، وابستگی‌ها را نصب می‌کند و سرویس را دوباره راه‌اندازی می‌کند.

فایل اسکریپت جدید خود را با دستور زیر قابل اجرا کنید:

```sh
$ chmod +x script/deploy
```

قبل از اینکه تغییرات خود را کامیت (commit) کنیم، اجازه دهید مرحله Deployment را به Jenkins Job خود اضافه کنیم:

{{< image src="/images/virgoolPosts/vupewjsnabik.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

و آن را ذخیره کنید.

#### بررسی درستی روال انجام کارها:

ما آماده ایم هر چیزی را که از ابتدا ساخته ایم آزمایش کنیم. به پروژه node-app خود بروید و فایل index.js را ویرایش کنید تا سرور نود ما پاسخ **"Hey world"** برگرداند. فراموش نکنید **test/test.js** خود را تغییر دهید تا این رشته جدید را نیز تست کند.

در نهایت تغییرات در کد ها را کامیت و پوش کنید

```sh
$ git add .
$ git commit -m ‘add deployment script’
$ git push origin master
```

بعد از پوش کردن خواهید دید که جاب جنکینز شروع خواهد شد و وقتی تمام بشود تغییرات را در آدرس http://NODE.SERVER.IP:3000 خواهید دید.

{{< image src="/images/virgoolPosts/uxjvqfmjtcw9.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

و پایان

[منبع](https://medium.com/@mosheezderman/how-to-set-up-ci-cd-pipeline-for-a-node-js-app-with-jenkins-c51581cc783c)