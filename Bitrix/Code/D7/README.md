## D7

- [Подсчет количества уникальных значений в столбце таблицы](подсчет-количества-уникальных-значений-в-столбце-таблицы)
- [D7 getList в mysql](#d7-getlist-в-mysql)
- [Генерация запроса в select](#Генерация-запроса-в-select)
- [Связывать таблицы](#Связывать-таблицы)
- [ORM](#orm)


## Подсчет количества уникальных значений в столбце таблицы
```php
result = НазваниеКлассаУнаследованногоОтDataManager::getList([
	'filter' => ['какой-нибудь фильтр'],
	'select' => [
		'Название_поля_для_группировки',
		'COUNT'
	],
	'runtime' => [
		//в качестве ключа используется поле, которое мы указали в select
		'COUNT' => [
			//тут указывается тип данных, для поля. 
			//я встречал только примеры с integer и reference
			'data_type' => 'integer',
			
			//тут указывается функция, вроде count, max и т.п.
			//важно следить за кавычками и указывать правильные имена таблицы
			//(они обычно не совпадают с названиями HL блока)
			'expression' => ['count(`имя_таблицы`.`поле_для_подсчета`)']
		]
	]
]);
```

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

## Генерация запроса в select
```php
$select[] = 'IS_OVERDUE';
$runtime['IS_OVERDUE'] = array(
    'data_type'=>'integer',
    'expression'=>array(
        "CASE WHEN %s IS NOT NULL AND (%s < %s OR (%s IS NULL AND %s < ".$GLOBALS['DB']->currentTimeFunction().")) THEN 'Y' ELSE 'N' END",
        'DEADLINE',
        'DEADLINE',
        'CLOSED_DATE',
        'CLOSED_DATE',
        'DEADLINE',
    )
);
```

## Связывать таблицы
```php
$runtime['IS_OVERDUE_DATA'] = array(
    'data_type' => 'Bitrix\Tasks\Internals\Task\LogTable',
    'expression'=>array(
        'RAND()'
    ),
    'reference' => array(
        '=ref.TASK_ID' => 'this.ID',
        '=ref.USER_ID' => 'this.RESPONSIBLE_ID',
        '=ref.FIELD' => new SqlExpression('?s', 'STATUS'),
        '=ref.TO_VALUE' => new SqlExpression('?s', self::PERORT_STATUS)
    ),
    'join_type' => "INNER LEFT"
);
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
