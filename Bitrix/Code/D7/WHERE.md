# Генерация where запроса для таблицы D7

```php
$query = $table::query();
$query->setFilter([
    'BLOCK_ID' => $ID,
    $code      => $delete,
]);
$sql = "DELETE FROM " . $query->getFrom() . " WHERE " . $query->getWhere();
static::getEntity()->getConnection()->queryExecute($sql);
```

```php
namespace Project\Editor\Entity;

use Bitrix\Main;

class Query extends Main\Entity\Query
{

    public function getFrom()
    {
        $connection = $this->init_entity->getConnection();
        $helper = $connection->getSqlHelper();
        $sqlFrom = $this->quoteTableSource($this->init_entity->getDBTableName());
        return $sqlFrom . ' ' . $helper->quote($this->getInitAlias());
    }

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
