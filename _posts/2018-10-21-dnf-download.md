---
layout: post
title: DNF download Plugin
category: 
- shell
- learn
- linux
tags: 
- linux 
- learn
- chocolate
---


<div align='center' style='font-size: 50px;'>
<i class="fa fa-terminal " aria-hidden="true" style='color: #333;'></i>
</div>
تا حالا پیش اومده بخواید برای یک سیستم گنولینوکسی با توزیع محبوب فدورا بسته ای رو نصب کنید اما امکان برقراری ارتباط اینترنت روی اون سیستم فراهم نباشه؟

راه حل اینکه از یک سیستم با توزیع فدورا پکیج های مربوط رو دانلود و بصورت آفلاین روی سیستم مورد نظر نصبشون کنید:

ولی چطوری؟

با dnf download


DNF download Plugin

دانلود باینری یا بسته های منبع
ابزاری برای دانلود rpm ها از مخازن اما نصب نمیکند.

درصورتی که نیاز به دانلود همه پیش نیازها است از سوئیچ --resolve استفاده میکنیم


شکل کلی دستور:

```
dnf download [options] <pkg-spec>...
```

## مثال ها :

```
dnf download dnf
    #Download the latest dnf package to the current directory.
dnf download --url dnf
    #Just print the remote location url where the dnf rpm can be downloaded from.
dnf download --url --urlprotocols=https --urlprotocols=rsync dnf
    #Same as above, but limit urls to https or rsync urls.
dnf download dnf --destdir /tmp/dnl
    #Download the latest dnf package to the /tmp/dnl directory (the directory must exist).
dnf download dnf --source
    #Download the latest dnf source package to the current directory.
dnf download rpm --debuginfo
    #Download the latest rpm-debuginfo package to the current directory.
dnf download btanks --resolve
    #Download the latest btanks package and the uninstalled dependencies to the current directory.
```



