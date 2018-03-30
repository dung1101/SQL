### Romote mysql server
- Thêm bind-address = IP mysql server vào file Ubuntu: /etc/mysql/my.cof Centos: /etc/my.cof
- `/etc/init.d/mysql restart` hoặc 'service mysql restart'
- check port 3306: netstat -anp | grep 3306
- `iptable -A INPUT -i all -p tcp --dport=3306 -j ACCEPT` 
- `firewall-cmd --zone=public --add-service=mysql`
- tạo user trong mysql server: CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
- phân quyền cho user đó: GRANT ALL PRIVILEGES ON * . * TO 'newuser'@'localhost';
- reload lại quyền mysql: FLUSH PRIVILEGES;
- remote từ client tới server: mysql -u user -h IP_mysql_server -p
