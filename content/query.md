# 1.Các từ khóa
|Create| On| Limit| Into| Or|From|
|------|---|------|-----|---|----|
|Alter |Order by| Drop| Where| Like|Right Join|
|Insert| Join| Delete |Group by |As|  Set|
|Select| Cross Join| Update| Left Join| And||

Cấu trúc của một câu lệnh truy vấn dữ liệu như sau :
```
SELECT * | column_names
FROM table_names
WHERE criterias
GROUP BY column_names
ORDER BY column_names
```
# 2.Truy vấn
## 2.1.Chọn dữ liệu
```
#Chọn tất cả cột trong bảng
SELECT * FROM table_name;
SELECT * FROM Customers;

#Chọn một vài cột trong bảng
SELECT column_names FROM table_name;
SELECT Lastname, Firstname from Customers;

#Chọn dữ liệu với điều kiện trích lọc dữ liệu
SELECT * | column_names FROM table_name WHERE criteria_1 and|or criteria_2
Select * from Customers Where Fistname="Thành" or Fistname="Nam";
Select * from Customers Where Fistname="Thành" and Lastname="Nguyễn";

#Truy vấn gần giống với chuỗi kí tự, ta sử dụng kí tự thay thế (%)
<trả về những người có firstname bắt đầu với một ký tự 'O'>
SELECT * FROM Persons WHERE FirstName LIKE 'O%'
<trả về những người có firstname kết thúc với một ký tự 'a'>
SELECT * FROM Persons WHERE FirstName LIKE '%a'
<trả về những người có firstname chứa mẫu 'la'>
SELECT * FROM Persons WHERE FirstName LIKE '%la%'

#Truy vấn kết quả nằm trong khoảng
SELECT column_name FROM table_name WHERE column_name BETWEEN value1 AND value2;
select * from customers where id between 1 and 5;

#Truy vấn hiện kết quả không trùng nhau
SELECT DISTINCT column-name FROM table-name;
select distinct Lastname from customers;

#Giới hạn số lượng dòng kết quả
SELECT * | column_names FROM table_name WHERE criterias LIMIT number;
Select * from Customers Limit 5;

#Chọn dữ liệu từ nhiều bảng
SELECT * | column_names FROM table_name_1, table_name_2 WHERE table_relationship
Select * from Customers,Orders Where Customers.Customer_ID = Orders. Customer_ID;

#Sắp xếp dữ liệu
SELECT *|column_names FROM table_names WHERE table_relationships and|or criterias ORDER BY column_names ASC|DESC
Select * from Customers , Orders  Where Customers.Customer_ID = Orders.Order_ID Order by OrderID;

#Đặt tên mới cho cột - bảng :
Table_name AS new_table_name
Column_name AS new_column_name
Select Lastname as Ho, Firstname as Ten, Order_ID as MaHoaDon From Customers As C, Orders As O
  Where C.Customer_ID = O. Customer_ID Order by Order_ID;
```
## 2.2.Truy vấn thống kê dữ liệu
```
SELECT Aggregate_function(#column_name)FROM table_names;
Select Count(Order_ID) From  Orders ;
```
Các Aggregate_function (hàm thống kê) : 

|Hàm|Mô tả|
|---|-----|
|Max()|max|
|Min()|min|
|Sum()|tổng|
|Avg()|trung bình|
|Std()||

