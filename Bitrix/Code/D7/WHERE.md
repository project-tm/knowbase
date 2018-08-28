# Генерация where запроса для таблицы D7

```php
$query = $table::query();
$query->setFilter([
    'BLOCK_ID' => $ID,
    $code      => $delete,
]);
$entity = static::getEntity();
$sql = "DELETE FROM ".$entity->getDBTableName()." WHERE ".$query->getWhere();
$entity->getConnection()->queryExecute($sql);
```

```php
namespace Project\Editor\Entity;

use Bitrix\Main;

class Query extends Main\Entity\Query
{
    public function getWhere()
    {
        $this->buildQuery();
        return parent::buildWhere();
    }
}
```

```php
namespace Megafon\Editor\Model\Block;

use Bitrix\Main\Entity;
use Bitrix\Main\Entity\DataManager;
use Project\Editor\Entity\Query;

class ClientTypeTable extends DataManager
{

    public static function query()
    {
        return new Query(static::getEntity());
    }

    public static function getTableName();
    public static function getMap();
}
```
