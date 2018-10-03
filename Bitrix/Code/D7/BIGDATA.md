## Вставка данных большим куском

сначало собирает данные, а потом делает один заспрос

```php
RegionTable::startBigData();
...
RegionTable::compileBigData();
```

```php
class RegionTable extends DataManager
{
    use TraitList\BigData;
}    
```

```php
namespace TraitList;

use Bitrix\Main\Entity\AddResult;
use Bitrix\Main\Entity\ScalarField;

trait BigData
{

    static private $isBigData = false;
    static private $arInsert = [];

    /**
     *
     */
    public static function startBigData()
    {
        self::$isBigData = true;
    }

    /**
     *
     */
    public static function compileBigData()
    {
        self::$isBigData = false;
        if (empty(self::$arInsert)) {
            return;
        }
        $entity = static::getEntity();
        foreach ($entity->getFields() as $field) {
            if ($field instanceof ScalarField) {
                $defaultValue = $field->getDefaultValue();
                if ($defaultValue !== null) {
                    foreach (self::$arInsert as &$arItem) {
                        if(!array_key_exists($field->getName(), $arItem)) {
                            $arItem[$field->getName()] = $field->getDefaultValue();
                        }
                    }
                }
            }
        }

        $connection = $entity->getConnection();
        $tableName = $entity->getDBTableName();

        $sql = '';
        foreach (self::$arInsert as &$arItem) {
            foreach ($arItem as $fieldName => $value) {
                $field = static::getEntity()->getField($fieldName);
                $arItem[$fieldName] = $field->modifyValueBeforeSave($value, $arItem);
            }
            $dataReplacedColumn = static::replaceFieldName($arItem);
            $insert = $connection->getSqlHelper()->prepareInsert($tableName, $dataReplacedColumn);
            if (empty($sql)) {
                $sql =
                    "INSERT INTO " . $tableName . "(" . $insert[0] . ") " .
                    "VALUES (" . $insert[1] . ")";
            } else {
                $sql .= ", (" . $insert[1] . ")";
            }
        }
        $connection->queryExecute($sql);
        self::$arInsert = [];
    }

    /**
     * @param array $data
     *
     * @return AddResult
     */
    public static function add(array $data)
    {
        if (self::$isBigData) {
            self::$arInsert[] = $data;
            $result = new AddResult();
            return $result;
        } else {
            return parent::add($data);
        }
    }

}
```
