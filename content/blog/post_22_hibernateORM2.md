---
title: یادداشتهایی بر رهیافت ORM با تاکید بر Hibernate – بخش دوم
image: images/post_22_hibernateORM/hi3-600x450.jpg
description: اگر در اینترنت لابلای کدهای نمونه بچرخید انواع روشهای پیکربندی پروژه Hibernateی را خواهید دید . و اگر کل داستان یا دست کم آغاز داستان و دسته بندی های مختلفی که روی پروژه های Hibernate ی و کلاً JPAی اعمال شده را ندانید خیلی زود گیج خواهید شد . عجیب هم نیست اگر حین مطالعه این کدها سؤالاتی به ذهنتان می‌رسد و دنبال جواب آن‌ها در اینترنت جستجو می‌کنید و می‌بینید دقیقاً و عیناً همان سؤال شما در مثلاً stackoverflow یا بقیه سایتها پرسیده شده ، نشان میدهد این سردرگمی و نامفهومی برامده از کدها برای اغلب برنامه نویسان تازه کار و حتی کهنه کار قبلاً پیش اومده .
date: 2019-05-02T15:44:09+04:30
author: solmaz_oskouie
tags:
- java
- connection pool
- hibernate
- hibernate configuration
- hibernate-entitymanager
- hibernate.cfg.xml
- Jndi
- jpa
- JTA
- ative hibernate
- persistence.xml
- transaction
categories:
- نرم افزار
- برنامه نویسی
---

فصل دوم کتاب Java persistence with Hibernate به معرفی اجمالی ساخت یک پروژه Hibernateی ساده می‌پردازد . نمونه کدهایی که در این فصل کتاب آورده شده بیشتر برای کسانی قابل فهم و درک است که چندین و چند سال با Hibernate از همان اوان کودکی و نوجوانی این تکنولوژی همراه بوده و در کنارش رشد کرده‌اند و از نزدیک تغییرات را پیگیری نموده اند . اما برای کسی که یادگیری این تکنولوژی را با حداقل نسخه ۴ آغاز کرده‌ شاید چندان این نمونه کدها قابل درک نباشد . هر چند هدف این فصل ارایه و معرفی ساختار اصلی یک پروژه Hibernateی ساده است که البته در حین سادگی به شما آدرس مفاهیمی چون Distributed Transaction ها و JTA API ها و پیکربندی یک پروژه Hibernate ی با ماژول نرم افزاری چون Bitronix رو هم میدهد . در این نمونه کدها شما اثری از ویژگی‌های یک Data source را نخواهید دید بلکه فقط در فایل پیکربندی Persistence.xml شما المنت jta-data-source را مشاهده خواهید کرد ( رجوع به ص ۲۱ کتاب بخش ۲-۲-۱) .

اگر در اینترنت لابلای کدهای نمونه بچرخید انواع روشهای پیکربندی پروژه Hibernateی را خواهید دید . و اگر کل داستان یا دست کم آغاز داستان و دسته بندی های مختلفی که روی پروژه های Hibernate ی و کلاً JPAی اعمال شده را ندانید خیلی زود گیج خواهید شد . عجیب هم نیست اگر حین مطالعه این کدها سؤالاتی به ذهنتان می‌رسد و دنبال جواب آن‌ها در اینترنت جستجو می‌کنید و می‌بینید دقیقاً و عیناً همان سؤال شما در مثلاً stackoverflow یا بقیه سایتها پرسیده شده ، نشان میدهد این سردرگمی و نامفهومی برامده از کدها برای اغلب برنامه نویسان تازه کار و حتی کهنه کار قبلاً پیش اومده .

من قبل از اینکه سراغ ادامه مطالعه این کتاب بروم ابتدا نشستم برای اولین و آخرین بار این سؤالات و سردرگمی های خودم را برطرف کردم با مطالعه و جستجو در اینترنت . در ادامه همه اون چیزی که دستگیرم شده را با شما به اشتراک می‌گذارم امیدوارم برای شما هم مفید باشد.

شما برنامه نویس با تجربه عزیز خواهشمندم اگر جایی در متن بنده یا در دسته بندی های ارایه شده توسط بنده اشکالی مشاهده نمودید لطفاً بهم تذکر بدهید تا هم من متوجه بشم و هم متن را اصلاح کنم .

