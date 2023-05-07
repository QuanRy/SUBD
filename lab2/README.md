
|   Лабораторная работа | ФИО     | *Novruzov* |
| :-------------: |:--------------|  -----:     |
|        №2       | Группа        | *IVT-363*   |
|       СУБД      | Преподаватель |             |
| `Магазин одежды`| Дата сдачи    |             |
<div id="header" align="center">
  <H1>Ход работы:</H1>
</div>
SQL-запросы
CREATE TABLE seasons (
id INTEGER PRIMARY KEY,
season TEXT,
description TEXT
);

CREATE TABLE types (
id INTEGER PRIMARY KEY,
type TEXT,
description TEXT
);

CREATE TABLE products (
id INTEGER PRIMARY KEY,
name TEXT,
price TEXT,
color TEXT,
seasons_id INTEGER,
types_id INTEGER,
FOREIGN KEY (seasons_id) REFERENCES seasons (id) ON DELETE CASCADE,
FOREIGN KEY (types_id) REFERENCES types (id) ON DELETE CASCADE

);



CREATE TABLE sizes (
id INTEGER PRIMARY KEY,
size_cm TEXT,
size TEXT
);

CREATE TABLE brands (
id INTEGER PRIMARY KEY,
name TEXT,
license_price TEXT
);

CREATE TABLE warehouses (
id INTEGER PRIMARY KEY,
name TEXT,
address TEXT,
quantity INTEGER
);



CREATE TABLE product_size (
product_id INTEGER,
size_id INTEGER,
FOREIGN KEY (product_id) REFERENCES products (id) ON DELETE CASCADE,
FOREIGN KEY (size_id) REFERENCES sizes (id) ON DELETE CASCADE
);

CREATE TABLE product_brand (
product_id INTEGER,
brand_id INTEGER,
FOREIGN KEY (product_id) REFERENCES products (id) ON DELETE CASCADE,
FOREIGN KEY (brand_id) REFERENCES brands (id) ON DELETE CASCADE
);

CREATE TABLE product_warehouse (
product_id INTEGER,
warehouse_id INTEGER,
FOREIGN KEY (product_id) REFERENCES products (id) ON DELETE CASCADE,
FOREIGN KEY (warehouse_id) REFERENCES warehouses (id) ON DELETE CASCADE
);

INSERT INTO brands (name, license_price) VALUES 
('nike', '2000000'),
('Louis Viton', '4400000'),
('Adidas', '100000'),
('Burberry', '2405000'),
('Bottega Veneta', '3400000'),
('Nike Jordan', '8400000'),
('Demix', '900000'),
('Kappa', '400000'),
('Fendi', '9700000'),
('Gucci', '95400000'),
('Levis', '2300000');

INSERT INTO seasons (season, description) VALUES 
('Зима', 'Очень холодный климат (-50...-30)'),
('Зима', 'Холодный климат (-30...-18)'),
('Зима', 'Теплая зима (-15...0)'),
('Весна', 'Холодная весна (-15...-8)'),
('Весна', 'Теплая весна(-7...+15)'),
('Лето', 'Теплое лето(+15...+28)'),
('Лето', 'Очень жаркое лето(+28...+40)'),
('Осень', 'Холодная осень (-20...-10)'),
('Осень', 'Обычная осень (-10...0)'),
('Осень', 'Жаркая осень (0...25)');

INSERT INTO types (type, description) VALUES 
('Кроссовки', 'Для занятия спортом'),
('Сапоги', 'Для походов'),
('Сандали', 'Для прогулок'),
('Спортивные штаны', 'Для занятия спортом'),
('Брюки', 'Для официального стиля'),
('Футболка', 'Для отдыха'),
('Кофта', 'Для отдыха'),
('Свитшот', 'Праздничный'),
('Куртка', 'Для отдыха'),
('Пуховик', 'Горнолыжный, для очень холодного климата');

INSERT INTO sizes (size_cm, size) VALUES 
('28', '44'),--обувь
('27', '43'),--обувь
('26', '42'),--обувь
('60', 'L'),--штаны
('64', 'XL'),--штаны
('70', 'L'),--футболка
('77', 'L'),--кофта
('80', 'XL'),--кофта
('70', 'M'),--куртка
('77', 'XL');--куртка

INSERT INTO warehouses (name, address, quantity) VALUES 
('Складика', 'Г. Волгоград, Рябиновая ул., 10, микрорайон', '11'),
('СкладСтор', 'Г. Волгоград, вл5, д. Саларьево', '12'),
('Волгоградский складской альянс', 'Г. Волгоград, ш. Авиаторов, 15', '122'),
('Оптовка', 'Г. Волгоград, Пушкина 39', '21'),
('Хоумсклад', 'Г. Волгоград, Кропоткина 5', '55'),
('Склад 24', 'Г. Волгоград, Дмитровское 6', '144'),
('Складской Комплекс', 'Г. Волгоград, Качуевской, 2д.', '12'),
('Raсклад', 'Г. Волгоград, Репищева ул., 14АП', '14'),
('МойСклад24.рф', 'Г. Волгоград, Менжинского 2', '43'),
('Ваш склад 24', 'Г. Волгоград, Набережная 2', '123');

