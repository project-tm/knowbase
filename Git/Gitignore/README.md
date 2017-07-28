# Примеры .gitignore

- [Пример для сайта](#gitignore)
- [Пример для папки bitrix](#gitignore-bitrix)

### .gitignore 

```sh
web.xml
/test
/webstat
/vendor
/composer.phar
/cgi-bin

*.tar
*.tar.gz
*.sql
*.log
*.tmp
*.xml
*.csv
*.doc
*.swf
*.rar
*.pdf

yandex_*.html
yandex_*.txt
xml_answers.txt
sitemap_iblock_*.xml
restore.php

/bitrix
/bitrix/*
!/bitrix/templates/
!/bitrix/components/
!/bitrix/modules/
/bitrix/modules/*/install/
/bitrix/modules/*/public/
/bitrix/modules/*/lang/
/bitrix/modules/*/tools/
/bitrix/modules/*/cvtables/
/bitrix/modules/*/ru/
/bitrix/modules/*/la/
/bitrix/modules/*/distr/
/bitrix/modules/*/ttf/
!/bitrix/php_interface
/bitrix/php_interface/dbconn.php
/bitrix/.settings.php

/upload
```

### .gitignore bitrix
```sh
*.tar
*.tar.gz
*.log
*.tmp
*.xml
*.csv
*.doc
*.swf
*.rar
*.pdf

tmp/
admin/
backup/
cache/
css/
fonts/
gadgets/
image_uploader/
images/
js/
managed_cache/
otp/
panel/
services/
sounds/
stack_cache/
themes/
tools/
html_pages/
wizards/
web.config

modules/*/install/
modules/*/public/
modules/*/lang/
modules/*/tools/
modules/*/cvtables/
modules/*/ru/
modules/*/la/
modules/*/distr/
modules/*/ttf/
php_interface/dbconn.php
.settings.php
```