در ادامه ابتدا این دسته بندی ها و مفاهیم ارایه خواهد شد و بعد یک نمونه پروژه ساده Hibernate را مشاهده خواهید کرد سوروس کدش رو هم در github قابل دسترس دوستان قرار می‌دهم .

**انواع فایل‌های پیکربندی در پروژه های Hibernate :**

وقتی وارد دنیای ORM و بخصوص Hibernate می‌شوید با سه واژه بیشتر برخورد می‌کنید :

* واژه JPA
* واژه Hibernate
* واژه native Hibernate

بهتر است معنی دقیق این سه تا را بدانیم بعد سراغ روشهای پیکربندی یک پروژه Hibernate برویم .

عمر پروژه Hibernate خیلی بیشتر از JPA است اما JPA یک استاندارد است برای دسترسی به دیتابیس های رابطه‌ای از درون دنیای Object oriented .

یکی از پیاده‌سازی های مشهور این استاندارد Hibernate است دیگر پیاده سازی مشهورش EclipseLink می باشد .

وقتی جایی سخن از native Hibernate می‌شود باید بدانیم منظور چیست همانطور که گفتیم عمر Hibernate خیلی بیشتر از استانداردJPA است حتی می‌توان گفت ظهور تکنولوژی بنام Hibernate باعث تولد استاندارد JPA شد . اما در خود Hibernate امکاناتی است که خیلی بیشتر و فراتر از ویژگی‌های گفته شده در مستندات JPA می‌باشد (مثلاً orphan removal ) . اگر نیاز به چنین قابلیتها و امکاناتی دارید که فقط در native Hibernate پیاده‌سازی شده است باید سراغ بسته نرم افزاری Hibernate -core بروید اما امروزه امکان استفاده از قابلیتهای هر دو یعنی هم Hibernate ی که استاندارد JPA را پیاده‌سازی می‌کند و هم native Hibernate در یک پروژه وجود دارد .

به طور کلی نیاز نیست از هر دو API استفاده کنید با این همه اگر درحال استفاده از Hibernate API ها و فایل پیکربندی hibernate.cfg.xml هستید و همچنین در پروژه شما فایل‌های mappingی چون hbm.xml ها وجود دارد .اما بنابه دلایلی نیاز دارید تا سراغ JPA بروید . خیلی راحت می‌توانید از همان فایل‌های پیکربندی موجود استفاده کرده تنها کاری که باید انجام بدهید این است که در فایل persistence.xml و در داخل المنت hibernate.ejb.cfgfile به اون فایل پیکربندی موجود یعنی hibernate.cfg.xml ارجاع بدهید .

_**نکته** : سعی کنید بسته نرم افزاری hibernate-entitymanager را در داخل پروژه خود استفاده کنید چرا که خود این بسته از بسته نرم افزاری hibernate-core استفاده کرده است ._

اگر به کدهای نمونه نگاهی بیاندازید دو نوع فایل پیکربندی را زیاد مشاهده خواهید کرد:

* فایل پیکربندی hibernate.cfg.xml
* فایل پیکربندی persistence.xml

از اولی موقعی که از خود native hibernate بخواهید بهره ببرید باید این فایل را بکار بگیرید و از دومی موقعی که بخواهید از پیاده‌سازی های مرتبط به JPA مثلاً hibernate استفاده کنید .

**در داخل یک فایل پیکربندی Hibernate چه چیزهایی پیدا می‌شود ؟**

اولین چیزی که در یک فایل پیکربندی باید اقدام به config ش کنیم موجودیتی بنام SessionFactory ( در native Hibernate ) یا EntityManager ( در پیاده‌سازی JPA ) ست .

به ازای هر data-source موردنیاز در یک پروژه ما معادلش به یک sessionFactory / entityManager نیاز داریم که پیکربندیش کنیم .

مورد دیگری که در داخل فایل پیکربندی بایستی مشخص کنیم نحوه اتصال به یک data-source می باشد آیا به صورت مستقیم از داخل خود برنامه می‌خواهیم به دیتابیس وصل بشویم یا نه از یک واسطی میخواهیم از یک به اصطلاح حوضچه connection خود به ما یک کانکشن فیزیکی تخصیص دهد .

