# SQL Database

## Character set
>Một character set là một tập hợp các ký tự và các phương thức chuyển mã ký tự (encoding). 

>Giả sử ta có 4 chữ cái `A`, `a`, `B`, `b` được encode là `A` = `65`, `B` = `66`, `a` = `97`, `c` = `98`. Thì trong máy tính, chữ `A` là một ký hiệu và 65 là mã của được chuyển của `A`. Thì sự kết hợp của một tập các ký tự và cách chuyển mã của chúng được gọi là character set.

Ví dụ trong MySQL, character set `latin1` (West European Character Sets) là character set mặc định 
## collation
>Một collation là một tập hợp các qui tắc để so sánh hai ký tự trong một tập hợp ký tự.

Ví dụ trong MySQL, collation `latin1_swedish_ci` là collation mặc địn .Theo qui tắc đặt tên collation của mysql, thì _ci có nghĩa là `case insensitive` do đó latin1_swedish_ci là một collation không phân biệt hoa thường.

Trong MySQL, có 2 dạng dữ liệu kiểu string đó là nonbinary string (char, varchar, text) và binary string (binary, varbinary, blob). Thì chỉ có kiểu non-binary string mới dùng collation khi sắp xếp và tìm kiếm. Còn kiểu binary string, sử dụng mã tương ứng để so sánh và tìm kiếm cho nên nó phân biệt hoa thường (case insensitive)
## 1) CREATE DATABASE 
>Dùng để tạo mới cơ sở dữ liệu

Cú pháp:
```
CREATE DATABASE databasename;

CREATE DATABASE sql_tutorial;
```
=> khai báo như bên trên ở mysql sẽ tạo database không hỗ trợ Tiếng Việt. Để hỗ trợ tiếng Việ phải thêm tùy chọn `CHARACTER SET` và `COLLATE`. 
```
CREATE DATABASE sql_tutorial CHARACTER SET utf8 COLLATE utf8_unicode_ci;
```

## 2) DROP DATABASE
>Dùng để xóa db đã tồn tại

Cú pháp:
```
DROP DATABASE databasename;

DROP DATABASE sql_tutorial;
```
## 3) BACKUP DATABASE
>backup dữ liệu
- full back up: sao lưu toàn bộ dữ liệu
- differential back up: sao lưu dữ liệu đã thay đổi tù lần full back up cuối cùng
 
=> back up lần đầu tiên phải dùng full back up

Cú pháp full back up:
```
BACKUP DATABASE databasename TO DISK = 'filepath';

BACKUP DATABASE sql_tutorial TO DISK = 'E:\sql_tutorial.bak';
```

Cú pháp differential back up:
```
BACKUP DATABASE databasename TO DISK = 'filepath' WITH DIFFERENTIAL;

BACKUP DATABASE sql_tutorial TO DISK = 'E:\sql_tutorial.bak' WITH DIFFERENTIAL;
```
=> mysql 5.5 trở về trước mới hỗ trợ `BACKUP`
## 4) CREATE TABLE
>tạo mới table trong db

Cú pháp tạo mới table:
```
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    column3 datatype,
   ....
);

create table persons( 
	id int,
    last_name varchar(20),
    first_name varchar(20),
    address varchar(255)
);
```

Cú pháp tạo table từ table đã có
```
CREATE TABLE new_table_name AS
    SELECT column1, column2,...
    FROM existing_table_name
    WHERE ....;
    
CREATE TABLE TestTable AS
SELECT customername, contactname
FROM customers;
```

## 5) DROP TABLE
> xóa table

Cú pháp
```
DROP TABLE tablename;

drop table copy_persons;
```

## 6) TRUNCATE TABLE
> Xóa tất cả dữ liệu trong table mà ko xóa table

Cú pháp:
```
TRUNCATE TABLE table_name;

truncate table persons;
```

## 7) ALTER TABLE
> dùng để thêm, xóa hoặc chỉnh sửa các column trong table

Cú pháp `ADD`:
```
ALTER TABLE table_name ADD column_name datatype;

alter table persons add married boolean;
```

Cú pháp `DROP`:
```
ALTER TABLE table_name DROP COLUMN column_name;

alter table persons drop column married;
```
=> mysql 5.7 bỏ `COLUMN` cũng được

Cú pháp `MODIFY`
```
ALTER TABLE table_name MODIFY COLUMN column_name datatype;

alter table persons modify last_name varchar(45);
alter table persons modify column last_name varchar(55);
```
=> cách này không thay đổi được tên, để thay đổi tên trong mysql 5.7 dùng `CHANGE`
```
ALTER TABLE table_name CHANGE `column_name` `column_name_new` datatype;

alter table persons change `last_name` `last_namee` varchar(45);
```
=> chú ý `` chứ không phải ' '


## 8) SQL Constraints
> rules for data in table

