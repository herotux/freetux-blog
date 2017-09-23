---
layout: post
title: ساخت ربات تلگرام ساده با پایتون ، از آماده سازی تا اجرا
category: آموزشی
postimg: /images/post/pythonanywhere.png
tags: [python,telegram,bot,robot,پایتون,تلگرام,ربات,بات تلگرام,هاست رایگان پایتون,میزبانی پایتون,ربات پایتون]
---
امروز برای اولین بار یک ربات بسیار ساده برای تلگرام با پایتون آماده کردم ، این ربات فقط این امکان رو داره که برای دستوراتی که شما ارسال می‌کنید یکسری پاسخ ثابت ارسال کند.
یکی از مشکلاتی که بعد از آماده سازی این ربات داشتم این بود که کجا میزبانی بشه ؟؟؟

روی سرورمجازی ؟ این مورد با توجه به امکانات ساده ربات اصلا توجیه اقتصادی نداشت ، با یک جستجوی کوتاه به سایت [PythonAnyWhere](https://www.pythonanywhere.com){:target="_blank"} رسیدم که پلن های مختلفی داشت اما یک سرویس رایگان با امکانات خیلی خیلی محدود هم داشت که برای این ربات من کاملا کافی بود .

خوب دیگه بریم شروع کنیم...  اول از همه باید عضو این سایت شد.  [پیوند](https://www.pythonanywhere.com/registration/register/beginner/){:target="_blank"}

![Dashboard](/images/post/pythonanywhere.png)

راستی تا یادم نرفته باید بگم در این آموزش ما باید از پایتون ۲ استفاده کنیم.

حالا باید از بخش Start new console روی گزینه‌ی Bash کلیک کنیدتا یک کنسول جدید برایمان باز شود.

سپس در کنسول باز شده دستور زیر را وارد می‌کنیم:

```bash
$ virtualenv --python=python2.7 myappname
```

این دستور باعث میشه یک محیط مجازی پایتون ۲.۷ برایمان در پوشه‌ی myappname ایجاد شود.

حالا با دستور زیر وارد این محیط مجازی میشویم

```bash
$ source myappname/bin/activate
```

حالا باید کتابخانه های مربوط به تلگرام را نصب کنیم با دستور زیر:

```bash
$ pip install python-telegram-bot
```

اگر همه چیز درست پیش رفته باشه مثل تصویر زیر داریم :

![Bash-Console](/images/post/bash-console.png)

حالا باید از تب Files برنامه‌ی ربات خودمون رو آپلود کنیم.

بهترین منبع برای یاد گیری نحوه‌ی ساختن ربات داکیومنت های خود کتابخانه هست [پیوند](https://python-telegram-bot.readthedocs.io/en/stable/){:target="_blank"}

یکسری کد آماده هم من توی اینترنت پیدا کردم و در گیت هاب آپلود کردم [پیوند](https://github.com/rasoolsf/python-telegram-bot-sample){:target="_blank"}

سورس کدی هم که امروز خودم نوشتم و فقط از ۳ تا دستور پشتیبانی می‌کنه:

```python
#!/usr/bin/env python2
# -*- coding: utf-8 -*-

from telegram.ext import Updater, CommandHandler
import logging
updater = Updater('TOKEN')
logging.basicConfig(filename='mylogs.txt', format='%(asctime)s - %(levelname)s - %(message)s',level=logging.INFO)

#Robot Function
def start_method(bot, update):
    bot.sendMessage(update.message.chat_id, "به ربات تلگرام انجمن مهندسی برق یادگار خوش امدید.")
def site_link(bot, update):
    bot.sendMessage(update.message.chat_id, "🔗 آدرس سایت انجمن علمی مهندسی برق یادگار:\nhttp://elecom-rey.ir")
def gp_link(bot, update):
    bot.sendMessage(update.message.chat_id, "👥 لینک گروه انجمن علمی مهندسی برق یادگار:\nhttps://t.me/joinchat/AAAAAELj1_CHowd_zNaphA")
def taghvim_amoozeshi(bot, update):
    payam="""
    🗓 تقویم آموزشی نیم سال اول ۹۶-۹۷:\n
    ⬅ نام‌نویسی:\nیک‌شنبه ۱۲/۶/۹۶ تا چهارشنبه ۲۲/۶/۹۶\n
    ⬅ شروع کلاس‌ها:\nشنبه ۲۵/۶/۹۶\n
    ⬅ ثبت نام با تاخیر:\nشنبه ۲۵/۶/۹۶ تا پنجشنبه ۱۳/۷/۹۶\n
    ⬅ حذف و اضافه:\nدوشنبه ۱۰/۷/۹۶ تا پنج‌شنبه ۱۳/۷/۹۶ \n
    ⬅ پایان‌کلاس‌ها:\nپنج‌شنبه ۱۴/۱۰/۹۶\n
    ⬅ امتحانات پایان ترم:\nشنبه ۱۶/۱۰/۹۶ تا پنج‌شنبه ۲۸/۱۰/۹۶\n\n@elecomrey_bot
    """
    bot.sendMessage(update.message.chat_id, payam)

# Robot Handler
updater.dispatcher.add_handler(CommandHandler('start', start_method))
updater.dispatcher.add_handler(CommandHandler('site', site_link))
updater.dispatcher.add_handler(CommandHandler('group', gp_link))
updater.dispatcher.add_handler(CommandHandler('taghvim', taghvim_amoozeshi))

updater.start_polling()
# for exit
updater.idle()
```

در تمامی نمونه کدها شما باید به جای عبارت TOKEN مقدار دریافتی از BotFather را وارد نمایید.

حالا نوبت به اجرا کردن کد میرسه

```bash
$ python2 myappname/telegrom_bot.py
```

در اینجا اسم فایلی که آپلود کردیم `telegram_bot.py` است.

حالا میتونیم پنجره مربوط به کنسول را ببندیم و برنامه اجرا خواهد ماند.

امیدوارم این مطلب مفید بوده باشه.