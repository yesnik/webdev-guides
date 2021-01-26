# Double Finder

We have a class with 2 bugs. Unit test helped to find them.

```php
class DoubleFinder
{    public static function find(Claims $claim): ?Claims
    {
        $criteria = new CDbCriteria();
        $criteria->addCondition('mobile = :mobile');
        $criteria->addCondition('product_id = :productId');
        $criteria->addCondition('create_time >= :createTime');
        $criteria->addCondition("ldg_utm_campaign != 'close'");
        $criteria->addCondition('id != :id');
        $criteria->addCondition('status >= :startStatus AND status < :endStatus');
        $criteria->params = [
            ':mobile' => $claim->info->mobile,
            ':productId' => $claim->product_id,
            ':id' => $claim->id,
            ':createTime' => (new DateTime())->sub(new DateInterval('P7D'))->format('Y-m-d H:i:s'),
            ':startStatus' => Claims::STATUS_CC_SEND_INTERNAL,
            ':endStatus' => Claims::STATUS_PI_ERROR_TIMEOUT,
        ];

        /** @var ClaimsIndeces $claimIndex */
        $claimIndex = ClaimsIndeces::model()->find($criteria);
        return $claimIndex->claim ?? null;
    }
}
```

*Hint*: This method generates SQL:

```sql
SELECT *
    FROM `claims_indeces` `t`
WHERE mobile = '79220005500'
    AND product_id = '1'
    AND create_time >= '2021-01-19 18:13:54'
    AND ldg_utm_campaign != 'close'
    AND id != '98094056'
    AND (status >= '210' AND status < '390')
```

## Solution

The error was in the SQL. In DB field `claims.ldg_utm_campaign` can have `NULL` values, so we need to add extra check for this field.
Also we need to use `claim_id` in `WHERE` clause.

```php
// Fixed condition:
$criteria->addCondition("ldg_utm_campaign != 'close' OR ldg_utm_campaign IS NULL");
$criteria->addCondition('claim_id != :id');
```
