---
title: ۱۵+ اصول ساده در JavaScript
image: images/post_28_jsHint/js-600x387.png
description: نوشتن چند مورد از اصول تسهیل کننده در برنامه نویسی js. در نظر سنجی ای که در سال 2017 توسط StackOverflow انجام شد.مشخض شد که js یکی از رایج ترین و جذاب ترین زبان ها برای تولید صفحات وب تعاملی می تونه باشه.این زبان توسط تعداد زیادی از مرورگر ها پشتیبانی میشه همچنین استفاده از جاوا اسکریپت در محصولات معرفی شده توسط شرکت توسعه Magento طراحی وب سایت های تجارت الکترونیک رو افزایش داده است.یادگیری js برای توسعه صفحات پویای وب خیلی آسونه اما استفاده از تکنیک ها و ترفندها ی js کارشما رو راحت تر میکنه و از طرفی توی زمان هم صرفه جویی میکنید.این هک های ساده، کدهای بهینه شده ای هستند که با استفاده از منطق برنامه نویسی هوشمندانه ساخته شده اند.
date: 2019-05-05T13:42:33+04:30
author: mam_niki
tags:
- js
categories:
- برنامه نویسی
- نرم افزار
---

با سلام

در نظر سنجی ای که در سال 2017 توسط [StackOverflow](https://stackoverflow.com/) انجام شد.مشخض شد که js یکی از رایج ترین و جذاب ترین زبان ها برای تولید صفحات وب تعاملی می تونه باشه.

این زبان توسط تعداد زیادی از مرورگر ها پشتیبانی میشه همچنین استفاده از جاوا اسکریپت در محصولات معرفی شده توسط شرکت توسعه Magento طراحی وب سایت های تجارت الکترونیک رو افزایش داده است.

یادگیری js برای توسعه صفحات پویای وب خیلی آسونه اما استفاده از تکنیک ها و ترفندها ی js کارشما رو راحت تر میکنه و از طرفی توی زمان هم صرفه جویی میکنید.

این هک های ساده، کدهای بهینه شده ای هستند که با استفاده از منطق برنامه نویسی هوشمندانه ساخته شده اند.

از استفاده شون لذت ببرید.

### **1- استفاده از Async/ await همراه با** **Array Destructing** **:**

**این روش شگفت انگیز به شما نشون میده که چطور میشه از Array Destructing برای اجرای توابع زیاد غیر همزمان (Async) و به صورت پشت سر هم استفاده کنید.به مثال زیر دقت کنید:**

```js
const [user, account] = await Promise.all([

 fetch('/user'),

 fetch('/account')

 ])
```

### **2- استفاده از عملگر !! برای تبدیل به boolean :**

**این علامت ‘!!’ در js تحت عنوان نفی دوگانه ( ** double negation ** ) شناخته میشه.برناlه نویس ها از اون استفاده میکنند برای اینکه مشخص بشه آیا متغیر مورد نظر وجود داره و مقدار معتبری هم داره یا نه.**

**اگر برای یک متغیر از این علامت استفاده کنیم.مثلا ** variable!! ** این متغیر در صورتی false برمیگردونه که مقدار متغیر یکی از ** 0 , “” , null , NAN ویا undefined **باشه .در غیر این صورت true برمیگردونه.**

**به مثال زیر دقت کنید:**

```js
function Account(cash) {

this.cash =cash

this.hasMoney = !!cash;

}

var account   =  new Account(100.50);

console.log(account.cash); // 100.50

console.log(account.hasMoney); // true

var emptyAccount = new Account(0);

console.log(emptyAccount.cash); // 0

console.log(emptyaccount.hasMoney); // fasle
```

### 3- **جابجایی متغیر ها (Swap variables) :**

**با استفاده از همین ** array destruction ** میشه عمل جابجایی مقدار بین دومتغیر رو به سادگی انجام داد:**

```js
let a = 'world', b = 'hello'

 [a, b] = [b, a]

 console.log(a) // -> hello

 console.log(b) // -> world
```

### **4- استفاده از عملگر ‘+’ برای تبدیل رشته به اعداد :**

**این روش زمانی استفاده میشه که عدد مورد نظر ما داخل یک رشته باشه.مثلا “5” .اگه داخل رشته مورد نظر ما چیزی غیر از اعداد باشه NaN برمیگردونه که یعنی ( ** not a number ** )**

```js
function toNumber( strNumber) {

return +strNumber;

}

console.log(toNumber( “1234”)); // 1234

console.log(toNumber(“ABC”)); // NaN
```

همینطور میشه از این روش برای ‘تاریخ’ در js هم استفاده کرد:

```js
console.log(+new Date( )) // 1461288164385
```

### **5- کوتاه کننده دستورات شرطی :**

**استفاده از کوتاه نویسی میتونه سرعت برنامه نویسی رو زیاد کنه و توی زمان هم صرفه جویی کنید.**

**مثلا هرجا کد زیر رو دید:**

```js
If(connected){

Login()

}
```

**میتونید متغیر و تابع رو در یک خط کوتاه کنید و بنویسید :**

```js
connected && login();
```

**یا یک مثال دیگه ازش:**

```js
user && user.login();
```

### **6- هک ساده در خطا یابی :**

**اگه از ()console.log برای خطا یابی استفاده میکنید ، داریم :**

```js
console.log({ a, b, c })

// outputs this nice object:

 // {

 //    a: 5,

 //    b: 6,

 //    c: 7

 // }
```

### **7- چطور تشخیص بدیم متد یا function یا کلا properties در یک شی ( object ) وجود داره :**

**مثال زیر میتونه جواب خیلی از سوال هایی از این دست باشه.**

**فرض کنید ما میخواهیم از تابعی استفاده کنیم که در مرورگر های IE 6 به پایین وجود نداره و همینطور انتظار داریم که کدمون توی این دست از مرورگر ها به مشکل نخوره به مثال زیر دقت کنید :**

```js
if (‘querySelector’ in document) {

document.querySelector(“\#id”);

} else {

Document.getElementById(“id”);

}
```

در مثال بالا تابع ‘querySelector’ در IE 6 به قبل وجود نداره.

### **8- استفاده از تک خطی ها :**

**این روش برای فشرده سازی عملیات آرایه ای در یک خط استفاده میشه.**

```js
// Find max value

 const max = (arr) =&gt; Math.max(...arr);

 max([123, 321, 32]) // outputs: 321

// Sum array

 const sum = (arr) =&gt; arr.reduce((a, b) =&gt; (a + b), 0)

 sum([1, 2, 3, 4]) // output: 10
```

### **9- پیوند بین آرایه ها ( Array concatenation) :**

**توی این روش به ‘…’ که توی کد استفاده شده میگویند “عملگر گسترش” یا (** spread operator **)**

```js
one = ['a', 'b', 'c']

 const two = ['d', 'e', 'f']

 const three = ['g', 'h', 'i']

// Old way #1

 const result = one.concat(two, three)

// Old way #2

 const result = [].concat(one, two, three)

// New

 const result = [...one, ...two, ...three]
```

### **10- استفاده از micro-optimization در کد با استفاده از عملگر ‘~~’ :**

**بجای استفاده ار** **math.floor میتوان از ‘~~’ استفاده نمود :**

```js
~~3.9 === Math.floor(3); //true, 3
```

### **11- گرفتن آخرین عناصر یک آرایه :**

```js
var array=  [1, 2, 3, 4, 5, 6];

console.log(array. Slice(-1)); // [6]

console.log(array. Slice(-2)); // [5, 6]

console.log(array. Slice(-3)); // [4, 5, 6]
```

**12- استفاده از کلون کردن (cloning) :**

**ساده ترین روش برای کلون (کپی) کردن یک آرایه و ابجکت ، استفاده ‘…’ یه همان عملگر گسترش است.**

```js
const obj = { ...oldObj }

const arr = [ ...oldArr ]
```

### **13- استفاده از switch به جای if/else :**

**علتش اینه که فقط یک بار عمل ارزش یابی یا ** ( evaluate ) ** انجام میشه.مگر اینکه در if/else دقت لازم رو انجام داده و از return هم استفاده کنید.**

### **14- استفاده از گزینه Replace all با استفاده از Regex ها :**

```js
var string = “john john”;

console.log(string.replace(/hn/, “ana”)); // “joana john”

console.log(string.replace(/hn/g, “ana”)); // “joana joana”
```

### **15- برهم زدن عانصر آرایه (Shuffle array’s elements) :**

**خبر خوب اینه که میشه بدون استفاده از کتابخانه های خارجی عناصر یک آریه رو مخلوط کرد یا اصطلاحا بهم زد.**

```js
var list = [1, 2, 3];

console.log(list.sort(function() {

return Math.randome () – 0.5

})); // [2, 1, 3]
```

### **16- تغییر اندازه یک ارایه با استفاده از ‘length’ :**

```js
var array = [1, 2, 3, 4, 5, 6];

console.log(array.length); // 6 

array.length = 3; 

console.log(array.length); // 3  

console.log(array); // [1,2,3]
```

### **17- ادغام آرایه ای :**

**اگه از ()array.concat استفاده کنید میشه دوتا آرایه رو merge کرد اما این روش هزینه مصرفی حافظه اش بالاست و وقتی خودشو نشون میده که با داده های زیادی سروکار داریم.مثلا :**

```js
var array1 = [1, 2, 3];

var array2 = [4, 5, 6];

console.log(array1.concat(array2)); // [1,2,3,4,5,6];
```

**اما روش بهینه تر، کد زیر میتونه باشه:**

```js
var array1 = [1, 2, 3];

var array2 = [4, 5, 6];

console.log( array1.push.apply(array1, array2) ); // [1,2,3,4,5,6];
```

[لینک](https://corpocrat.com/2018/04/25/20-simple-wow-hacks-in-javascript/) مقاله اصلی.

موفق باشید.