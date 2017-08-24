# Куски кода

- [Форматирование даты](#Форматирование-даты)
- [Кеширование в компаненте](#Кеширование-в-компаненте)
- [Отложенные переменные в шаблоне](#Отложенные-переменные-в-шаблоне)
- [Добавление стилей, js прямо в шаблоне](#Добавление-стилей-js-прямо-в-шаблоне)
- [ORM](#ORM)

## Форматирование даты
```php
MakeTimeStamp($date, CSite::GetDateFormat();
$arItem["DISPLAY_ACTIVE_FROM"] = CIBlockFormatProperties::DateFormat($arParams["ACTIVE_DATE_FORMAT"], MakeTimeStamp($arItem["ACTIVE_FROM"], CSite::GetDateFormat()));
```

## Кеширование в компаненте
```php
$this->getComponent()->setResultCacheKeys(array('BREADCRUMBS'));

/* в ядре *
final public function setResultCacheKeys($arResultCacheKeys) {
    if ($this->arResultCacheKeys === false)
        $this->arResultCacheKeys = $arResultCacheKeys;
    else
        $this->arResultCacheKeys = array_merge($this->arResultCacheKeys, $arResultCacheKeys);
}
```

## Отложенные переменные в шаблоне
```php
$APPLICATION->ShowProperty("contentClass");
$APPLICATION->SetPageProperty("contentClass", "дизайн, веб, сайт");
```
## Добавление стилей, js прямо в шаблоне
```php
$this->addExternalCss('/bitrix/css/main/bootstrap.css');
$this->addExternalJs('/bitrix/css/main/bootstrap.js');
```

## ORM
```php
BookTable::getList(array(
    'select'  => ... // имена полей, которые необходимо получить в результате
    'filter'  => ... // описание фильтра для WHERE и HAVING
    'group'   => ... // явное указание полей, по которым нужно группировать результат
    'order'   => ... // параметры сортировки
    'limit'   => ... // количество записей
    'offset'  => ... // смещение для limit
    'runtime' => ... // динамически определенные поля
));
```

```php
$rsItems = Bitrix\Iblock\ElementTable::getList(
   array(
      'select' => array(
         'ID',
         'TOTAL_VALUE' => 'VOTE.TOTAL_VALUE', 
         'PRO' => 'VOTE.TOTAL_POSITIVE_VOTES', 
         'CONTRAR' => 'TOTAL_NEGATIVE_VOTES',
         new Entity\ExpressionField(
            'RATING',
            'IFNULL(%d)', array('VOTE.TOTAL_VALUE')
         ),
      ),
      'runtime' => array(
         new Entity\ReferenceField(
            'VOTE',
            'My\Module\Vote',
            array(
               '=this.ID' => 'ref.ENTITY_ID',
               '=ref.ENTITY_TYPE_ID' => new DB\SqlEx * pression('?s', 'IBLOCK_ELEMENT')
            ),
            array(
               'join_type' => 'LEFT'
            )
         ),
         new Entity\ReferenceField(
            'S155',
            'My\Module\S155',
            array(
               '=this.ID' => 'ref.IBLOCK_ELEMENT_ID',
            ),
            array(
               'join_type' => 'INNER'
            )
         )
      ),
      'filter' => array(
         '=IBLOCK_ID' => 155,
         '=S155.PROPERTY_1442' => 879
      ),
      'order' => array(
         'RATING' => 'DESC',
         'ID' => 'ASC'
      )
   )
);
```
