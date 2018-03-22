Командная строка Linux
======================

### Полезные запросы
- `grep -r -n --exclude-dir={upload,xhprof,managed_cache,cache,test,webstat,backup,images,import,old,html_pages} '' ./` - поиск слова в файлах
- `ps -ef | grep 'test' | grep -v grep | awk '{print $2}' | xargs -r kill -9` - удаление процесса
- `grep -lr 'emailRinger' *`
- `find . -mindepth 1 -newermt '2016-10-04 16:00' -newermt '2016-10-04 16:50' -ls` - поиск по времени изменения
- `find ~ -name "*.php" -type f -mtime -4` - поиск последних измененных файлов
- `df -h` - место на диске
- `du -sh */` - место в папках
- `du -s *|sort -nr|cut -f 2-|while read a;do du -hs $a;done` - размеры файлов и папок
- `w` - список запущенных консолей

### База данных
- `mysql -u root -p`

### [Экспорт импорт базы данных](mysqldump.md)
- `mysqldump -u positions -p positions > positions.sql`
- `mysqldump --complete-insert --no-create-db -u positions -p positions table > positions.sql`
- `mysql -u devpositions -p devpositions < positions.sql`

### Права на файлы
```sh
chmod 777 -R ./
chown -R developer ./

sudo adduser bitrix5-test
sudo usermod -d /var/www/developer/data/www/bitrix5-test.dev-z.ru -s /bin/false bitrix5-test
sudo usermod -a -G webdevelopers bitrix5-test
sudo chown -R devel:webdevelopers /var/www/developer/data/www/bitrix5-test.dev-z.ru
sudo chmod -R 0777 /var/www/developer/data/www/bitrix5-test.dev-z.ru
sudo passwd bitrix5-test
```

### Архивы
```sh
tar -cvzf files.tar.gz dir
tar -xvf archive.tar.bz2 -C /path/to/folder
tar -tf archive.tar.gz
tar  -cvzf  ../site.ru.tar.gz --exclude="/path/*" --exclude="/path2/*" ./
```

### Список повседневных команд:

- [`ls`](http://www.opennet.ru/man.shtml?topic=ls&category=1) - выводит содержимое каталога
- [`pwd`](http://www.opennet.ru/man.shtml?topic=pwd&category=1) - выводит путь текущего каталога  
- [`cd`](http://www.opennet.ru/man.shtml?topic=cd&category=1) - смена текущего каталога
- [`grep`](http://www.opennet.ru/man.shtml?topic=grep&category=1) - поиск строки в тексте по шаблону (подстроке)
- [`cat`](http://www.opennet.ru/man.shtml?topic=cat&category=1) - вывод содержимого файла (файлов)
- [`mv`](http://www.opennet.ru/man.shtml?topic=mv&category=1) - перемещение файлов и каталогов
- [`cp`](http://www.opennet.ru/man.shtml?topic=cp&category=1) - копирование файлов и каталогов
- [`rm`](http://www.opennet.ru/man.shtml?topic=rm&category=1) - удаление файлов и каталогов
- [`mkdir`](http://www.opennet.ru/man.shtml?topic=mkdir&category=1) - создание каталогов
- [`touch`](http://www.opennet.ru/man.shtml?topic=touch&category=1) - создание пустых файлов
- [`sudo`](http://www.opennet.ru/man.shtml?topic=sudo&category=1) - запуск программ от имени администратора (root)
- [`su`](http://www.opennet.ru/man.shtml?topic=su&category=1) - создание нового терминального сеанса из под администратора (root)
- [`less`](http://www.opennet.ru/man.shtml?topic=less&category=1) - просмотр и навигация по файлам, которые не помещаются на экран
- [`reboot`](http://www.opennet.ru/man.shtml?topic=reboot&category=1) - перезагрузка системы
- [`clear`](http://www.opennet.ru/man.shtml?topic=clear&category=1) - очистка вывода окна терминала
- [`killall`](http://www.opennet.ru/man.shtml?topic=killall&category=1) - завершение процессов по имени

### Список повседневных программ:

- [`wget`](http://www.opennet.ru/man.shtml?topic=wget&category=1) - утилита для загрузки файлов из интернет
- [`curl`](http://www.opennet.ru/man.shtml?topic=curl&category=1) - утилита для загрузки файлов из интернет
- [`ssh`](http://www.opennet.ru/man.shtml?topic=ssh&category=1) - ssh-клиент
- [`ftp`](http://www.opennet.ru/man.shtml?topic=ftp&category=1) - терминальный ftp-клиент
- [`scp`](http://www.opennet.ru/man.shtml?topic=scp&category=1) - копирование файлов посредством протокола ssh
- [`git`](http://www.opennet.ru/man.shtml?topic=git&category=1) - система управления версиями
- [`apt-get`](http://www.opennet.ru/man.shtml?topic=apt-get&category=1) - менеджер программ Ubuntu

### Список консольных редакторов:

- [`nano`](http://www.opennet.ru/man.shtml?topic=nano&category=1)
- [`vim`](http://www.opennet.ru/man.shtml?topic=vim&category=1)
- [`vi`](http://www.opennet.ru/man.shtml?topic=vi&category=1)
