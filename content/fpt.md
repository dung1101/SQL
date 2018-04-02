# 1.Function
## 1.1.Định nghĩa
Function là phương thức dùng để định nghĩa ra các hàm mới do chính người quản trị CSDL tạo ra.Function sẽ được định nghĩa để thực hiện một yêu cầu tính toán nào đó và trả về một giá trị (kết quả) mà ta cần. Để tạo được Function cần có quyền CREATE ROUTINE.Function không thể chỉnh sửa dữ liệu.
## 1.2.Cú pháp 
```
DELIMITER $$
CREATE FUNCTION function_name(arguments)
RETURNS datatype characteristics
BEGIN
…
RETURN value;
END;$$
DELIMITER;
```
Trong đó :
* Function_name : tên function muốn tạo
* Arguments : danh sách các tham số cần thiết cho function
* Datatype : kiểu dữ liệu mà function trả về
* Characteristics : các đặc trưng của function DETERMINISTIC, NO SQL, READS SQL DATA
* RETURN : kết quả trả về của function.
ví dụ
```
CREATE FUNCTION hello (s CHAR(20))
RETURNS CHAR(50) DETERMINISTIC
RETURN CONCAT('Hello, ',s,'!');

DELIMITER
CREATE FUNCTION TuoiTB()
RETURNS Float Reads SQL Data
BEGIN
RETURN select avg(Year(Birthday)) From Customer;
END;$$
DELIMITER;
```
## 1.3.Gọi 
```
select function_name(arguments)
```
# 2.Procedure
# 2.1.Định nghĩa
Procedure là phương thức dùng để tạo ra các hàm xử lý tuy nhiên Procedure không trả về giá trị (kết quả) như Function. Procedure thường được dùng để thao tác lên CSDL nhiều hơn.Procedure ta có thể tạo ra các hàm dùng để chỉnh sửa, xoá…dữ liệu. Kết quả của Procedute không phải là một giá trị như Function, mà là một bộ giá trị (kết quả của một truy vấn…)
# 2.2.Cú pháp
```
DELIMITER $$
CREATE PROCEDURE procedure_name(IN | OUT | INOUT arguments)
BEGIN
 ...
END; $$
DELIMITER;
```
Trong đó :
* delimiter :dùng để phân cách bộ nhớ lưu trữ thủ tục Cache và mở ra một ô lưu trữ mới. Đây là cú pháp nên bắt buộc.
* Procedure_name : tên Procedure muốn tạo
* Arguments : các tham số cần thiết cho procedure, các tham số phải sử dụng các từ khoá In, Out, InOut để chỉ định loại argument (In – tham số chỉ cho giá trị vào, Out – tham số chỉ cho giá trị ra, InOut – tham số cho giá trị vào và ra).
Ví dụ :
```
```
# 2.3.Gọi 
```
CALL procedure_name(argument_values);
```
# 3.Trigger
# 3.1.Định nghĩa
Trigger có thể hiểu đơn giản là một thủ tục SQL được thực thi ở phía server khi có một sự kiện như Inser, Delete, hay Update. Tuy nhiên khác với Stored Procedure, Trigger hoàn toàn không có tham số. Chúng ta không thể gọi thực hiện trực tiếp Trigger bằng lệnh EXECUTE như Strore Procedure hoặc bằng bấy kỳ một lệnh nào khác. Thay vào đó Trigger sẽ được thực hiện một cách tự động. Trigger được lưu trữ và quản lý trong Server Database, được dùng trong trường hợp ta muốn kiểm tra các ràng buộc toàn vẹn trong Database.
# 3.2.Cú pháp
```
DELIMITER $$
CREATE TRIGGER trigger_name trigger_time trigger_event
[BEFORE | AFTER] [INSERT | UPDATE | DELETE]
ON table_name
FOR EACH ROW
BEGIN
 ...
END;$$
DELIMITER;
```
Ví dụ
```
DELIMITER $$
CREATE TRIGGER before_employee_update 
    BEFORE UPDATE ON employees
    FOR EACH ROW 
BEGIN
    INSERT INTO employees_edit
    SET action = 'update',
     employeeNumber = OLD.employeeNumber,
        lastname = OLD.lastname,
        changedat = NOW(); 
END$$
DELIMITER ;
```
Trong thân của cú pháp TRIGGER, chúng ta sử dụng từ khóa OLD để truy cập vào hàng của cột employeeNumber và lastname bị ảnh hưởng bới TRIGGER. Lưu ý, với TRIGGER định nghĩa cho INSERT, bạn chỉ có thể sử dụng từ khóa NEW. Tuy nhiên, TRIGGER cho DELETE không có hàng mới nào nên chỉ có thể sử dụng từ khóa OLD. Với TRIGGER cho hàm UPDATE, OLD dùng để đề cập đến hàng trước khi được cập nhật, NEW đề cập đến hàng sau khi được cập nhật.
