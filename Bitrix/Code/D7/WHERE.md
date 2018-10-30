# Генерация where запроса для таблицы D7

```php
$table::query()->deleteByWhere([
    'BLOCK_ID' => $ID,
    $code      => $delete,
]);
```

```php
namespace Project\Editor\Entity;

use Bitrix\Main;

class Query extends Main\Entity\Query
{

    public function deleteByWhere($where)
    {
        $helper = static::getEntity()->getConnection()->getSqlHelper();
        $this->is_executing = true;
        $this->setFilter($where);
        $this->buildQuery();
        $query = "DELETE FROM " . $this->quoteTableSource($this->init_entity->getDBTableName()) . " WHERE " . str_replace($helper->quote($this->getInitAlias()) . '.',
                '', parent::buildWhere());
        $result = $this->query($query);
        $this->is_executing = false;
        return $result;
    }
    
    protected function buildGroup()
    {
        foreach ($this->global_chains as $key=>$chain) {
            $alias = $chain->getAlias();
            if(!in_array($alias, $this->group)) {
                unset($this->global_chains[$alias]);
            }
        }
        return parent::buildGroup();
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
