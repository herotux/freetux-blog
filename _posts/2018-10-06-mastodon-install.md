---
layout: post
title: نصب Mastodon در اوبونتو
category: 
- mastodon
- learn
tags: 
- mastodon 
- learn
---


<div align='center' style='font-size: 50px;'>
<i class="fab fa-mastodon" aria-hidden="true" style='color: #333;'></i>
</div>
Mastodon یک پلت فرم شبکه اجتماعی رایگان و رایگان است که بسیار شبیه توییتر است. هر کسی می تواند Mastodon را اجرا کند و در یک شبکه اجتماعی یکپارچه شرکت کند. این در روبی و جاوا اسکریپت نوشته شده است.

در این آموزش ما Mastodon را در سرور اوبونتو 16.04 نصب خواهیم کرد. 

## الزامات

  - یک سرور اوبونتو
  -  یک کاربر معمولی با تنظیمات sudo در سرور شما. 

## نصب وابستگی های مورد نیاز

اولا توصیه می شود سرور خود را با آخرین نسخه به روز کنید. شما می توانید این کار را با اجرای دستور زیر انجام دهید:

```
sudo apt-get update -y sudo apt-get upgrade -y
```

هنگامی که سیستم شما به روز می شود، سیستم خود را مجددا راه اندازی کنید و با کاربر sudo وارد شوید.

بعد، شما باید برخی از وابستگی های مورد نیاز را در سرور خود نصب کنید. شما می توانید همه آنها را با اجرای دستور زیر نصب کنید:


```
sudo curl -sL https://deb.nodesource.com/setup_4.x | sudo bash - sudo curl -sL https://deb.nodesource.com/setup_4.x | sudo bash - sudo apt-get install imagemagick ffmpeg libpq-dev libxml2-dev libxslt1-dev nodejs -y sudo npm install -g yarn

```

بعد، شما همچنین باید سرور redis را در سیستم خود نصب کنید. شما می توانید آن را فقط با اجرای دستور زیر نصب کنید:


```
sudo apt-get install redis-server redis-tools -y
```

بعد Postgresql را نصب کنید.

## نصب و پیکربندی PostgreSQL

Mastodon از پایگاه داده PostgreSQL استفاده می کند، بنابراین شما باید PostgreSQL و daemon را نصب کنید. شما می توانید آنها را با دستور زیر نصب کنید:


```
sudo apt-get install postgresql pidentd -y
```


پس از اتمام نصب، daemon id را با دستور زیر شروع کنید:


```
sudo systemctl start pidentd
```

بعد، برای ورود به کاربر postgre وارد شوید و یک کاربر برای Mastodon ایجاد کنید:

```
sudo su - postgres psql CREATE USER mastodon CREATEDB; exit
```

بعد، شما نیاز دارید که کاربران بتوانند بدون رمز عبور وارد شوند. شما می توانید این کار را با اجرای دستور زیر انجام دهید:

```
sudo sed -i '/^local.*postgres.*peer$/a host all all 127.0.0.1/32 ident' /etc/postgresql/9.?/main/pg_hba.conf
```

پس از پایان کار، PostgreSQL را مجددا راه اندازی کنید تا تغییرات اعمال شود.

```
sudo systemctl postgresql restart
```

## نصب روبی

Mastodon برنامه مبتنی بر Ruby است. بنابراین شما باید Ruby و تمام وابستگی های مورد نیاز را به سرور خود نصب کنید.

شما می توانید همه آنها را با اجرای دستور زیر نصب کنید:

```
sudo apt-get install autoconf bison build-essential libssl-dev libyaml-dev libncurses5-dev libffi-dev libgdbm3 libreadline6-dev zlib1g-dev libgdbm-dev -y
```

پس از نصب تمام بسته ها، یک کاربر به نام mastodon بدون رمز عبور ایجاد کنید:


```
sudo adduser --disabled-password --disabled-login mastodon
```

بعد، با mastodon وارد شوید و rbenv و rbenv-build را با دستور زیر نصب کنید:

```
su - mastodon git clone https://github.com/rbenv/rbenv.git ~/.rbenv echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc echo 'eval "$(rbenv init -)"' >> ~/.bashrc source ~/.bashrc
```


بعد از کاربر mastodon خارج شده و دوباره وارد شوید تا تغییرات bash اعمال شود:


```
exit su - mastodon git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
```


بعد، برای نصب ruby 2.4.1 برای mastodon دستور زیر را وارد کنید:

```
rbenv install 2.4.1 rbenv global 2.4.1
```

هنگامی که نصب کامل است، می توانید نسخه ruby ​​را با دستور زیر بررسی کنید:

```
ruby -v
```


## نصب Mastodon

اکنون، تمام وابستگی های لازم برای راه اندازی Mastodon نصب شده اند. بنابراین آخرین نسخه Mastodon را از مخزن Git دانلود کنید. برای انجام این کار، دستور زیر را اجرا کنید:


```
git clone https://github.com/tootsuite/mastodon.git live cd live git checkout $(git tag | tail -n 1)
```

بعد نصب bundler برای مدیریت وابستگی ها و غیرفعال کردن داکیومنت های gem دستور زیر را وارد کنید:

```
echo "gem: --no-document" > ~/.gemrc gem install bundler --no-ri
```


بعد، Mastodon را با دستور زیر نصب کنید:

```
bundle install --deployment --without development test
```

پس از تکمیل نصب، یک فایل پیکربندی برای Mastodon ایجاد کنید:

```
cp .env.production.sample .env.production nano .env.production
```


مطالب زیر را اضافه کنید:


