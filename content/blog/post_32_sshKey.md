---
title: دسترسی به سرور شخصی بدون داشتن رمز عبور و تنها با کلید عمومی
image: images/post_32_sshkey/download1.jpg
description: اگه شما هم مثل من دسترسی به سرور های شخصی و شرکت براتون معضل هست و هر بار باید پسورد های هر سرور رو یادداشت و وارد کنید این آموزش بدردتون خواهد خورد.
date: 2019-08-30T10:13:25+04:30
author: mam_niki
tags:
- server
- linux
- network
categories:
- نرم افزار
---

توی این آموزش نحوه تولید کلید برای ssh را توضیح میدیم و اینکه چطور این کلید تولید شده را روی سرور شخصی قرار بدیم تا دیگه نیاز نباشه هربار موقع login پسورد رو وارد کنیم.

اگه شما هم مثل من دسترسی به سرور های شخصی و شرکت براتون معضل هست و هر بار باید پسورد های هر سرور رو یادداشت و وارد کنید این آموزش بدردتون خواهد خورد.

روال کار به این صورت هست که ما یک کلید عمومی روی کامپیوتر شخصی مون درست میکنیم و اون کلید رو روی سرور کپی میکنیم (دفعه اول با پسورد) بعد از اون دیگه نیازی به وارد کردن پسورد از سمت ما نیست و خود کلید کپی شده وظیفه اعتبار سنجی رو برای ما انجام میده.

_نکته:به روش استفاده از کلید جهت متصل شدن به ssh رو [public key authentication](https://www.ssh.com/ssh/public-key-authentication) میگیم._

#### _**مرحله اول: تولید کلید در کامپیوتر شخصی**_

با استفاده از ابزار [OpenSSH](https://www.ssh.com/ssh/openssh/) ما با فرمان [ssh-keygen](https://www.ssh.com/ssh/keygen/) ، ابتدا کلید خود را می سازیم.

فقط کافیه فرمان [ssh-keygen](https://www.ssh.com/ssh/keygen/) رو توی ترمینال بنویسیم و به یک سری سوال پاسخ بدیم.

مثال زیر نمونه ای از اجرای این فرمان است.در اینجا نام کلید ما mykey هست.
```sh
# ssh-keygen

Generating public/private rsa key pair.

Enter file in which to save the key (/home/ylo/.ssh/id_rsa): mykey

Enter passphrase (empty for no passphrase): 

Enter same passphrase again: 

Your identification has been saved in mykey.

Your public key has been saved in mykey.pub.

The key fingerprint is:

SHA256:GKW7yzA1J1qkr1Cr9MhUwAbHbF2NrIPEgZXeOUOz3Us ylo@klar

The key\'s randomart image is:

+---[RSA 2048]----+

|.\*++ o.o.        |

|.+B + oo.        |

| +++ \*+.         |

| .o.Oo.+E        |

|    ++B.S.       |

|   o * =.        |

|  + = o          |

| + = = .         |

|  + o o          |

+----[SHA256]-----+

#
```

فرمان بالا برای ما یک جفت کلید میسازه (public key , private key) این کلیدها معمولا در مسیر زیر ذخیره میشوند.

```sh
~/.ssh
```

#### _**مرحله دوم: انقال کلید عمومی به سرور شخصی**_

پس از ساختن کلید ، با فرمان ssh-copy-id لازم است تنها کلید عمومی را روی سرور قرار بدیم.برای این منظور فرمان زیر را مینویسیم .در اینجا نام کلید ما mykey هست.

```sh
ssh-copy-id -i ~/.ssh/mykey user@host
```

حالا اگه توی ترمینال فقط فرمان زیر را بنویسیم ، به سرور خود بدون وارد کردن پسورد وارد خواهیم شد.

```sh
ssh -i ~/.ssh/mykey user@host
```

نکته:به جای user نام کاربری سرور و بجای host ادرس سرور را وارد میکنیم.

_منبع: https://www.ssh.com/ssh/copy-id_