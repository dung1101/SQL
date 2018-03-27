# 1 Định nghĩa
Bảng là nơi lưu trữ dữ liệu.Mỗi một dòng (row) chứa 1 thể hiện riêng của đối tượng nào đó.Mỗi cột là một thông tin về đối tượng(field).
# 2 Kiểu dữ liệu
## 2.1 Kiểu số
|Tên kiểu|Bộ nhớ(Byte)|Tên kiểu|Bộ nhớ(Byte)|
|--------|------------|--------|------------|
|Tinyint|1|Bigint|8|
|Smallint|2|float(M,D)|4|
|Mediumint|3|double(M,D)|8|
|Int|4|Decimal(M,D)|M+2|
## 2.2 Kiểu chuỗi
|Tên kiểu|Kích thước tối đa|Tên kiểu|Kích thước tối đa|
|--------|-----------------|--------|-----------------|
|char(x)|255 Bytes|blob|65535 Bytes|
|varchar(x)|255 Bytes|mediumtext|1.6 MB|
|tinytext|255 Bytes|mediumblob|1.6 MB|
|tinyblob|255 Bytes|longtext|4.2 GB|
|text|65535 Bytes|longblob|4.2 GB|
## 2.3 Kiểu hỗn hợp
|Tên kiểu|Mô tả|Ví dụ|
|--------|-----|-----|
|ENUM|kiểu dữ liệu liệt kê,cho phép định nghĩa trước các giá trị và chỉ lưu trữ 1 trong các giá trị định sẵn|color ENUME('black','white')|
|SET|giống ENUM nhưng cho phép lưu nhiều giá trị|favorite SET('manga','music','movie','soccer')|
## 2.4 Kiểu ngày giờ
|tên kiểu|định dạng chuẩn|tên kiểu|định dạng chuẩn|
|--------|---------------|--------|---------------|
|datetime|YYYY-MM-DD HH:MM:SS|timestamp(4)|YYMM|
|date|YYYY-MM-DD|timestamp(6)|YYMMDD|
|time|HH:MM:SS|timestamp(8)|YYYYMMDD|
|year|YYYY|timestamp(10)|YYYYMMDDHH|
|timestamp(2)|YY|timestamp(12)|YYYYMMDDHHMM|
|||timestamp(14)|YYYYMMDDHHMMSS|

## 2.5 Các từ khóa định nghĩa cột
|Từ khóa|Kiểu dữ liệu phù hợp|Ý nghĩa|
|-------|--------------------|-------|
|Auto_increment|int|tự động tăng dữ liệu|
|Binary|char,varchar|lưu trữ dạng nhị phân|
|Default|tất cả trừ text,blob|đặt giá trị mặc định|
|Not null|tất cả|không được phép bỏ trống|
|Null|tất cả|được phép bỏ trống|
|Primary key|tất cả|khóa chính|
|Unique|tất cả|giá trị duy nhất|
|Unsigned|số|không dấu|
|Zerofill|số|điền 0 cho đủ chiều dài|
# 3 Các thao tác trên bảng
## 3.1 Tạo bảng
```
#Tạo bản mới
CREATE TABLE tabel_name(column_name datatype modified)
CREATE TABLE SinhVien(ten VARCHAR(50) not null default='Nguyễn Văn A',mssv VARCHAR(8) primary key
,gioiTinh ENUM('Nam','Nữ'));

#Tạo bảng tạm mới
CREATE TEMPORARY TABLE tabel_name(column_name datatype modified)

#Tạo bảng tạm từ câu truy vấn
Create temporary table select column_name from table_name;
Create temporary table select ten,mssv from SinhVien;

#Tạo bảng sao chép từ một bảng khác
Create table table_name select column_name from table_name_1;
create table SinhVien_copy select * from SnhVien;

#Kiểm tra sự tồn tại của bảng trước khi tạo
Create table If not Exists table_name (column_names datatypes modifiers);
```
## 3.2 Xem thông tin csdl,bảng
```
#Xem các bảng của CSDL
Show tables [from database_name];

#Xem các cột của bảng
Show columns [from table_name];

#Xem cấu trúc của bảng
Discribe table_name [from database_name];
```
## 3.3 Xóa bảng
`Drop table table_name [from database_name];`
## 3.4 Thay đổi cấu trúc
```
#Thay đổi tên cột
Alter table table_name change old_column_name new_column_name old_datatype;
ALTER TABLE Customers CHANGE First_Name FirstName VARCHAR(20);

#Thay đổi kiểu dữ liệu
Alter table table_name change column_name column_name new_datatype;
ALTER TABLE Customers CHANGE Last_Name Last_Name VARCHAR(50);

#Đổi tên bảng
Alter table table_name Rename new_table_name;
ALTER TABLE Customers RENAME Customer_Table;

#Thêm cột vào bảng
Alter table table_name add column_name datatype;
ALTER TABLE Customer ADD Last_Name VARCHAR(30);

#Xoá một cột
Alter table table_name Drop column_name;
ALTER TABLE Customers DROP Last_Name;

#Thêm khoá chính
Alter table table_name Add Primary Key (column_names);
ALTER TABLE Customers ADD PRIMARY KEY (Customer_ID);

#Xoá khoá chính
Alter table table_name Drop Primary Key;
```
## 3.5 Chèn dữ liệu vào bảng
```
#Chèn một dòng dữ liệu
Insert into table_name (column_names) values (column_values);
INSERT INTO Test_Table (Test_ID, Test_Name, Test_Date, Test_Giver) VALUES (NULL, 'Test','2000-01-01','Glen');

#Chèn nhiều dòng dữ liệu
Insert into table_name (column_names) values (column_values),(column_values), (…);
INSERT INTO Test_Table (Test_ID, Test_Name, Test_Date, Test_Giver)VALUES (NULL, 'John','2000-01 -01','Glen'),
(NULL, 'Thomas','2000-01-01','Jose');
```
