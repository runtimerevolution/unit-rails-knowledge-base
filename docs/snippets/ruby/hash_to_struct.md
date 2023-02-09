
## Hash to Struct


```ruby
# :Keyword_init forces Struct::new to take thyword ards instead of normal args
User = Struct.new(:email, :gid, :name, keyword_init: true)

# you don't need to order the keys (:gid and :email are reserved)
user_hash = {
    gid: "nhg-123-fd3-Eee",
    email: "rails-unit@runtime-revolution.com",
    name: "Rails Unit"
}

user = User.new(user_hash)

user # => #<struct User email: ... >
        
```

> `Source`: <https://www.rubycademy.com/cards/hash-to-struct>