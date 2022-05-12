---
title: یادداشتهایی بر رهیافت ORM با تاکید بر Hibernate – بخش ششم
image: images/post_22_hibernateORM/hi3-600x450.jpg
description: در این مقاله به بررسی روشها و استراتژی های تولید id در Hibernate خواهیم پرداخت .همانطور که در مقاله قبلی هم گفتیم یکی از ویژگی‌هایی که در مورد انتخاب یک استراتژی تولید id جذاب و مهم است تولید مقدار id برای یک entity جدید قبل از اجرای عمل واقعی insert در db است .در ادامه ۱۲ استراتژی تولید id در Hibernate را خلاصه وار بررسی می کنیم بیشتر آنها در محیطهای عملیاتی فعلی چندان کاربردی ندارند .
date: 2019-05-14T05:32:41+04:30
author: solmaz_oskouie
tags:
- java
category:
- نرم افزار
- برنامه نویسی
---

در [بخش قبلی](http://nikitv.ir/downloads/%DB%8C%D8%A7%D8%AF%D8%AF%D8%A7%D8%B4%D8%AA%D9%87%D8%A7%DB%8C%DB%8C-%D8%A8%D8%B1-%D8%B1%D9%87%DB%8C%D8%A7%D9%81%D8%AA-orm-%D8%A8%D8%A7-%D8%AA%D8%A7%DA%A9%DB%8C%D8%AF-%D8%A8%D8%B1-hibernate-5/) در مورد روشهای تولید id در JPA صحبت کردیم . در این مقاله به بررسی روشها و استراتژی های تولید id در Hibernate خواهیم پرداخت .

همانطور که در مقاله قبلی هم گفتیم یکی از ویژگی‌هایی که در مورد انتخاب یک استراتژی تولید id جذاب و مهم است تولید مقدار id برای یک entity جدید قبل از اجرای عمل واقعی insert در db است .

در ادامه ۱۲ استراتژی تولید id در Hibernate را خلاصه وار بررسی می کنیم بیشتر آنها در محیطهای عملیاتی فعلی چندان کاربردی ندارند .

1- استراتژی Native : در این روش انتخاب استراتژی را به عهده DBMS می‌گذارد که می‌تواند Sequence یا Identity باشد ( بهتر است به مستندات DBMS مورد استفاده نگاهی بیندازید تا دقیقتر بدانید از چه استراتژی پشتیبانی می‌کند ) این استراتژی هایبرنتی معادل با استراتژی استاندارد JPA یعنی GenerationType.AUTO می‌باشد .

۲- استراتژی Sequence : در این روش از یک sequence دیتابیسی native بنام HIBERNATE_SEQUENCE استفاده می‌کنند در این حالت sequence قبل از هر عمل درج فراخوانی می‌شود . ( به مستندات کلاس org.hibernate.id.SequenceGenerator مراجعه کنید ) .

۳- استراتژی sequence-identity : در این روش یک جفت (key,value) تولید می‌شود با فراخوانی یک db sequence به هنگام درج یک آیتم جدید. مثلاً به این صورت :

ITEM(ID) values (HIBERNATE_SEQUENCE.nextval)

در این روش مقدار id بعد از عمل درج بازیابی می‌شود درست شبیه استراتژی identity ( به مستندات کلاس org.hibernate.id.SequenceIdentityGenerator مراجعه کنید ) .

۴- استراتژی enhanced-sequence : در مورد استراتژی enhanced-sequence باید گفت که مقادیر عددی متوالی تولید می‌کند. اگر DBMS مورد نظر شما از مفهوم Sequence پشتیبانی کند، هایبرنت از یک sequence واقعی دیتابیسی استفاده می‌کند ولی اگر DBMS از sequence های native پشتیبانی نکند هایبرنت از یک جدول sequence اضافی استفاده می‌کند که باهاش رفتار یک آبجکت sequence را شبیه سازی کند . این استراتژی به شما portability واقعی می‌دهد به این معنی که تولید کننده id همیشه می‌تواند قبل تشکیل یک عمل درج sqlی فراخوانی بشود برخلاف ستونهای id خود افزایشی که لازم است اول عمل درج رخ دهد بعدش مقدار id به application برگردانده شود .

( به مستندات کلاس org.hibernate.id.enhanced.SequenceStyleGenerator مراجعه کنید ) . این روش معادل استراتژی GenerationType.SEQUENCE و GenerationType.AUTO در استاندارد JPA می‌باشد .

۵- استراتژی seqhilo : از یک sequence دیتابیسی native بنام HIBERNATE_SEQUENCE استفاده می‌کند . با استفاده از الگوریتم hilo ( مقادیر hi/lo ) مقدار id را قبل از درج entity در db تولید و بهش منتسب می‌کند .

۶- استراتژی hilo :اینجا از یک جدول بنام HIBERNATE_UNIQUE_KEY استفاده می‌شود و همان الگوریتم hilo بکار می‌گیرد با این تفاوت که این جدول یه سطر و ستون تکی دارد که درش مقدار بعدی sequence را نگه می‌دارد . مقدار پیش‌فرض این عدد بزرگترین مقدار 32767 است.

برای کسب اطلاعات بیشتر در مورد الگوریتم Hilo به [مقاله](http://nikitv.ir/downloads/%DB%8C%D8%A7%D8%AF%D8%AF%D8%A7%D8%B4%D8%AA%D9%87%D8%A7%DB%8C%DB%8C-%D8%A8%D8%B1-%D8%B1%D9%87%DB%8C%D8%A7%D9%81%D8%AA-orm-%D8%A8%D8%A7-%D8%AA%D8%A7%DA%A9%DB%8C%D8%AF-%D8%A8%D8%B1-hibernate-5/) پیشین مراجعه کنید.

۷- استراتژی enhanced-table : از یک جدول بنام HIBERNATE_SEQUENCES استفاده می‌کند با یک سطر که به طور پیش‌فرض یک sequence را نشان می‌دهد برای ذخیره مقدار بعدی id . این مقدار select و update می‌شود موقعی یک مقدار برای id تولید می‌شود . شما می‌توانید پیکربندی کنید این تولید کننده id را به طوری که به ازای یک تولید کننده id بتوانید چندین سطر داشته باشید ( به مستندات org.hibernate.id.enhanced.TableGenerator مراجعه کنید ) . این روش معادل با روش GenerationType.TABLE در استاندارد JPA می‌باشد .

۸- استراتژی identity : مقدار id برای کلید primary موقع انجام عمل insert‌تولید می‌شود دارای هیچ optionی نیست . متأسفانه به علت نواقصی در کد هایبرنت شما امکان پیکربندی اون رو درGenericGenerator ندارید . تنها روش برای استفاده از اون استفاده از GenerationType.IDENTITY می‌باشد .

۹- استراتژی increment : در هر بار که یک سطر جدید می‌خواهد insert بشود مقدار ستون id یک واحد زیادمی شود . این روش برای پروژه های هاییرنتی که در محیط های non-clustered اجرا می‌شوند خیلی کاربردی است . در غیر این صورت در سناریوی دیگر ازش استفاده نکنید .

۱۰- استراتژی select : در این حالت خود هایبرنت هیچ مقداری برای id تولید نمی‌کند به عبارت بهتر، موقع تولید عبارتهای sql معادل ،جهت انجام عمل insert حتی ستون id یا primary key ضمیمه نمی‌کند. بلکه هایبرنت از DBMS‌انتظار دارد تا یک مقداری به ستون primary key منتسب کند بعدش هایبرت این مقدار ستون primary key را بازیابی می‌کند . این روش چندان کارآمد نیست و فقط با درایورهای قدیمی JDBC ازش استفاده می‌شود .

۱۱- استراتژی uuid2 : یک عدد منحصربفرد ۱۲۸ بیتی UUID در لایه application تولید می‌کند برای مواقعی خوب است که با چندین db سروکار دارید و می‌خواهید این entity دارای id منحصربفردی مابین همه دیتابیس ها باشد ( مثلاً وقتی که می‌خواهید داده را از چندین db متمایز در یک اجرای batch بازیابی و مرج کرده و آرشیوش کنید . ) . برای پیکربندی آن از استراتژی org.hibernate.id.UUIDGenerationStrategy استفاده کنید و برای اطلاعات بیشتر به مستندات کلاس org.hibernate.id.UUIDGenerator مراجعه کنید .

۱۲- استراتژی guid : از یک id منحصربفرد سراسری استفاده می‌کند که توسط دیتابیس با یک تابع sql قابل دسترس در Oracle,Ingres,Ms SQL و mySql تولید می شود.هایبرنت قبل از عمل درج این تابع sql را فراخوانی کرده و مقدار با فیلد id موجود در entity که از نوع String است نگاشت می‌کند . اگر شما نیاز به کنترل کامل این فرایند تولید id دارید باید از @GenericGenerator استفاده کنید .

بعد از مرور تیتر وار روشهای تولید id در Hibernate نوبت می رسد به توصیه کتاب در مورد انتخاب بهترین استراتژی .

در ص ۷۴ کتاب چنین توصیه ای برمی خوریم :

To summarize, our recommendations on identifier generator strategies are as follows:

* In general, we prefer pre-insert generation strategies that produce identifier values independently before INSERT .
* Use enhanced-sequence, which uses a native database sequence when sup-ported and otherwise falls back to an extra database table with a single column and row, emulating a sequence.