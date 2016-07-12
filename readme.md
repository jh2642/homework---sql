Explorer Mode

sqlite3 store.sqlite3

CREATE TABLE "users" ("id" INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL, "first_name" varchar, "last_name" varchar, "email" varchar);

CREATE TABLE "addresses" ("id" INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL, "user_id" integer, "street" varchar, "city" varchar, "state" varchar, "zip" integer);

CREATE TABLE "items" ("id" INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL, "title" varchar, "category" varchar, "description" text, "price" integer);

CREATE TABLE "orders" ("id" INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL, "user_id" integer, "item_id" integer, "quantity" integer, "created_at" datetime);

How many users are there?
sqlite> SELECT COUNT (*) FROM users;
COUNT (*)
50

What are the 5 most expensive items?
sqlite> SELECT price,title FROM items ORDER BY price desc LIMIT 5;
price|title
9984|Small Cotton Gloves
9859|Small Wooden Computer
9790|Awesome Granite Pants
9390|Sleek Wooden Hat
9341|Ergonomic Steel Car


What's the cheapest book? (Does that change for "category is exactly 'book'" versus "category contains 'book'"?)
sqlite> SELECT price, title FROM items WHERE category LIKE '%books%' ORDER BY price LIMIT 1;
price|title
1496|Ergonomic Granite Chair




Who lives at "6439 Zetta Hills, Willmouth, WY"? Do they have another address?
sqlite> SELECT first_name, last_name FROM users INNER JOIN addresses ON users.id = addresses.user_id WHERE street = '6439 Zetta Hills';
first_name|last_name
Corrine|Little

sqlite> SELECT street FROM addresses WHERE user_id IN (SELECT users.id FROM users INNER JOIN addresses ON users.id = addresses.user_id WHERE street = '6439 Zetta Hills');
street
6439 Zetta Hills
54369 Wolff Forges

Correct Virginie Mitchell's address to "New York, NY, 10108".
This is how I found her: sqlite> SELECT first_name, last_name, city, state, zip FROM users INNER JOIN addresses ON users.id = addresses.user_id WHERE first_name = 'Virginie' AND last_name = 'Mitchell';
Virginie|Mitchell|Roxanehaven|NY|34705
Virginie|Mitchell|Bahringerland|WY|24028

sqlite> SELECT first_name, last_name, city, state, zip FROM users INNER JOIN addresses ON users.id = addresses.user_id WHERE first_name = 'Virginie' AND last_name = 'Mitchell';
Virginie|Mitchell|New York|NY|10108
Virginie|Mitchell|New York|NY|10108


How much would it cost to buy one of each tool?
sqlite> SELECT SUM (price) FROM items WHERE category LIKE '%tool%';
46477

How many total items did we sell?
sqlite> SELECT SUM (quantity) FROM orders;
SUM (quantity)
2125

How much was spent on books?
sqlite> SELECT quantity, price, title, category FROM items INNER JOIN orders ON items.id = orders.item_id WHERE category LIKE '%books%';

SELECT SUM (price) FROM items INNER JOIN orders ON items.id = orders.item_id WHERE category LIKE '%books%';

sqlite> SELECT SUM (quantity * price) FROM items INNER JOIN orders ON items.id = orders.item_id WHERE category LIKE '%books%';
1081352

Simulate buying an item by inserting a User for yourself and an Order for that User.
Tried adding these but received this error:
Error: no such column: ‘James’
Used this resource:
http://www.tutorialspoint.com/sql/sql-insert-query.htm

INSERT INTO users (first_name, last_name, email)
VALUES (‘James’, ‘Hildreth’, ‘hildreth@gmail.com’);

INSERT INTO addresses (user_id, street, city, state, zip)
VALUES (‘NEED TO GET ID FROM THE 1ST INPUT’, ‘1000 W. First St.’, ‘Zionsville’, ‘IN’, ‘46077’);

INSERT INTO items (title, category, description, price)
VALUES (‘1’, ‘Fantastic Granite Pants’ ‘Games & Outdoors’, ‘Optional object-oriented concept’, ‘1151’)

INSERT INTO orders (user_id, item_id, quantity, created_at)
VALUES (‘500’,’1’, ‘666’, ‘2015-02-09 00:40:30.457665’)
