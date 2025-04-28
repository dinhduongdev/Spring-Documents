# Hướng Dẫn về Annotation, IoC và Spring Bean trong Java/Spring Framework

## 1. Annotation (Chú Thích)

Annotation là một tính năng quan trọng trong lập trình Java, cho phép bạn thêm các thông tin bổ sung vào mã nguồn, giúp trình biên dịch và các công cụ phát triển hiểu và xử lý mã nguồn một cách thông minh. Annotation được sử dụng rộng rãi trong Java để đánh dấu và cung cấp metadata cho các lớp, phương thức, biến, hoặc gói.

### Cú Pháp
- `@` + tên của annotation, ví dụ: `@Override`, `@Deprecated`

### Một Số Annotation Phổ Biến
- **`@Component`**: Đánh dấu trên các class để chỉ định chúng là các bean được quản lý bởi Spring Boot. Spring Boot sẽ tạo và quản lý các instance của các class được đánh dấu bởi `@Component`.
- **`@Autowired`**: Được sử dụng để tiêm (inject) các dependency vào các thành phần khác. Khi bạn đánh dấu một thuộc tính bằng `@Autowired`, Spring Boot sẽ tự động tiêm một instance của dependency tương ứng vào thuộc tính đó.

## 2. IoC (Inversion of Control)

Inversion of Control (IoC) là một nguyên tắc lập trình, trong đó luồng điều khiển trong ứng dụng không được quyết định bởi ứng dụng mà được quyết định bởi một framework hoặc container bên ngoài.

### IoC Thường Đi Kèm Với DI
IoC thường đi kèm với Dependency Injection (DI), nơi các dependency được quản lý và cung cấp bởi một framework hoặc container. Framework sẽ quản lý việc tạo và quản lý các đối tượng tương ứng và phụ thuộc.

