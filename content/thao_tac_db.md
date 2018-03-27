# 1 Import
chèn một lượng dữ liệu lớn có trước vào trong CSDL một cách nhanh nhất và ít tốn thời gian.
## 1.1 Import từ file text
```
mysqlimport -u user_name -p database_name /var/lib/mysql-files/table_name.txt
mysqlimport -u root -p test /var/lib/mysql-files/SinhVien.txt
```
Dữ liệu trong file text sẽ được tải vào bảng có tên cùng với tên file text. Nếu bảng chưatồn tại, chương trình sẽ tự tạo bảng mới. Định dạng của file text phải được trình bày theo quy định sau :
* Mỗi dòng dữ liệu được trình bày trên 1 dòng.
* Giá trị text phải được đóng bằng dấu nháy đơn (') hoặc nháy kép (").
* Các giá trị cách bởi dấu phẩy (,).
* Các giá trị phải được sắp theo thứ tự tương ứng
Ví dụ SinhVien.txt
```
'Chau','Viet','Cuong','6789'
```
## 1.2 Import từ file SQL
Một cách khác để import dữ liệu đó là thực thi hàng loại các câu lệnh sql từ một file *.sql (hay còn gọi là batching).
Một mẫu ví dụ về file data.sql như sau :
```
USE QLBanHang;
INSERT INTO Customers (Customer_ID, Last_Name, First_Name)
VALUES(NULL, "Nguyen Minh","Thanh");
INSERT INTO Customers (Customer_ID, Last_Name, First_Name)
VALUES(NULL, "Nguyen Thien","Nam");
INSERT INTO Customers (Customer_ID, Last_Name, First_Name)
VALUES(NULL, "Nguyen Khoa","Danh");
```
Để thực thi file sql ta sẽ sử dụng lệnh sau :
Load Data Infile filename.sql Into Table table_name;
Vd : LOAD DATA INFILE "C:\MyDocs\data.sql" INTO TABLE Orders;
Nếu muốn chỉ định file sql nằm trên máy cục bộ, cá nhân :
Load Data Local Infile filename.sql Into Table table_name;
Để thay thế các dòng giá trị trùng nhau :
Load Data Local Infile filename.sql Replace Into Table table_name;
Tuy nhiên, ta cũng có thể sử dụng phương thức Load Data này cho các file text
Load Data Infile filename.txt Into Table table_name;
Các tuỳ chọn được dùng thêm khi tải dữ liệu từ file text (được dùng sau từ khoá
Fields) :
 Terminated by char
 Enclosed by char
 Escaped by char
Vd :
LOAD DATA INFILE "Orders.txt" REPLACE INTO TABLE Orders
FIELDS TERMINATED BY ',' ENCLOSED BY '"';
Chỉ định các cột được tải dữ liệu:
vd : LOAD DATA INFILE "/home/Order.txt"
INTO TABLE Orders
(Order_Number, Order_Date, Customer_ID);
# 2 Export
## 2.1 mysqldumb
```
#Export cả db
mysqldump –u username –p database_name > filename.txt
mysqldump –u root –p test > /var/lib/mysql-files/test.txt

#Export 1 bảng
mysqldump –u username –p database_name table_name > filename.txt
mysqldump –u root –p test SinhVien > /var/lib/mysql-files/SinhVien.txt
# đọc file text vừa tạo
cat /var/lib/mysql-files/SinhVien.txt
...
-- MySQL dump 10.13  Distrib 5.5.59, for debian-linux-gnu (x86_64)
--
-- Host: localhost    Database: test
-- ------------------------------------------------------
-- Server version	5.5.59-0ubuntu0.14.04.1

/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8 */;
/*!40103 SET @OLD_TIME_ZONE=@@TIME_ZONE */;
/*!40103 SET TIME_ZONE='+00:00' */;
/*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */;
/*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;
/*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;
/*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;

--
-- Table structure for table `SinhVien`
--

DROP TABLE IF EXISTS `SinhVien`;
/*!40101 SET @saved_cs_client     = @@character_set_client */;
/*!40101 SET character_set_client = utf8 */;
CREATE TABLE `SinhVien` (
  `Ho` varchar(12) DEFAULT NULL,
  `TenDem` varchar(12) DEFAULT NULL,
  `Ten` varchar(12) DEFAULT NULL,
  `MSSV` int(11) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1;
/*!40101 SET character_set_client = @saved_cs_client */;

--
-- Dumping data for table `SinhVien`
--

LOCK TABLES `SinhVien` WRITE;
/*!40000 ALTER TABLE `SinhVien` DISABLE KEYS */;
INSERT INTO `SinhVien` VALUES ('Ngo','Van','Ma',1234),('Nguyen','Vi','Vu',3434),('Nong','Duc','Tuan',1424);
/*!40000 ALTER TABLE `SinhVien` ENABLE KEYS */;
UNLOCK TABLES;
/*!40103 SET TIME_ZONE=@OLD_TIME_ZONE */;

/*!40101 SET SQL_MODE=@OLD_SQL_MODE */;
/*!40014 SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS */;
/*!40014 SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS */;
/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;
/*!40111 SET SQL_NOTES=@OLD_SQL_NOTES */;

-- Dump completed on 2018-03-27 11:02:28
...

```
## 2.2 Lệnh Select ... into outfile
```
Select column_names from table_name Into Outfile filename.txt[ Fields Terminated by char 
Enclosed by char Lines Terminated by char ];
SELECT * FROM Customer INTO OUTFILE '/var/lib/mysql-files/SinhVien.txt' FIELDS TERMINATED BY ',' 
ENCLOSED BY ' " ' LINES TERMINATED BY '\r\n';
#Nội dụng file vừa tạo
...
"Ngo","Van","Ma","1234"
"Nguyen","Vi","Vu","3434"
"Nong","Quoc","Tuan","1424"
...
```
## 2.3 mysql client
```
mysql –e "Select column_names from table_name" --skip-column-names \ database_name > filename.txt
mysql -e "SELECT * FROM SinhVien" --skip-column-names \ test > SinhVien.txt
```
