# northwind

1.	Використавши SELECT та не використовуючи FROM вивести на екран назву виконавця та пісні, яку ви слухали останньою. Імена колонок вказати як Artist та Title. 

**SELECT "Justin Bieber" AS Artist, "Peaches" AS Title;**


2.	Вивести вміст таблиці Order Details, замінивши назви атрибутів OrderID та ProductID на OrderNumber та ProductNumber.

**SELECT OrderID AS OrderNumber, 
ProductID AS ProductNumber, 
UnitPrice, Quantity, Discount 
FROM `Order Details`;**


3.	Вивести назву всіх продуктів, що продаються у банках (jar), відсортувати за алфавітом.

**SELECT ProductName 
FROM Products 
WHERE QuantityPerUnit LIKE '%jar%' 
ORDER BY ProductName;**


4.	Вивести всі назви та кількість на складі продуктів, що належать до категорій Dairy Products, Grains/Cereals та Meat/Poultry.

**SELECT ProductName, UnitsInStock 
FROM Products 
WHERE CategoryID IN 
(SELECT CategoryID FROM Categories 
WHERE CategoryName IN ('Dairy Products', 'Grains/Cereals', 'Meat/Poultry'));**


5.	Вивести всі замовлення, де вартість одиниці товару 50.00 та вище. Відсортувати за номером, позбавитися дублікатів.

**SELECT DISTINCT * 
FROM Orders 
WHERE OrderID IN 
(SELECT OrderID FROM `Order Details` WHERE UnitPrice >= 50.00) 
ORDER BY OrderID;**


6.	Відобразити всіх постачальників, де контактною особою є власник, або менеджер і є продукти, що зараз знаходяться на стадії поставки.

**SELECT * FROM Suppliers 
WHERE ContactTitle = 'Owner' 
OR ContactTitle LIKE '%Manager%'  
AND SupplierID IN (SELECT SupplierID FROM Products WHERE UnitsOnOrder > 0);**


7.	Вивести всіх замовників з Мексики, де контактною особою є власник, а доставка товарів відбувалася через Federal Shipping.

**SELECT * FROM Customers 
WHERE Country = 'Mexico' AND ContactTitle = 'Owner' 
AND CustomerID IN 
(SELECT CustomerID FROM Orders WHERE ShipVia = 
(SELECT ShipperID FROM Shippers WHERE CompanyName = 'Federal Shipping'));**


