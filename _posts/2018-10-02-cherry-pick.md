---
layout: post
title:  توضیح cherry-pick یک commit با git
category: 
- chocolate
- learn
tags: 
- chocolate 
- learn
---


cherry-pick در git به معنای انتخاب یک commit از یک branch و اعمال آن بر سایر branch ها است

خب یه مثال بریم:


اول به branch مقصد برید مثلا master

‍‍‍‍`git checkout master`

بعد دستور cherry-pick (به جای commit-hash هش مربوط به commit مورد نظر رو وارد کنید)رو اجرا کنید:

‍‍‍`git cherry-pick <commit-hash>`

commit شما بر branch که اینجا master هست اعمال شد.

در صورتی که از یک public branch عمل cherry-pick را انجام میدهید باید از این دستور استفاده کنید:


‍‍‍`git cherry-pick -x <commit-hash>`

این -x درواقع یک کامیت استاندارد شده ایجاد میکند که شما و سایر همکارانتان همچنان بتوانید تغییرات را دنبال کنید (بدونید چی به چیه)و در آینده از conflict جلوگیری میشود.

اگر  کامیتی که cherry-pick کردید یادداشت ضمیمه داشته همراه cherry-pick به branch مقصد نمیآید و میبایست یک کپی از آن را با دستور زیر روی branch اعمال کنید:

`git notes copy <from> <to>‍`