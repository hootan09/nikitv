---
title: قسمت دوم -آموزش نصب Mosquitto Broker server برای پروتکل MQTT
image: images/post_10_mqtt/mqtt.png
description: در این آموزش ما یک broker سرور جهت تعامل اشیاء وکلاینت ها باهم را پیاده سازی خواهیم نمود . سپس به تست اولیه آن خواهیم پرداخت و نقل وانتقالات پیام ها از طریق پروتکل MQTT را مشاهده خواهیم کرد. امید است که در مراحل بعدی ، پیاده سازی اپلیکیشن و اتصال سخت افزار به این سرور مورد بررسی قرار گیرد.
date: 2017-08-27T06:14:39+04:30
author: mam_niki
tags:
- iot
categories:
- برنامه نویسی
- نرم افزار
- سخت افزار
---

توی آموزش قبل مقدمه ای بر پروتکل MQTT داشتیم و توضیح دادیم که تمامی کلاینت ها از طریق broker  با هم ارتباط برقرار میکنند . توی این قسمت ما می خواهیم broker سرور راه اندازی کنیم ، اونم توی ویندوز .

broker ای که من قصد نصب کردنش رو دارم **[Mosquitto](http://mosquitto.org/)** نام داره.این broker با زبان C نوشته شده و به همین علت بسیار سبک هستش و تا اونجایی که می دونم برای بقیه پلتفرم ها هم قابل نصبه .شدیدا توصیه می کنم که اگه می خواید به صورت حرفه ای ازش استفاده کنید اونو روی **لینوکس** نصب کنید تا حداکثر کارایی و پرفورمنس رو داشته باشید . از خدا که پنهون نیست ، از شما چه پنهون که ویندوز خیلی مزخرفه .اما خیلی از ماها نمی تونییم کامل سوییچ کنیم روی لینوکس متاسفانه.

خب  اینو بگم که [Mosquitto](http://mosquitto.org/) براساس نسخه MQTT v 3.1 و  v3.1.1  پروتکل نوشته شده .البته تا اینجا که دارم براتون می نویسم و ممکنه بروز هم بشه .

برای نصب ابتدا به سایت **[Mosquitto.org](http://mosquitto.org/)** برید و در قسمت دانلود نسخه ویندوزش رو پیدا کرده و دانلود کنید .

{{< image src="images/post_10_mqtt/061915_0748_stepbystepi1.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

سپس روی فایل نصبی کلیک کنید تا setup اش بیاد بالا و اونو نصب کنید.دقت کنید که توی صفحه زیر چی میگه :

{{< image src="images/post_10_mqtt/061915_0748_stepbystepi3.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

خط اولش میگه که این broker سرور برای اجرا نیاز به یه سری dll داره که شما باید اونا رو توی پوشه نصبی این Mosquitto Broker بریزید .

آدرس سایت اش رو هم داده . برای openssl فقط کافیه به سایتش رفته و نسخه light اش رو نصب کنید و از پوشه نصبیش dll ها رو به پوشه Mosquitto کپی کنید.

این broker همین طور نیاز به dll داره که اسمش pthreadvc2.dll هست. این dll برای پردازش موازی توی زبان C مورد استفاده قرار می گیره (هنوز از دروس مزخرف دانشگاهی یه چیزایی یادم مونده ) برای دانلود این dll باید از طریق ftp وصل شید به سرورهای redhat .اما نگران نباشید در پایان من همه بسته های نصبی مورد نیاز رو میذارم که [**دانلود**](/uploads/installing-mqtt-broker-on-windows.zip) کنید و زحمت این کار هارو نکشید.

در پایان مرحله نصب ازتون اجازه میگره که Mosquitto رو به عنوان سرویس نصب کنه روی ویندوز یا نه.

{{< image src="images/post_10_mqtt/061915_0748_stepbystepi6.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

سپس محل نصب رو می پرسه .که حتما این محل رو به خاطر بسپارید که باید بعدا dll ها رو اونجا کپی کنید .

{{< image src="images/post_10_mqtt/061915_0748_stepbystepi7.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

در پایان هم برنامه رو شروع به نصب میکنه .

{{< image src="images/post_10_mqtt/061915_0748_stepbystepi8.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

خب وقتی که نصب تموم شد نیازه که این dll ها رو که من براتون می ذارم که شما استفاده کنید ، باید به پوشه Mosquitto انتقال بدید .

(Required DLLs: libeay32.dll ssleay32.dll (Look for these files in the OpenSSL-Win32 or OpenSSL-Win32\Bin folder

Required DLLs: pthreadVC2.dll

حالا می تونید به پوشه mosquitto رفته و روی mosquitto.exe کلیک کنید تا سرور اجرا بشه.

اگه توی cmd دستور **netstat -an** رو بزنید می بینید که پورت مربوط به سرور باز شده.

{{< image src="images/post_10_mqtt/061915_0748_stepbystepi10-1.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

خب تمام شد تمام مراحل نصب .اما پیشنیاز های و تمامی بسته های مورد نیاز رو از **[اینجا](/uploads/installing-mqtt-broker-on-windows.zip)  هم** میتونید دریافت کنید.

اما بریم سراغ تستش.

برای تست کردن راه های خیلی خیلی زیادی هست .اما من ترجیح میدم دو تا راه رو معرفی کنم .یکی راه مستقیم که ویدیوشو براتون می ذارم پایین همین پست و راه دوم هم استفاده از اکستنشن های مرورگر کروم هست .

راه دوم آسون تره به نظرم .

یه app تحت مرورگر (extension) هست که اسمش MQTTLens هست و جهت کار با پروتکل MQTT نوشته شده و محیط ویژوآل خوبی هم داره.

فراموش نکنید که قبلش broker سرور رو اجرا کنید.

به **[آدرس ](https://chrome.google.com/webstore/detail/mqttlens/hemojaaeigabkbcookmlgmdigohjobjm)** رفته و این app رو نصب کنید روی مرورگرتون .بعد از نصب آیکونش این شکلیه .

{{< image src="images/post_10_mqtt/chrome-app-launcher.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

با کلیک بر روش برنامه باز میشه .

{{< image src="images/post_10_mqtt/mqttlens-startup-screen.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

با زدن دکمه **+** یه فرم باز میشه که باید چند تا چیزشو پر کنید.

1- **connection name**

اسمی هست که به این ارتباط داده میشه .

2- **host name**

در اینجا آدرس ip لوکال خودتون رو بدید مثلا 127.0.0.1

سپس دکمه create connection رو بزنید.

{{< image src="images/post_10_mqtt/mqttlens-add-new-connection-1-768x470.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}


یه صفحه جدید باز میشه که اینجا میشه از طریق پروتکل تعاملات برنامه رو دید.

{{< image src="images/post_10_mqtt/mqttlens-publish-subscribe-example-sensor1-1-768x409.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

توی این صفحه جدید به قسمت topic  و publish یک نام یکسان بدید .مثلا niki

سپس دکمه subscribe رو بزنید .

حالا توی قسمت message هرچی بنویسید و دکمه publish رو بزنید پیامتون انتشار داده میشه و به همه کسایی که اون topic رو subscribe کردند این مسیج رو میفرسته.

این نکته رو توی قسمت قبل یادم رفت بگم که فیس بوک هم سیستم پیام رسانش از همین پروتکل استفاده میکنه به دو دلیل **یک** امنیت و **دوم** سرعت بالا.

در پایان ویدیو راه استفاده دوم رو هم در همین پایین می تونید ببینید .

موفق باشید.