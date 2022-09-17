# northwind

### ER-diagram

<img width="955" alt="Снимок экрана 2022-09-17 в 15 50 46" src="https://user-images.githubusercontent.com/85234616/190857749-12619b9e-969f-46d1-8670-d2ebb573096e.png">

### Tasks

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

**8.	Знайти всіх співробітників, що ніколи не надавали знижок. Навіть якщо такі на даний момент відсутні.**

>SELECT e.EmployeeID, SUM(od.Discount) FROM Employees e <br/>
>JOIN Orders o ON e.EmployeeID = o.EmployeeID <br/>
>JOIN 'Order Details' od ON od.OrderID = o.OrderID <br/>
>GROUP BY e.EmployeeID HAVING SUM(od.Discount) = 0; <br/>

**9.	Показати всі персональні дані з бази Northwind: повне ім’я, країну, місто, адресу, телефон. Звернути увагу, що ця інформація присутня в різних таблицях.**

>SELECT ContactName AS FUllName, Address, City, Country, Phone FROM Suppliers <br/>
>UNION ALL <br/>
>SELECT CONCAT(FirstName, ' ', LastName) AS FullName,  <br/>
>Address, City, Country, HomePhone AS Phone FROM Employees <br/>
>UNION ALL <br/>
>SELECT ContactName AS FUllName, Address, City, Country, Phone FROM Customers; <br/>

**10.	Відобразити список всіх країн та міст, куди компанія робила відправлення. Позбавитися порожніх значень на дублікатів.**

>SELECT DISTINCT ShipCity, ShipCountry FROM Orders  <br/>
>WHERE ShipCity IS NOT NULL and ShipCountry IS NOT NULL; <br/>

**11.	Використовуючи базу Northwind вивести в алфавітному порядку назви продуктів та їх сумарну кількість в замовленнях.**

>SELECT p.ProductName, SUM(Quantity) AS GeneralQuantity FROM 'Order Details' od  <br/>
>JOIN Products p ON p.ProductID = od.ProductID <br/>
>GROUP BY p.ProductName <br/>
>ORDER BY p.ProductName; <br/>

**12.	Вивести імена всіх постачальників та сумарну вартість їх товарів, що зараз знаходяться на складі Northwind за умови, що ця сума більше $1000.**

>SELECT s.CompanyName, SUM(UnitPrice * UnitsInStock) AS TotalPrice FROM Suppliers s <br/>
>JOIN Products p ON p.SupplierID = s.SupplierID <br/>
>GROUP BY s.CompanyName HAVING TotalPrice > 1000; <br/>

**13.	Знайти кількість замовлень, де фігурують товари з категорії «Сири». Результат має містити дві колонки: опис категорії та кількість замовлень.**

>SELECT Description,  SUM(Quantity) FROM Categories c <br/>
>JOIN Products p ON p.CategoryID = c.CategoryID <br/>
>JOIN 'Order Details' od ON p.ProductID = od.ProductID <br/>
>GROUP BY Description HAVING Description LIKE "%Cheese%"; <br/>

**14.	Відобразити всі імена компаній-замовників та загальну суму всіх їх замовлень, враховуючи кількість товару та знижки. Показати навіть ті компанії, у яких замовлення відсутні. Позбавитися від відсутніх значень замінивши їх на нуль. Округлити числові результати до двох знаків після коми, відсортувати за алфавітом.**

>SELECT CompanyName, IFNULL(ROUND(SUM(UnitPrice * Quantity * (1 - Discount)), 2), 0) AS OrderSum FROM Customers c <br/>
>JOIN Orders o ON c.CustomerID = o.CustomerID   <br/>
>JOIN 'Order Details' od ON o.OrderID = od.OrderID <br/>
>GROUP BY CompanyName; <br/>

