---
title: مقدمه ای بر پردازش موازی (parallel programing) ، همزمانی (concurrent programing) در پایتون
image: images/post_9_pythonParallel/train-your-python-part-1-introduction.1280x600-600x450.jpg
description: این آموزش ما برنامه نویسی multi-processing و multi-threading را به طور کامل در پایتون شرح خواهیم داد . همین طور یک درک کلی از نحوه کارکرد برنامه نویسی همزمانی و برنامه نویسی موازی را توضیح و تفاوت این دو را به شما نشان خواهیم داد(concurrent programing – parallel programing)
date: 2017-05-04T12:30:33+04:30
author: mam_niki
tags:
- python
- همزمانی
- برنامه نویسی موازی
- چندنخی
categories:
- برنامه نویسی
- نرم افزار
---

پایتون یکی از معروف ترین زبان های برنامه نویسی در زمینه پردازش داده و  داده کاوی هست .علت اون هم وجود کتابخونه های زیاد و قابل توسعه در رابطه با این زبون هست .این روز ها سادگی استفاده از framework  های پایتون باعث شده تعداد زیادی از برنامه نویس ها به این زبون رو بیارند .

توی این آموزش ما مقدمه ای در رابطه با پردازش موازی و همزمانی خواهیم داشت . موارد مورد بحث شامل :

1.  **چرا برنامه نویسی موازی توی پایتون نیاز به مهارت داره  (بخاطر وجود GIL یا همون global interpreter lock هستش)**
2.  **threads در مقابل processes  روش های استفاده از این دو و چه موقع باید یکی از اون هارو انتخاب کنیم .**
3.  **پردازش موازی در مقابل همزمانی (parallel vs concurrent)** 
4.  **ساخت یک نمونه برنامه آزمایشی و استفاده از همه این تکنیک ها**

## Global Interpreter Lock

این مبحث **Global Interpreter Lock** یا به اختصار (GIL) یکی از بحث برانگیزترین موضوعات در دنیای پایتون هستش .در CPython که یکی از معروف ترین پیاده سازی های پایتون هستش این GIL که باعث میشه برنامه در مقابل اجرای thread ها امن بشه ( thread-safe ) یا بهتر بگم که اجرای thread یا نخ ها در برنامه مشکلی ایجاد نکنه و برنامه رو به بن بست نکشونه . خود GIL توی CPython  خیلی راحت با کتابخونه های خارجی که non-thread-safe اند مجتمع و هماهنگ میشه و در حالی که برنامه کلی رو غیر موازی یا non-parallel اجرا میکنه در عین حال به اجرای سریع تر کد های برنامه کمک میکنه .

پس نتیجه میگیریم که این GIL اونقدرها هم بد نیست . همین طور میشه کاری کرد که برنامه هایی که خارج از GIL اجرا میشند مشکلی در پردازش موازی نداشته باشند . مثلا وظایف (task) هایی که زمان اجراشون ممکنه که طولانی باشه مثل IO و یا کتاب خونه هایی مثل **numpy** از این دسته اند.

## Thread ها  در مقابل Process ها

پایتون به معنای حقیقی MultiThread یا چند نخی نیست . خُب اما thread یا همون نخ اصلا چی هست؟!!

بذارید یک قدم به عقب برگردیم و یه دید کلی به همه این چیزها داشته باشیم. یک process  یک فعالیت انتزاعی از سیستم عامل هستش . اون در حقیقت یک برنامه ست که در حال اجرا هستش به عبارت دیگه یک مجموعه کد هستش که داره اجرا میشه . چندین و چند process همیشه در کامپیوتر های امروزی در حال اجرا هستند . این process ها به صورت موازی اجرا میشند و هر کدوم یک کار تعریف شده رو انجام میدند .

یک process میتونه چند تا thread داشته باشه این thread ها در حالت کلی یک کد یکسان (از لحاظ چهارچوب برنامه ای ) که متعلق به process پدرشون هست رو اجرا میکنند. در حالت ایده آل اونها باید کد یکسانی رو اجرا کنند .اما لزوما این طور نیست .

علتش هم اینه که یک process نیاز داره که مثلا به فعالیت کاربرهای خودش که باهاش کار میکنند خدمات و پاسخ بده و در عین حال همین طور صفحه نمایش خودش رو بروز رسانی کنه و یا یک فایل رو ذخیره کنه . اینجاست که thread بکار میاد.

**اگه هنوز براتون روشن نشده قضیه به جدول زیر نگاه کنید.**

