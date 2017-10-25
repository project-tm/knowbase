# Отладка

- [Стек вызова](#Стек-вызова)
- [file_var_dump](#file_var_dump)

## Стек вызова
```php
$e = new Exception();
?><pre><?= $e->getTraceAsString() ?></pre><?
```

## file_var_dump
```php
function file_var_dump($arParams = array(), $mode = FILE_APPEND) {
    file_put_contents($filename, "===================\r\n" . var_export($arParams, true) . "===================\r\n", $mode);
}
```
