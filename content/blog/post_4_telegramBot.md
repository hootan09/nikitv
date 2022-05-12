---
title: نوشتن ربات برای بازی تلگرامی محبوب Lumberjack
image: images/post_4_telegramBot/telegram_game2-3-600x360.jpg
description: ا استفاده از این آموزش شما یاد میگیرید گه چگونه یک ربات نرم افزاری بنویسید
  که برنامه ها ، بازی ها ، و به طور کلی کارهای شما رو به صورت اتوماتیک و بدون دخالت
  دست انجام بده.
date: 2017-04-09T07:16:23.000+04:30
author: mam_niki
tags:
- python
- robot
categories:
- برنامه نویسی
- نرم افزار

---

{{< image src="images/post_4_telegramBot/512x512bb.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

بدون شک یکی از بازی های خیلی خیلی خوب تلگرام بازی Lumberjack می تونه باشه .این بازی با سبک Score Base و جذابیت امتیازی بودنش می تونه دوستان توی یک گروه تلگرامی رو سرگرم کنه .

این بازی نفرات بازی کننده رو بر حسب ID تلگرامشون و امتیازی که بدست می آوردند لیست میکنه و توی گروه نشون میده که کی از همه بازیش بهتره.

این بازی با java Script و Html5 نوشته شده برای همین هم ، قابل بازی کردن در گروه های تلگرامی و تحت وب نیز هست.

من توی یکی از گروه ها با این بازی آشنا شدم و دلم می خواست که از لحاظ امتیاز نفر اول باشم و از اونجایی که بازیم خیلی خوب نبود ، دست به کار شدم تا یک ربات بنویسم تا خودش بازی رو پیش ببره .

{{< image src="images/post_4_telegramBot/Jack-Back.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

**اگه دوست دارید که شما هم این بازی رو تجربه کنید میتونید از** [**این لینک**](https://telegram.games/games/lumberjack/) **این بازی رو امتحان کنید. سرگرم کننده است.**

اما برسیم به نحوه پیاده سازی ربات نرم افزاری این بازی

### **نحوه کار کرد ربات**

این ربات با استفاده از پردازش تصویر مختصات پیکسلی صفحه گیم شما رو در میاره و با استفاده از شناسایی رنگ ، تشخیص میده که چه موقع بالای سر کاراکتر بازی درخت خواهد بود . و در صورت تشخیص درخت از زیر اون کنار میره.کارکردش بسیار ساده هستش.

### **زبان نوشتن برنامه ربات**

**زبان مورد استفاده برای نوشتن ربات ، زبان محبوب پایتون python هستش. این زبان برای کارهای اتوماتیک سازی یک روند بخصوص بسیار جذاب هستش و نوشتن ربات نرم افزاری با اون بسیار ساده است.**

قبل از هر چیز پیشنهاد می کنم که ویدیو زیر رو ببینید تا یک دید کلی از این آموزش داشته باشید.

{{< video "/uploads/lumberjack.mp4" "mx-auto d-block" >}}

در ویدیو من از IDE محبوب pycharm برای برنامه نویسی استفاده کردم .لینک خود گیم رو از توی تلگرام در آوردم و توی مرورگر بازش کردم و گیم به خوبی کار کرد.

موقعیت صفحه مرورگر من از لحاظ مکانی توی صفحه دسکتاپ خاص هست چون من نقاط x,y صفحه رو به برنامه دادم .تا برنامه بتونه مختصات دقیق مرورگر رو با پردازش تصویر ، پردازش کنه .

### **نرم افزار های مورد نیاز**

1- python 2.7 که می تونید از سایتش دریافت کنید .

2 – IDE pycharm البته به دلخواهه و اجباری به نصبش نیست.

3- پکیج های numpy و PIL و pywin32 این پکیج ها ضروری هستند .

* اگه پکیجی جا افتاده می تونید از طریق کنسول برنامه هنگام اجرای برنامه بفهمید که به چی نیاز داره.
* **البته فکر کنم سورس برنامه رو به کلی تغییر دادیم و الان فقط بیشتر به pywin32 و pyoutogui نیاز داره.**

**علت اینکه من لینکشون رو نمی ذارم اینه که از طریق pip می تونید اونا رو نصب کنید.از طرفی هم این پکیج ها نسخه های 32 بیتی و 64 بیتی دارند و حجمشون هم حدودا با هم 40 مگابایت میشه .پس بهتره که اونا رو از طریق pip نصب کنید یا توی اینترنت بگردید و پیداشون کنید.**

### **کد برنامه**

```py
import time
from pyautogui import press
import win32ui
from win32gui import GetWindowText, GetForegroundWindow
def main():
    time.sleep(3)
    press('space')
    tman = 1
    w = win32ui.FindWindow( None, GetWindowText(GetForegroundWindow()) )
    dc = w.GetWindowDC()
    while(1==1):
           
            if(tman==1):
                rgb=dc.GetPixel (193, 300) & 0xff
                if(rgb in xrange(190,215)):
                    press('left')
                    time.sleep(.05)
                    continue
                else:
                    press('right')
                    #time.sleep(.0)
                    tman = 0
                    continue
            else :
                rgb=dc.GetPixel (308, 300) & 0xff
                if(rgb in xrange(190,215)):
                    press('right')
                    time.sleep(.05)
                    continue
                else:
                    press('left')
                    #time.sleep(.0)
                    tman = 1
                    continue
if __name__ == '__main__':
	main()
```

کد برنامه بسار سادست ..فقط یک نکته هست که باید بدونید اون هم اینه که زمانی که tman = 1 میشه یا هست منظور اینه که کاراکتر بازی سمت چپ درخت قرار داره.

البته نوشت این جور برنامه ها سادست و فقط کمی پیچیدگی کار با پکیج ها رو داره .

```py
rgb=dc.GetPixel (193, 300) & 0xff
if(rgb in xrange(190,215)):
```

این دو خط از کد هم پیکسل های بالا سر کاراکتر رو میگیره و شرط بررسی میکنه که رنگش طیفی از رنگ سبز هست یا نه (یعنی آیا بالای سرش شاخه ظاهر شده یا نه)

**خُب همه اینا رو گفتم تا به معرفی یک کتاب خوب برسم . پیاده سازی این ربات اصلا مهم نیست .مهم اینه که ربات های نرم افزاری خودتون رو بتونید بسازید و برنامه نویسی کنید.**

برای این کار یک کتاب خیلی خوب هست که اسمش هست :

##### **Automate the Boring Stuff with Python: Practical Programming for Total Beginners 1st Edition**

این کتاب رو توی سایت آمازون از [**این لینک**](https://www.amazon.com/Automate-Boring-Stuff-Python-Programming/dp/1593275994) می تونید پیدا کنید.قیمت این کتاب حدود 25 دلار هست . اما لینک دانلود pdf اون رو در آخر این پست براتون می ذارم.

{{< image src="images/post_4_telegramBot/book-python.jpg" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

### موضوعات کتاب

```txt
- Search for text in a file or across multiple files

  Create, update, move, and rename files and folders

- Search the Web and download online content

- Update and format data in Excel spreadsheets of any size

- Split, merge, watermark, and encrypt PDFs

- Send reminder emails and text notifications

- Fill out online forms
```

امیدوارم که خدا منو ببخشه . می دونم که نویسندش زحمت زیادی برای نوشتن این کتاب کشیده.

[**لینک دانلود کتاب در کانال تلگرام**](http://t.me/pcbooks)

##### **درپایان یه تشکر خیلی خیلی ویژه از دوست برنامه نویسم ‘علی سلمانی’ عزیز که توی نوشتن این برنامه به من خیلی خیلی کمک کرد.**

**شاید براتون جالب باشه که بگم این برنامه رو از طریق اسکایپ با هم نوشتیم چون هر کدوم توی یک شهر متفاوتیم .**

تمام شد.