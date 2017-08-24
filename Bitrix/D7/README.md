# D7

- [ORM](#ORM)

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
