---
title: آموزش کنترل کردن یک لامپ LED توسط گوشی اندروید – بلوتوث و برد Arduino
image: images/post_3_arduinoBluetoothLed/LED.gif
description: آموزش اتوماتیک سازی منازل با اپلیکیشن اندرویدی و برد Arduino و ماژول بلوتوث به همراه سورس کد برنامه
date: 2017-04-05T15:51:55.000+04:30
author: mam_niki
tags:
- android
- arduino
- iot
categories:
- سخت افزار

---

آیا علاقه دارید که لوازم خانه و اطراف خود را با دستگاه های هوشمند نظیر اندروید و … کنترل کنید؟

آیا این جالب نیست که حتی رباط هوشمند و کنترلی خودتون رو بسازید.این آموزش اصول پایه و اولیه این کار را به شما یاد خواهد داد.

### **ابزار های لازم :**

**سخت افزار :**

1- ماژول بلوتوث HC-05/06 این ماژول به سادگی در بازار های ایرانی و مارکت های الکترونیک یافت می شود.[**اینجا**](http://www.jahankitshop.com/market/d/5710) را ببینید.

2- برد Arduino ترجیحا مدل uno .[**اینجا**](http://www.jahankitshop.com/market/d/6589) را ببینید.

{{< image src="images/post_3_arduinoBluetoothLed/ABB1-min.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

3- باتری 9 ولت کتابی و مقداری سیم

4-یک عدد LED یا همان دیود نوری

5-مقاومت 220Ω اهم

6- یک دستگاه اندرویدی دارای بلوتوث

**نرم افزار :**

1- Arduino IDE برای دریافت آن می توانید به [**سایت**](https://www.arduino.cc/en/main/software) مراجعه کنید.

### **پیاده سازی**

**دریافت اپلیکیشن :**

ابتدا نرم افزار مورد نیاز دستگاه اندرویدی خود را از [**این لینک**](/uploads/LED-Controller.apk) دریافت و نصب کنید. نام این اپلیکیشن **LED Controller.apk** می باشد.

نحوه کار کرد کلی روش آموزشی به صورت شکل زیر میباشد.

این پروژه از سه قسمت اصلی تشکیل شده است. که شامل **دستگاه اندرویدی – ماژول بلوتوث – برد آردواینو** می باشد.

{{< image src="images/post_3_arduinoBluetoothLed/ABB9-min.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

ماژول بلوتوث HC-05/06 قابلیت کارکرد به صورت ارتباط سریال را داراست به همین منظور استفاده از آن بسیار آسان است.

دستگاه اندرویدی داده ای را به صورت سریال به ماژول بلوتوث سخت افزاری ارسال می کند و این ماژول به محض دریافت داده آن را بی کم و کاست به سریال برد آردواینو می فرستد .

برد آردواینو داده دریافتی را پردازش می کند و برحسب نوع داده ورودی عکس العمل لازم را انجام می دهد .این داده ورودی در این آموزش مقدار عددی 0 و 1 می باشد ، بدین صورت که در صورت دریافت مقدار **یک** LED را روشن میکند و همچنین اگر مقدار **صفر** را دریافت کند آن LED را خاموش میکند.

شما می توانید برای مشاهده مقدار ورودی حین متصل کردن برد به کامپیوتر جهت دریافت تغذیه ولتاژی از گزینه Serial Monitor برنامه Arduino IDE نیز استفاده کنید.

### **نحوه اتصال سخت افزاری ماژول بلوتوث به برد آردواینو**

{{< image src="images/post_3_arduinoBluetoothLed/ABB2-min.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

این مدار و اتصالات آن (شکل بالا) بسیار ساده است و نیازی به توضیح زیادی ندارد.

فقط و فقط 4 سیم اتصالی از ماژول بلوتوث به برد آردواینو متصل میشود . که دو تای آن تغذیه (سیم های مثبت و منفی) ولتاژ کاری این ماژول هستند.

دو عدد سیم دیگر نیز جهت اتصال سریال برد آردواینو به سریال ماژول بلوتوث استفاده می شود.

**پین های ماژول بلوتوث پین های آردواینو**

RX (Pin 0) ———> TX

TX (Pin 1) ———> RX

5V ———> VCC

GND ———> GND

GND به معنی زمین یا همان سیم منفی می باشد.

VCC به معنی سیم مثبت می باشد.

RX/TX پایه های مربوط به اتصال سریال هستند.

اکنون پایه مثبت (پایه بلندتر) LED را به پین شماره 13 برد آردواینو متصل کنید و برای این کار از مقاومت 220Ω یا 1KΩ استفاده کنید.

**نکته : علت استفاده از مقاومت این است که LED با ولتاژ 1.5 ولت کار می کند در حالی که ولتاژ کاری برد آردواینو 5 ولت است . با این کار به LED آسیبی نمی رسد.**

سپس پایه کوچکتر و منفی LED را به GND یا زمین یا همان پایه منفی برد آردواینو متصل کنید.

اکنون مدار شما آماده است.

{{< image src="images/post_3_arduinoBluetoothLed/ABB3-min.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

### **برنامه نویسی برد آردواینو Arduino uno**

**نکته: فراموش نکنید که قبل از آپلود کد بر روی آردواینو حتما اتصالات TX/RX را غیر فعال کنید تا آپلود به درستی انچام شود.**

```ino
/* 
 *  Bluetooh Basic: LED ON OFF
 *  This program lets you to control a LED on pin 13 of arduino using a bluetooth module
 */
char data = 0;                //Variable for storing received data
void setup() 
{
  Serial.begin(9600);         //Sets the data rate in bits per second (baud) for serial data transmission
  pinMode(13, OUTPUT);        //Sets digital pin 13 as output pin
}
void loop()
{
  if(Serial.available() > 0)  // Send data only when you receive data:
  {
    data = Serial.read();      //Read the incoming data and store it into variable data
    Serial.print(data);        //Print Value inside data in Serial monitor
    Serial.print("\n");        //New line 
    if(data == '1')            //Checks whether value of data is equal to 1 
      digitalWrite(13, HIGH);  //If value is 1 then LED turns ON
    else if(data == '0')       //Checks whether value of data is equal to 0
      digitalWrite(13, LOW);   //If value is 0 then LED turns OFF
  }                            
 
}                 
```

کد بالا بسار ساده می باشد و نیازی به توضیحات اضافه در رابطه با آن نیست.فقط کافیست این کد را با استفاده از Arduino IDE بر روی برد آپلود کنید.لینک دانلود نرم افزاار Arduino IDE را در ابتدای سایت قرار دادیم.

### **نصب اپلیکیشن اندرویدی**

{{< image src="images/post_3_arduinoBluetoothLed/ABB5-min.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

لینک دانلود اپلیکیشن را بالاتر قرار دادیم . می توانید آن را نصب کنید . پس از اتصال آردواینو به منبع تغذیه (باتری) می توانید به محض روشن شدن برد آن را با بلوتوث دستگاه اندرویدی خود یکسان سازی (pair) کنید.

معمولا کد یکسان سازی pair را عدد هایی نظیر 1234 و یا 0000 می گذارند.

برای یکسان سازی باید به تنظیمات بلوتوث دستگاه خود بروید . پس از یکسان سازی اپلیکیشن را باز کنید و دکمه قرمز آن را بزنید.

{{< image src="images/post_3_arduinoBluetoothLed/ABB7-min.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

بعد از آن دستگاه های pair شده در اپلیکیشن نشان داده خواهد شد .بر روی آن کلیک کنید تا به آن ماژول بلوتوث متصل شوید.

فراموش نکنید که نام آن ماژول HC-05 یا HC-06 بود.

{{< image src="images/post_3_arduinoBluetoothLed/ABB8-min.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

اکنون داستگاه آماده ارسال سیگنال به ماژول بلوتوث و برد آردواینو خواهد بود.

{{< image src="images/post_3_arduinoBluetoothLed/LED.gif" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

این فقط یک آموزش ساده بود برای آموزش های حرفه ای تر بقیه سایت را جستجو کنید.

جهت توسعه و یا تغییر اپلیکیشن می توانید سورس آن را خریداری کنید. این برنامه را میتوان جهت کنترل ربات و دیگر دستگاه ها توسعه داد.

در پایان شما را به دیدن  ویدیو زیر دعوت می کنیم .

{{< video "/uploads/Arduino-Bluetooth-Basic-BLINK-LED.mp4" "mb-5 mx-auto d-block" >}}

[**دانلود سورس کد**](/uploads/Arduino-Bluetooth-Basic.zip)