---
title: آموزش برنامه نویسی NodeMCU
image: images/post_18_nodemcu/3811d6e-IMG_1288-600x450.jpg
description: توی این مقاله آموزش کار با برد های NodeMcu رو بررسی خواهیم کرد و یک آموزش ویدیویی ساده جهت برنامه نویسی این برد را به شما نشان خواهیم داد.لازم به ذکر است که این برد دارای wifi داخلی جهت برنامه نویسی است.
date: 2017-12-03T10:58:26+03:30
author: mam_niki
tags:
- arduino
- iot
categories:
- سخت افزار
- برنامه نویسی
- نرم افزار
---

سلام.

این بار قصد دارم تا یک آموزش ساده جهت کار با NodeMcu رو بذارم براتون.همانطور که توی آموزش های قبل تر توضیح دادم کار با ماژول ESP8266 کمی وقت گیر و رو اعصابه .اینجا بود که NodeMcu اومد روی کار تا برنامه نویسا بتونند بیشتر روی برنامه نویسی تمرکز کنند تا کانفیگ سخت افزار.

{{< image src="images/post_18_nodemcu/esp8266-nodemcu-pinout.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

عکس بالا یک نمونه از NodeMcu هستش .این برد ها به صورت درونی روشون سخت افزار wifi تعبیه شده .

خوشبختانه توی ایران قیمت این برد ها مقرون به صرفه هست و قیمتی حدود 16 تا 17 هزار تومن داره.(البته الان که دارم براتون این مقاله رو مینویسم)

شما میتونید این برد ها رو از طریق اینترنت هم سفارش بدید .توی بازار ایران 2 نمونه از این محصول به وفور یافت میشه که میتونید از [**اینجا**](https://www.jahankitshop.com/market/d/6566) و [**اینجا** ](https://www.jahankitshop.com/market/d/6803)این دو مدل رو ببینید و یا بخرید.

توی عکس های زیر این دو مدل رو ببینید.

{{< image src="images/post_18_nodemcu/3811d6e-IMG_1288-768x483.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}


{{< image src="images/post_18_nodemcu/c617fee-NodeMCU_0_9_vs_NodeMCU_1_0_Large-768x291.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

همانطور که میبینید یکی شون کمی بزرگتره .

این دوتا برد برای شناخته شدن توسط کامپیوتر نیاز دارند که درایور ها شون روی سیستم کامپیوتر شما نصب بشه.

### **لینک دانود درایور های NodeMcu** 

[**دانلود درایور CH34X**](/uploads/CH34X.zip)

[**دانلود درایور CP210X**](/uploads/cp210x.zip)

اما از کجا بفهمیم که کدوم درایور برای کدوم برده؟

جواب ساده است. پشت برد ها ورژن درایور نرم افزاریشون رو نوشته.

{{< image src="images/post_18_nodemcu/photo_2017-12-03_12-21-34-768x639.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

خب.پس از نصب درایور ها ، برای نوشتن برنامه روی این برد ها با Arduino IDE نیاز هست که کتابخانه Esp8266 رو نصب کنیم .برای نصب این کتابخانه میتونید به **[این آموزش](/blog/post_17_esp/)** مراجعه کنید ویا ویدیو پایانی این مقاله رو مشاهده کنید.

حالا همه چی آماده هست برای برنامه نویسی .

درپایان یک آموزش ویدیو ساده براتون میذارم که نحوه آپلود کد روی Nodemcu رو نشون میده.

موفق باشید.

{{< video "/uploads/Getting-Started-with-ESP8266-12E-or-NodeMCU-using-Arduino-IDE.mp4" "mb-5 mx-auto d-block" >}}
