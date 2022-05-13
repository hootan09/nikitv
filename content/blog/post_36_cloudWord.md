---
title: ساخت یک ابر کلمه ای جهت انالیز داده ای توییت های شخصی و...
image: images/virgoolPosts/iqdkozedpwdn.png
description: ساخت ابر کلمه ای برای توییت های ترامپ یا هر شخص دیگر
date: 2021-11-06
author: mam_niki
tags:
- python
- data mining
categories:
- نرم افزار
- برنامه نویسی
---

سلام. توی این نوشته قصد داریم که یک ابر کلمه ای (wordcloud) برای توییت های دونالد ترامپ رئیس جمهور پیشین آمریکا درست کنیم تا دریابیم از چه کلماتی بیشتر استفاده کرده است.

حساب POTUS (یک حساب کاربری توییتری مخفف [President of the United States](https://en.wikipedia.org/wiki/President_of_the_United_States)) این اواخر توییت های زیادی کرده بود ، دقیقا زمانی که این حساب کاربری دست دونالد ترامپ بود.

شبکه های اجتماعی خصوصا توییتر اصطلاحا نون و کره شده بود برایش، یعنی از طریق این کانال رسمی ، پیام های خودش رو بدون دردسر برای حامیان خودش ارسال میکرد.

اما من کنجکاو هستم که بدونم بیشتر چه کلماتی رو در توییت های خودش استفاده میکرده است، چطور این کلمات بخصوص رو انتخاب میکرده تا پیام خودش رو قدرتمند و رسا به دیگران برساند.

بیاید با هم بفهمیم.

#### ایمپورت کتابخانه های پایتونی:

اول باید کتابخانه های مورد نیاز خودمون رو ایمپورت کنیم تا بتونیم با استفاده از اون ها ابر کلمه ای رو تولید کنیم.این پکیج ها شامل کتابخانه های پایه ای مثل numpy , pandas و matplotlib و همچنین wordcloud می باشد.(نگران نباشید سورس کد رو در انتها قرار میدم)

```py
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from wordcloud import WordCloud, STOPWORDS, ImageColorGenerator
from PIL import Image
from matplotlib.colors import LinearSegmentedColormap
```

#### 

#### منبع داده های توییتی مورد نیاز:

داده هایی که من در این آموزش استفاده میکنم یک مجموعه داده (دیتاست) خام از توییت های ترامپ هست که میتونید از [اینجا ](https://www.kaggle.com/gpreda/trump-tweets)دانلودش کنید.این توییت ها شامل بازه زمانی جولای 2020 تا اکتبر 2020 میباشد که به ما اجازه میده توییت های انتخاباتی 2020 و کمپین انتخاباتی ش رو دنبال کنیم.

این داده ها به صورت یک فایل csv (مخفف Comma-separated values) میباشد و اون رو با استفاده از کتابخانه pandas به یک دیتافریم تبدیل میکنیم.

```py
df = pd.read_csv('./content/trump_tweets.csv')
```

بیاید با هم ببینیم که چی داریم:

```py
df.head()
```

{{< image src="/images/virgoolPosts/vtbnmqgh8ifo.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

{{< image src="/images/virgoolPosts/5g0df5t2vfzi.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

به نظر میرسه که داده مورد علاقه ما ستون "text" هستش که شامل توییت ها میشه.شما میتونید باقی ستون ها رو دوربریزید اما من در دیتافریم نگه خواهم داشت ولی در ابرکلمه ای استفاده نخواهد شد.

#### بررسی داده ای:

اکنون بررسی میکنیم که ستون text مقادیرnull توی خودش داره یا نه.

```py
df.isnull().sum()
```

{{< image src="/images/virgoolPosts/59khn3nnbjht.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

همانطور که میبینید (عکس بالا) ستون hashtags مقدار null داره اما مشکلی نیست چون ما استفاده ای از این ستون نداریم.مهم اینه که ستون text مقدار null نداشته باشه.

حالا تمام توییت ها رو در قالب یک رشته string به هم وصل میکنیم تا به صورت خوراک به توابع wordcloud به عنوان ورودی بدیم.

```py
tweets = " ".join(line for line in df['text'])
```

#### ایمپورت تصویر ماسک به عنوان عکس نمونه:

در قدم بعدی ما تصویر ماسک رو به عنوان تصویر نمونه برای ساخت ابر کلمه ای ایمپورت میکنیم.

میتونید ببینید که این ماسک به صور شکل سر دونالدترامپ هست.

{{< image src="/images/virgoolPosts/dx1v91ptxwbw.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

با استفاده از تابع open عکس رو وارد برنامه میکنیم و سپس با استفاده از numpy اونو به صورت ارایه ای از پیکسل ها در میاریم

```py
mask = np.array(Image.open("./content/mask.png"))
```

#### ساخت ابر کلمه ای:

در نهایت ما میتونیم ابر کلمه ای خودمون رو بسازیم.توی این ابر کلمه ای ما از رنگ های ابی و قرمز به عنوان رنگ های اصلی استفاده خواهیم کرد و همچنین رنگ پس زمینه سفید خواهد بود(این ترکیب رنگ پرچم آمریکا هست)

اول یک لیست بارنگ های ابی و قرمز درست میکنیم

```py
colors = ["#BF0A30", "#002868"]
```

سپس این رنگ ها رو به عنوان ورودی به LinearSegmentedColormap از پیکیج matplotlib میدیم تا نقشه رنگ مون (کالر مپ) آماده بشه.

```py
cmap = LinearSegmentedColormap.from_list("mycmap", colors)
```

> یک نکته : چیزی به اسم stopwords داریم که معادل فارسی اون همون کلمات اضافه یا چیزی شبیه به این هست و هدف اینه که این کلمات توی ساخت ابر کلمه ای مون لحاظ نشه.

```py
stop_words = set(STOPWORDS)
stop_words.update(["https","t","s","will","now"])
```

سپس چیزهایی که تاکنون ساختیم شامل رشته ی توییت ها (متغیر tweets) و رنگ ها و ماسک و استاپ ورد رو به عنوان ورودی به تابع wordcloud میدیم تا تصویر رو برامون بسازه.

```py
wc = WordCloud(width = 600, height = 400, random_state=1, background_color='white', colormap=cmap , mask=mask, collocations=True, stopwords = stop_words).generate(tweets)
```

در پایان با استفاده از matplotlib عکسمون رو نمایش میدیم.

```py
plt.figure(figsize=[15,15])
plt.imshow(wc, interpolation="bilinear")
plt.axis('off')
plt.show()
```

{{< image src="/images/virgoolPosts/0jkwhpsjpxy2.png" caption="" command="fill" option="q95" class="img-fluid mx-auto d-block" title="" >}}

#### تحلیل ابر کلمه ای:

همان طور که میبینید کلمات بیشتر استفاده شده شامل "Biden" , "MAGA" , "Fake News" و همینطور "Pennsylvania" در ایالت مورد مناقشه انتخاباتی میشه

#### نتیجه گیری:

کتابخانه wordcloud یک کتابخانه مفید جهت داده کاوی و انالیز و پردازش زبان طبیعی (NLP) میباشد.

میشه به صورت خلاقانه ای از این کتابخانه برای معرفی کردن خودتون در جمع مخاطبین استفاده نمود.

پایان

[لینک مقاله اصلی](https://medium.com/@kyawsawhtoon/a-wordcloud-for-trumps-tweets-f40c350271b4)

[سورس کد](https://github.com/hootan09/wordcloud)