PROCESSES یا پردازه ها                      | THREADS یا نخ ها                                                                                     
------------------------------------------- | -----------------------------------------------------------------------------------------------------
حافظه خودشون رو به اشتراک نمی گذارند        | نخ ها حافظه اشتراکی دارند                                                                            
ساخت و سوییچ کردن بین process ها هزینه داره | ساخت و سوییچ کردن بین نخ ها به مراتب هزینش کمتره                                                     
منابع مصرفی بیشتری نیاز دارند               | منابع مصرفی کمتری نیاز دارند .و بعضی وقت ها به اونها process های سبک وزن هم می گند.                  
همگام سازی حافظه بین پراسس ها نیاز نیست.    | به همگام سازی داده نیاز دارند تا داده ها سالم بمونند و خراب نشند . بحث بن بست بین نخ ها هم وجود داره.

دلیلی وجود نداره که یکی شون از اون یکی بهتر باشه ، بسته به شرایط و کاری که می خواین انجام بدیدباید در نظر بگیرید که کدوم بیشتر به کارتون میاد.

### **پردازش موازی در مقابل همزمانی (parallel vs concurrent)** 

حالا یک قدم جلوتر می ریم و مبحث همزمانی رو باز میکنیم .همزمانی معمولا درست فهمیده یا درک نمیشه و همیشه با پردازش موازی اشتباه گرفته میشه .

در همزمانی ما برنامه ریزی میکنیم که کد های مستقلی نسبت به هم و البته با همکاری هم اجرا بشند .که به اون شیوه تعاونی هم می گند . مثلا در زمانی که یک کد منتظر ورودی و خروجی I/O هستش یک قسمت دیگه از کد داره یک دستور متفاوت رو اجرا میکنه . البته این یکی از مثال هاش می تونه باشه.

در پایتون ما می تونیم به یک نوع سبک وزن از همزمانی دست پیدا کنیم که این کار با استفاده از greenlet ها انجام میشه .از دید پردازش موازی استفاده از نخ ها و greenlet مساوی و یکسان به نظر می رسه چون هردوشون به صورت موازی اجرا میشند .اما با هم کمی فرق دارند .

greenlet ها بسیار کم هزینه تر از نخ ها threads اجرا میشند . به همین علت هم  greenlet ها بیشتر برای مقدار زیادی از کارهایی که اجراشون وابسته به I/O یا همون ورودی و خروجی هست مورد استفاده قرار می گیرند ، مثل استفاده در مبحث شبکه و پیکربندی شبکه و همین طور توی سرور های وب .

حالا که ما تفاوت بین نخ ها و process ها و پردازش موازی و همزمانی رو می دونیم ، می تونیم کارها و نمونه های متفاوتی برای استفاده از اون ها رو تصور کنیم .

در این جا به عنوان نمونه ما می خواهیم که یک وظیفه بخصوص رو چندین و چند بار اجرا کنیم ، اونم در حالتی که بیرون از GIL هستیم . البته حالتی رو هم که داخل اون هستیم در نظر می گیریم.

سپس اون ها رو با هم مقایسه می کنیم یعنی حالاتی که ما یک برنامه رو به صورت سریال اجرا میکنیم و یا از process ها و نخ ها کمک می گیریم .

```py
import os
import time
import threading
import multiprocessing
 
NUM_WORKERS = 4
 
def only_sleep():
    """ Do nothing, wait for a timer to expire """
    print("PID: %s, Process Name: %s, Thread Name: %s" % (
        os.getpid(),
        multiprocessing.current_process().name,
        threading.current_thread().name)
    )
    time.sleep(1)
 
 
def crunch_numbers():
    """ Do some computations """
    print("PID: %s, Process Name: %s, Thread Name: %s" % (
        os.getpid(),
        multiprocessing.current_process().name,
        threading.current_thread().name)
    )
    x = 0
    while x < 10000000:
        x += 1
```

در کد بالا ما دو تا کار رو ساختیم که هر کدوم به صورت یک تابع هستند .هردوشون زمان اجرایی طولانی ای دارند اما فقط تابع `crunch_numbers`  به صورت فعال محاسبات انجام میده . بیایید به صورت سریال تابع `only_sleep`  رو اجرا کنیم اونم به صورت چند نخی و همین طور multi-process و نتایج رو با هم مقایسه کنیم :

