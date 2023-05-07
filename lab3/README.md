|   Лабораторная работа | ФИО     | *Novruzov* |
| :-------------: |:--------------|  -----:     |
|        №2       | Группа        | *IVT-363*   |
|       СУБД      | Преподаватель |             |
| `Магазин одежды`| Дата сдачи    |             |
<div id="header" align="center">
  <H1>Ход работы:</H1>
</div>

<div id="header" align="center">
  <img src="https://github.com/QuanRy/SUBD/blob/Novruzov-(serega854)/lab3/img/111.PNG" width="500"/>

# ----------------------------------- Библиотеки -----------------------------------#
import sqlite3
import time

# ----------------------------------- Cоединение с БД -----------------------------------#
conn = sqlite3.connect('sqlite.db')
cursor = conn.cursor()

# ----------------------------------- Приветствие -----------------------------------#
flag_cycle = 0

print("\n\n Подключение к БД установлено.\n")

# ------------------------- Работа с запросами к БД по выбору пользователя -------------------------#

while flag_cycle != 1:
    print("   Выберите запрос:\n")
    print(" 1. вывести названия товаров, которых осталось меньше введенного числа")
    print(" 2. показать количество определенного товара на складе")
    print(" 3. добавить определенный бренд")
    print(" 4. изменить определенный бренд")
    print(" 5. удалить определенный бренд")

    choice_querry = input("\n Ваш выбор: ")
    print()

    index = 1

    if choice_querry == "1":
        i = int(input("Введите количество товара: "))

        cursor.execute(
            "SELECT products.name FROM products INNER JOIN product_warehouse on products.id = product_warehouse.product_id INNER JOIN warehouses on warehouses.id = product_warehouse.warehouse_id INNER JOIN product_brand on products.id = product_brand.product_id INNER JOIN brands on brands.id = product_brand.brand_id INNER JOIN product_size on products.id = product_size.product_id INNER JOIN sizes on sizes.id = product_size.size_id WHERE warehouses.quantity < " + str(
                i) + ";")

        result1 = cursor.fetchall()

        # вывести названия товаров, которых осталось меньше 30
        print("Количество товара  которого меньше " + str(i) + " :")
        str1 = str(result1).replace("[", "").replace("(", "").replace("\'", "").replace(")", "\n").replace(",",
                                                                                                           "").replace(
            "]", "")

        print(str1)

    if choice_querry == "2":
        i = input("Введите название товара: ")

        cursor.execute(
            "SELECT warehouses.quantity FROM products JOIN product_warehouse on products.id = product_warehouse.product_id JOIN warehouses on warehouses.id = product_warehouse.warehouse_id where products.name == \"" + str(
                i) + "\";")

        result1 = cursor.fetchall()

        # показать количество определенного товара на складе
        print("Количество товара и название склада(складов) где он находится : " + str(i) + " :")

        print(str(result1).replace("[", "").replace("(", "").replace(",", "").replace("]", "").replace(")", ""))

    if choice_querry == "3":
        i = input("Введите название бренда: ")
        h = int(input("Введите цену лицензии бренда: "))

        cursor.execute("INSERT INTO brands (name, license_price) VALUES (\"" + str(i) + "\"," + "\"" + str(h) + "\");")

        res2 = cursor.execute("SELECT * FROM brands;")
        r = res2.fetchall()
        r1 = str(r).replace("[","").replace("(","").replace(","," ").replace("'","").replace(")","\n").replace("]","")
        # показать количество определенного товара на складе
        print(r1)

    if choice_querry == "4":
        where = input("Введите название бренда для изменения: ")

        print("Выбранный товар: ")
        res2 = cursor.execute("SELECT * FROM brands WHERE name = \"" + str(where) +"\";")
        r = res2.fetchall()
        r1 = str(r).replace("[","").replace("(","").replace("\'","").replace(")","\n").replace("]","")

        print(r1)

        name = input("Введите название бренда для изменения: ")
        i = input("Введите цену бренда для изменения: ")


        res3 = cursor.execute("UPDATE brands SET name = \"" + str(name) + "\" , liCense_price = \"" +  str(i) + "\" where name = \"" + str(where)   +"\";")
        r3 = res3.fetchall()


        res4 = cursor.execute("SELECT * FROM brands;")
        r5 = res4.fetchall()
        r7 = str(r5).replace("[","").replace("(","").replace("\'","").replace(")","\n").replace("]","").replace(",","")

        print(r7)

    if choice_querry == "5":
        i = input("Введите название бренда для удаления: ")

        res24 = cursor.execute("DELETE FROM brands where name = \"" + str(i) + "\";")
        res24.fetchall()

        res77 = cursor.execute("SELECT * FROM brands;")
        r89 = res77.fetchall()
        r25 = str(r89).replace("[","").replace("(","").replace("\'","").replace(")","\n").replace("]","").replace(",","")
        print(r25)


# ------------------------------ Закрытие соединения с БД ------------------------------#
conn.close()

Примеры работы программы:


<div id="header" align="center">
  <img src="https://github.com/QuanRy/SUBD/blob/Novruzov-(serega854)/lab3/img/222.PNG" width="500"/>
  
<div id="header" align="center">
  <img src="https://github.com/QuanRy/SUBD/blob/Novruzov-(serega854)/lab3/img/333.PNG" width="500"/>
  
<div id="header" align="center">
  <img src="https://github.com/QuanRy/SUBD/blob/Novruzov-(serega854)/lab3/img/444.PNG" width="500"/>
