# northwind

**1.	Використавши SELECT та не використовуючи FROM вивести на екран назву виконавця та пісні, яку ви слухали останньою. Імена колонок вказати як Artist та Title.**

>SELECT "Justin Bieber" AS Artist, "Peaches" AS Title;


**2.	Вивести вміст таблиці Order Details, замінивши назви атрибутів OrderID та ProductID на OrderNumber та ProductNumber.**

> SELECT OrderID AS OrderNumber,  <br/>
> ProductID AS ProductNumber, <br/>
> UnitPrice, Quantity, Discount <br/>
> FROM 'Order Details';<br/>


**3.	Вивести назву всіх продуктів, що продаються у банках (jar), відсортувати за алфавітом.**

>SELECT ProductName <br/>
>FROM Products <br/>
>WHERE QuantityPerUnit LIKE '%jar%' <br/>
>ORDER BY ProductName;<br/>


**4.	Вивести всі назви та кількість на складі продуктів, що належать до категорій Dairy Products, Grains/Cereals та Meat/Poultry.**

>SELECT ProductName, UnitsInStock <br/>
>FROM Products <br/>
>WHERE CategoryID IN <br/>
>(SELECT CategoryID FROM Categories <br/>
>WHERE CategoryName IN ('Dairy Products', 'Grains/Cereals', 'Meat/Poultry'));<br/>


**5.	Вивести всі замовлення, де вартість одиниці товару 50.00 та вище. Відсортувати за номером, позбавитися дублікатів.**

>SELECT DISTINCT * <br/>
>FROM Orders <br/>
>WHERE OrderID IN <br/>
>(SELECT OrderID FROM 'Order Details' WHERE UnitPrice >= 50.00) <br/>
>ORDER BY OrderID;<br/>


**6.	Відобразити всіх постачальників, де контактною особою є власник, або менеджер і є продукти, що зараз знаходяться на стадії поставки.**

>SELECT * FROM Suppliers <br/>
>WHERE ContactTitle = 'Owner' <br/>
>OR ContactTitle LIKE '%Manager%'  <br/>
>AND SupplierID IN (SELECT SupplierID FROM Products WHERE UnitsOnOrder > 0);<br/>


**7.	Вивести всіх замовників з Мексики, де контактною особою є власник, а доставка товарів відбувалася через Federal Shipping.**

>SELECT * FROM Customers <br/>
>WHERE Country = 'Mexico' AND ContactTitle = 'Owner' <br/>
>AND CustomerID IN <br/>
>(SELECT CustomerID FROM Orders WHERE ShipVia = <br/>
>(SELECT ShipperID FROM Shippers WHERE CompanyName = 'Federal Shipping'));<br/>


