---
title: آموزش ساخت ربات فرمان صوتی بر پایه هوش مصنوعی با Nodejs
image: images/post_13_nodejsVoiceCommand/webspeech-api-demo.gif
description: حتما فیلم ironman رو دیدید ، توی این فیلم یه سیستم هوش مصنوعی با نام jarvis هست که به صورت محاوره ای با کاربرای خودش تعامل داره.نوشتن یه چنین چیزی اصلا سخت نیست.توی این آموزش ما با کمک سیستم تشخیص گفتار مرورگر ها و هوش مصنوعی گوگل یک دستیار صوتی برای خودمون خواهیم ساخت.
date: 2017-10-02T06:28:03+03:30
author: mam_niki
tags:
- iot
- nodejs
- هوش مصنوعی
categories:
- برنامه نویسی
- نرم افزار
---

استفاده از فرمان صوتی این روز ها خیلی فراگیر تر شده و تقریبا اکثر افراد از دستیار های صوتی موجود در تلفن همراهشون نظیر Siri  و Cortana استفاده میکنند .

همچین غول های بزرگ صنعت IT هم از این به روز رسانی عقب نموندن و سرویس ها و سخت افزار هایی نظیر Amazon Echo  و [GoogleHome ](https://madeby.google.com/home/) رو ساختند.

این سرویس ها اکثرشون قابلیت اتصال به سیستم تشخیص گفتار یا Speech recognition رو دارند و می تونند از طریق فرمان صوتی متن گفته شده شما رو تشخیص بدند و حتی جواب رو هم در قالب گفت و گو و محاوره با شما درمیون بذارند .

اما…

اما نکته جالب اینجاست که شرکت هایی نظیر گوگل و موزیلا دارند سعی می کنند تمامی این خدمات رو در قالب اجرای یک برنامه تحت وب و روی مرورگر به شما تحویل بدند . به عنوان مثال پردازش تصویر که اگه سرچ بزنید احتمالن پروژه شو پیدا خواهید کرد .یا [**اینجا** ](https://webrtc.github.io/samples/)رو ببینید.

توی این آموزش ما می خواهیم که یک صفحه وب داشته باشیم که کاربر با زدن یک دکمه بتونه حرف خودش رو به صورت کلامی بزنه و جواب رو هم به صورت گفتار بشنود.

این نکته رو بدونید که سیستم تشخیص گفتار به صورت خودکار روی اکثر مرورگر ها نصبه و ما از خود api  های مرورگر که کدهای جاوا اسکریپت هست استفاده میکنیم تا حرف های گفته شده کاربر رو تشخیص بدیم .البته این سیستم تشخیص گفتار مرورگر ها هنوز در نسخه آزمایشی به سر میبره و در شکل زیر می تونید ببینید که توسط چه ورژن هایی از مرورگر ها پشتیبانی میشه.

![](http://nikitv.ir/wp-content/uploads/edd/2017/10/browser-webspeech-800w-opt-1.png)

**نکته: فعلا مرورگر هایی نظیر IE و Safari و Mozilla فقط و فقط از سیستم سنتز گفتگو یا speech synthesis پشتیبانی میکنند و این یعنی فقط می تونند جواب یا متن رو براتون بخونند .این قابلیت رو به صورت شکل بالای آیکون هر مرورگر دیده میشه . (عکس بالا)**

ویدیو زیر یک مثال کوچولو از چیزیه که ما قراره بسازیم .

<!--[if lt IE 9]><script>document.createElement('video');</script><![endif]-->نمایشگر ویدیو

<mediaelementwrapper id="video-2178-1"><video class="wp-video-shortcode" id="video-2178-1_html5" width="770" height="481" preload="metadata" src="http://nikitv.ir/wp-content/uploads/2017/10/simpleAIbot.mp4?_=1" style="width: 691.344px; height: 431.866px;"><source type="video/mp4" src="http://nikitv.ir/wp-content/uploads/2017/10/simpleAIbot.mp4?_=1"><a href="http://nikitv.ir/wp-content/uploads/2017/10/simpleAIbot.mp4">http://nikitv.ir/wp-content/uploads/2017/10/simpleAIbot.mp4</a></video></mediaelementwrapper>

Media error: Format(s) not supported or source(s) not found[Download File: http://nikitv.ir/wp-content/uploads/2017/10/simpleAIbot.mp4?_=1](http://nikitv.ir/wp-content/uploads/2017/10/simpleAIbot.mp4?_=1)

<button type="button" aria-controls="mep_0" title="پخش" aria-label="پخش" tabindex="0"></button>

00:00

00:00

00:00

<button type="button" aria-controls="mep_0" title="ساکت" aria-label="ساکت" tabindex="0"></button>[برای افزایش یا کاهش صدا از کلیدهای بالا و پایین استفاده کنید.](javascript:void(0);)

<button type="button" aria-controls="mep_0" title="تمام‌صفحه" aria-label="تمام‌صفحه" tabindex="0"></button>

برای ساخت یک چنین چیزی نیاز به سه مرحله کار داریم .

**1-** استفاده از api تشخیص گفتار مرورگر برای شنیدن گفتار و تبدیل آن به متن مورد نظر (`SpeechRecognition` )

**2-** فرستادن متن مورد نظر به یکی از سرویس های هوش مصنوعی رایج به کمک nodejs  و دریافت جواب (در اینجا ما از سرویس گوگل که اسمش API.AI هست استفاده میکنیم)

**3-** تبدیل جواب دریافت شده به صوت یا همون خوندن متن با api های داخلی مرورگر (SpeechSynthesis)

اگه کمی درکش سخته میتونید به عکس زیر نگاه کنید تا متوجه این سه قسمت بالا بشید.

![](http://nikitv.ir/wp-content/uploads/edd/2017/10/chatapp_with_web-speech_api-preview-opt-1.png)

خب تمام سوروس کد رو  میتونید از **[اینجا ](https://github.com/hootan09/web-speech-ai)**دریافت کنید.

اما برسیم به بخش توضیح پیاده سازی و پیش نیاز های این آموزش:

این آموزش برپایه nodejs نوشته شده و شما باید یه حداقل هایی رو از nodejs و javascript بلد باشید تا راحت تر کد هارو درک کنید.

### **پیاده سازی کد:**

ابتدا یک فولدر درست کنید و با دستور زیر فایل package.json رو به پروژه تون اضافه کنید .

<!-- Crayon Syntax Highlighter v_2.7.2_beta -->


<textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly="" style="tab-size: 4; font-size: 12px !important; line-height: 15px !important; z-index: 0; opacity: 0; overflow: hidden;">npm init --yes</textarea>

col 1 | col 2         
----- | --------------
1     | npm init --yes

<!-- [Format Time: 0.0005 seconds] -->

سپس داخل پوشه ساخته شده فایل های مورد تظر رو طبق ساختار درختی زیر ایجاد کنید.

<!-- Crayon Syntax Highlighter v_2.7.2_beta -->


<textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly="" style="tab-size: 4; font-size: 12px !important; line-height: 15px !important; z-index: 0; opacity: 0; overflow: hidden;">.
├── index.js
├── public
│   ├── css
│   │   └── style.css
│   └── js
│       └── script.js
└── views
    └── index.html</textarea>

col 1                     | col 2                                                                                                                            
------------------------- | ---------------------------------------------------------------------------------------------------------------------------------
1

2

3

4

5

6

7

8

9 | .

├── index.js

├── public

│   ├── css

│   │   └── style.css

│   └── js

│       └── script.js

└── views

    └── index.html

<!-- [Format Time: 0.0015 seconds] -->

سپس با دستور npm اقدام به نصب ماژول های مورد نیاز خودمون برای نوشتن برنامه سمت سرور می کنیم.

<!-- Crayon Syntax Highlighter v_2.7.2_beta -->


<textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly="" style="tab-size: 4; font-size: 12px !important; line-height: 15px !important; z-index: 0; opacity: 0; overflow: hidden;">$ npm install express socket.io apiai --save</textarea>

col 1 | col 2                                       
----- | --------------------------------------------
1     | $ npm install express socket.io apiai --save

<!-- [Format Time: 0.0005 seconds] -->

این فرمان علاوه بر دانلود ماژول های مورد استفاده ، ورژن و نوع برنامه نصب شده رو در فایل package.json قرار میده تا برای استفاده های بعدی به مشکل نخورید.

توضیح بدم که express یک framework هست که بر مبنای معماری MVC کار میکنه و وظیفه اش اجرای صفحات وب برای کاربر ها و نوشتن route هستش .البته خیلی بیشتر از این حرفاست و واقعا express محشره و اکثر برنامه نویس های nodejs از این ماژول استفاده میکنند.

مورد بعدی socketIO هست که ما نصبش کردیم . این کتابخونه در حقیقت وظیفه ارسال و دریافت پیام از صفحه مرورگر ما به سمت سرور رو به صورت realTime بر عهده داره و دیگه نیازی به refresh صفحه وب نخواهد بود . البته از ajax و یا fetch هم میشه برای فرستادن پیام در قالب GET یا POST استفاده کرد ولی این بهترین راهه.

**نکته:این socketIO که اینجا نصبش کردیم فقط و فقط وظیفه سمت سرور رو انجام میده و برای سمت کلاینت از نسخه CDN این کتابخونه استفاده خواهیم کرد.تا ارتباط دوطرفه بین سرور ما و صفحه وب از طریق socketIO برقرار بشه.**

مورد بعدی ماژول apiai هست که کتابخونه ای جهت برقراری ارتباط با سرور های هوش مصنوعی گوگل هست. در حقیقت ما متنی که از مرورگر دریافت کردیم به سرور خودمون میفرستیم و سپس اون متن رو به سرور گوگل انتقال میدیم تا سرور گوگل اونو آنالیز کنه و جواب متناسب رو برگردونه و در نهایت ما اون جواب رو با socketIO به مرورگر کلاینت مون انتقال میدیم.

اگه مطالب گفته شده بالا کمی گُنگ بود به عکس زیر نگاه کنید تا کاملا مفهومش رو درک کنید.

![](http://nikitv.ir/wp-content/uploads/edd/2017/10/using_socketio-preview-opt-1.png)

من کد ها رو به ترتیب میذارم و خیلی توضیح نمیدم چون خیلی ساده هستند و نیازی به توضیح نداره.توضیح بیشتر فقط خسته تون میکنه.

کد اول index.js در حقیقت این کد اجرا میشه و وب اپلیکیشن ما آماده کار میشه.

<!-- Crayon Syntax Highlighter v_2.7.2_beta -->


JavaScript

<textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly="" style="tab-size: 4; font-size: 12px !important; line-height: 15px !important; z-index: 0; opacity: 0; overflow: hidden;">const APIAI_TOKEN = "";
const APIAI_SESSION_ID = "";

const express = require('express');
const app = express();

app.use(express.static(__dirname + '/views')); // html
app.use(express.static(__dirname + '/public')); // js, css, images

const server = app.listen(process.env.PORT || 5000, () =&gt; {
  console.log('Express server listening on port %d in %s mode', server.address().port, app.settings.env);
});

const io = require('socket.io')(server);
io.on('connection', function(socket){
  console.log('a user connected');
});

const apiai = require('apiai')(APIAI_TOKEN);

// Web UI
app.get('/', (req, res) =&gt; {
  res.sendFile('index.html');
});

io.on('connection', function(socket) {
  socket.on('chat message', (text) =&gt; {
    console.log('Message: ' + text);

    // Get a reply from API.ai

    let apiaiReq = apiai.textRequest(text, {
      sessionId: APIAI_SESSION_ID
    });

    apiaiReq.on('response', (response) =&gt; {
      let aiText = response.result.fulfillment.speech;
      console.log('Bot reply: ' + aiText);
      socket.emit('bot reply', aiText);
    });

    apiaiReq.on('error', (error) =&gt; {
      console.log(error);
      socket.emit('bot reply', "we get error sir");
    });

    apiaiReq.end();

  });
});</textarea>

col 1                                                                                                                                                                                         | col 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

26

27

28

29

30

31

32

33

34

35

36

37

38

39

40

41

42

43

44

45

46

47

48

49

50 | const APIAI_TOKEN = "";

const APIAI_SESSION_ID = "";

const express = require('express');

const app = express();

app.use(express.static(__dirname + '/views')); // html

app.use(express.static(__dirname + '/public')); // js, css, images

const server = app.listen(process.env.PORT || 5000, () => {

  console.log('Express server listening on port %d in %s mode', server.address().port, app.settings.env);

});

const io = require('socket.io')(server);

io.on('connection', function(socket){

  console.log('a user connected');

});

const apiai = require('apiai')(APIAI_TOKEN);

// Web UI

app.get('/', (req, res) => {

  res.sendFile('index.html');

});

io.on('connection', function(socket) {

  socket.on('chat message', (text) => {

    console.log('Message: ' + text);

    // Get a reply from API.ai

    let apiaiReq = apiai.textRequest(text, {

      sessionId: APIAI_SESSION_ID

    });

    apiaiReq.on('response', (response) => {

      let aiText = response.result.fulfillment.speech;

      console.log('Bot reply: ' + aiText);

      socket.emit('bot reply', aiText);

    });

    apiaiReq.on('error', (error) => {

      console.log(error);

      socket.emit('bot reply', "we get error sir");

    });

    apiaiReq.end();

  });

});

<!-- [Format Time: 0.0273 seconds] -->

دقت داشته باشید که دو خط اول از این کد نیاز به وارد کردن اطلاعات ثبت نامی شما در سایت **[API.AI](http://api.ai/)**  هستش که البته گوگل ایران رو تحریم کرده.

اما نحوه ساخت حساب کاربری در این سایت راحته شما واردش بشید و یک حساب بسازید برای اطلاعات بیشتر به [**اینجا**](https://docs.api.ai/docs/get-started) مراجعه کنید.

پس از ساخت حساب بر روی گزینه “Small Talk” برید و اون دکمه تغییر وضعیت یا toggle اش رو روشن کنید . عکس زیر بیشتر توضیح میده.

![](http://nikitv.ir/wp-content/uploads/edd/2017/10/apiai-smalltalk-800w-opt-1.png)

بعد از این کار به قسمت “General Settings” برید (همون آیکون چرخ دنده) و در اونجا برای APIAI_TOKEN خودتون که توی کد برنامتون بود مقدار “client access token”

کپی کنید و در کد قرار بدید توی این قسمت (index.js)

<!-- Crayon Syntax Highlighter v_2.7.2_beta -->


JavaScript

<textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly="" style="tab-size: 4; font-size: 12px !important; line-height: 15px !important; z-index: 0; opacity: 0; overflow: hidden;">const APIAI_TOKEN = "";</textarea>

col 1 | col 2                  
----- | -----------------------
1     | const APIAI_TOKEN = "";

<!-- [Format Time: 0.0009 seconds] -->

در قسمت زیر هم یک نام دلخواه رو جای گذاری کنید (index.js)

<!-- Crayon Syntax Highlighter v_2.7.2_beta -->


JavaScript

<textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly="" style="tab-size: 4; font-size: 12px !important; line-height: 15px !important; z-index: 0; opacity: 0; overflow: hidden;">const APIAI_SESSION_ID = "";</textarea>

col 1 | col 2                       
----- | ----------------------------
1     | const APIAI_SESSION_ID = "";

<!-- [Format Time: 0.0007 seconds] -->

همین حالا بقیه کد ها رو به ترتیب میذارم

**index.html**

<!-- Crayon Syntax Highlighter v_2.7.2_beta -->


<textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly="" style="tab-size: 4; font-size: 12px !important; line-height: 15px !important; z-index: 0; opacity: 0; overflow: hidden;">&lt;html lang="en"&gt;

&lt;head&gt;
    &lt;meta name="description" content="Simple AI"&gt;
    &lt;meta name="author" content="niki"&gt;

    &lt;title&gt;Simple AI Demo with Web Speech API&lt;/title&gt;
    &lt;link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css"&gt;
    &lt;link rel="stylesheet" href="css/style.css"&gt;
&lt;/head&gt;

&lt;body&gt;
    &lt;section&gt;
        &lt;h1&gt;Simple AI Bot&lt;/h1&gt;
        &lt;h2&gt;with Web Speech API&lt;/h2&gt;

        &lt;button&gt;&lt;i class="fa fa-microphone"&gt;&lt;/i&gt;&lt;/button&gt;

        &lt;div&gt;
            &lt;p&gt;You said: &lt;em class="output-you"&gt;...&lt;/em&gt;&lt;/p&gt;
            &lt;p&gt;Bot replied: &lt;em class="output-bot"&gt;...&lt;/em&gt;&lt;/p&gt;
        &lt;/div&gt;
    &lt;/section&gt;
    &lt;script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/2.0.1/socket.io.js"&gt;&lt;/script&gt;
    &lt;script src="js/script.js"&gt;&lt;/script&gt;
&lt;/body&gt;

&lt;/html&gt;</textarea>

col 1                                                                                                 | col 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       
----------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

26

27

28 | <html lang="en">

<head>

    <meta name="description" content="Simple AI">

    <meta name="author" content="niki">

    <title>Simple AI Demo with Web Speech API</title>

    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">

    <link rel="stylesheet" href="css/style.css">

</head>

<body>

    <section>

        <h1>Simple AI Bot</h1>

        <h2>with Web Speech API</h2>

        <button><i class="fa fa-microphone"></i></button>

        <div>

            <p>You said: <em class="output-you">...</em></p>

            <p>Bot replied: <em class="output-bot">...</em></p>

        </div>

    </section>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/2.0.1/socket.io.js"></script>

    <script src="js/script.js"></script>

</body>

</html>

<!-- [Format Time: 0.0056 seconds] -->

**script.js**

<!-- Crayon Syntax Highlighter v_2.7.2_beta -->


JavaScript

<textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly="" style="tab-size: 4; font-size: 12px !important; line-height: 15px !important; z-index: 0; opacity: 0; overflow: hidden;">// 'use strict';

const socket = io();

const outputYou = document.querySelector('.output-you');
const outputBot = document.querySelector('.output-bot');

const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
const recognition = new SpeechRecognition();

recognition.lang = 'en-US';
recognition.interimResults = false;
recognition.maxAlternatives = 1;

document.querySelector('button').addEventListener('click', () =&gt; {
  recognition.start();
});

recognition.addEventListener('speechstart', () =&gt; {
  console.log('Speech has been detected.');
});

recognition.addEventListener('result', (e) =&gt; {
  console.log('Result has been detected.');

  let last = e.results.length - 1;
  let text = e.results[last][0].transcript;

  outputYou.textContent = text;
  console.log('Confidence: ' + e.results[0][0].confidence);

  socket.emit('chat message', text);
});

recognition.addEventListener('speechend', () =&gt; {
  recognition.stop();
});

recognition.addEventListener('error', (e) =&gt; {
  outputBot.textContent = 'Error: ' + e.error;
});

function synthVoice(text) {
  const synth = window.speechSynthesis;
  const utterance = new SpeechSynthesisUtterance();
  utterance.text = text;
  synth.speak(utterance);
}

socket.on('bot reply', function(replyText) {
  synthVoice(replyText);

  if(replyText == '') replyText = '(No answer...)';
  outputBot.textContent = replyText;
});</textarea>

col 1                                                                                                                                                                                                             | col 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

26

27

28

29

30

31

32

33

34

35

36

37

38

39

40

41

42

43

44

45

46

47

48

49

50

51

52

53

54

55 | // 'use strict';

const socket = io();

const outputYou = document.querySelector('.output-you');

const outputBot = document.querySelector('.output-bot');

const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;

const recognition = new SpeechRecognition();

recognition.lang = 'en-US';

recognition.interimResults = false;

recognition.maxAlternatives = 1;

document.querySelector('button').addEventListener('click', () => {

  recognition.start();

});

recognition.addEventListener('speechstart', () => {

  console.log('Speech has been detected.');

});

recognition.addEventListener('result', (e) => {

  console.log('Result has been detected.');

  let last = e.results.length - 1;

  let text = e.results[last][0].transcript;

  outputYou.textContent = text;

  console.log('Confidence: ' + e.results[0][0].confidence);

  socket.emit('chat message', text);

});

recognition.addEventListener('speechend', () => {

  recognition.stop();

});

recognition.addEventListener('error', (e) => {

  outputBot.textContent = 'Error: ' + e.error;

});

function synthVoice(text) {

  const synth = window.speechSynthesis;

  const utterance = new SpeechSynthesisUtterance();

  utterance.text = text;

  synth.speak(utterance);

}

socket.on('bot reply', function(replyText) {

  synthVoice(replyText);

  if(replyText == '') replyText = '(No answer...)';

  outputBot.textContent = replyText;

});

<!-- [Format Time: 0.0153 seconds] -->

**style.css**

<!-- Crayon Syntax Highlighter v_2.7.2_beta -->


CSS

<textarea wrap="soft" class="crayon-plain print-no" data-settings="dblclick" readonly="" style="tab-size: 4; font-size: 12px !important; line-height: 15px !important; z-index: 0; opacity: 0; overflow: hidden;">* { -moz-box-sizing: border-box; -webkit-box-sizing: border-box; box-sizing: border-box; }

html {
  height: 100%;
}
body {
  padding: 0;
  margin: 0;
  font-family: "HelveticaNeueLight", "HelveticaNeue-Light", "Helvetica Neue Light", "HelveticaNeue", "Helvetica Neue", 'TeXGyreHerosRegular', "Helvetica", "Tahoma", "Geneva", "Arial", sans-serif;
  font-weight: 300;
  /* background: #f7f7f7 url(../images/geometry.png) 0 0 no-repeat; */
  background-size: cover;
  color: #333;
}
footer {
  font-size: 0.75em;
  color: silver;
  position: fixed;
  bottom: 1em;
  left: 1em;
}
footer a {
  color: #f80;
}
header {
  margin: 2em 0;
  text-align: center;
}
h1, h2, h3 {
  margin: 0;
  text-rendering: optimizeLegibility;
  text-align: center;
}
h1 {
  font-weight: 400;
  font-size: 3em;
}
h2 {
  font-weight: 300;
  margin-top: 0.5em;
}
section {
  margin: 1.5em auto;
  width: 400px;
}
button {
  display: block;
  -webkit-appearance: none;
  -moz-appearance: none;
  width: 200px;
  height: 200px;
  border: 0;
  border-radius: 50%;
  padding: .7em 1em;
  margin: 4em auto 3em;
  text-align: center;
  color: #fff;
  background: linear-gradient(180deg, #39C2C9 0%, #3FC8C9 80%, #3FC8C9 100%);
  box-shadow: 2px 5px 30px rgba(63, 200, 201, .4);

  will-change: transform, filter;
  transition: all 0.3s ease-out;
}
button .fa {
  font-size: 130px;
  line-height: 200px;
  margin: 0;
  text-shadow: 1px 2px 2px #2a8b90;
}
button:hover {
  transform: scale(.92);
}
button:active {
  filter: brightness(.8);
}
button:focus {
  outline: 0;
}

.fa {
  display: inline-block;
  font: normal normal normal 14px/1 FontAwesome;
  font-size: 1.3em;
  text-rendering: auto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin-left: 10px;
}
.fa-github::before {
    content: "\f09b";
}</textarea>

col 1                                                                                                                                                                                                                                                                                                                                                             | col 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

26

27

28

29

30

31

32

33

34

35

36

37

38

39

40

41

42

43

44

45

46

47

48

49

50

51

52

53

54

55

56

57

58

59

60

61

62

63

64

65

66

67

68

69

70

71

72

73

74

75

76

77

78

79

80

81

82

83

84

85

86

87

88

89

90

91 | \* { -moz-box-sizing: border-box; -webkit-box-sizing: border-box; box-sizing: border-box; }

html {

  height: 100%;

}

body {

  padding: 0;

  margin: 0;

  font-family: "HelveticaNeueLight", "HelveticaNeue-Light", "Helvetica Neue Light", "HelveticaNeue", "Helvetica Neue", 'TeXGyreHerosRegular', "Helvetica", "Tahoma", "Geneva", "Arial", sans-serif;

  font-weight: 300;

  /* background: #f7f7f7 url(../images/geometry.png) 0 0 no-repeat; */

  background-size: cover;

  color: \#333;

}

footer {

  font-size: 0.75em;

  color: silver;

  position: fixed;

  bottom: 1em;

  left: 1em;

}

footer a {

  color: \#f80;

}

header {

  margin: 2em 0;

  text-align: center;

}

h1, h2, h3 {

  margin: 0;

  text-rendering: optimizeLegibility;

  text-align: center;

}

h1 {

  font-weight: 400;

  font-size: 3em;

}

h2 {

  font-weight: 300;

  margin-top: 0.5em;

}

section {

  margin: 1.5em auto;

  width: 400px;

}

button {

  display: block;

  -webkit-appearance: none;

  -moz-appearance: none;

  width: 200px;

  height: 200px;

  border: 0;

  border-radius: 50%;

  padding: .7em 1em;

  margin: 4em auto 3em;

  text-align: center;

  color: \#fff;

  background: linear-gradient(180deg, \#39C2C9 0%, \#3FC8C9 80%, \#3FC8C9 100%);

  box-shadow: 2px 5px 30px rgba(63, 200, 201, .4);

  will-change: transform, filter;

  transition: all 0.3s ease-out;

}

button .fa {

  font-size: 130px;

  line-height: 200px;

  margin: 0;

  text-shadow: 1px 2px 2px \#2a8b90;

}

button:hover {

  transform: scale(.92);

}

button:active {

  filter: brightness(.8);

}

button:focus {

  outline: 0;

}

.fa {

  display: inline-block;

  font: normal normal normal 14px/1 FontAwesome;

  font-size: 1.3em;

  text-rendering: auto;

  -webkit-font-smoothing: antialiased;

  -moz-osx-font-smoothing: grayscale;

  margin-left: 10px;

}

.fa-github::before {

    content: "\f09b";

}

<!-- [Format Time: 0.0165 seconds] -->

و خروجی برنامه پس از اجرا به این صورت خواهد بود.

![](http://nikitv.ir/wp-content/uploads/edd/2017/10/webspeech-api-demo.gif)

در پایان می تونید با زدن کلید F12 در مرورگر خودتون خطاهای تولید شده رو رد گیری کنید.

بازم به این نکته تاکیید می کنم که سیستم هوش مصنوعی گوگل برای ایرانی ها مورد تحریم واقع شده.

موفق باشید.