- NOT NULL: không cho phép null
- UNIQUE: giá trị là duy nhất không trùng lặp
- PRIMARY KEY: khóa chính
- FOREIGN KEY: khóa phụ
- CHECK: đặt điều kiện cho giá trị (mysql sẽ bỏ qua check) 
- DEFAULT: đặt giá trị mặc định
- INDEX - Used to create and retrieve data from the database very quickly (chưa biết)
- AUTO INCREMENT: tự động tăng khi tạo mới

### 8.1) NOT NULL
Cú pháp với `CREATE TABLE`:
```
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255) NOT NULL,
    Age int
);
```
Cú pháp với `ALTER TABLE`:
```
ALTER TABLE Persons MODIFY Age int NOT NULL;
```

### 8.2) UNIQUE
Cú pháp với `CREATE TABLE`:
```
/* MySQL / SQL Server / Oracle / MS Access */
CREATE TABLE Persons (
    ID int NOT NULL UNIQUE,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int
);

/* MySQL */
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    UNIQUE (ID)
);

/* MySQL/ SQL Server / Oracle / MS Access */
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    CONSTRAINT UC_Person UNIQUE (ID,LastName)
);
```
Cú pháp với `ALTER TABLE`
```
ALTER TABLE Persons ADD UNIQUE (ID);
ALTER TABLE Persons ADD CONSTRAINT UC_Person UNIQUE (ID,LastName);
```
`DROP`:
```
/* MySQL */
ALTER TABLE Persons DROP INDEX UC_Person;
/* SQL Server / Oracle / MS Access */
ALTER TABLE Persons DROP CONSTRAINT UC_Person;
```
### 8.3) PRIMARY KEY
Cú pháp với `CREATE TABLE`:
```
/* MySQL / SQL Server / Oracle / MS Access */
SQL Server / Oracle / MS Access
CREATE TABLE Persons (
    ID int NOT NULL PRIMARY KEY,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int
);

/* MySQL */
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    PRIMARY KEY (ID)
);

/* MySQL/ SQL Server / Oracle / MS Access */
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    CONSTRAINT PK_Person PRIMARY KEY (ID,LastName)
);
```
Cú pháp với `ALTER TABLE`
```
ALTER TABLE Persons ADD PRIMARY KEY (ID);
ALTER TABLE Persons ADD CONSTRAINT PK_Person PRIMARY KEY (ID,LastName);
```
`DROP`:
```
/* MySQL */
ALTER TABLE Persons DROP PRIMARY KEY;

/* SQL Server / Oracle / MS Access */
ALTER TABLE Persons DROP CONSTRAINT PK_Person;
```
### 8.4) FOREIGN KEY
Cú pháp với `CREATE TABLE`:
```
/* SQL Server / Oracle / MS Access */
CREATE TABLE Orders (
    OrderID int NOT NULL PRIMARY KEY,
    OrderNumber int NOT NULL,
    PersonID int FOREIGN KEY REFERENCES Persons(PersonID)
);

/* MySQL */
CREATE TABLE Orders (
    OrderID int NOT NULL,
    OrderNumber int NOT NULL,
    PersonID int,
    PRIMARY KEY (OrderID),
    FOREIGN KEY (PersonID) REFERENCES Persons(PersonID)
);

/* MySQL/ SQL Server / Oracle / MS Access */
CREATE TABLE Orders (
    OrderID int NOT NULL,
    OrderNumber int NOT NULL,
    PersonID int,
    PRIMARY KEY (OrderID),
    CONSTRAINT FK_PersonOrder FOREIGN KEY (PersonID) REFERENCES Persons(PersonID)
);
/* FK_PersonOrder là tên của liên kết do mình tự đặt */
```
Cú pháp với `ALTER TABLE`
```
ALTER TABLE Persons ADD PRIMARY KEY (ID);
ALTER TABLE Persons ADD CONSTRAINT PK_Person PRIMARY KEY (ID,LastName);
```
`DROP`:
```
/* MySql */
ALTER TABLE Orders DROP FOREIGN KEY FK_PersonOrder;
/* SQL Server / Oracle / MS Access */
ALTER TABLE Orders DROP CONSTRAINT FK_PersonOrder;
```
## 8.5) CHECK
=> mysql không sử dụng check mặc dù khai báo sẽ không báo lỗi

Cú pháp với `CREATE TABLE`:
```
/* SQL Server / Oracle / MS Access */
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int CHECK (Age>=18)
);

/* SQL Server / Oracle / MS Access */
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    City varchar(255),
    CONSTRAINT CHK_Person CHECK (Age>=18 AND City='Sandnes')
);
```
Cú pháp với `ALTER TABLE`:
```
ALTER TABLE Persons ADD CHECK (Age>=18);
ALTER TABLE Persons ADD CONSTRAINT CHK_PersonAge CHECK (Age>=18 AND City='Sandnes');
```
`DROP`:
```
ALTER TABLE Persons DROP CONSTRAINT CHK_PersonAge;
```
