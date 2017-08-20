# SSL на Битрикс виртуальной машине 7 с помощью Let’s Encrypt

```sh
cd /usr/local/sbin
sudo wget https://dl.eff.org/certbot-auto
cd /tmp
git clone https://github.com/certbot/certbot
sudo chmod a+x /usr/local/sbin/certbot-auto
certbot-auto certonly --webroot --agree-tos --email info@site.ru -w /home/bitrix/www/ -d site.ru -d www.site.ru -d test.site.ru
openssl dhparam -out /etc/nginx/ssl/dhparam.pem 2048
service nginx reload
```

Да, на Хабре уже много текстов про сертификаты Let’s Encrypt, но полной пошаговой инструкции я, к сожалению, не нашёл. Хотел восполнить пробел. К тому же, с мая 2016 года в процессе установки произошли незначительные изменения, которые могут сбить с толку новичка. Поэтому я решил написать эту инструкцию. Так сказать себе на память и другим в помощь.

Эта инструкция, в первую очередь, должна быть интересна новичкам.

image

Если у вас все настройки установлены по умолчанию, можно смотреть те пути, которые я привёл. То есть, если вы используете систему, установленную с помощью скрипта Bitrix environment на операционной системе CentOS 6.X. Если же нет, вы и сами знаете где что лежит.

Установка

Первое, что необходимо сделать — установить git:

```sh yum install git```

Далее переходим в директорию /tmp:

```sh cd /tmp```

С помощью git скачиваем файлы Let’s Encrypt. Сам скрипт теперь называется certbot:

`git clone https://github.com/certbot/certbot`


Переходим в скачанную директорию:

`cd certbot`

На всякий случай, даем права на выполнение для файла скрипта:

`chmod a+x ./certbot-auto`

Получение сертификата

Далее следует команда непосредственно получения сертификата:

`./certbot-auto certonly --webroot --agree-tos --email mypost@my-domain.ru -w /home/bitrix/www/ -d my-domain.ru -d www.my-domain.ru`

--webroot — так как автоматическая установка для nginx пока не надежна, используем этот ключ;
--agree-tos — соглашаемся с лицензионным соглашением;
--email mypost@my-domain.ru — указываем свой e-mail. В дальнейшем он может пригодиться для восстановления своего аккаунта;
-w /home/bitrix/www — указываем корневую директорию сайта;
-d my-domain.ru — наш домен. так же можно указывать и поддомены, например -d site.my-domain.ru.

После этого скрипт начнет работу и предложит установить недостающие пакеты. Соглашаемся и ждём.

Если всё завершится успешно, вы увидите сообщение:

IMPORTANT NOTES:
- Congratulations! Your certificate and chain have been saved at
/etc/letsencrypt/live/my-domain.ru/fullchain.pem. Your
cert will expire on 2016-08-21. To obtain a new version of the
certificate in the future, simply run Certbot again.
- If you lose your account credentials, you can recover through
e-mails sent to mypost@my-domain.ru.
- Your account credentials have been saved in your Certbot
configuration directory at /etc/letsencrypt. You should make a
secure backup of this folder now. This configuration directory will
also contain certificates and private keys obtained by Certbot so
making regular backups of this folder is ideal.
- If you like Certbot, please consider supporting our work by:

Donating to ISRG / Let's Encrypt: https://letsencrypt.org/donate
Donating to EFF: https://eff.org/donate-le

Сертификаты установлены, осталось только указать nginx'у, где они лежат.

Настройка

Открываем конфигурационный файл ssl.conf:

`vim /etc/nginx/bx/conf/ssl.conf`

Если у вас уже были установлены сертификаты, удаляем или комментируем строки с ними и вставляем новые:

`ssl_certificate /etc/letsencrypt/live/my-domain.ru/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/my-domain.ru/privkey.pem;`

Не забываем включить ssl, если этого не было сделано ранее:

`ssl on;
keepalive_timeout 70;
keepalive_requests 150;
ssl_session_cache shared:SSL:10m;
ssl_session_timeout 10m;`

После этого перезапускаем nginx:

`service nginx reload`

Если он не выдал никаких ошибок, значит всё в порядке. Можно зайти на сайт и посмотреть что получилось.

Обновление

Сертификат выдается на 90 дней, так что после этого срока нужно будет его обновить. Делается это командой:

`certbot-auto renew`

Её так же можно поставить в cron.
