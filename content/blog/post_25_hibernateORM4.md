---
title: یادداشتهایی بر رهیافت ORM با تاکید بر Hibernate – بخش چهارم
image: images/post_22_hibernateORM/hi3-600x450.jpg
description: مطالب مورد بررسی دراین مقاله :مفهوم Fine-grained domain modelروشهای شناسایی entity ها و value type ها در یک domain model
date: 2019-05-12T17:56:21+04:30
author: solmaz_oskouie
tags:
- java
category:
- نرم افزار
- برنامه نویسی
---

در بخش قبل ما در مورد نحوه طراحی و پیاده‌سازی domain model صحبت کردیم و در ادامه در مورد مفاهیم entity و value type ها و نیز نحوه تمییز دادن آن‌ها از همدیگر از روی UML Diagram صحبت خواهیم کرد

مطالب مورد بررسی دراین مقاله :

* مفهوم Fine-grained domain model
* روشهای شناسایی entity ها و value type ها در یک domain model

**مفهوم fined-grained domain model :**

یکی از اهداف اصلی Hibernate‌پشتیبانی از به اصطلاح fine-grained and rich domain model است . در‌واقع یکی از دلایل بکارگیری POJO‌ها همین مفهوم می‌باشد اما معنی این اصطلاح چیه ؟

**به زبان ساده کاری کنیم تعداد کلاسها بیشتر از تعداد table ها باشند !**

با یک مثال کاربردی منظور را بیان می‌کنیم :

فرض کنید در سطح domain model ، ما یک کلاس بنام User داریم این کاربر دارای یک آدرس محل زندگی می‌باشد می‌توانیم این آدرس رو به صورت تعدادی property ( فیلدهای در سطح کلاس ) نمایش دهیم مثلاً نام شهر ، نام خیابان و پلاک و…. و از طرف دیگر همین فیلدها رو به صورت ستونهایی از جدول User در سطح db داشته باشیم . اما بجای اینکار ما می‌آییم یک کلاس بنام Address تعریف می‌کنیم که حاوی فیلدهای نام شهر و خیابان و کد پستی و… باشد . اینکار هم اصل cohesion را بهبود می‌بخشد هم قابلیت استفاده مجدد از کد را . بنابراین در سطح domain model ما دو کلاس بنامهای User و Address داریم در حالی که در سطح db فقط یک جدول User که حاوی آدرس کاربر می‌باشد.

شاید این مثال چندان مفهوم را نرساند فرض کنید کلاس User دارای یک فیلد دیگری بنام email باشد شما چطور این فیلد را پیاده سازی می کنید ؟ با افزودن یک فیلد از نوع String ؟

بله ، اغلب برنامه نویسان همین کار را می کنند اما یک روش بهتر و البته کمی پیچیده تر این است که یک کلاس بنام EmailAddress تعریف کنیم و همه مفاهیم و رفتارهای سطح بالا مرتبط با ایمیل را بهش اضافه کنیم . مانند متد prepareMail . البته باید توجه کنید که هیچ کدام از کلاس های domain model نباید درگیر مفاهیم ارسال ایمیل و … بشود . اما در سطح db نیازی به این کارها نیست افزودن یک ستون اضافی برای email کافیست .

دو تا کلاس در سطح app‌داریم در حالی که یک جدول در سطح db . یکی از کلاس ها ( address ) موجودیتش به موجودیت کلاس دیگر (User) بستگی دارد . اگر User حذف بشود ناچارا باید address هم حذف بشود ارجاع به کلاس Address به طور مستقل امکان پذیر نیست . برای جواب دادن به این سوالات سراغ بخش بعدی می رویم یعنی مفهوم entity و value type .

**روشهای شناسایی entity ها و value type ها در یک domain model :**

برای شناسایی entity ها و value type‌ها سه ویژگی را باید بررسی کنیم :

1. یک entity class در اغلب موارد نیاز به یک فیلد بنام id‌ دارد . درحالی که یک value type به چنین ویژگی نیاز ندارد .

2. یک entity class دارای چرخه زندگی مستقل به خود است با حذف زدن موجودیت دیگر موجودیت او به خطر نمی‌افتد و زیر سؤال نمی‌رود در حالی که یک value type چرخه زندگیش وابسته به چرخه زندگی یک entity class است .

3. اگر به یک کلاس چندین کلاس دیگر مراجعه می‌کنند این خود می‌تواند نشانه ای باشد که کلاس مورد نظر یک entity class است .به عبارت دیگر اگر به یک کلاس فقط و فقط یک کلاس مراجعه می‌کند در این صورت این می‌تواند نشانه ای باشد مبنی براینکه اون یک value type است .

در جاهایی به وضوح می‌توان تشخیص داد این کلاس یک entity است یا value type ولی مواقعی نمی‌توان به این راحتی ها entity‌ها را از value type ها تمییز داد در این موقعیت ها فرض را بر value type می‌گذارند و سعی می‌کنند با بررسی سه شرط بالایی (بخصوص دو شرط آخری ) خلاف آن را ثابت کنند (برهان خلف ) .

به شکل زیر توجه کنید ( شکل کتاب ) :

{{< image src="images/post_22_hibernateORM/bajarfo3nunm.jpeg" caption="دیاگرام ارتباط entity ها با value type ها" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

در شکل بالایی دو کلاس Item و User به وضوح مشخص است که entity می‌باشند. کلاس Address هم با توجه به اینکه موجودیتش وابسته به موجودیت User است و فقط User بهش ارجاع داده . اما در مورد Bid چی ؟

از یک طرف وجود Bid به وجود Item وابسته است (با توجه به شکل ، Item مالک bid است ) و اگر Item حذف بشود دلیلی برای نگهداری نمونه‌های bid وجود ندارد . اما اگر در آینده نیاز داشته باشیم که تعداد bid ها یی که یک User ثبت کرده چند تا بوده در اینجا نیاز است کمی در مورد چرخه زندگی bid تأمل کنیم . در اینجا درسته که bid چرخه زندگیش وابسته به Item است ولی از طرف دیگر کلاس User نیز نیاز دارد بهش ارجاع کند بنابراین باید برای کلاس bid یک ویژگی id در نظر بگیریم .چرا که در آینده نیاز است به نمونه‌های کلاس Bid به صورت مستقل ارجاع بشود .

**نکته** :سعی کنید ارتباطات مابین entity ها ساده نگه دارید و از ارتباطات از نوع collection‌های اضافی بپرهیزید .مثلا گذاشتن یک فیلد از نوع کلکسیونی از bid‌ها در داخل کلاس User اصلاً نیاز نیست می‌توانیم آن‌ها را بعداً با نوشتن کوئیری های مناسب هندل کنیم .

_**تذکر :** برای فیلد Id نباید متد setter روی کلاس تعریف کنیم چرا که این فیلد نباید از بیرون ست بشود ._

**نکته :** در مواردی مانند Address که از نوع value type است و وجودش وابسته به وجود یک entity‌ مانند User می‌باشد بهتر است همان لحظه اول ایجاد آبجکت address یک متد constructor تعریف کنیم که فیلد user ش را ست کند.

```java
Public class Address {
    private User user;
     public void Address(User user){
      this.user=user;
    }
}
```