# Solution

### Solution 1
[Flowchart Link for the reward system](https://app.creately.com/diagram/GvDfIyhcuer)

[DB Diagram for the reward system](https://dbdiagram.io/d/60e9707e7e498c3bb3f070c9)

[Use of reward points](https://docs.google.com/document/d/1DA_kp--SZToUQozXIXjjC2i_iCUmqdNFWp8pFAIVLlU/edit?usp=sharing)

### PHP function to credit user reward points after completion.

```
    public function complete(Order $order): Order
    {
        // 1 point of reward is equivalent to 1 dollar.
        // We can make this dynamic to make it configurable.
        $amountRewardEquivalent = 1;

        $operatedOrder = $order;
        $operatedOrder['status'] = 'completed';
        $operatedOrder['price_details'] = $this->convertCurrencyConditionally($operatedOrder);

        $customer = (object)[];
        // assuming a customer gives us a customer object.
        // A customer object will have a one to many relationship with the rewards point.
        // for ease we are assuming that we are using the eloquent ORM of Laravel in any PHP project.
        $reward = (object)[
            'amount' => $operatedOrder['price_details']['price'] / $amountRewardEquivalent,
            'expiry_date' => Carbon::today()->addYear(1)
        ];
        // reward point will have amount.
        // we can use the created_at timestamp of the reward to check the validity of the reward.
        // assuming customer has many rewards
        $customer->rewards()->save($reward);
        return $operatedOrder;
    }

    public function getCurrentExchangeRates(string $toCovertCurrency, string $fromConvertCurrency): float
    {
        $currentExchangeRates = [
            'usd' => [
                'npr' => 119.08,
                'inr' => 74.49
            ]
        ];

        return $currentExchangeRates[$toCovertCurrency][$fromConvertCurrency];
    }

    public function convertCurrencyConditionally(Order $order): array
    {
        $orderPriceDetails = $order['price_details'];
        if ($orderPriceDetails['currency'] !== 'usd') {
            // get this from Db to make it more configurable
            $getCurrencyToConvert = 'usd';
            $updatedPrice = $order;
            $currentRate = $this->getCurrentExchangeRates(
                $getCurrencyToConvert, $orderPriceDetails['currency']
            );
            $updatedPrice['price_details']['price'] = ($orderPriceDetails['price']  / $currentRate);
            $updatedPrice['price_details']['currency'] = $getCurrencyToConvert;

            return $updatedPrice['price_details'];
        }

        return $orderPriceDetails;
    }

    public function completeOrder()
    {
        $orders = [
            [
                'items' => [
                    'minecraft',
                    'league_of_legends',
                    'apex_battle'
                ],
                'price_details' => [
                    'price' => 300,
                    'currency' => 'npr'
                ],
                'customer_id' => 4
            ],
            [
                'items' => [
                    'pubg',
                    'coc',
                    'cs'
                ],
                'price_details' => [
                    'price' => 5,
                    'currency' => 'usd'
                ],
                'customer_id' => 3
            ]
        ];

        $operatedOrders = [];
        array_map(function ($order) use (&$operatedOrders) {
            array_push($operatedOrders, $this->complete($order));
        }, $orders);

        var_dump($operatedOrders);
    }
    
 ```
##### I am assuming to use Eloquent of Laravel.
##### The codes are just shared above.

## Solution 2 => To get data from the multiple orders table.

```
       select
              count(distinct order_id) as total_order_count,
              sum(normal_price) as total_normal_price,
              sum(promotion_price) as total_promotion_price,
              sum(normal_price + kunyo_test_order_products_table.promotion_price) as total_sales_amount
       from kunyo_test_order_products_table inner join
           kunyo_test_order_table on kunyo_test_order_table.id = kunyo_test_order_products_table.order_id
```


## Solution 3 => To get the calculated fee.
```
       $totalAmount = 5.00 MYR;
       $applicableGstPercentage = 6;
       $applicableGstAmount = ($applicableGstPercentage / 100 ) * $totalAmount;
       $applicableGstAmount = (6 / 100 ) * 5.00;
       $applicableGstAmount = 0.3
```

