---
title: آموزش کار با ماژول وای فای ESP8266 با دستورات AT Command
image: images/post_15_espAtCommand/0yEju-600x450.png
description: در این آموزش ما نحوه اتصال ماژول وای فای ESP8266 را به کامپیوتر به شما نشان خواهیم داد و نحوه تنظیم کردن آن با دستورات AT Command را بررسی خواهیم نمود.
date: 2017-11-18T14:01:25+03:30
author: mam_niki
tags:
- arduino
- atcommand
categories:
- سخت افزار
---

سلام دوستان توی این آموزش قصد دارم روش کلی دستور دادن به ماژول ESP8266 برای کانفیگ شدنش رو نشونتون بدم.

همانطور که توی آموزش قبلی گفتم این برد در حالت معمولی با مجموعه دستورات AT Command تنظیمات اولیه خودشو می گیره.

برای اتصال این ماژول یعنی ESP8266 به کامپیوتر دو تا راه داریم .

1-استفاده از FTDI که خودش یه برد کوچولو هست که وظیفه تبدیل سریال به usb  رو برا ما ایفا میکنه ، توی این آموزش مد نظر ما نیست.(شکل زیر)

{{< image src="images/post_15_espAtCommand/UnoEspWiring2_bb.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

2- استفاده از خود Arduino که من اینجا از Arduino uno استفاده میکنم که هم ارزونه و هم کار باهاش راحته.

{{< image src="images/post_15_espAtCommand/0yEju.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

طبق عکس بالا ماژول ESP8266 رو به Arduino  وصل کنید ودقت لازم رو داشته باشید که اینجا هدف ما اصلا برنامه نویسی Arduino نیست بلکه در این آموزش ما از Arduino uno فقط به عنوان یک واسط جهت اتصال ESP8266 به کامپیوتر استفاده میکنیم.

پس توی مثال حتما دقت کنید که پایه Rx/Tx دقیقا مثل زیر وصل شده باشند.

ESP8266 RX pin<======> Arduino uno RX

ESP8266 TX pin <======> Arduino uno TX

پس حتما Rx به Rx در ESP8266  و پایه Tx به ESP8266 Tx وصل بشه.

بعد از اتصال پایه ها به هم .دیگه کار ما تمومه و حالا برنامه Arduino رو باز کنید

{{< image src="images/post_15_espAtCommand/Untitledf.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

قبلش این نکته رو بگم که اگه قبلا برد Arduino uno تون برنامه نویسی شده حتما یک پروژه خالی را روی uno کامپایل کنید تا برنامه ای برای اجرا روی میکرو کنترلر نباشه تا کار ما رو خراب کنه.مثلا کد زیر که توی عکس اومده رو روی uno آپلود کنید.

**نکته: برای upload کد حتما باید پایه های Rx/Tx که بالاتر وصل کردید رو آزاد کنید تا مشکلی پیش نیاد.**

{{< image src="images/post_15_espAtCommand/Untitledss.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

بعد از آپلود ، دوباره از متصل بودن درست پایه ها اطمینان حاصل کنید و کابل USB رو به کامپیوتر خودتون متصل کنید و از برنامه Arduino از منوی Tools گزینه Serial Monitor رو بزنید .

{{< image src="images/post_15_espAtCommand/Untitledsss.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

در صفحه باز شده حتما دقت کنید که baud Rate روی 115200 و گزینه کناریش روی **Both NL & CR** باشه .مثل عکس زیر

{{< image src="images/post_15_espAtCommand/Untitledssxss.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

حالا از طریق باکس موجود (کنار دکمه Send)  میتونید فرمان های خودتونو به ماژول ESP8266 بفرستید و نتیجه رو ببینید.

مثلا بانوشتن کلمه  **AT با حروف بزرگ** ، ماژول کلمه OK رو برمیگردونه که یعنی آمادست که فرمان بگیره .

همینطور بانوشتن **AT+RST** ماژول Reset میشه.

{{< image src="images/post_15_espAtCommand/Untitldrgrted.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

در پایان  عکس زیر که شامل دستورات پر استفاده برای ماژول ESP8266 هست رو میذارم که بهش نگاه کنید.(توی یک صفحه دیگه از مرورگر بازش کنید کاملا واضح هستش)

{{< image src="images/post_15_espAtCommand/ESP_12_HCMODU0077_AT_Commands.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

پایان.