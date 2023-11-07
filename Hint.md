**Выбор, смена базы данных**
use shop(имя базы);

-- создание базы данных shop

CREATE SCHEMA `shop` DEFAULT CHARACTER SET utf8 COLLATE utf8_bin ;

**отобразить все базы данных**
show databases;

-- работать с указанной базой данных (после выполнения этой команды вместо `shop`.`category` можно будет писать просто category )

use shop;

**создание таблицы "категория товаров"**

CREATE TABLE `shop`.`category` (
  
  `id` INT NOT NULL,
  
  `name` VARCHAR(128) NOT NULL,
  
  `discount` TINYINT NOT NULL,
  
PRIMARY KEY (`id`));

**добавление нового столбца**

ALTER TABLE `shop`.`category`  (изменить таблицу  `имя базы` `имя таблицы`)

ADD COLUMN `alias_name` VARCHAR(128) NULL AFTER `discount`;

-- посмотреть структуру таблицы
SHOW COLUMNS FROM category;


-- удалить таблицу
DROP TABLE `shop`.`category`;


-- удалить базу данных



DROP DATABASE `shop`;

ALTER TABLE `shop`.`brends` 
RENAME TO  `shop`.`brand` ; переименование таблицы

Удаление колонки

Alter table category drop column `curl_girl`;

-- == **SELECT** == --
-- вывести все категории товаров
SELECT * FROM category;

-- == **WHERE** == --
-- вывести категорию товаров с идентификатором, равным 3
SELECT * FROM category WHERE id = 3;

-- вывести категории товаров, у которых скидка не равна 0
SELECT * FROM category WHERE discount <> 0;

-- вывести категории товаров, у которых скидка больше 5
SELECT * FROM category WHERE discount > 5;

-- вывести категории товаров, у которых скидка больше 5 и меньше 15
SELECT * FROM category WHERE (discount > 5) AND (discount < 15);

-- вывести категории товаров, у которых скидка меньше 5 или больше или равен 10
SELECT * FROM category WHERE (discount < 5) OR (discount >= 10);

-- вывести категории товаров, у которых скидка не меньше 5
SELECT * FROM category WHERE NOT (discount < 5);

-- вывести категории товаров, у которых есть псевдоним
SELECT * FROM category WHERE alias_name IS NOT NULL;

-- вывести категории товаров, у которых нет псевдонима
SELECT * FROM category WHERE alias_name IS NULL;

**ЗАПОЛНЕНИЕ ТАБЛИЦЫ**

use shop;

INSERT INTO category (id, name, discount, alias_name) VALUES (3, 'Женская обувь', 10, NULL);

INSERT INTO category (id, name, discount, alias_name) VALUES (4, 'Мужская обувь', 15, 'mans shoes');

**Автоматическое добавление id номера**
ALTER TABLE `shop`. `category`
CHANGE COLUMN `id` `id` INT(11) NOT NULL AUTO_INCREMENT;

Добавление с автоматическим ID
INSERT INTO category (name, discount) VALUES ('Шляпы', 0);


**DISTINCT, ORDER BY, LIMIT**

-- == SELECT <столбец> == --
-- вывести названя всех категорий товаров
SELECT name FROM category;

-- вывести названя и скидки товаров
SELECT discount, name  FROM category;

-- вывести все скидки
SELECT discount FROM category;


-- DISTINCT
-- вывести все уникальные значения скидок
SELECT DISTINCT discount FROM category;

-- == ORDER BY == --
-- вывести все категории товаров, и отсортировать их по размеру скидки
SELECT * FROM category ORDER BY discount; -- ASC;

-- == ORDER BY == --
-- вывести все категории товаров, и отсортировать их по размеру скидки в обратном порядке
SELECT * FROM category ORDER BY discount DESC;

-- вывести все категории товаров с ненулевой скидкой, и отсортировать их по размеру скидки в обратном порядке
SELECT * FROM category WHERE discount <> 0 ORDER BY discount DESC;

-- == LIMIT == --
-- вывести первые 2 категории товара
SELECT * FROM category LIMIT 2;

-- вывести первые 2 категории товара со скдикой не равной нулю
SELECT * FROM category WHERE discount <> 0 LIMIT 2;

UPDATE._DELETE

USE shop;

-- Получить название бренда с идентификатором 3;
SELECT name FROM brand WHERE id = 3;


-- Получить первые 2 типа товара;
SELECT * FROM product_type LIMIT 2;


-- Получить все категории товаров со скидкой < 10%, и отсортировать их по названию
SELECT * FROM category where discount < 10 ORDER BY name;

UPDATE category SET name = 'Головные уборы' WHERE id = 5;

SELECT * FROM category;

UPDATE category SET discount = 3 WHERE id IN ( 2 , 5 );

