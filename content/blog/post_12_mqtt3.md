---
title: قسمت سوم – آموزش اتصال یک Mqtt client به سرور با برنامه نویسی Nodejs
image: images/post_10_mqtt/nodejs-mqtt (1).jpeg
description: در این آموزش ما نحوه متصل شدن به یک mqtt broker را بوسیله nodejs تجربه خواهیم کرد و مقدار داده های دریافتی و ارسالی را مشاهده خواهیم کرد.
date: 2017-08-28T13:53:15+04:30
author: mam_niki
tags:
- iot
- node.js
categories:
- برنامه نویسی
- نرم افزار
- سخت افزار
---

در قسمت قبل ما نحوه پیاده سازی سرور broker  رو توی ویندوز توضیح دادیم و در آخر اون سرور رو اجرا کردیم .

حالا توی این قسمت می خواهیم که به اون سرور ایجاد شده از طریق یک برنامه nodejs ارتباط برقرار کنیم و بهش وصل بشیم .

این آموزش خیلی جذاب خواهد بود و به احتمال زیاد از کد های این قسمت برای پیاده سازی اپلیکیشن اندرویدی که به broker  وصل بشه ، استفاده خواهیم کرد.

ممکنه که بگید چطوری از nodejs برای برنامه نویسی اندروید استفاده میکنی ؟

خب جوابش سادست .باید بگم که دیگه دوره برنامه نویسی native داره تموم میشه .این روز توی دنیا البته غیر از ایران عزیز خودمون دارند از برنامه نویسی Hibrid استفاده میکنند . یعنی ترکیبی از برنامه نویسی native و html و CSS و **JS** .خیلی این بحث رو توی این قسمت باز نمی کنم و می رم سراغ پیاده سازی این mqtt client .

فقط این نکته رو بگم که با برنامه نویسی Hibrid یک بار کد میزنید و برای هر پلتفرمی که خواستید بیلد یا خروجی میگیرید . این خروجی میتونه ios و android  و windowsphone و… باشه همین .

خب اما mqtt client .

ابتدا باید nodejs رو نصب کرده باشید تا بتونید کد ها رو اجرا کنید . برای نصب nodejs به [**این سایت** ](https://nodejs.org/en/)مراجعه کنید .حجم فایل نصبی خیلی کمه شاید در حدود 12 مگابایت باشه.

خب حالا یه پوشه خالی ایجاد کنید و داخل پوشه خالی با نگه داشتن کلین shift و راست کلیک کردن روی گزینه Open command window here کلیک کنید تا cmd باز بشه .

{{< image src="images/post_10_mqtt/Untitled.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

سپس با نوشتن

```sh
$ npm init
```

یه پروژه توی این فولدر خالی ایجاد کنید . وقتی که cmd از شما سوال پرسید کلمه yes  رو بنویسید . حالا داخل پوشه شما یه فایل به اسم package.json ایجاد شده .

این فایل برای نگه داشتن اطلاعات برنامه مثل پکیج های مورد نیاز و … استفاده میشه .

قبل از اینکه cmd رو ببندید توش این رو بنویسید.

```sh
$ npm install mqtt --save
```

این خط میره و ماژول های مورد نیاز برای کار با mqtt رو برای ما دانلود میکنه و با نوشتن save– ما ورژن نصب شده رو داخل package.json ثبت کردیم .

حالا داخل فولدر یه فایل خالی با نام app.js ایجاد کنید.این فایل قراره برنامه ما بشه .

داخل فایل ایجاد کرده خود این کد های زیر رو قرار بدید .

```js
var mqtt = require('mqtt');

var settings = {
    //clientId: 'lens_zTb7uJRBbRmUa45r4CAKnqQRhtE',
    //clean: false,
    //reconnectPeriod: 1000 * 10
}

var mqtt    = require('mqtt');
var client  = mqtt.connect('mqtt://127.0.0.1',settings);

client.on('connect', function () {
    console.log('connected to the server');
   client.subscribe('niki', { qos: 1 });
});

client.on('message', function (topic, message) {
  console.log('received', message.toString());
});

client.on("error", function(error) {
    console.log("ERROR: ", error);
});

client.on('offline', function() {
    console.log("offline");
});

client.on('reconnect', function() {
    console.log("reconnect");
});

//start sending
var i = 0;
setInterval(
    function(){
        var message = i.toString();
        console.log("sending ", message)
        client.publish("niki", message, {qos: 1}, function(){
            console.log("sent ", message)
        });
        i += 1;
    },
10000)
```

این کد ها خودشون رو به عنوان یک pub-sub به broker  وصل میکنند. و هم می تونند دیتا به broker بفرستند و هم ازش داده بخونند.

خب فقط کافیه کد تغییرات رو ذخیره کنید و توی cmd بنویسید .

```sh
$ node app.js
```

خواهید دید که برنامه اجرا میشه . در رابطه با کد ها خیلی توضیح نمیدم چون سادست .اگه دو قسمت قبل رو خونده باشید راحت اجرا میشه .

خب این رو هم بگم که برنامه ما به topic ای با نام niki وصل میشه.و کدهای بالا هر یک ثانیه یک بار یک مقدار عددی  رو publish میکنند.

دقت کنید که من قبل از اجرای این کدها mosquitto رو که broker ما هست اجرا کردم و برنامه سرور روی پورت 1883 درحال اجرا هست.

پس از اجرا برنامه کلاینت ، خروجی به شکل زیر میشه.

برا اینکه ارسال و دریافت پیام هارو ببینیم من از mqttLens هم استفاده کردم.

{{< image src="images/post_10_mqtt/Untitlesdfd.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

همانطور که میبینید برنامه پس از اتصال به brocker عدد 0 رو فرستاد و چون خودشم هم publish میکنه و هم subscribe پس مقدار 0 رو دریافت هم میکنه .

اما بگم که کلمه salam رو من با برنامه mqttLens فرستادم که توی عکس زیر خواهید دید.

در ثانیه دوم از اجرای برنامه هم مقدار 1 رو به سرور فرستاد .(هر یک ثانیه یک مقدار عددی میفرسته).

{{< image src="images/post_10_mqtt/Untitlezzd.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

توی این عکس (قسمت message )هم صفر رو گرفته و هم سلام رو فرستاده و هم دریافتش کرده . مقدار 1 اینجا نیست چون من عکس هارو در فاصله های زمانی جدا از صفحه کامپیوترم گرقتم .

این نکته رو بگم که برنامه nodejs ما براساس event listener ها کار میکنه که این خیلی خوبه .

موفق باشید.