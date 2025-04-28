# Annotation và IoC trong Java
# Annotation (chú thích)
- Annotation là một tính năng quan trọng trong lập trình Java, cho phép bạn thêm các thông tin bổ sung vào mã nguồn của bạn, giúp trình biên dịch và các công cụ phát triển hiểu và xử lý mã nguồn của bạn một cách thông minh. Annotation được sử dụng rộng rãi trong Java để đánh dấu và cung cấp metadata cho các lớp, phương thức, biến, hoặc gói.
Cú pháp:
- @ + tên của annotation, ví dụ: @Override, @Deprecated

- Một số annotation phổ biến:

@Component annotation: Là một annotation đánh dấu trên các class để cho biết chúng là các bean được quản lý bởi Spring Boot. Điều này có nghĩa là Spring Boot sẽ tạo và quản lý các instance của các class được đánh dấu bởi @Component.

@Autowired annotation: Được sử dụng để tiêm (inject) các dependency vào các thành phần khác. Khi bạn đánh dấu một thuộc tính bằng @Autowired, Spring Boot sẽ tự động tiêm một instance của dependency tương ứng vào thuộc tính đó.


# IoC (Inversion of Control)
Inversion of Control (IoC) là một nguyên tắc lập trình, trong đó luồng điều khiển trong ứng dụng không được quyết định bởi ứng dụng mà được quyết định bởi một framework hoặc container bên ngoài.
IoC thường đi kèm với DI
IoC thường đi kèm với DI, nơi các dependency được quản lý và cung cấp bởi một framework hoặc container. Framework sẽ quản lý việc tạo và quản lý các đối tượng tương ứng và phụ thuộc.