```
REDIS_HOST=localhost
REDIS_PORT=6379
DB_HOST=/var/run/postgresql
DB_USER=mastodon
DB_NAME=mastodon_production
DB_PASS=
DB_PORT=5432# Federation
LOCAL_DOMAIN=yourdomain.com
LOCAL_HTTPS=true
```


پس از اتمام، فایل را ذخیره کرده و آن را ببندید.


## فایل Mastodon Systemd را ایجاد کنید

بعد، شما باید یک وب سرویس Mastodon systemd، خدمات پس زمینه و سرویس سرویس API برای Mastodon ایجاد کنید.

اول، یک فایل وب سرویس با دستور زیر ایجاد کنید:
```
nano /etc/systemd/system/mastodon-web.service
```

خطوط زیر را اضافه کنید:



```
 [Unit]
 Description=mastodon-web
 After=network.target[Service]
 Type=simple
 User=mastodon
 WorkingDirectory=/home/mastodon/live
 Environment="RAILS_ENV=production"
 Environment="PORT=3000"
 ExecStart=/home/mastodon/.rbenv/shims/bundle exec puma -C config/puma.rb
 TimeoutSec=15
 Restart=always[Install]
 WantedBy=multi-user.target
```
بعد، یک فایل سرویس Backgroud ایجاد کنید:

`nano /etc/systemd/system/mastodon-sidekiq.service`

خطوط زیر را اضافه کنید:

```
 Unit]
 Description=mastodon-sidekiq
 After=network.target[Service]
 Type=simple
 User=mastodon
 WorkingDirectory=/home/mastodon/live
 Environment="RAILS_ENV=production"
 Environment="DB_POOL=5"
 ExecStart=/home/mastodon/.rbenv/shims/bundle exec sidekiq -c 5 -q default -q mailers -q pull -q push
 TimeoutSec=15
 Restart=always[Install]
 WantedBy=multi-user.target
```

بعد، یک فایل سرویس API ایجاد کنید:

```
nano /etc/systemd/system/mastodon-streaming.service
```

خطوط زیر را اضافه کنید:




```
[Unit]
 Description=mastodon-streaming
 After=network.target[Service]
 Type=simple
 User=mastodon
 WorkingDirectory=/home/mastodon/live
 Environment="NODE_ENV=production"
 Environment="PORT=4000"
 ExecStart=/usr/bin/npm run start
 TimeoutSec=15
 Restart=always[Install]
 WantedBy=multi-user.target
```

فایل را ذخیره و بسته کنید، سپس این فایل سرویس را با دستور زیر فعال کنید:


```
sudo systemctl enable /etc/systemd/system/mastodon-*.service sudo systemctl enable /etc/systemd/system//etc/systemd/system//etc/systemd/system/mastodon-web.service sudo systemctl enable /etc/systemd/system//etc/systemd/system/mastodon-streaming.service
```

بعد، سرور Mastodon را با دستور زیر شروع کنید:


```
sudo systemctl start mastodon-web.service sudo systemctl start mastodon-sidekiq.service sudo systemctl start mastodon-streaming.service
```

## پیکربندی Nginx برای Mastodon

بعد، شما باید reverse-proxy را با استفاده از Nginx برای دسترسی به Mastodon با نام دامنه خود تنظیم کنید.

اول، Nginx را با دستور زیر نصب کنید:


```
sudo apt-get install nginx -y
```

بعد، بلوک سرور مجازی Nginx را برای Mastodon ایجاد کنید.


```
sudo nano /etc/nginx/sites-enabled/mastodon.conf
```

خطوط زیر را اضافه کنید:


```
map $http_upgrade $connection_upgrade {
 default upgrade;
 '' close;
}
server {
 listen 80;
 listen [::]:80;
 server_name www.yourdomain.com yourdomain.com;
 return 301 https://votredomaine.com$request_uri;access_log /dev/null;
 error_log /dev/null;
}server {
 listen 443 ssl http2;
 listen [::]:443 ssl http2;
 server_name www.yourdomain.com yourdomain.com;access_log /var/log/nginx/yourdomain.com-access.log;
 error_log /var/log/nginx/yourdomain.com-error.log;ssl_certificate /etc/letsencrypt/live/fullchain.pem;
 ssl_certificate_key /etc/letsencrypt/live/privkey.pem;
 ssl_protocols TLSv1.2;
 ssl_ciphers EECDH+AESGCM:EECDH+AES;
 ssl_prefer_server_ciphers on;
 add_header Strict-Transport-Security "max-age=15552000; preload";keepalive_timeout 70;
 sendfile on;
 client_max_body_size 0;
 gzip off;root /home/mastodon/live/public;location / {
  try_files $uri @proxy;
 }location @proxy {
  proxy_set_header Host $host;
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  proxy_set_header X-Forwarded-Proto https;
  proxy_pass_header Server;
  proxy_pass http://127.0.0.1:3000;
  proxy_buffering off;
  proxy_redirect off;
  proxy_http_version 1.1;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection $connection_upgrade;
  tcp_nodelay on;
 }location /api/v1/streaming {
  proxy_set_header Host $host;
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  proxy_set_header X-Forwarded-Proto https;
  proxy_pass http://127.0.0.1:4000;
  proxy_buffering off;
  proxy_redirect off;
  proxy_http_version 1.1;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection $connection_upgrade;
  tcp_nodelay on;
 }

error_page 500 501 502 503 504 /500.html; } 
```

فایل را ذخیره کنید، سپس Nginx را با دستور زیر دوباره راه اندازی کنید:

```
systemctl restart nginx

```

در نهایت، مرورگر وب خود را باز کنید و  http // yourdomain.com خود را تایپ کنید تا به Mastodon install دسترسی داشته باشید. 