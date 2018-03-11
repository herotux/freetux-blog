---
layout: post
title: اسکریپت پایتون پرداخت آنلاین از طریق زرین پال
category: نمونه کار
postimg: /images/post/zarinpal-easypay.png
tags: [zarinpal,python,python3,bottle,mysql,apache,nginx,زرین‌پال,زرین پال,پایتون,دیتابیس,پرداخت,پرداخت آنلاین]
---
دیروز یک اسکریپت برای پرداخت آنلاین از طریق زرین پال با استفاده از پایتون ۳ و فریم ورک [Bottle](https://bottlepy.org) داخل [گیت‌هاب](https://github.com/rasooll/zarinpal-easypay) منتشر کردم و امروز هم تصمیم گرفتم داخل این وبلاگ هم آن را قرار دهم ، برای اطلاعات بیشتر و آموزش نصب بریم به ادامه مطلب...

#### چرا پایتون ؟ چرا از PHP استفاده نکرده‌ام؟
به نظر من پروژه‌های متن باز در اولین قدم برای استفاده‌ی شخصی ساخته شده‌اند و پس از کامل شدن در اختیار عموم قرار می‌گیرد و اما در مورد این پروژه من روی سرورم PHP را نصب نداشتم و نیازی هم نبود که فقط برای یک اسکریپت آن را نصب کرد ، وقتی پایتون هست PHP چرا ؟؟!!! 😬😬

### پیشنیازها:

- پایتون ۳
	- نصب ماژول‌های پایتون از داخل فایل requirement.txt
- وب سرور ( آپاچی یا و... ) *اختیاری
- بانک اطلاعاتی MySQL

### اسکرین شات
![پرداخت آنلاین از طریق زرین پال]({{ site.url }}{{ page.postimg }} "zarinpal easypay")

### آموزش نصب:
ابتدا فایل `config.py` را ویرایش کرده و اطلاعات مربوط به بانک اطلاعاتی و مرچنت کد دریافتی از زرین پال را وارد نمایید سپس آدرس مربوط به نمایش سایت را نیز باید ویرایش نمایید.

حال باید اسکریپت را با دستور زیر اجرا نمایید.

```bash
python3 main_application.py
```

اکنون اسکریپت از طریق آدرس زیر در دسترس است:

```bash
http://localhost:8080
```

* درصورتی که نمی‌خواهید از وب سرور استفاده کنید پورت 8080 را می‌توانید با ویرایش آخرین خط فایل main_application.py به پورت مورد نظر تغییر دهید.

در اولین اجرا لازم است شاخه‌ی /install را اجرا نمایید تا جداول مربوط در بانک اطلاعاتی ایجاد گردد ، به عنوان مثال:

```bash
http://localhost:8080/install
```

### استفاده از وب سرور آپاچی
در آپاچی می‌توانیم به صورت Reverse-Proxy نیز از این اسکریپت استفاده کنیم.

برای اینکار باید ماژول های زیر فعال باشد:

```bash
proxy, proxy_ajp, proxy_http, rewrite, deflate, headers, proxy_balancer, proxy_connect, proxy_html
```

یک سایت جدید ایجاد می‌کنیم:

```bash
sudo nano /etc/apache2/sites-enabled/zarinpal-easypay.conf
```

سپس تنظیمات زیر را در آن وارد می‌کنیم:
    
```html
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://0.0.0.0:8080/
    ProxyPassReverse / http://0.0.0.0:8080/
    ServerName YourDomain.com
    ServerAlias www.YourDomain.com
</VirtualHost>
```
سپس با کلید های Ctrl+X و پس از آن Y فایل را ذخیره می‌کنیم.

```bash
sudo systemctl reload apache2
```

حال با استفاده از آدرس زیر به اسکریپت دسترسی داریم:

```bash
http://YourDomain.com
```

<a href='https://github.com/rasooll/zarinpal-easypay' target='_blank'><span class='btn btn-secondary'>گیت هاب پروژه</span></a>
