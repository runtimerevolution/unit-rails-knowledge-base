
## Explain

Runs EXPLAIN on the query or queries triggered by this relation and returns the result as a string. The string is formatted imitating the ones printed by the database shell.

Note that this method actually runs the queries, since the results of some are needed by the next ones when eager loading is going on.


```ruby
Customer.where(active: true)
        .joins(:orders)
        .explain(:analyze, :verbose)
```

> Note: `on rails 7.1` and above versions.