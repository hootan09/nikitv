---
title: اتوماتیک سازی منازل با اپلیکیشن اندروید
image: images/post_2_homeAutomation/homeauto-600x450.png
description: آموزش اتوماتیک سازی منازل با اپلیکیشن اندرویدی و برد Arduino و ماژول
  بلوتوث به همراه سورس کد برنامه
date: 2017-04-04T12:12:17.000+04:30
author: mam_niki
tags:
- android
- arduino
- iot
- home automation
categories:
- سخت افزار

---

{{< image src="images/post_2_homeAutomation/DIY_SmartHome1.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

تا حالا شده که بخواین خونتون رو اتوماتیک کنید ؟

مثلا چراغ ها و بقیه وسایل خونه به وسیله گوشی هوشمندتون کنترل بشه ؟

این روزها مبحث IOT یا ترجمه شده **اینترنت اشیاء** که البته به غلط هم ترجمه شده خیلی همه گیر شده و خیلی از افراد و شرکت ها دنبال راهی برای پیاده سازی افکارشون در زمینه IOT هستند.

امروز به شمااپلیکیشنی رو معرفی میکنم که بدون هیچچ پیش زمینه ای راجع به برنامه نویسی اندروید میشه ، ایده ها و افکارتون رو پیاده سازی بکنید.استفاده از این اپلیکیشن کاملا رایگان و قابل دانلوده و قابل توسعه نیز هست .

به وسیله اپلیکیشن که عکسش رو بالا گذاشتم می تونیم 5 دستگاه مختلف رو به وسیله بلوتوث کنترل کنیم.

### **پیاده سازی**

##### **وسایل لازم :**

**سخت افزار مورد نیاز:**

1- Arduino یا آردواینو clone شده و یا میکرو کنترلری که بوت لودر آردواینو روش ریخته شده باشه.برای خرید این برد میتونید [**اینجا **](http://www.jahankitshop.com/market/d/6589)رو ببینید.

2- یک ماژول بلوتوث 5 ولت دارای قابلیت TTL -UART (نحوه اتصال) مثل [**HC-05**](http://www.jahankitshop.com/market/d/5710) که توی بازار ایران خیلی راحت پیدا میشه . (**نکته : اکثر ماژول های بلوتوث ولتاژ کاری 3.3 ولت تا 6 ولت رو قبول میکنند.**)

3- تعداد 5 عدد رله 5 ولت .(**دقت کنید که حتما 5 ولت باشه**)

4- برد های کمکی و نمونه (**Breadboard**)

5- تعدادی سیم جهت اتصال.

**نرم افزار مورد نیاز :**

1- Arduino IDE که از [**سایت اصلیش**](https://www.arduino.cc/en/main/software) قابل دریافت هستش.

**این برنامه چطوری کار میکنه ؟**

خب شما داخل منزلتون یا هر جای دیگه تعدادی دستگاه دارید که برای کنترل کردنشون اون ها رو باید وصل کنید به یکی از رله ها .وقتی که اپلیکیشن به ماژول بلوتوث وصل میشه ، فرمان خودشو از طریق بلوتوث به برد آردواینو می فرسته و برد آردواینو بسته به نوع فرمان دریافتی با رله ها تعامل داره و اون ها رو روشن یا خاموش میکنه .

این نکته رو هم بگم که رله ها یک طرفشون به دستگاه مورد نظر شما وصل میشه و طرف دیگش به پین های دیجیتال (digital pin) برد آردواینو وصل هست .

{{< image src="images/post_2_homeAutomation/smarthome1.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

### **پیاده سازی اتصالات برای هوشمند سازی منازل به کمک آردواینو و بلوتوث**

شمای کلی اتصال وسایل به هم مانند شکل زیر هست :

{{< image src="images/post_2_homeAutomation/homeauto.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

دقت داشته باشید که در شکل بالا پایه Tx ماژول بلوتوث به پایه Rx برد آردواینو (digital pin 0) متصل شده است و همینطور پایه RX ماژول بلوتوث به پایه TX برد آردواینو (digital pin 1) متصل است.

همینطور پایه VCC (پایه مثبت) در ماژول بلوتوث به پایه 5 ولت آردواینو متصل است و GND یا پایه منفی این ماژول نیز به GND آردواینو متصل است.

{{< image src="images/post_2_homeAutomation/relay.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

شکل بالا تصوری از پایه های رله 5 ولت را نشان می دهد. پایه های شماره 1 و 3 در حقیقت به برد آردواینو متصل می شود.پایه شماره 1 رله به یکی از digital pin های برد آردواینو و پایه شماره 3 به پایه منفی یا GND متصل می شود.

پایه شماره 2 رله به برق 220 ولت شهری متصل می شود.(AC 220v) و در نهایت پایه شماره 4 رله به دستگاه مد نظر شما متصل میشود تا آن دستگاه را روشن و یا خاموش نماید.

#### **توجه:لطفا در انتخاب رله مناسب دقت کنید زیرا بسیار خطر ناک می باشد. همچنیناز مشورت مهندسین برق استفاده نمایید تا دچار سانحه برق گرفتگی و یا آتش سوزی نشوید.**

{{< image src="images/post_2_homeAutomation/relays.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

شکل کلی پیاده سازی برد :

{{< image src="images/post_2_homeAutomation/arrangmnt3-300x246.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

در صورتی که می خواهید برد PCB خود را بسازید می توانید از شماتیک زیر نیز کمک بگیرید.برای این کار فقط کافیست که بوت لودر BootLoader مربوط به آردواینو را در میکرو کنترلر های ATmega 168/328 بریزید و دیگر نیازی به برد آردواینو نیز نخواهید داشت.

{{< image src="images/post_2_homeAutomation/SmartHomeSchematic.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

### **برنامه نویسی برد آردواینو جهت فرمان پذیری از ماژول بلوتوث**

{{< image src="images/post_2_homeAutomation/arduino-768x412.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

ابتدا برنامه Arduino IDE را از لینک بالا دریافت کنید و سپس کد مورد نظر را در این محیط اجرا کنید . در صورتی که قبلا با این محیط آشنایی نداشته اید برای یادگیری آن به اینترنت مراجعه کنید. بارگذاری کد در برد های آردواینو بسیار راحت می باشد.**به زودی آموزشی در این رابطه خواهیم داشت.**

```ino
/******************  Smartphone Controlled Home ******************/
/*Coder - Arvind Sanjeev*/

/*The following is the code required to be run on arduino to enable control of your 
home appliances from a smartphone, please install DIY SmartHome android application 
first on your android phone*/

/*This project requires 2 continous rotation servos connected to pins 9 and 10 of arduino*/ 

byte val;

void setup()
{
  Serial.begin(115200);//Change the baud rate value depending on the default baud rate of your bluetooth module, for Bluesmirf-115200 and for JY-MCU-9600
  
  pinMode(2, OUTPUT);//Light1 pin
  pinMode(3, OUTPUT);//Light2 pin
  pinMode(4, OUTPUT);//Light3 pin
  pinMode(5, OUTPUT);//AC pin
  pinMode(6, OUTPUT);//Door Lock
}

void loop()
{
 int a=0;
 if(Serial.available())
  {
    val=Serial.read();
    Serial.println(int(val));//Display received value on Serial Monitor

if(int(val)==49)//Turn Light1 ON
   digitalWrite(2,HIGH);

 else if (int(val)==50)//Turn Light1 OFF
         digitalWrite(2,LOW);

if(int(val)==51)//Turn Light2 ON
   digitalWrite(3,HIGH);
   
 else if(int(val)==52)//Turn Light2 OFF
      digitalWrite(3,LOW);
      
if(int(val)==53)//Turn Light3 ON
   digitalWrite(4,HIGH);

 else if(int(val)==54)//Turn Light3 OFF
       digitalWrite(4,LOW);
       
if(int(val)==55)//Turn AC ON
   digitalWrite(5,HIGH);
   
 else if(int(val)==56)//Turn AC OFF
       digitalWrite(5,LOW);
       
if(int(val)==57)//Lock the DOOR
   digitalWrite(6,HIGH);
   
 else if(int(val)==48)//Unlock the DOOR
       digitalWrite(6,LOW);
    }
}
```

کد بالا بسیار ساده و خوانا است و همچنین دارای کامنت جهت توضیح خطوط کد می باشد.

این کد منتظر ورودی داده از ماژول بلوتوث می ماند و زمانی که داده ای دریافت می کند آن را با مقدار ASCII مورد نظر در شرط های if خود بررسی میکند و اگر شرط مورد نظر برقرار بود رله مربوطه را روشن می کند.

این کار با دستور “(digitalWrite(pin,HIGH” انجام می شود.و کمله HIGH در حقیقت ولتاژ 5 ولت را به رله می رساند.

**نکته: بسته به نوع ماژول بلوتوث شما ممکن است که برنامه درست کار نکند. علت آن هم Baud rate متفاوت این نوع ماژول ها است .**

**برای رفع این اشکال مقدار 115200 را در خط کد**

```ino
Serial.begin(115200);
```

**به زیر تغییر دهید**

```ino
Serial.begin(9600);
```

**نکته : دقت داشته باشید که در هنگام آپلود این کد بر روی برد خود ماژول بلوتوث به برد متصل نباشد. و Tx و Rx را قطع کنید تا به ماژول بلوتوث آسیبی نرسد . پس از آپلود کد روی برد می توانید مجدد آنها را متصل کنید.**

### **دانلود اپلیکیشن اندروید و اتصال بلوتوث آن به برد آردواینو**

{{< image src="images/post_2_homeAutomation/4.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

برای نصب اپلیکیشن [**این فایل apk.**](/uploads/ars-automation.apk) را دانلود و نصب نمایید.

قبل از باز کردن برنامه شما نیاز خواهید داشت تا بلوتوث دستگاه هوشمند خود را با ماژول بلوتوث هنگام سازی (pair) کنید .برای این کار آردواینو را روشن کنید (به usb یا اداپتور و یا باتری وصل کنید) سپس بلوتوث دستگاه اندرویدی خود را روشن کرده و آن را برای بقیه دستگاه ها نمایان سازی کنید .(make it visible to other devices) سپس جستجوی دیگر دستگاه ها را بزنید . خواهید دید که ماژول بلوتوث در دستگاه شما نمایان می شود.کد هنگام سازی معمولا ‘1234’ و یا ‘0000’ می باشد.

در این مثال نام ماژول باوتوث ما در دستگاه اندرویدی HC-06 می باشد . برنامه را باز کنید و در قسمت مربوطه این نام را بنویسید و دکمه OK را بزنید.

اکنون دستگاه اندرویدی شما به برد متصل است و می توانید از آن استفاده کنید.

{{< image src="images/post_2_homeAutomation/DIY_SmartHome1.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

{{< image src="images/post_2_homeAutomation/DIYSmartHome52.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

##### **نکات:**

##### **1- این سایت هیچگونه مسئولیتی در قبال روش انجام این آموزش را گردن نمی گیرد . حتما قبل از انجام این کار با متخصص برق مشورت های لازم را انجام دهید.**

##### **2- درصورتی که برنامه نتوانست تمامی رله ها را روشن کند بدانید که منبع تغذیه برد شما مناسب نیست و باید ولتاژ بیشتری به برد برسانید.**

[**دانلود سورس اپلیکیشن**](/uploads/home-automation.zip)

تمام شد.