# Куски кода

- [D7 getList в mysql](#d7-getlist-в-mysql)
- [Форматирование даты](#Форматирование-даты)
- [Кеширование в компаненте](#Кеширование-в-компаненте)
- [Статические блоки](#Статические-блоки)
- [Отложенные переменные в шаблоне](#Отложенные-переменные-в-шаблоне)
- [Добавление стилей, js прямо в шаблоне](#Добавление-стилей-js-прямо-в-шаблоне)
- [Пользовательские свойства](#Пользовательские-свойства)
- [Обработка 404 init.php](#Обработка-404-initphp)
- [ORM](#orm)

## D7 getList в mysql
```php
        $res = self::getList(array(
                    'select' => array('ID'),
                    'filter' => array(
                        'TYPE' => $arData['TYPE'],
                        'PAGE' => $arData['PAGE'],
                        'CODE' => $arData['CODE']
                    ),
        ));
        
SELECT 
	`project_upload_model_import`.`ID` AS `ID`
FROM `d_project_upload_import` `project_upload_model_import` 

WHERE UPPER(`project_upload_model_import`.`TYPE`) like upper('Project\\Upload\\Agent\\Pwrs')
AND UPPER(`project_upload_model_import`.`PAGE`) like upper('Шины (Склад 2)')
AND `project_upload_model_import`.`CODE` = '875678'  
```

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

## Статические блоки
```php
<? $APPLICATION->ShowViewContent('compare.list.filter'); ?>

<?$this->SetViewTarget('compare.list.filter');?>
compare-list-filter
<?$this->EndViewTarget();?>
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

## Пользовательские свойства
1. Две функции для удобного получения и задания пользовательских свойств:
```php
function GetUserField ($entity_id, $value_id, $property_id)
{
	$arUF = $GLOBALS["USER_FIELD_MANAGER"]->GetUserFields ($entity_id, $value_id);
	return $arUF[$property_id]["VALUE"];
}

function SetUserField ($entity_id, $value_id, $uf_id, $uf_value)
{
   return $GLOBALS["USER_FIELD_MANAGER"]->Update ($entity_id, $value_id,
	Array ($uf_id => $uf_value));
}
```
2. Пример использования:
```php
SetUserField ("BLOG_COMMENT", $CommentID, "UF_RATING", $Rating);
echo "Рейтинг комментария: ".GetUserField ("BLOG_COMMENT", $CommentID, "UF_RATING");
````


## Обработка 404 init.php
```php
AddEventHandler('main', 'OnEpilog', function(){
    if (defined('ERROR_404') && ERROR_404 == 'Y' && !defined('ADMIN_SECTION')) {
        global $APPLICATION;
        $APPLICATION->RestartBuffer();
        include $_SERVER['DOCUMENT_ROOT'] . SITE_TEMPLATE_PATH . '/header.php';
        include $_SERVER['DOCUMENT_ROOT'] . '/404.php';
        include $_SERVER['DOCUMENT_ROOT'] . SITE_TEMPLATE_PATH . '/footer.php';
        exit;
    }
});
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
