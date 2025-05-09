# Tight-coupling
Trong lập trình Java, khái niệm **tight-coupling** (liên kết rắng buộc) ám chỉ mối quan hệ giữa các lớp (classes) quá chặt chẽ. Khi sử dụng tight-coupling, các lớp kết nối với nhau một cách mạnh mẽ, và sự thay đổi trong một lớp có thể ảnh hưởng đến toàn bộ hệ thống hoặc các lớp khác. Điều này có thể tạo ra sự phụ thuộc không mong muốn và làm cho mã nguồn trở nên khó bảo trì và mở rộng.

# Loose coupling
Ngược lại, **loose coupling** (liên kết lỏng) là cách để giảm bớt sự phụ thuộc giữa các lớp với nhau. Trong loose coupling, các lớp hoạt động độc lập và không biết gì về cấu trúc hoặc chi tiết triển khai của các lớp khác. Điều này tạo điều kiện thuận lợi cho việc mở rộng và bảo trì mã nguồn.

# Dependency Injection (DI)

Một cách để thực hiện loose coupling là sử dụng **Dependency Injection (DI)**:

- Dependency Injection là một mô hình lập trình và thiết kế phần mềm, không chỉ áp dụng cho Java mà còn cho nhiều ngôn ngữ khác. Đây là một phương pháp giúp giảm sự phụ thuộc giữa các thành phần (hoặc lớp) trong ứng dụng.
- Trong DI, các phụ thuộc của một đối tượng được tạo bên trong đối tượng đó, mà được cung cấp từ bên ngoài. Cụ thể, DI thường được thực hiện thông qua ba cách chính: Constructor Injection, Setter Injection và Interface Injection.

