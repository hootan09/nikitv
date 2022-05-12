---
title: آموزش کنترل لامپ LED با Nodemcu و اپلیکیشن اندروید از طریق WIFI
image: images/post_19_nodemcuWifi/esp8266-web-server-home-automation-using-android-app-mit-app-inventor-project-600x450.png
description: در این آموزش ما نحوه کنترل یک لامپ را از طریق وای فای و به وسیله Nodemcu و اپلیکیشن اندرویدی بررسی خواهیم نمود.
date: 2017-12-08T09:53:28+03:30
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

توی این آموزش می خوام بهتون نحوه کنترل کردن یک لامپ یا LED  رو از طریق Nodemcu و اپلیکیشن اندرویدی آموزش بدم .

**قبل از هرچیز آموزش های قبلی مارو بخونید تا بدونید که چطور  Nodemcu  رو برنامه نویسی کنید.**

روال کلی اینه که ما Nodemcu رو از طریق وای فای به شبکه داخلی یا لوکال وصل میکنیم.

داخل NodeMcu  برنامه نوشتیم که اگه کسی آدرس مد نظر ما رو وارد کرد لامپ LED  رو روشن یا خاموش بکنه .سپس با محیط بصری MIT App Inventor یک برنامه ساده اندرویدی براش مینویسیم که با اپلیکیشن بشه اون لامپ یا LED  رو کنترل کرد.

در حقیقت برنامه نوشته شده برای Nodemcu اونو تبدیل به یک webserver میکنه که با وارد کردن آدرس url در مرورگر میشه لامپ رو کنترل کرد.

{{< image src="images/post_19_nodemcuWifi/Client-Server.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}


وقتی ما اپلیکیشن اندرویدش رو میسازیم در حقیقت بجای مرورگر از اپلیکیشن استفاده میکنیم

{{< image src="images/post_19_nodemcuWifi/esp8266-web-server-home-automation-using-android-app-mit-app-inventor-project.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

خب بریم سراغ کد های مربوط به Nodemcu:

```ino
#include <ESP8266WiFi.h>

const char* ssid = "your-ssid";
const char* password = "your-password";

// Create an instance of the server
// specify the port to listen on as an argument
WiFiServer server(80);

void setup() {
  Serial.begin(115200);
  delay(10);

  // prepare GPIO2
  pinMode(2, OUTPUT);
  digitalWrite(2, 0);
  
  // Connect to WiFi network
  Serial.println();
  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(ssid);
  
  WiFi.begin(ssid, password);
  
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("WiFi connected");
  
  // Start the server
  server.begin();
  Serial.println("Server started");

  // Print the IP address
  Serial.println(WiFi.localIP());
}

void loop() {
  // Check if a client has connected
  WiFiClient client = server.available();
  if (!client) {
    return;
  }
  
  // Wait until the client sends some data
  Serial.println("new client");
  while(!client.available()){
    delay(1);
  }
  
  // Read the first line of the request
  String req = client.readStringUntil('\r');
  Serial.println(req);
  client.flush();
  
  // Match the request
  int val;
  if (req.indexOf("/gpio/0") != -1)
    val = 0;
  else if (req.indexOf("/gpio/1") != -1)
    val = 1;
  else {
    Serial.println("invalid request");
    client.stop();
    return;
  }

  // Set GPIO2 according to the request
  digitalWrite(2, val);
  
  client.flush();

  // Prepare the response
  String s = "HTTP/1.1 200 OK\r\nContent-Type: text/html\r\n\r\n<!DOCTYPE HTML>\r\n<html>\r\nGPIO is now ";
  s += (val)?"high":"low";
  s += "</html>\n";

  // Send the response to the client
  client.print(s);
  delay(1);
  Serial.println("Client disonnected");

  // The client will actually be disconnected 
  // when the function returns and 'client' object is detroyed
}
```

دقت کنید که باید در خط 3 بجای “your-ssid” ، اسم مودم Wifi تون  رو که اندروید بهش وصل میشه وارد کنید و در خط 4 هم باید بجای “your-password” پسورد مودم Wifi تون رو بذارید تا Nodemcu هم به مودمتون وصل بشه و ازش ip بگیره.

حالا لامپ LED رو وصل کنید.

{{< image src="images/post_19_nodemcuWifi/Circuit-Diagram-ESP8266-Home-automation-server-768x564.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

**نکته1: شما میتونید مستقیم پایه مثبت LED رو به پایه D4 و پپایه منفی رو به GND وصل کنید و دیگه نیازی به این کار نیست که مثل عکس بالا عمل کنید اما عمر LED تون پایین میاد.**

**نکته2: میتونید به جای LED از رله استفاده کنید و یک وسیله که با ولتاژ 220v ولت کار میکنه رو کنترل کنید (اگه پیش ضمینه ندارید لطفا با برق 220 ولت بازی نکنید چون خطرناکه)**

خب وقتی که Nodemcu  رو روشن کنید و سریال مانیتور رو از Arduino IDE باز کنید ، توی اون می نویسه که این بُرد به wifi مون وصل شده یا نه .اگه وصل شده باشه ip خودش رو مینویسه.

{{< image src="images/post_19_nodemcuWifi/Untitled.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

الان توی تصویر نشون میده که ip ای که Nodemcu من گرفته 192.168.1.102 هستش.

فقط کافیه که مرور گر رو باز کنم و این رو توی آدرس بار بنویسم تا LED روشن شه

http://192.168.1.102/gpio/1 –>روشن

http://192.168.1.102/gpio/0 –>خاموش

به همین راحتی.

{{< image src="images/post_19_nodemcuWifi/Untitled-1.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

{{< image src="images/post_19_nodemcuWifi/photo_2017-12-08_12-53-24-600x450.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

{{< image src="images/post_19_nodemcuWifi/Untitledffffffdd.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

{{< image src="images/post_19_nodemcuWifi/photo_2017-12-08_12-53-29-600x450.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

**ساخت اپلیکیشن اندرویدی:**

برای برنامه نویسی اپلیکیشن ما از App Inventor استفاده میکنیم که احتیاجی به هیچ دانش برنامه نویسی ای نداره .

ابتدا آدرس [http://ai2.appinventor.mit.edu](http://ai2.appinventor.mit.edu/) توی مرورگرتون بازکنید .

روی Projects > Start New Projects کلیک کنید.

دو عدد دکمه Button مطابق شکل ایجاد کنید.

{{< image src="images/post_19_nodemcuWifi/MIT-App-Inventor-Create-Button-768x437.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

این کامپوننت رو به صفحه تون اضافه کنید Web Component

{{< image src="images/post_19_nodemcuWifi/Mit-App-Inventor-Add-Web-esp8266-768x413.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

به قسمت Block Section برید تا منطق این دکمه ها رو پیاده سازی کنیم.

مثل شکل زیر این کار رو انجام بدید.

{{< image src="images/post_19_nodemcuWifi/Untitled-2.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

**به عکس بالا دقت کنید که ip خودتون رو جایگذاری باید بکنید.**

همین و تموم شد .حالا میتونید از منوی build اپلیکیشن رو دانلود کنید و استفاده کنید.

در پایان اگه حوصله پیاده سازی App Inventor  رو ندارید میتونید فایلشو دانلود کنید و ایمپورتش کنید توی App Inventor  ، فقط ip رو تغییر بدید و build بگیرید.

[**دانلود فایل**](/uploads/App_copy.rar)

موفق باشید.