## 2.3.Truy vấn lồng
Khi một truy vấn được sử dụng trong một truy vấn khác, thì đó là câu truy vấn lồng.
Câu truy vấn bên trong gọi là truy vấn con (subquery).
```
select customer_id from (select * from customer where Lastname='Nguyen');
```
## 2.4.Mệnh đề Join
INNER JOIN trả về tất cả các hàng từ hai bảng khi điều kiện được so trùng. Nếu các hàng trong bảng 1 không sotrùng trong bảng Orders, hàng đó sẽ không được liệt kê ra.
```
SELECT field1, field2, field3 FROM first_table INNER JOIN second_table
  ON first_table.keyfield = second_table.foreign_keyfield
Select Lastname, Firstname, Order_ID From Customer as C INNER Join Order as O 
  On C.Customer_ID = O.Customer_ID;
```
LEFT JOIN trả về tất cả các hàng từ bảng thứ nhất , cho dù nó không được so trùng trong bảng thứ hai .Nếu các hàng trong bảng1 không so trùng trong bảng 2, những hàng này cũng được liệt kê.
```
Select Lastname, Firstname, Order_ID From Customer as C LEFT Join Order as O 
  On C.Customer_ID =O.Customer_ID;
```
Tương tự cho RIGHT JOIN.
## 2.5.Truy vấn chèn dữ liệu
```
INSERT INTO table_name(column_names) VALUES(column_values)
Insert Into Customers(Customer_ID, Firstname, Lastname) Values(null, "Thành","Nguyễn Minh");
```
## 2.6.Truy vấn xoá dữ liệu
```
DELETE FROM table_names WHERE criterias;
Delete From Customers Where Customer_ID=2;
```
## 2.7.Truy vấn cập nhật dữ liệu
```
UPDATE table_name SET column_name=value,… WHERE criterias;
Update From Customers Set Firstname="Danh" Where Customer_ID=3;
```
# 3.Hàm và toán tử
## 3.1.Toán tử
### Phép cộng / trừ / nhân / chia : dùng để tính toán 2 cột dữ liệu số
```
Select ThanhTien + Thue  From Orders;
Select ThanhTien - GiamGia From Orders;
Select SoLuong * DonGia From Orders;
```
### Phép ghép chuỗi 
`Select Lastname & Firstname as Fullname From Customers;`
### Phép IN : xác định một giá trị có nằm trong một tập hợp
## 3.2.Hàm toán học
### Mod (số bị chia, số chia) : lấy phần dư của phép chia.<br>
`Select Mod(ThanhTien,2) From Orders as HoaDon;`
### Round(số, vị trí làm tròn) : hàm làm tròn số.<br>
`Select Round(ThanhTien,1) From Orders as HoaDon;`

Vị trí làm tròn :<br>
2 : 2 số thập phân<br>
1 : 1 số thập phân<br>
0 : 0 số thập phân<br>
-1 : hàng đơn vị<br>
-2 : hàng chục<br>
## 3.3.Hàm điều kiện
### IF(logic_expression,true_result,false_result) : hàm kiểm tra điều kiện đúng/sai.
`Select If(SoLuong>20,5%,2%) as GiamGia From Orders;`
### IFNULL(result_1,result_2) : hàm trả về kết quả result_1 nếu nó không null ngược lại sẽ trả về result_2.
`Select IfNull(10/0,1) as Exam`
### CASE value WHEN expression THEN result_1 ELSE result_2 : hàm trả về result_1 khi expression đúng, ngược lại trả về result_2.
`Select CASE 1 WHEN Column1="Y" THEN 1 WHEN Column2="Y" THEN 2 WHEN Column3="Y" THEN 3 ELSE "NONE";`
## 3.4.Hàm logic
Các hàm logic được dùng trên các biểu thức điều kiện (logic_expression).
* AND : phép và.
* OR : phép hoặc.
* NOT : phép phủ định.
## 3.5 Hàm thời gian

|Hàm|Mô tả|
|---|-----|
|MONTHNAME(date)|trả về tên tháng của date|
|DAYOFYEAR(date)|trả về số ngày tính từ đầu năm đến date|
|DAYOFMONTH(date)|trả về số ngày tính từ đầu tháng đến date|
|DAYOFWEEK(date)|trả về số ngày tính từ đầu tuần đến date.|
|YEAR(date)|trả về năm của date.|
|QUARTER(date)|trả về quý của date.|
|MONTH(date)|trả về tháng của date.|
|DAY(date)| trả về ngày của date.|
|WEEK(date)|trả về số tuần của date tính từ ngày đầu tiên của năm.|
|YEARWEEK(date)|trả về số tuần của date tính từ ngày đầu tiên của năm.|
|NOW() , SYSDATE(), CURRENT_TIMESTAMP|trả về ngày giờ hệ thống.|
