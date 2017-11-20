# [Инструментарий](https://bitbucket.org/project-tm/project.core.v2)

- [Настройка](#Настройка)
- [Кеширование данных](#Кеширование-данных)
- [Ресайтинг фото, Ватермарки](#Ресайтинг-фото-Ватермарки)
- [Отладка](#Отладка)

## Настройка
```php
Project\Tools\Utility\Config::set(array(
    /* Отладка */
    'adminDebug' => [1],
    /* Кеширование */
    'isCacheDebug' => false,
    'isCache' => true,
    /* Каталог */
    'catalogPriceId' => 1,
    /* ватермарки */
    'isWarermark' => true,
    'warermarkPath' => '/images/warermark.png',
    'warermarkСoefficient' => 0.8,
));

pre(Project\Tools\Config::isDebug());
pre(Project\Tools\Config::isCache());
pre(Project\Tools\Config::warermarkPath());
```

## Кеширование данных
```php
if (defined('Project\Tools\IS_START')) {
    Project\Tools\Utility\Cache::get(array('game', $gameId), function() use($gameId) {
        $arSelect = Array("ID", "NAME", 'PROPERTY_SELLER');
        $arFilter = Array("IBLOCK_ID" => Game\Config::DZHO_IBLOCK, "ID" => $gameId);
        $res = CIBlockElement::GetList(Array(), $arFilter, false, false, $arSelect);
        while ($arItem = $res->GetNext()) {
            return array(
                'ID' => $arItem['ID'],
                'SELLER_ID' => $arItem['PROPERTY_SELLER_VALUE'],
                'THEME' => [
                    'USER' => 'Покупка игры',
                    'SELLER' => 'Продажа игры',
                    'FORUM' => 'Продажа игры: «' . $arItem['NAME'] . '»'
                ]
            );
        }
        return false;
    });
}
```

## Ресайтинг фото, Ватермарки
```php
if (defined('Project\Tools\IS_START')) {
    $item['SRC'] = Project\Tools\Utility\Image::resize($item['ID'], 200, 200);
};
if (defined('Project\Tools\IS_START')) {
    $item['SRC'] = Project\Tools\Utility\Image::watermark($item['ID'], 200, 200, '/images/warermark.png');
};
```

## Отладка
```php
вывод данных для админа
if (defined('Project\Tools\IS_START')) {
    pre();
};
pre(); - данные
preDate(); - метка даты
preTrace(); - трассировка вывоза
preMemory(); - потребляемая память
preDebug(); - запись в файл
```

## Получение всех товаров, которые имеют скидку
```php
if (defined('Project\Tools\IS_START')) {
    $searchFilter = array(
        "=ID" => Project\Tools\Sale\Discount::getProductsList(),
    );
}
$APPLICATION->IncludeComponent(
    "bitrix:catalog.section",
    "template",
    array(
        ...   
        "FILTER_NAME" => "searchFilter",
        ...
    ),
);
```
