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
        return $this->quoteTableSource($this->init_entity->getDBTableName());
    }

    public function getWhere()
    {
        $this->buildQuery();
        $helper = static::getEntity()->getConnection()->getSqlHelper();
        return str_replace($helper->quote($this->getInitAlias()) . '.', '', parent::buildWhere());
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