```py
## Run tasks serially
start_time = time.time()
for _ in range(NUM_WORKERS):
    only_sleep()
end_time = time.time()
 
print("Serial time=", end_time - start_time)
 
# Run tasks using threads
start_time = time.time()
threads = [threading.Thread(target=only_sleep) for _ in range(NUM_WORKERS)]
[thread.start() for thread in threads]
[thread.join() for thread in threads]
end_time = time.time()
 
print("Threads time=", end_time - start_time)
 
# Run tasks using processes
start_time = time.time()
processes = [multiprocessing.Process(target=only_sleep()) for _ in range(NUM_WORKERS)]
[process.start() for process in processes]
[process.join() for process in processes]
end_time = time.time()
 
print("Parallel time=", end_time - start_time)
```

```txt
PID: 95726, Process Name: MainProcess, Thread Name: MainThread
PID: 95726, Process Name: MainProcess, Thread Name: MainThread
PID: 95726, Process Name: MainProcess, Thread Name: MainThread
PID: 95726, Process Name: MainProcess, Thread Name: MainThread
Serial time= 4.018089056015015

PID: 95726, Process Name: MainProcess, Thread Name: Thread-1
PID: 95726, Process Name: MainProcess, Thread Name: Thread-2
PID: 95726, Process Name: MainProcess, Thread Name: Thread-3
PID: 95726, Process Name: MainProcess, Thread Name: Thread-4
Threads time= 1.0047411918640137

PID: 95728, Process Name: Process-1, Thread Name: MainThread
PID: 95729, Process Name: Process-2, Thread Name: MainThread
PID: 95730, Process Name: Process-3, Thread Name: MainThread
PID: 95731, Process Name: Process-4, Thread Name: MainThread
Parallel time= 1.014023780822754
```

چند تا از مشاهده های ما در اجرای این کد:

 در این مورد **اجرای سریال** کد ها همه چی خوب به نظر میرسه و ما task هامون رو یکی بعد از دیگری انجام دادیم . همه اون چهار تا روی  یک نخ و یک process اجرا شد.
<hr>
 **استفاده از process :** با استفاده از اون ما زمان اجرا رو به یک چهارم کاهش دادیم سادست چون ما task ها رو به صورت موازی اجرا کردیم .حتما متوجه شدید که هر task روی یک process متفاوته در حالی که روی  `MainThread`  اون process ها اجرا شدند .

 **استفاده از threads یا نخ ها :** ما از مزیت اینکه این وظایف ( task ها ) می تونند به صورت همزمان (concurrent) اجرا بشند استفاده کردیم .زمان اجرا هم به یک چهارم کاهش پیدا کرد با وجود اینکه هیچ چیز به صورت موازی اجرا نشد .چطوری این اتفاق افتاد ؟ ما نخ اول رو ایجاد کردیم و اون رو شروع به اجرا کردیم بصورتی که منتظر زمان timer  بمونه تا منقضی شه .ما از اجرای اون دست نگه داشتیم (pause) تا زمان اجرای اون تموم شه و در همین حال ما نخ دوم رو ایجاد کردیم و همین کارو رو برای همه نخ ها انجام دادیم ، در همین حال که زمان اجرای نخ اول تموم شد ما سوییچ میکنیم به نخ اول و اون رو متوقف میکنیم (terminate) . این الگوریتم همین کار رو برای نخ دوم و همه نخ ها انجام میده در پایان نتیجه ای که به ما میده مثل این می مونه که ما اجرا رو موازی انجام دادیم .حتما این نکته رو متوجه شدید که همه این نخ های متفاوت یک شاخه از process اصلی هستند.(MainProcess)
<hr>
 حتما این نکته رو هم متوجه شدید که استفاده از نخ کمی سریع تر از موازی شده اون هست .بخاطر اینکه هزینه تولید process ها و سوییچ بین اون ها بیشتر از اسفاده از نخ ها هستش .

بیایید همین روال رو برای اجرای task یا تابع `crunch_numbers`  انجام بدیم :
<hr>

```py
start_time = time.time()
for _ in range(NUM_WORKERS):
    crunch_numbers()
end_time = time.time()
 
print("Serial time=", end_time - start_time)
 
start_time = time.time()
threads = [threading.Thread(target=crunch_numbers) for _ in range(NUM_WORKERS)]
[thread.start() for thread in threads]
[thread.join() for thread in threads]
end_time = time.time()
 
print("Threads time=", end_time - start_time)
 
 
start_time = time.time()
processes = [multiprocessing.Process(target=crunch_numbers) for _ in range(NUM_WORKERS)]
[process.start() for process in processes]
[process.join() for process in processes]
end_time = time.time()
 
print("Parallel time=", end_time - start_time)
```
اینم خروجی برنامه بالا :

