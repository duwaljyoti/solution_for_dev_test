# Solution

### Solution 1
[Flowchart Link for the reward system](https://app.creately.com/diagram/GvDfIyhcuer)

[DB Diagram for the reward system](https://dbdiagram.io/d/60e9707e7e498c3bb3f070c9)

### PHP function to credit user reward points after completion.


## Solution 2

```
select
       count(distinct order_id) as total_order_count,
       sum(normal_price) as total_normal_price,
       sum(promotion_price) as total_promotion_price,
       sum(normal_price + kunyo_test_order_products_table.promotion_price) as total_sales_amount
from kunyo_test_order_products_table inner join
    kunyo_test_order_table on kunyo_test_order_table.id = kunyo_test_order_products_table.order_id
```


## Solution 3
```
$totalAmount = 5.00 MYR;
$applicableGstPercentage = 6;
$applicableGstAmount = ($applicableGstPercentage / 100 ) * $totalAmount;
$applicableGstAmount = (6 / 100 ) * 5.00;
$applicableGstAmount = 0.3
```
