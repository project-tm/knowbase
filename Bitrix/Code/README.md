# Куски кода

## [D7](D7/README.md)

## [Bitrix шаблоны](#bitrix-шаблоны-1)
- [Отложенные переменные в шаблоне](#Отложенные-переменные-в-шаблоне)
- [Добавление стилей, js прямо в шаблоне](#Добавление-стилей-js-прямо-в-шаблоне)
- [Кеширование в компаненте](#Кеширование-в-компаненте)
- [Статические блоки](#Статические-блоки)
- [Добавление метки к файлу](#Добавление-метки-к-файлу)
- [Позволяет добавлять правила валидации через атрибут data-validate \*](#Позволяет-добавлять-правила-валидации-через-атрибут-data-validate-)

## [Bitrix](#bitrix-1)
- [Кеширование данных](#Кеширование-данных)
- [Форматирование даты](#Форматирование-даты)
- [Пользовательские свойства](#Пользовательские-свойства)
- [Обработка 404 init.php](#Обработка-404-в-initphp)
- [Обработка Canonical в init.php](#Обработка-canonical-в-initphp)


# Bitrix шаблоны

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

## Добавление метки к файлу
```php
CUtil::GetAdditionalFileURL()
```

## Позволяет добавлять правила валидации через атрибут data-validate *
```php
Пример: <input class="form__input" data-validate="{ minlength: 3, maxlength: 10 } >"

function addValidateRules() {
    const $inputs = $('.form__input').filter('[data-validate]');
    $inputs.each((ind, input) => {
        let rules = input.getAttribute('data-validate');
        
        if (!rules) {
            return;
        }
        if (typeof rules === 'string') {
            rules = JSON.parse(rules);
        }
        $(input).rules('add', rules);
    });
}
```

# Bitrix

## Кеширование данных
```php
if (Bitrix\Main\Loader::includeModule('project.core')) {
    Project\Core\Utility::useCache(array('game', $gameId), function() use($gameId) {
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

## Форматирование даты
```php
MakeTimeStamp($date, CSite::GetDateFormat();
$arItem["DISPLAY_ACTIVE_FROM"] = CIBlockFormatProperties::DateFormat($arParams["ACTIVE_DATE_FORMAT"], MakeTimeStamp($arItem["ACTIVE_FROM"], CSite::GetDateFormat()));
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


## Обработка 404 в init.php
```php
AddEventHandler('main', 'OnEpilog', function() {
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

## Обработка Canonical в init.php
```php
AddEventHandler('main', 'OnEpilog', function() {
    if (!empty($_REQUEST['PAGEN_1'])) {
        global $APPLICATION;
        $APPLICATION->SetPageProperty('canonical', "https://" . $_SERVER["HTTP_HOST"] . $APPLICATION->GetCurPage());
    }
});
```
