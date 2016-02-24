
# Using `merge` to cleanup  scopes that use associations

Say we have two models that are associated and one of them has a scope:
```ruby
class Author < ActiveRecord::Base
    has_many :books
end

class Book < ActiveRecord::Base
  belongs_to :author
  scope :available, ->{ where(available: true) }
end
```

Let's say we want to join the tables to find all Authors who have books that are available. Without ActiveRecord#merge, we have to make this query:

```ruby
Author.joins(:books).where("books.available = ?", true)
```
The generated SQL request is :
```sql
SELECT "authors".* FROM "authors" INNER JOIN "books" ON "books"."author_id" = "authors"."id" WHERE "books"."available" = 't'
```

But with `ActiveRecord#merge`, this becomes **a whole lot cleaner and we don't duplicate the available scope** :

```ruby
Author.joins(:books).merge(Book.available)
````
The generated SQL request is the same : 

``` sql
SELECT "authors".* FROM "authors" INNER JOIN "books" ON "books"."author_id" = "authors"."id" WHERE "books"."available" = 't'
```

____

[Source](https://gorails.com/blog/activerecord-merge)