```txt
PID: 96285, Process Name: MainProcess, Thread Name: MainThread
PID: 96285, Process Name: MainProcess, Thread Name: MainThread
PID: 96285, Process Name: MainProcess, Thread Name: MainThread
PID: 96285, Process Name: MainProcess, Thread Name: MainThread
Serial time= 2.705625057220459
PID: 96285, Process Name: MainProcess, Thread Name: Thread-1
PID: 96285, Process Name: MainProcess, Thread Name: Thread-2
PID: 96285, Process Name: MainProcess, Thread Name: Thread-3
PID: 96285, Process Name: MainProcess, Thread Name: Thread-4
Threads time= 2.6961309909820557
PID: 96289, Process Name: Process-1, Thread Name: MainThread
PID: 96290, Process Name: Process-2, Thread Name: MainThread
PID: 96291, Process Name: Process-3, Thread Name: MainThread
PID: 96292, Process Name: Process-4, Thread Name: MainThread
Parallel time= 0.8014059066772461
```

این دفعه نتیجه حالت سریال خیلی به حالت multi thread  نزدیک شده و این نشون میده که پردازش محاسباتی در پایتون واقعا موازی نیست و نخ ها اصولا یکی پس از دیگری اجرا میشند و محصول اجرا شدنشون رو به همیدیگه می دند تا تمامی اون نخ ها تموم شه .

### **روش های کلی برنامه نویسی موازی/همزمانی در پایتون**

پایتون api های قوی ای برای برنامه نویسی موازی/ همزمانی (parallel/concurrent) داره . در این آموزش ما چند تا از محبوب ترین این api ها رو پوشش می دیم اما باید این نکته رو بدونید که بسته به شرایط از کدومشون استفاده کنید .

در بخش بعدی همین آموزش ما یک برنامه تمرینی از تمام حالاتی که مطرح خواهیم کرد می نویسیم . اما ماژول ها و کتابخونه هایی که مورد بحث ما هستند شامل :

 **threading** **:** یک روش استاندارد برای کار با نخ ها در پایتون هستش. این api سطح بالا ، برنامه نویسی تابعی رو با استفاده از ماژول thread_  به صورت کامل پوشش میده . خود این thread_  در پایین ترین سطح ، واسط بین سیستم عامل و برنامه هستش و از نخ های خود سیستم عامل استفاده میکنه .
<hr>

 **concurrent.futures** **:** یک ماژوله که این هم قسمتی از کتابخونه استاندارد پایتون هستش ، این ماژول یک لایه انتزاعی از کتابخانه سطح بالای thread شده.توی این ماژول نخ ها جوری شکل داده شدند که بتونند به صورت غیر متقارن و ناهمگام  (asynchronous ) کار کنند .

<hr>

 **multi-processing** **:** مثل ماژول threading هستش منتها به جای نخ از process ها استفاده میکنه .

<hr>

 **gevent و greenlets** : این Greenlet ها همچنین به نام micro-threads یا نخ های کوچک هم خوانده می شند.این ها واحد هایی کوچک از اجرای برنامه هستند که می تونند زمانبندی بشند و با همکاری همدیگه یک وظیفه رو به صورت همزمان انجام بدند در حالی که هزینه و سرباری برای برنامه نداشته باشند .

<hr>

 **celery** **:** یک توزیع سطح بالا از صفِ وظایف  (task queue) هستش . این وظایف یا task ها به صورت صف و همزمان اجرا میشند و از نمونه هایی مثل `multiprocessing`  و gevent استفاده میکنند.

### **ساخت یک برنامه تمرینی**

دانستن تئوری و اصول اولیه این موضوعات خوبه اما بهترین راه برای یادگیری این مفاهیم ،ساخت یک نمونه تمرینی از برنامه هستش.در این قسمت ما یک برنامه کلاسیک و قدیمی رو با این روش های متفاوت پیاده سازی می کنیم .