DELETE FROM category WHERE id = 5;

-- Проверка домашнего задания

use shop;

-- С помощью команды UPDATE заполнить alias_name для всех категорий
UPDATE category set alias_name = 'women''s clothing' WHERE id = 1;
UPDATE category set alias_name = 'man''s clothing' WHERE id = 2;
UPDATE category set alias_name = 'women''s shoes' WHERE id = 3;


-- Добавить новый бренд «Тетя Клава Company»
INSERT INTO category (name, discount) VALUES ('Тетя Клава Company', 0);

-- Удалить этот бренд
DELETE FROM category WHERE id = 5;

**Объединение  таблиц**
use shop;
SELECT * FROM product;
select * from category where id = 1 or id = 3 or id = 2;
select * from category where id >= 1 and id <=3;
select * from category where id IN (1, 2, 3);


INNER_JOIN


select * from product 
	inner join category on product.category_id = category.id;
    
select product.id, price, name from product  
	inner join category on product.category_id = category.id; 
    
select * from category  
	 inner join product on product.category_id = category.id;
     

select * from product  
	inner join category on product.category_id = category.id
    where discount <= 5;
    -- where price < 10000;
    
    
select * from product  
	inner join category on product.category_id = category.id 
    inner join brand on brand.id = product.brand_id
    inner join product_type on product_type.id = product.product_type_id;
    
select product.id, brand.name, product_type.name, category.name, product.price from product  
	inner join category on product.category_id = category.id 
    inner join brand on brand.id = product.brand_id
    inner join product_type on product_type.id = product.product_type_id; 



    
15._LEFT_JOIN._RIGHT_JOIN.
use shop;

select * from category
	left join product on product.category_id = category.id;
    
select * from category
	left join product on product.category_id = category.id
    where product.id is null;
    
select * from category
	right join product on product.category_id = category.id
    where product.id is null;    


16._UNION
use shop;

select * from product_type where id = 1
union
select * from product_type where id = 2;


select * from `order`
	left join order_products on order_products.order_id = `order`.id
    left join product on order_products.product_id = product.id
    
union

select * from `order`	
	inner join order_products on order_products.order_id = `order`.id
    right join product on order_products.product_id = product.id
    where `order`.id is null;
    
17._Агрегирующие_функции
use shop;

SELECT * FROM product;
SELECT count(*) FROM product where product.price < 10000;
SELECT sum(price) as total_price, min(price) as min_price, max(price) as max_price FROM product;

18._GROUP_BY
use shop;

insert into order_products (order_id, product_id, `count`) values (2, 3, 2);
insert into order_products (order_id, product_id, `count`) values (2, 4, 3);


SELECT * FROM shop.`order`;





SELECT sum(price * `count`) as total_price from `order` 
	inner join order_products on order_products.order_id = `order`.id
    inner join product on product.id = order_products.product_id
    where `order`.id = 2;
    
    
    
SELECT `order`.user_name, sum(price * `count`) as total_price  from `order` 
	inner join order_products on order_products.order_id = `order`.id
    inner join product on product.id = order_products.product_id
    group by `order`.user_name;
    
SELECT `order`.user_name, max(price), sum(`count`)  from `order` 
	inner join order_products on order_products.order_id = `order`.id
    inner join product on product.id = order_products.product_id
    group by `order`.user_name;
    
   
19._HAVING

use shop;

insert into order_products (order_id, product_id, `count`) values (2, 3, 2);
insert into order_products (order_id, product_id, `count`) values (2, 4, 3);


SELECT * FROM shop.`order`;

SELECT sum(price * `count`) as total_price from `order` 
	inner join order_products on order_products.order_id = `order`.id
    inner join product on product.id = order_products.product_id
    where `order`.id = 2;
  
    
    
SELECT `order`.user_name, sum(price * `count`) as total_price  from `order` 
	inner join order_products on order_products.order_id = `order`.id
    inner join product on product.id = order_products.product_id
    group by `order`.user_name;
    
SELECT `order`.user_name, max(price), sum(`count`) as order_count from `order` 
	inner join order_products on order_products.order_id = `order`.id
    inner join product on product.id = order_products.product_id
    -- where user_name like 'Р’%'
    
    group by `order`.user_name
    having order_count >= 5; 
   
    
20._TRANSACTION
insert into `shop`.`user_bank_account` (id, money, user_name) values ('1', '100', 'Р”РјРёС‚СЂРёР№');
insert into `shop`.`user_bank_account` (id, money, user_name) values ('2', '200', 'Р•РІРіРµРЅРёР№');

select * from `shop`.`user_bank_account`;

start transaction;
	update `shop`.`user_bank_account` set money = money - 100 where id = 1;
    update `shop`.`user_bank_account` set money = money + 100 where id = 2;
commit;
