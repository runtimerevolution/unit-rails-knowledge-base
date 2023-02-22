## OR Precedence

A problem with the activerecord is that usage of OR relies on natural precedence of OR and AND. So, the following query
```ruby
Order.joins(:address)
     .where(addresses: {state: 'TX'})
     .where('amount < ?', 40)
     .or(
       Order.where('amount > ?', 100)
      )
     .order('orders.id')
```
generates this SQL.
```SQL
SELECT *
FROM "orders"
       INNER JOIN "addresses" ON "addresses"."order_id" = "orders"."id"
WHERE ("addresses"."state" = "TX" AND (orders.amount < 40)
   OR  orders.amount > 100)
ORDER BY orders.id
```
This will return all orders whose address is on Texas and have amounts less than 40 OR all orders with amount over 100 (indepedently of their address).

But, what if we need to ensure the precedence of OR on the query? For our example, how do we make it so that we return all orders from Texas with amount either less than 40 or grater than 100?

The solution is to use the `merge` method:
```ruby
Order.joins(:address)
     .where(addresses: {state: 'TX'})
     .merge(Order.where('orders.amount < ?', 40)
                 .or(Order.where('orders.amount > ?', 100))
     .order('orders.id')
```
and this will produce:
```SQL
SELECT *
FROM orders
       INNER JOIN addresses on addresses.order_id = orders.id
WHERE addresses.state = 'TX' AND (orders.amount < 40 or orders.amount > 100)
ORDER BY orders.id
```

> Source: https://github.com/chrismo/activerecord_or_precedence