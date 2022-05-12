---
title: آموزش بازنویسی ESP8266 و اتصال اون به Arduino
image: images/post_17_esp/2282-00-600x450.jpg
description: در این آموزش ما نحوه نصب کردن کتابخانه ESP8266 را بررسی میکنیم و سپس در قالب یک ویدیو آموزش آپلود کد در ESP8266 را به شما نشان خواهیم داد.
date: 2017-12-02T09:10:06+03:30
author: mam_niki
tags:
- arduino
- iot
categories:
- سخت افزار
- برنامه نویسی
---

سلام.

امروز توی این آموزش می خوام بهترین روش استفاده از ماژول ESP8266  رو به شما نشون بدم.

شرکت ها و افراد متفاوتی روی این ماژول wifi  کار کردند و تقریبا همشون به یک اجماع رسیدند که یک کتابخونه استاندارد برای این ماژول ارائه بدند تا همگان از اون استفاده کنند.برای اطلاعات بیشتر می تونید به صفحه **[GitHub ](https://github.com/esp8266/Arduino)**شون مراجعه کنید و توضیحات لازم رو بخونید.

همچنین توضیحات و آموزش های کامل نحوه استفاده از این کتابخونه توی **[این سایت](https://arduino-esp8266.readthedocs.io/en/latest/esp8266wifi/readme.html)** قابل خوندنه. پیشنهاد میکنم حتما یک نگاه بهش بندازید.

اما کمی توضیح بدم که نحوه استفاده از این کتابخونه یعنی **ESP8266WiFi library** ساده هستش .ابتدا باید این کتابخونه رو از طریق Arduino IDE نصب کنید

{{< image src="images/post_17_esp/Untitledf-300x205.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

 حجم کتابخونه اش حدود 160 مگابایته .پس از نصب شدن کافیه که توی کدتون اون کتابخونه رو include کنید .یعنی

<!-- Crayon Syntax Highlighter v_2.7.2_beta -->

```ino
#include <ESP8266WiFi.h>
```

### **نحوه نصب کتابخانه ESP8266 درArduino IDE**

ابتدا از منوی بالا به این مسیر برید.

File → Preferences

{{< image src="images/post_17_esp/1-1.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

سپس توی صفحه باز شده این آدرش رو مطابق شکل در قسمت تعیین شده قرار بدید.

##### **http://arduino.esp8266.com/stable/package_esp8266com_index.json**

{{< image src="images/post_17_esp/2-1.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}


خب OK رو بزنید تا صفحه بسته شه .سپس به منوی Tools→ Board: → Boards Manager برید

{{< image src="images/post_17_esp/3-1.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

در صفحه باز شده مطابق شکل زیر دنبال کتابخونه ESP8266 Library بگردید.

{{< image src="images/post_17_esp/4-1.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

{{< image src="images/post_17_esp/5-1.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

نصبش کمی زمان میبره و همین طور که بالاتر گفتم حدودا 160 مگابایته.بعد از نصب شدن یک بار Arduino IDE رو restart کنید.

سپس به دوباره به منوی Tools → Boards برید و توی اون منو بُرد انتخابی خودتون رو به ESP8266 تغییر بدید.

{{< image src="images/post_17_esp/6.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

همین و مراحل نصب کتابخونه تمام شد.حالا می تونیم برنامه نویسی رو شروع کنیم.

**نکته: برای استفاده از Nodemcu  هم باید این کتابخانه نصب باشه.**

درپایان من یک ویدیو میذارم براتون که توضیح میده چطوری برنامه هامون رو ازطریق Arduino توی ESP8266 آپلود کنید.


{{< video "/uploads/Easiest-ESP8266-Tutorial-Using-arduino.mp4" "mb-5 mt-5 mx-auto d-block" >}}

**نکته1:پس از دیدن این ویدیو فراموش نکنید که برای آپلود شدن کد حتما باید پین GPIO 0 به پین GND آردواینو وصل بشه والا ارور میده.**

**نکته2: حتما در برد آردواینو پایه Reset رو به پایه GND وصل کنید و یا بجاش میکروکنترلر رو از روی برد آردواینو در بیارید.اگه این کار رو نکنید کد های توی میکرو کنترلر آردواینو آپلود میشه و نه ماژول ESP8266.**

  ویدیو دوم که بجای استفاده از سخت افزار Arduino از FTDI یا همون مبدل سریال به USB استفاده میکنه.

{{< video "/uploads/Program-your-ESP8266-with-Arduino-IDE.mp4" "mb-5 mx-auto d-block" >}}

> فکر کنم یکمی این آموزش گُنگ شد. یه توضیح کلی بدم .روال کلی اینه که شما اول یک کدی رو توی ماژول ESP8266 آپلود میکنی مثلا کد این میتونه باشه که اگه کلمه hello  از wifi دریافت شد عدد 1 رو روی سریال (RX,TX) پرینت کن.
>
> وقتی که ماژول برنامه نویسی شده رو به آردواینو وصل میکنی برای آردواینو برنامه مینویسی که اگه عدد 1 رو از سریال دریافت کرد یک پایه از بُرد رو فعال کنه.
>
> همین.

موفق باشید.