بیایید یک برنامه بنویسیم که بررسی کنه آیا وب سایت های شما بالا و uptime هستند یا نه .یک عالمه راهکار و برنامه در این مورد توی اینترنت هست چند تا از مشهور ترین اونها احتمالا  [Jetpack Monitor](https://jetpack.com/features/security/downtime-monitoring/) و [Uptime Robot](https://uptimerobot.com/) هستند . هدف اصلی این برنامه ها اینه که وقتی وب سایت های شما پایین اومد یا اصطلاحا down شد به شما خبر بده که فورا رسیدگی کنید.روش کار اونها این طوریه :

 این برنامه ها لیستی از وب سایت ها رو از شما دریافت می کنند و چک میکنند که وب سایت ها آنلاین و بالا باشند.

 هر وب سایت هر 5 الی 10 دقیقه چک میشه درنتیجه در زمان down شدن ما خیلی سریع می فهمیم.

 بجای استفاده از نوع سنتی گرفتن کل متن سایت با درخواست HTTP GET ما فقط HEAD اون سایت رو چک میکنیم .این باعث میشه ترافیک خیلی مصرف نشه و سریع تر هم این روند اجرا بشه .

 اگه وضعیت سایت (HTTP status) توی بازه خطرناک باشه ، معمولا (400+, 500+) یعنی ارور بالای 400 و یا 500 بده ، سریعا صاحب سایت خبر دار میشه.

 صاحب سایت با استفاده از ایمیل یا پیامک یا هرنوع دیگه ای خبر دار میشه.

اینجاست که می فهمیم که چرا همزمانی/پردازش موازی برای حل موضوعات و مشکلات ما ضروریه .همانطوری که لیست چک شدن وب سایت ها بالا بره ، دیگه چک کردن سریال این وب سایت ها (دونه دونه) جواب گوی کار ما نیست .ممکنه یک سایت ساعت ها از کار بیافته اما صاحب اون خبر دار نشه .

اجازه بدید کمی از ابزار های ضروری مورد استفاده این برنامه رو بنویسیم (utilities).

```py
# utils.py
 
import time
import logging
import requests
 
 
class WebsiteDownException(Exception):
    pass
 
 
def ping_website(address, timeout=20):
    """
    Check if a website is down. A website is considered down 
    if either the status_code >= 400 or if the timeout expires
     
    Throw a WebsiteDownException if any of the website down conditions are met
    """
    try:
        response = requests.head(address, timeout=timeout)
        if response.status_code >= 400:
            logging.warning("Website %s returned status_code=%s" % (address, response.status_code))
            raise WebsiteDownException()
    except requests.exceptions.RequestException:
        logging.warning("Timeout expired for website %s" % address)
        raise WebsiteDownException()
         
 
def notify_owner(address):
    """ 
    Send the owner of the address a notification that their website is down 
     
    For now, we're just going to sleep for 0.5 seconds but this is where 
    you would send an email, push notification or text-message
    """
    logging.info("Notifying the owner of %s website" % address)
    time.sleep(0.5)
     
 
def check_website(address):
    """
    Utility function: check if a website is down, if so, notify the user
    """
    try:
        ping_website(address)
    except WebsiteDownException:
        notify_owner(address)
```

ما در حقیقت یک لیست از وب سایت هامون رو نیاز داریم تا به برنامه بدیم . شما میتونید لیست خودتون رو بنویسید یا از لیست من استفاده کنید.

```txt
# websites.py
 
WEBSITE_LIST = [
    'http://envato.com',
    'http://amazon.co.uk',
    'http://amazon.com',
    'http://facebook.com',
    'http://google.com',
    'http://google.fr',
    'http://google.es',
    'http://google.co.uk',
    'http://internet.org',
    'http://gmail.com',
    'http://stackoverflow.com',
    'http://github.com',
    'http://heroku.com',
    'http://really-cool-available-domain.com',
    'http://djangoproject.com',
    'http://rubyonrails.org',
    'http://basecamp.com',
    'http://trello.com',
    'http://yiiframework.com',
    'http://shopify.com',
    'http://another-really-interesting-domain.co',
    'http://airbnb.com',
    'http://instagram.com',
    'http://snapchat.com',
    'http://youtube.com',
    'http://baidu.com',
    'http://yahoo.com',
    'http://live.com',
    'http://linkedin.com',
    'http://yandex.ru',
    'http://netflix.com',
    'http://wordpress.com',
    'http://bing.com',
]
```

معمولا بجای لیست از database استفاده میشه تا اطلاعات صاحب سایت رو هم داشته باشیم تا بهش خبر بدیم .اما این هدف اصلی این آموزش نیست و ما می خواهیم سادگی این آموزش رو رعایت کرده باشیم برای همین از لیست پایتون استفاده کردیم.

اگه خوب به لیست بالا توجه کرده باشید ، باید متوجه شده باشید که دو تا آدرس سایت طولانی در این لیست وجود داره که واقعا آدرس سایت نیست. من این دو تا رو اضافه کردم تا حداقل دو تا سایت رو بهمون بگه که down شده .

اسم برناممون رو می ذاریم **UptimeSquirrel .**

### **حالت سریال برنامه** 

اول حالت سریال شده برنامه رو امتحان می کنیم و خواهیم دید که زمان اجرایی چقدر نامطلوب شده.

```py
# serial_squirrel.py
 
import time
 
 
start_time = time.time()
 
for address in WEBSITE_LIST:
    check_website(address)
         
end_time = time.time()        
 
print("Time for SerialSquirrel: %ssecs" % (end_time - start_time))
 
# WARNING:root:Timeout expired for website http://really-cool-available-domain.com
# WARNING:root:Timeout expired for website http://another-really-interesting-domain.co
# WARNING:root:Website http://bing.com returned status_code=405
# Time for SerialSquirrel: 15.881232261657715secs
```

### **حالت استفاده از سیستم Threading**

ما می خواهیم کمی خلاقیت در حالت استفاده از thread و نخ انجام بدیم .ما از یک صف استفاده میکنیم و تمام آدرس های سایتمون رو داخل اون قرار می دیم و یک worker یا کارگر نرم افزاری برای نخ ها ایجاد می کنیم تا اون ها رو از صف خارج و اجرا کنه .صبر می کنیم تا صف خالی شه که این یعنی همه آدرس های سایت ها بررسی شده.

```py
# threaded_squirrel.py
 
import time
from queue import Queue
from threading import Thread
 
NUM_WORKERS = 4
task_queue = Queue()
 
def worker():
    # Constantly check the queue for addresses
    while True:
        address = task_queue.get()
        check_website(address)
         
        # Mark the processed task as done
        task_queue.task_done()
 
start_time = time.time()
         
# Create the worker threads
threads = [Thread(target=worker) for _ in range(NUM_WORKERS)]
 
# Add the websites to the task queue
[task_queue.put(item) for item in WEBSITE_LIST]
 
# Start all the workers
[thread.start() for thread in threads]
 
# Wait for all the tasks in the queue to be processed
task_queue.join()
 
         
end_time = time.time()        
 
print("Time for ThreadedSquirrel: %ssecs" % (end_time - start_time))
 
# WARNING:root:Timeout expired for website http://really-cool-available-domain.com
# WARNING:root:Timeout expired for website http://another-really-interesting-domain.co
# WARNING:root:Website http://bing.com returned status_code=405
# Time for ThreadedSquirrel: 3.110753059387207secs
```

### **حالت استفاده از** **concurrent.futures**

همانطوری که قبلا گفتیم concurrent.futures یک api سطح بالا جهت استفاده از نخ یا thread ها هستش .هدفی که ما اینجا بهش اشاره میکنیم در حقیقت استفاده از ThreadPoolExecutor هستش .ما می خواهیم یک وظیفه را ثبت کنیم و اون رو در یک مخزن (pool) نگه داریم .این باعث میشه که بعدا بتونیم به ویژگی هاش در آینده (future) دسترسی داشته باشیم.البته ما می توانیم که صبر کنیم تا همه future ها آماده بشه و اونوقت به جواب دست پیدا کنیم . (اگه فکر می کنیم چرت و پرت گفتم به کد نگاه کنید .)

```py
# future_squirrel.py
 
import time
import concurrent.futures
 
NUM_WORKERS = 4
 
start_time = time.time()
 
with concurrent.futures.ThreadPoolExecutor(max_workers=NUM_WORKERS) as executor:
    futures = {executor.submit(check_website, address) for address in WEBSITE_LIST}
    concurrent.futures.wait(futures)
 
end_time = time.time()        
 
print("Time for FutureSquirrel: %ssecs" % (end_time - start_time))
 
# WARNING:root:Timeout expired for website http://really-cool-available-domain.com
# WARNING:root:Timeout expired for website http://another-really-interesting-domain.co
# WARNING:root:Website http://bing.com returned status_code=405
# Time for FutureSquirrel: 1.812899112701416secs
```

### **حالت استفاده از Multi-processing**

کتابخانه `multi-processing`  تقریبا یک جایگزین برای کتابخانه `threading`  هستش .منظور اینه که به هم خیلی شباهت دارند .در این مورد ما می خواهیم که هدفمون خیلی شبیه به concurrent.futures بشه . ما در این مورد multi-processing.Pool رو طوری تنظیم می کنیم که وظایف رو ثبت کنه اون هم به وسیله مدل کردن لیست آدرس سایت ها به یک تابع .(think of the classic Python `map` function).

```py
# multiprocessing_squirrel.py
 
import time
import socket
import multiprocessing
 
NUM_WORKERS = 4
 
start_time = time.time()
 
with multiprocessing.Pool(processes=NUM_WORKERS) as pool:
    results = pool.map_async(check_website, WEBSITE_LIST)
    results.wait()
 
end_time = time.time()        
 
print("Time for MultiProcessingSquirrel: %ssecs" % (end_time - start_time))
 
# WARNING:root:Timeout expired for website http://really-cool-available-domain.com
# WARNING:root:Timeout expired for website http://another-really-interesting-domain.co
# WARNING:root:Website http://bing.com returned status_code=405
# Time for MultiProcessingSquirrel: 2.8224599361419678secs
```

### **حالت** **Gevent**

یکی از مشهورترین جایگزین ها برای رسیدن به حجم زیادی از همزمانی همین کتابخونه Gevent  هستش . چند تا مورد رو قبل از استفاده از اون باید بدونید:

* کد هایی که همزمانی رو با greenlets انجام می دند ، قطعی هستند .به عبارت دیگه این توصیف تضمین میکنه که برای هر دو اجرای منحصر بفرد که اجرا میشه جواب یکسان خواهد بود .
* شما نیاز به وصله monkey patch دارید که یک تابع استاندارده که با gevent برای اجرای کد همکاری میکنه .علت استفاده از monkey patch اینه که اگه ما از socket بجاش استفاده کنیم ممکنه که روند اجرا به بن بست بخوره و ما باید صبر کنیم تا یک عملیات کارش با سوکت تموم شه و بعد عمل بعدی رو بهش بدیم .برای حل این مشکل ما از monkey patch استفاده میکنیم تا بن بست نداشته باشیم یا به عبارت دیگه به non-blocking نزدیک تر بشیم .

برای نصب gevent این کد رو توی ترمینال یا cmd اجرا کنید تا نصب بشه .

```sy
$ pip install gevent
```

اینم کد مربوط به استفاده از gevent  که نشون میده چطور از gevent.pool.Pool برای اجرای وظایفمون استفاده کنیم.

```py
# green_squirrel.py
 
import time
from gevent.pool import Pool
from gevent import monkey
 
# Note that you can spawn many workers with gevent since the cost of creating and switching is very low
NUM_WORKERS = 4
 
# Monkey-Patch socket module for HTTP requests
monkey.patch_socket()
 
start_time = time.time()
 
pool = Pool(NUM_WORKERS)
for address in WEBSITE_LIST:
    pool.spawn(check_website, address)
 
# Wait for stuff to finish
pool.join()
         
end_time = time.time()        
 
print("Time for GreenSquirrel: %ssecs" % (end_time - start_time))
# Time for GreenSquirrel: 3.8395519256591797secs
```

### **حالت Celery**

این یکی یک مقدار نسبت به چیزهایی که بالاتر توضیح دادایم و تا حالا دیدیم فرق داره . **این کتابخونه در واقع یک نبرد برای اجرای کد در محیط های بسیار پیچیده و با عملکرد بالا رو در بر داره** .راه اندازی اون یک مقداری نیاز به فکر بیشتری داره (نسبت به موارد بالاتر) .

اول نیاز داریم که نصبش کنیم . با استفاده از کد :

```sh
$ pip install celery
```

وظایف و task ها یک مفهوم پایه و اصلی در مجموعه Celery هستند .هر چیزی که در Celery اجرا میشه باید به صورت یک وظیفه یا task باشه .کتابخونه Celery برای اجرای task ها یک انعطاف پذیری بزرگی رو به شما پیشنهاد میده .شما می تونید اون ها (task) رو همگام یا ناهمگام اجرا کنید (synchronously/ asynchronously)

البته حتی میتونه این اجرا رو به صورت real-time  و یا زمانبندی شده (scheduled) در بر بگیره .حتی میشه اون رو روی همون ماشین اجرا کرد و یا از چند تا ماشین برای اجرای کد ها بهره برد.حتی نوع اجراشون رو میشه مشخص کرد که نخ یا thread باشند ، process باشند و یا از Eventlet یا gevent استفاده کنید.

**خیلی خوبه نه !!** .تنظیم کردنش خیلی خیلی پیچیدست . Celery از سرویس ها برای ارسال و دریافت پیام ها استفاده میکنه .این پیام ها معمولا خود task هستند یا جواب حاصل از اجرای اون task رو شامل میشه .هدف ما اینه که از redis برای این هدف (سیستم نگهداری پیام ها ) استفاده کنیم .redis بهترین انتخاب ما هستش علتشم اینه که خیلی راحت نصب و فعال میشه .مورد دیگشم اینه که می تونید برای کارهای دیگه هم ازش استفاده کنید .

اگه نمی دونید Redis چیه به اینترنت مراجعه کنید . فقط اینو بگم که یک cache-server هستش که البته به عنوان دیتا بیس هم میشه ازش استفاده کرد . اینستاگرام قبلا از این (Redis) استفاده میکرد . خوبیش اینه که همه چی رو توی Ram سرور ها و کامپیوتر ها نگه میداره در نتیجه بسیار سریعه .

شما می تونید برای نصب redis به این سایت [Redis Quick Start](https://redis.io/topics/quickstart) مراجعه کنید و دستور العمل ها شو بخونید .فراموش نکنید که برای کار باهاش توی پایتون نیاز دارید که کتابخونه اش رو توی پایتون نصب کنید . برای این کار دستور زیر رو بزنید .

```sh
$ pip install redis
$ pip install celery[redis]</textarea>
```

برای اجرای redis دستور زیر رو توی ترمینال یا cmd بزنید .

```sh
$ redis-server</textarea>
```

برای اجرای کارها در Celery اول ما نیاز داریم که یک اپلیکیشن در Celery ایجاد کنیم .بعد از اون باید بدونیم که چه نوع وظیفه task  ای رو می خواهیم اجرا کنیم .برای این کار ما باید این وظایف رو در اپلیکیشن Celery ثبت کنیم .این کار به وسیله دکوراتور های (همون دِکور خودمون تلفظ میشه) app.task@ انجام میشه.

```PY
# celery_squirrel.py
 
import time
from utils import check_website
from data import WEBSITE_LIST
from celery import Celery
from celery.result import ResultSet
 
app = Celery('celery_squirrel',
             broker='redis://localhost:6379/0',
             backend='redis://localhost:6379/0')
 
@app.task
def check_website_task(address):
    return check_website(address)
 
if __name__ == "__main__":
    start_time = time.time()
 
    # Using `delay` runs the task async
    rs = ResultSet([check_website_task.delay(address) for address in WEBSITE_LIST])
     
    # Wait for the tasks to finish
    rs.get()
 
    end_time = time.time()
 
    print("CelerySquirrel:", end_time - start_time)
    # CelerySquirrel: 2.4979639053344727
```

تعجب نکیند اگه هیچ اتفاقی نیافتاد . به یاد بیارید که ما گفتیم که Celery سرویس محوره و ما نیاز داریم که اون رو (خود Celery ) اجرا کنیم .تا اینجا ما فقط task ها رو قرار دادیم داخل redis اما Celery رو استارت نزدیم تا این وظایف اجرا بشند .

برای اجرا شون کد زیر رو داخل فولدری که کد ها توش هستند اجرا کنید .یعنی ترمینال یا cmd مون توی مسیر کد های اجرایی ما باشه.

```SH
$ celery worker -A do_celery --loglevel=debug --concurrency=4</textarea>
```

حالا برگردید به کد پایتونی که اجرا کردید و ببینید که چه اتفاقی افتاد .چیزی که باید بهش توجه کنید اینه که چطور ما آدرس redis رو به کتابخونه redis داخل برنامه مون دادیم اونم 2 بار .پارامتر `broker`  مشخص میکنه که چه وظایفی به سمت Celery فرستاده میشند و همین طور پارامتر `backend`  جایی هستش که Celery نتیجه حاصل از اجرای اون وظیفه رو داخل اون قرار میده . حالا ما می تونیم از اون توی برناممون استفاده کنیم.اگه backend رو به کدتون معرفی نکنید ما نمی تونیم بفهمیم که نتیجه چی بوده .

این رو هم در نظر داشته باشید که log های حاصل از اجرای پراسس Celery توی خروجی ترمینال نشون داده میشه . اگه اشکالی رخ داد میتونید اون ها رو هم چک کنید .

### **نتیجه گیری :**

درپایان امیدوارم که این موضوع براتون جذاب بوده باشه .دیدید که چطور باید برنامه نویسی همزمان و موازی رو توی پایتون انجام بدیم .

این یکی از طولانی ترین ترجمه های من بود .

موفق باشید.