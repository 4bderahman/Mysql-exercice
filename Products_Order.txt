Last login: Mon Feb 12 13:34:57 on ttys000

The default interactive shell is now zsh.
To update your account to use zsh, please run `chsh -s /bin/zsh`.
For more details, please visit https://support.apple.com/kb/HT208050.
AbderrahmanMacBookPro:~ macbook$ /usr/local/mysql/bin/mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.3.0 MySQL Community Server - GPL

Copyright (c) 2000, 2024, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> create database store;
Query OK, 1 row affected (0.01 sec)

mysql> use store;
Database changed

mysql> create table products (
    ->     product_id int auto_increment,
    ->     name varchar(255) not null,
    ->     price decimal(10, 2) not null,
    ->     primary key (product_id)
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql> create table orders (
    ->     order_id int auto_increment,
    ->     product_id int,
    ->     order_date date not null,
    ->     quantity int default 1,
    ->     primary key (order_id),
    ->     foreign key (product_id) references products(product_id)
    -> );
Query OK, 0 rows affected (0.01 sec)

mysql> desc products;
+------------+---------------+------+-----+---------+----------------+
| Field      | Type          | Null | Key | Default | Extra          |
+------------+---------------+------+-----+---------+----------------+
| product_id | int           | NO   | PRI | NULL    | auto_increment |
| name       | varchar(255)  | NO   |     | NULL    |                |
| price      | decimal(10,2) | NO   |     | NULL    |                |
+------------+---------------+------+-----+---------+----------------+
3 rows in set (0.00 sec)

mysql> desc orders;
+------------+------+------+-----+---------+----------------+
| Field      | Type | Null | Key | Default | Extra          |
+------------+------+------+-----+---------+----------------+
| order_id   | int  | NO   | PRI | NULL    | auto_increment |
| product_id | int  | YES  | MUL | NULL    |                |
| order_date | date | NO   |     | NULL    |                |
| quantity   | int  | YES  |     | 1       |                |
+------------+------+------+-----+---------+----------------+
4 rows in set (0.00 sec)

mysql> insert into products (name, price) values 
    -> ('Argan Oil', 250.00),
    -> ('Couscous', 50.00),
    -> ('Amlou', 150.00),
    -> ('Honey', 30.00);
Query OK, 4 rows affected (0.00 sec)
Records: 4  Duplicates: 0  Warnings: 0

mysql> insert into orders (product_id, order_date, quantity) values 
    -> (1, '2024-01-15', 2),
    -> (2, '2024-01-20', 1),
    -> (3, '2024-01-25', 1),
    -> (4, '2024-01-30', 3),
    -> (1, '2024-02-05', 3),
    -> (2, '2024-02-10', 2),
    -> (3, '2024-02-15', 4),
    -> (4, '2024-02-20', 1);
Query OK, 8 rows affected (0.00 sec)
Records: 8  Duplicates: 0  Warnings: 0


mysql> select * from orders;
+----------+------------+------------+----------+
| order_id | product_id | order_date | quantity |
+----------+------------+------------+----------+
|        1 |          1 | 2024-01-15 |        2 |
|        2 |          2 | 2024-01-20 |        1 |
|        3 |          3 | 2024-01-25 |        1 |
|        4 |          4 | 2024-01-30 |        3 |
|        5 |          1 | 2024-02-05 |        3 |
|        6 |          2 | 2024-02-10 |        2 |
|        7 |          3 | 2024-02-15 |        4 |
|        8 |          4 | 2024-02-20 |        1 |
+----------+------------+------------+----------+
8 rows in set (0.00 sec)

mysql> select * from products;
+------------+-----------+--------+
| product_id | name      | price  |
+------------+-----------+--------+
|          1 | Argan Oil | 250.00 |
|          2 | Couscous  |  50.00 |
|          3 | Amlou     | 150.00 |
|          4 | Honey     |  30.00 |
+------------+-----------+--------+
4 rows in set (0.00 sec)

mysql> select orders.order_id, products.name, orders.order_date, orders.quantity
    -> from orders
    -> join products on orders.product_id = products.product_id;
+----------+-----------+------------+----------+
| order_id | name      | order_date | quantity |
+----------+-----------+------------+----------+
|        1 | Argan Oil | 2024-01-15 |        2 |
|        5 | Argan Oil | 2024-02-05 |        3 |
|        2 | Couscous  | 2024-01-20 |        1 |
|        6 | Couscous  | 2024-02-10 |        2 |
|        3 | Amlou     | 2024-01-25 |        1 |
|        7 | Amlou     | 2024-02-15 |        4 |
|        4 | Honey     | 2024-01-30 |        3 |
|        8 | Honey     | 2024-02-20 |        1 |
+----------+-----------+------------+----------+
8 rows in set (0.00 sec)


--ghan copyiw table products lwahed table products_backup


mysql> create table products_backup as select * from products;
Query OK, 4 rows affected (0.00 sec)
Records: 4  Duplicates: 0  Warnings: 0

mysql> select * from products_backup;
+------------+-----------+--------+
| product_id | name      | price  |
+------------+-----------+--------+
|          1 | Argan Oil | 250.00 |
|          2 | Couscous  |  50.00 |
|          3 | Amlou     | 150.00 |
|          4 | Honey     |  30.00 |
+------------+-----------+--------+
4 rows in set (0.00 sec)

mysql> 
