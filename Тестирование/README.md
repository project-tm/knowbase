# Тестирование

- [Развертываем свой сайт на Heroku](https://habrahabr.ru/post/232679/)
- [Отслеживание js-ошибок с помощью Метрики](https://habrahabr.ru/post/324366/)
- [Отладка на живом сайте](#Отладка-на-живом-сайте)

## Отладка на живом сайте
- в консоле подключаем [jquery](https://code.jquery.com/jquery-3.1.1.min.js) и [jqeuty.cookie](https://cdnjs.cloudflare.com/ajax/libs/jquery-cookie/1.4.1/jquery.cookie.min.js)
- заполняем куку, и спокойно выполняем код, только для нас 
```javascript
$.cookie('is-debug-3985', '2352536', {path: '/', expires: 60});
```
```php
if(isset($_COOKIE['is-debug-3985']) and $_COOKIE['is-debug-3985'] == '2352536') {
    $e = new \Exception();
    ?><pre><?= $e->getTraceAsString() ?></pre><?
}
```
