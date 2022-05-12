---
title: آموزش اتصال ESP8266 به Arduino به وسیله AT command و گوشی اندروید با WIFI
image: images/post_16_espAtAndroid/photo_2017-11-30_11-45-55-600x450.jpg
description: در این آموزش ما نحوه کنترل کردن سخت افزار را به وسیله wifi و اندروید بررسی خواهیم کرد و به شما یک مثال کوچک از روشن و خاموش کردن لامپ LED از طریق اپلیکیشن اندروید را نشان خواهیم داد
date: 2017-11-30T09:17:33+03:30
author: mam_niki
tags:
- arduino
- atcommand
- iot
categories:
- سخت افزار
- برنامه نویسی
- نرم افزار
---

سلام.

همانطور که قولش رو داده بودم توی این آموزش می خوام کنترل یک LED رو از طریق گوشی هوشمندمون **اونم با wifi** در دست بگیریم .

{{< image src="images/post_16_espAtAndroid/photo_2017-11-30_11-49-39-768x1024.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

توی این آموزش ما مثل آموزش قبلی پایه های ماژول esp8266  رو به برد arduino uno  وصل کردیم با این تفاوت که پایه های TX و RX ماژول ESP8266 به پایه های 10 و 11 برد آردواینو وصل شده.پس ترتیب پایه ها میشه:

ESP8266 pin 3.3v —> Arduino pin 3.3v

ESP8266 pin CH_PD(EN) —> Arduino pin 3.3v

ESP8266 pin GND —> Arduino pin GND

ESP8266 pin TX —> Arduino pin 10

ESP8266 pin RX —> Arduino pin 11

{{< image src="images/post_16_espAtAndroid/ArduinoUno_R3_Front_450px.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

{{< image src="images/post_16_espAtAndroid/download.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

در ضمن ما یک ELD هم جهت روشن و خاموش کردن با وای فای به arduino وصل کردیم که برای اون باید پایه مثبت LED رو به پین 13 آردواینو و پایه منفی اون رو به GND آردواینو وصل کنید.

{{< image src="images/post_16_espAtAndroid/518d2d78ce395f2675000000.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

{{< image src="images/post_16_espAtAndroid/photo_2017-11-30_11-45-27-600x450.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

{{< image src="images/post_16_espAtAndroid/photo_2017-11-30_11-45-55-600x450.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

پس از اتصال درست این پایه ها ، نوبت به آپلود کد مورد نظرمون در آردواینو میرسه.

```ino
#include <SoftwareSerial.h>

SoftwareSerial ESP8266(10, 11);//10==>TX , 11==>RX
String mychar;//for store the esp8266 serial input


//String WifiName = "wifi name"; //for join to wifi modem
//String WifiPass = "wifi password";

/****************************************************************/
/*                             INIT                             */
/****************************************************************/
void setup()
{
  Serial.begin(115200);
  ESP8266.begin(115200);
  pinMode(13, OUTPUT); 
  initESP8266();
}
/****************************************************************/
/*                        LOOP                        */
/****************************************************************/
void loop()
{
   while(ESP8266.available())
   {    
     mychar = ESP8266.readString();
     if(mychar.length()>1){
        Serial.print(mychar);
        int colonPosition = mychar.indexOf(':');
        switch(mychar.charAt(colonPosition+1)){
          case 'a' :
              Serial.println('a');
              digitalWrite(13, HIGH); 
              break;
          case 'b' :
                Serial.println('b');
                digitalWrite(13, LOW); 
                break;
          case 'c' :
                ESP8266.println("AT+CIPCLOSE=0");
                break;
          default:
              break;
          }
        ESP8266.flush();
        mychar="";
      }   
    }
}
/****************************************************************/
/*                 SETUP THE ESP8266             */
/****************************************************************/
void initESP8266()
{  
  Serial.println("**********************************************************");  
  Serial.println("**************** SETTING UP WIFI MODULE ***************");
  Serial.println("**********************************************************");  
  SendToESP8266("AT+RST");
  WaitForESP8266(2000);
  Serial.println("**********************************************************");
//  SendToESP8266("AT+CWMODE=3");
//  WaitForESP8266(5000);
//  Serial.println("**********************************************************");
//  SendToESP8266("AT+CWJAP=\""+ WifiName + "\",\"" + WifiPass +"\"");
//  WaitForESP8266(10000);
//  Serial.println("**********************************************************");
  SendToESP8266("AT+CIFSR");//for sow the ip address thedefault is 192.168.4.1
  WaitForESP8266(1000);
  Serial.println("**********************************************************");
  SendToESP8266("AT+CIPMUX=1"); //enable multiple connection
  WaitForESP8266(1000);
  Serial.println("**********************************************************");
  SendToESP8266("AT+CIPSERVER=1,9999"); //open port 9999
  WaitForESP8266(1000);
  Serial.println("**********************************************************");
  Serial.println("***************** INITIALISATION COMPELETE ****************");
  Serial.println("**********************************************************");
  Serial.println("");
}

/****************************************************************/
/*        SEND AT COMMAND TO ESP8266          */
/****************************************************************/
void SendToESP8266(String commande)
{  
  ESP8266.println(commande);
}
/****************************************************************/
/*        WAIT FOR RESPONSE TIME ESP8266      */
/****************************************************************/
void WaitForESP8266(const int timeout)
{
  String reponse = "";
  long int time = millis();
  while( (time+timeout) > millis())
  {
    while(ESP8266.available())
    {
      char c = ESP8266.read();
      reponse+=c;
    }
  }
  Serial.print(reponse);   
}
```

توی کد توضیحات لازم رو کامنت کردم . اما روش کلی رو توضیح میدم .

کار کرد کد به این صورته که اول ماژول wifi esp8266 رو ریست میکنه و سپس از ماژول درخواست میکنه که ip اصلی ماژول وای فای رو به ما بده (اگه Serial Monitorباز باشه قابل دیدن خواهد بود)

به صورت دیفالت و پیش فرض این ip  عدد 192.168.4.1  هستش (صرفا جهت اطلاع)

سپس به بورد esp8266 با AT COMMAND  میگه که اجازه قبول کردن اتصال کانکشن به ماژول وای فای مون داده بشه.

در آخر هم پورت 9999  برامون باز کن که بهش متصل شیم.

اما بعد از کانفیگ ESP8266  توی قسمت تابع setup ما دستورات ورودی که ماژول وای فای مون از طریق وای فای گرفته رو آنالیز میکنیم .

این دستورات به طور کلی میگه که اگه حرف a ارسال شد، برای ما LED رو روشن کن.

اگه حرف b  ارسال شد LED رو خاموش کن.

و اگه حرف c ارسال شد ، کلا کانکشن رو ببند.

**سوالی که پیش میاد اینه که اصلا چطوری این حروف a , b, c رو از طریق WIFI بفرستیم؟**

جواب اینه که باید برنامه نویسی سوکت بکنیم مثلا اگه هدفمون app andriod هست باید براش سوکت بنویسیم که به ip ماژولمون که عدد 192.168.4.1 هست سوکت بزنه و به پورت 9999 وصل بشه.

گزینه دوم من که اینجا حال برنامه نویسی اندروید ندارم استفاده از application ای به نام TCP CLient هست.

{{< image src="images/post_16_espAtAndroid/unnamed-150x150.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

عکس بالا ایکون اون برنامه هست و از گوگل پلی یعنی **[اینجا ](https://play.google.com/store/apps/details?id=com.sollae.eztcpclient)** می تونید نصبش کنید.تنظیم کردنش بسیار راحته.

ما فرض میکنیم که کدتون رو توی آردواینو upload کردید و حالا با گوشی اندروید به wifi متصل بشید احتمالا اسم وای فای اش AI-THIMKER هستش.بعد از وصل شدن ، اپلیکیشن رو باز کنید و مثل عکس کانفیگش کنید (عکس زیر)

{{< image src="images/post_16_espAtAndroid/photo_2017-11-30_11-50-08-576x1024.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

{{< image src="images/post_16_espAtAndroid/photo_2017-11-30_11-50-03-576x1024.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

بعد که بازش کنید اگه همه چی درست انجام شده باشه بالا صفحه گوشه سمت چپ مینویشه **Connected** حالا میتونید فرمان a رو بفرستید و خواهید دید که LED روشن خواهد شد .با b  هم خاموش میشه .و با c از ترمینال میاد بیرون و ارتباط رو می بنده.

{{< image src="images/post_16_espAtAndroid/photo_2017-11-30_11-49-59-576x1024.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}


{{< image src="images/post_16_espAtAndroid/photo_2017-11-30_11-49-51-576x1024.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

{{< image src="images/post_16_espAtAndroid/photo_2017-11-30_11-49-45-576x1024.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

{{< image src="images/post_16_espAtAndroid/photo_2017-11-30_11-45-36-600x450.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}


{{< image src="images/post_16_espAtAndroid/photo_2017-11-30_11-45-55-600x450.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

همین و تمام شد.

این نکته رو در پایان بگم که این روش کُنده و delay ای در حدود 1 تا 1.5 ثانیه داره .اگه برنامه نویسیتون خوب باشه میتونید این delay رو کمتر کنید.من یک کد دیگه البته برای دانلود میذارم توی همین آموزش که کمی بهتر کار میکنه (البته کار خاصی هم نکردم توش)
ممنون و موفق  باشید.

#### [**دانلود کد**](/uploads/nikiTvIOT.rar)