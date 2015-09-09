#SQL homework

##Normal Mode
1. How many users are there?
    User.count = 51 (I added myself yesterday)

2. What are the 5 most expensive items?
Item.order('price DESC').limit(5)

  60|Ergonomic Steel Car
  40|Sleek Wooden Hat
  100|Awesome Granite Pants
  83|Small Wooden Computer
  25|Small Cotton Gloves

3. What’s the cheapest book? (Does that change for “category is exactly ‘book’” versus “category contains ‘book’”?)
  SELECT * FROM items WHERE category LIKE "%books%" ORDER BY price


4. Who lives at “6439 Zetta Hills, Willmouth, WY”? Do they have another address?
Address.where(street: "6439 Zetta Hills")
ID=40
USer.find(40)




5. Correct Virginie Mitchell’s address to “New York, NY, 10108”.

Address.where(user_id: 39).update_all(city: "New York", state: "NY", zip: 10108)

6. How much would it cost to buy one of each tool?
Item.where("category LIKE ?", "%tool%").sum(:price)
46477


7. How many total items did we sell?
Order.sum(:quantity)
2125


8. How much was spent on books?

SELECT SUM(price * quantity) FROM orders INNER JOIN items ON orders.item_id = items.id WHERE LOWER(category) LIKE '%book%';
($1.8m)

Try #1:
Order.joins(:items).where("category LIKE ?", "%book%").sum(:price * :quantity)

Try #2:
Order.joins("INNER JOIN items ON orders.item_id = items.id").where("category LIKE ?", "%book%").sum("price * quantity")

9. Simulate buying an item by inserting a User for yourself and an Order for that User.
INSERT INTO users (id, first_name, last_name, email) VALUES (9088, "Michelle", "Dotzenrod", "dotzenrods@gmail.com");

User.create({ first_name: "Michelle",
               last_name: "Dotzenrod,
                   email: 'michelle@hotmail.com' })

##Hard Mode
What item was ordered most often? Grossed the most money?
item_id 66

What user spent the most?
SELECT MAX(price) FROM items NATURAL JOIN orders WHERE = (SELECT first_name, last_name FROM users);

What were the top 3 highest grossing categories?
This folder structure should be suitable for starting a project that uses a database:

* Fork this repo
* Clone this repo
* Run `bundle install` to install `active_record`
* `rake generate:migration <NAME>` to create a migration (Don't include the `<` `>` in your name, it should also start with a capital)
* `rake db:migrate` to run the migration and update the database
* Create models in lib that subclass `ActiveRecord::Base`
* ... ?
* Profit


## Rundown

```
.
├── Gemfile             # Details which gems are required by the project
├── README.md           # This file
├── Rakefile            # Defines `rake generate:migration` and `db:migrate`
├── config
│   └── database.yml    # Defines the database config (e.g. name of file)
├── console.rb          # `ruby console.rb` starts `pry` with models loaded
├── db
│   ├── dev.sqlite3     # Default location of the database file
│   ├── migrate         # Folder containing generated migrations
│   └── setup.rb        # `require`ing this file sets up the db connection
└── lib                 # Your ruby code (models, etc.) should go here
    └── all.rb          # Require this file to auto-require _all_ `.rb` files in `lib`
```