INSERT INTO products (name, price, color, seasons_id ,types_id) VALUES 
('Jordan 1 mid all star karbon','40000','черно-белый','9','1'),
('Nike x-trail','15000','оранжевый','9','1'),
('Addidas T-short','1499','зеленый','6','6'),
('Nike dunk','5499','белые','9','1'),
('Nike airmax 95','7699','белые','9','1'),
('Burrberry x','16999','красно-коричневый','2','10'),
('Levis sweet','4300','серый','9','8'),
('Nike 700','14000','белый','8','9');

INSERT INTO products (name, price, color, seasons_id ,types_id) VALUES 
('Jordan Thirts','10000','черно-белый','7','6'),
('Adiddas train','15000','оранжевый','7','6');

INSERT INTO product_brand (product_id,brand_id) VALUES 
('1','6'),
('2','1'),
('3','3'),
('4','1'),
('5','1'),
('6','4'),
('7','10'),
('8','1'),
('9','6'),
('10','3');

INSERT INTO product_size (product_id,size_id) VALUES 
('1','1'),
('2','1'),
('3','6'),
('4','1'),
('5','1'),
('6','10'),
('7','6'),
('8','6'),
('9','6'),
('10','6');

INSERT INTO product_warehouse (product_id,warehouse_id) VALUES 
('1','1'),
('2','2'),
('3','3'),
('4','4'),
('5','5'),
('6','6'),
('7','7'),
('8','8'),
('9','9'),
('10','10');
Запросы:
/*
SELECT * FROM products
JOIN product_warehouse on products.id = product_warehouse.product_id
JOIN warehouses on warehouses.id = product_warehouse.warehouse_id;
*/


--ОБЩИЙ ВЫВОД
SELECT * FROM products
INNER JOIN product_warehouse on products.id = product_warehouse.product_id
INNER JOIN warehouses on warehouses.id = product_warehouse.warehouse_id

INNER JOIN product_brand on products.id = product_brand.product_id
INNER JOIN brands on brands.id = product_brand.brand_id

INNER JOIN product_size on products.id = product_size.product_id
INNER JOIN sizes on sizes.id = product_size.size_id

INNER JOIN seasons on seasons.id = products.seasons_id
INNER JOIN types on types.id = products.types_id
;



-- показать количество определенного товара на складе
SELECT warehouses.quantity as "Количество товара" FROM products
JOIN product_warehouse on products.id = product_warehouse.product_id
JOIN warehouses on warehouses.id = product_warehouse.warehouse_id
where products.name == 'Jordan 1 mid all star karbon';

SELECT warehouses.quantity as "Количество товара" FROM products
JOIN product_warehouse on products.id = product_warehouse.product_id
JOIN warehouses on warehouses.id = product_warehouse.warehouse_id
where products.name == 'Addidas T-short';
Image

--показать в каком складе находится выбранный товар
SELECT warehouses.name FROM products
INNER JOIN product_warehouse on products.id = product_warehouse.product_id
INNER JOIN warehouses on warehouses.id = product_warehouse.warehouse_id

INNER JOIN product_brand on products.id = product_brand.product_id
INNER JOIN brands on brands.id = product_brand.brand_id

INNER JOIN product_size on products.id = product_size.product_id
INNER JOIN sizes on sizes.id = product_size.size_id
WHERE products.name == 'Nike x-trail'
;


--вывести названия товаров, которых осталось меньше 20
SELECT products.name FROM products
INNER JOIN product_warehouse on products.id = product_warehouse.product_id
INNER JOIN warehouses on warehouses.id = product_warehouse.warehouse_id

INNER JOIN product_brand on products.id = product_brand.product_id
INNER JOIN brands on brands.id = product_brand.brand_id

INNER JOIN product_size on products.id = product_size.product_id
INNER JOIN sizes on sizes.id = product_size.size_id
WHERE warehouses.quantity < 20;
;

--вывести товары и их кол-ва оранжевого цвета
SELECT products.name, warehouses.quantity FROM products
INNER JOIN product_warehouse on products.id = product_warehouse.product_id
INNER JOIN warehouses on warehouses.id = product_warehouse.warehouse_id

INNER JOIN product_brand on products.id = product_brand.product_id
INNER JOIN brands on brands.id = product_brand.brand_id

INNER JOIN product_size on products.id = product_size.product_id
INNER JOIN sizes on sizes.id = product_size.size_id
WHERE products.color == 'оранжевый';

-- вывести названия всех товаров типа кросовок
SELECT products.name as "Название", products.price as "цена" FROM products
INNER JOIN product_warehouse on products.id = product_warehouse.product_id
INNER JOIN warehouses on warehouses.id = product_warehouse.warehouse_id

INNER JOIN product_brand on products.id = product_brand.product_id
INNER JOIN brands on brands.id = product_brand.brand_id

INNER JOIN product_size on products.id = product_size.product_id
INNER JOIN sizes on sizes.id = product_size.size_id

INNER JOIN seasons on seasons.id = products.seasons_id
INNER JOIN types on types.id = products.types_id
WHERE types.type == 'Кроссовки';

-- Crud
UPDATE brands set name = 'Nike SB' where name = 'nike';
DELETE FROM products where products.price = 15000;
DELETE from seasons where id = 1;