_**داخل پرانتز :** _ _برقراری اتصال با دیتابیس و کلاً منابع داده‌ ای کار هزینه بری است ما باید به صورت بهینه اینکار را انجام دهیم طوری که یک کانکشن بیهوده باز نماندیا مدت زمان زیادی به صورت idle_ _رها نشود . برای اینکه این موضوع را بهتر درک کنیم بهتر است به چرخه زندگی یک کانکشن دیتابیسی از نزدیک نگاهی بیاندازیم :_

* یک کانکشن به دیتابیس از طریق database drive ایجاد می‌شود .
* یک tcp socket برای خواندن / نوشتن داده باز می‌شود .
* اطلاعات روی اون سوکت خوانده / نوشته می‌شود .
* کانکشن ایجاد شده بسته می‌شود
* سوکت ایجاد شده بسته می‌شود .

همین باز و بسته شدن سوکت خودش کلی هزینه بر است علاوه بر این اگر فرض کنیم به ازای هر کانکشن یک thread جدیدی ایجاد می‌شود برای مدیریت کارها میزان این هزینه خیلی زیادتر هم می‌شود . برای صرفه جویی در کار و بهبود performance

برای مدیریت کانکشن ها معمولاً از یک سری واسط هایی استفاده می‌شود بنام connection pool

از معروفترین connection pool ها می‌توان به :

* فریم ورک HikariCP
* فریم ورک C3PO

اشاره کرد . همانطور که در بالا اشاره کردیم برای بدست آوردن یک کانکشن ما یا از داخل خود پروژه اقدام به اینکار می‌کنیم یا اینکه از یک واسط می‌خواهیم این کانکشن را برای ما فراهم کند اسم این واسط اغلب اوقات یک application server است بسیاری از application server ها به صورت توکار از connection pooling پشتیبانی می‌کنند .اگر بخواهیم از application server تا برای ما کانکشن را برقرار کند یکی از روشهای رایج استفاده از مکانیسم JNDI است .

معرفی کوتاه JNDI :

مخفف JNDI یا Java Naming and Directory Interface مکانیسمی است که به کلاینتهای جاوایی امکان می‌دهد به یک سرویس / آبجکت فقط از طریق نامش مستقل از مکان دسترسی داشته باشند . یکی از رایج ترین موارد کاربرد آن در تنظیم یک db connection pool روی یک java EE Appliation server است . اینطوری هر application جاوایی که روی application server به اصطلاح deployا بشود می‌تواند به دیتا سورس از طریق نامشان رجوع کند و درنتیجه با آن‌ها ارتباط برقرار نماید . بدون اینکه app ما هیچ معلوماتی از دیتابیس و نحوه کانکشن به آن‌ها داشته باشد .

همین مکانیسم باعث می‌شود وابستگی پروژه ما به یک دیتابیس خاص از بین برود ( البته نه کاملاً بلکه این روش کمک می‌کنید میزان تغییرات در پروژه خیلی کم باشد و تنها قسمتهای خاصی از پروژه تحت تأثیر قرار بگیرد ) .

**یک پروژه Hibernate ساده همراه با بررسی فایل پیکربندی و نحوه تنظیم و تعریف یک data-source از طریق JNDI :**

در ادامه یک پروژه hibernate ساده را مشاهده می‌کنیم که از مکانیسم JNDI برای برقراری ارتباط با data-source استفاده می‌کند .

**بیزنس پروژه :**

این پروژه ساده یک رکورد را از دیتا بیس خونده و روی ص وب نمایش خواهد داد .

**تکنولوژی های بکار رفته در این پروژه :**

* بسته maven
* بسته hibernate -core نسخه 5-4-1
* بسته servlet-api نسخه ۴-۰-۱
* دیتا بیس postgres
* وب سرور tomcat نسخه ۹

ابتدا نگاهی به محتویات فایل hibernate.cfg.xml می‌اندازیم

