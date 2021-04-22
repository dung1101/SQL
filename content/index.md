### Index
> Index là một cấu trúc dữ liệu được dùng để định vị và truy cập nhanh nhất vào dữ liệu trong các bảng database

### Tại sao phải đánh index
* Khi database được lưu trữ trên các ổ đĩa thiết bị, nó được lưu trữ dưới dạng khối dữ liệu(blocks of data.). Các khối ổ đĩa được cấu trúc giống như cách cấu trúc `link list`; bao gồm cả section data và thành phần con trỏ (pointer) đến node tiếp theo, và cả hai thành phần không cần phải được lưu trữ liên tục. 

* Do thực tế các records chỉ có thể được sắp xếp theo một field, chúng ta có thể nói rằng tìm kiếm trên một trường không được sắp xếp bằng `Linear Search` phải mất N/2 block access (tính trung bình), với N là số block của table. Nếu một trường không có key (không yêu cầu tính uniques), thì phải mất tới N lượt block access. Trong khi dữ liệu được sắp xếp, chúng ta có thể search bằng `Binary Search` chỉ với log2(N) lượt block access. Nhưng cũng vì dữ liệu sắp xếp theo field không phải là key, nên chúng ta không cần phải tìm kiếm các giá trị trùng lặp nếu các giá trị thỏa mãn cao hơn được tìm thấy. Như vậy, hiệu suất sẽ được tăng đáng kể.

### Kiểu index
* Clustered index
* Non-clustered index

### Indexing là gì?
Indexing là cách sắp xếp các record trên nhiều fields. Tạo index trên một field của một table là việc tạo ra một cấu trúc dữ liệu khác giữ field value, và trỏ tới record mà nó liên quan tới. Index đó sẽ được sắp xếp, và cho phép Binary Search thực hiện.  Nhược điểm của các index này là yêu cầu thêm không gian bộ nhớ

### Indexing type
- PostgreSql: <a href="#b-trees">B-trees</a>(default), <a href="#hash">Hash</a>, GiST, GIN, spgist, BRIN
- mssql: <a href="#hash">Hash</a>, memory-optimized Nonclustered, Clustered, Nonclustered, Unique, Columnstore, Index with included columns, Index on computed columns, Filtered, Spatial, XML, Full-text
- MySql: 

##### <b id="b-trees">B-trees</b>
* Dữ liệu index được tổ chức và lưu trữ theo dạng tree, tức là có root, branch, leaf.
* Giá trị của các node được tổ chức tăng dần từ trái qua phải.
* B-trees có thể xử lý truy vấn bằng hoặc khoảng dữ liệu có thể sắp xếp. Cụ thể: `<`, `<=`, `=>`, `>`, `=`, `BETWEEN`, `LIKE`
* Có thể tối ưu tốt cho câu lệnh ORDER BY
* Khi truy vấn dữ liệu thì CSDL sẽ không scan dữ liệu trên toàn bộ bảng để tìm dữ liệu, việc tìm kiếm trong B-Tree là 1 quá trình đệ quy, bắt đầu từ root node và tìm kiếm tới branch và leaf, đến khi tìm được tất cả dữ liệu – thỏa mãn với điều kiện truy vấn thì mới dùng lại.

##### <b id="hash">Hash</b>
* Hash index dựa trên giải thuật Hash Function (hàm băm). Tương ứng với mỗi khối dữ liệu (index) sẽ sinh ra một bucket key(giá trị băm) để phân biệt.
* Hash chỉ xử lý so sáng bằng cơ bản sử dụng toán tử `=`
* Không thể tối ưu hóa toán tử ORDER BY bằng việc sử dụng Hash index bởi vì nó không thể tìm kiếm được phần từ tiếp theo trong Order.
* Hash có tốc độ nhanh hơn kiểu B-Tree.

##### <b id="full-text">Full-text</b>
Thường được sử dụng để tìm kiếm 1 hoặc nhiều từ nằm trong 1 nhóm từ hoặc tìm substring trong 1 string lớn. Thay vì đánh index cả value thì đánh index từng từ đơn lẻ => tốc độ tìm kiếm 1 từ nhanh.
