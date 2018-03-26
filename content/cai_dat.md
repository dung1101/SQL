# 1.Cài đặt trên ubuntu
## 1.1 Các bước cài đặt mysql 5.5
```
apt-get update
apt-get install mysql-server
```
Trong quá trình cài đặt sẽ hiện ra thông báo đặt pwd cho tài khoản root.
```
#cài đặt bảo mật cho MySQL Server
mysql_secure_installation
#kích hoạt 
mysql_install_db
```
## 1.2 Các bước cài đặt mariadb 5.5.59
```
apt-get update
apt-get install mariadb-server
```
Trong quá trình cài đặt sẽ hiện ra thông báo đặt pwd cho tài khoản root.
```
#cài đặt bảo mật cho mariadb 
mysql_secure_installation
#kích hoạt 
mysql_install_db
```
## port mặc định của mysql,mariadb là 3306
# 2.Đăng nhập và sử dụng
## 2.1 Đăng nhập bằng cmd
### Đăng nhập
```
mysql -u root -p
#Nhập pwd đã tạo lúc cài đặt
```
### Sử dụng
|Câu lệnh|Mô tả|
|--------|-----|
|SHOW DATABASES;|hiển thị list database|
|CREATE DATABASE mydb;|tạo db mới có tên là mydb|
|USE mydb;|chọn mydb để tiến hành làm việc|
|SHOW tables;|list các table có trong db|
|CREATE TABLE nguoi (id INT NOT NULL PRIMARY KEY AUTO_INCREMENT, ten NVARCHAR(20), tuoi INT);|tạo một bảng mới với 3 cột là id,ten,tuoi| 
|DESCRIBE nguoi;|xem lại cấu trúc bảng|
|ALTER TABLE nguoi ADD email VARCHAR(40);|thêm một cột vào bảng|
|ALTER TABLE nguoi DROP email;|xóa một cột khỏi bảng|
|INSERT INTO nguoi(ten,tuoi) values ('NV A',17);|thêm một đối tượng vào bảng|
|SHOW * FROM nguoi|hiển thị tất cả thông tin từ bảng nguoi|
|UPDATE nguoi SET ten ="Nguyen Van A" WHERE nguoi.ten ="NV A";|thay đổi thông tin|
|DELETE FROM nguoi WHERE ten ='Nguyen Van A';|xóa một hàng(đối tượng)|
## 2.2 Đăng nhập bằng GUI
Cài mysql workbench để làm việc thông qua GUI