```xml
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
    <session-factory>
        <!-- Database Connection Settings -->
        <property name="hibernate.connection.driver_class">org.postgresql.Driver</property>
        <property name="hibernate.connection.datasource">java:comp/env/jdbc/TestDb</property>
        <property name="show_sql">true</property>

        <!-- SQL Dialect -->
        <property name="hibernate.dialect">org.hibernate.dialect.PostgreSQL9Dialect</property>

        <!-- Specifying Session Context -->
        <property name="hibernate.current_session_context_class">org.hibernate.context.internal.ThreadLocalSessionContext</property>
        <property name="packagesToScan">home.solmaz.hibernatePlusJndi.model</property>
        <!-- Mapping With Model Class Containing Annotations -->
        <mapping class="home.solmaz.hibernatePlusJndi.model.Employee" />
    </session-factory>
</hibernate-configuration>
```

به این قسمت نگاه کنید :

```xml
<property name="hibernate.connection.datasource">java:comp/env/jdbc/TestDb</property>
```

مقدار :

```txt
java:comp/env/jdbc/TestDb
```

در‌واقع از مکانیسم JNDI استفاده می‌کند برای دسترسی به data-source نامی که به این data-source تخصیص داده شده است jdbc/TestDb می‌باشد . حال بیایید ببینم این اسم چطوری داخل Tomcat تنظیم شده است :

برای تنظیم این اسم به دو فایل داخل پوشه Tomcat مراجعه می‌کنید :

```sh
$ tomcat home/conf/server.xml
$ tomcat home/conf/context.xml
```

ابتدا در داخل فایل server.xml در داخل تگ GlobalNamingResources مقدار زیر را وارد می‌کنیم :

```xml
<Resource name="jdbc/TestDb"
 auth="Container"
 type="javax.sql.DataSource"
 driverClassName="org.postgresql.Driver"
 url="jdbc:postgresql://localhost:5432/tutorialdb"
 username="user123" password="" />
```

در مرحله بعدی در داخل فایل context.xml در داخل تگ context مقدار زیر را وارد می‌کنیم :

```xml
<Context>
<!-- Default set of monitored resources -->
<WatchedResource>WEB-INF/web.xml</WatchedResource>
    <ResourceLink name="jdbc/TestDb"
                  global="jdbc/TestDb"
                  type="javax.sql.DataSource" />
</Context>
```

همانطور که می‌بینید همه جا ما براساس نام jdbc/TestDb به این data-source مراجعه می‌کنیم . کاری به این که کجا و چطوری تنظیم شده و چطوری ساخته می‌شود نداریم . هر موقع خواستیم با اسمش صداش می‌زنیم و اون آبجکت / سرویس در اختیارمون است .

لزومی نمی‌بینم بقیه کدها رو اینجا لیست کنم چیزی عجیب یا جالب و نویی برای توضیح دادن نمی‌بینم کدی ست مثل خیلی از نمونه کدهای موجود در سطح وب . سورس کدها رو روی گیت هاب می‌گذارم تا اگر خواستید clone کرده و روی سیستم خودتون اجراش کنید اگرم سؤالی داشتید من در خدمتم . 

[لینک کدها در گیت هاب](https://github.com/solmaz-oskouie/hibernatePlusJNDI.git)

منابع مورد استفاده در این نوشتن و فهم این مقاله :

https://www.baeldung.com/java-connection-pooling

https://stackoverflow.com/questions/20820880/hibernate-native-vs-hibernate-jpa

https://www.baeldung.com/jee-jta

https://dzone.com/articles/hibernate-5-xml-configuration-example

https://stackoverflow.com/questions/3807503/what-is-the-purpose-of-two-config-files-for-hibernate

https://examples.javacodegeeks.com/enterprise-java/hibernate/hibernate-jndi-example/

https://stackoverflow.com/questions/5153556/understanding-jta-spring-and-bitronix

https://stackoverflow.com/questions/4051273/what-to-put-into-jta-data-source-of-persistence-xml

[JPA Tutorial – Setting Up JPA in a Java SE Environment](https://sayemdb.wordpress.com/2014/08/15/jpa-tutorial-setting-up-jpa-in-a-java-se-environment/)

https://docs.oracle.com/javase/tutorial/jdbc/basics/transactions.html

https://www.baeldung.com/jee-jta