# Hướng Dẫn về Component Scan, Lazy-Initialized Beans và Lifecycle của Bean trong Spring Boot

## 1. Component Scan Là Gì?

Component Scan cho phép Spring Boot tự động tìm kiếm và quét các bean trong ứng dụng của bạn. Mặc định, Spring Boot sẽ quét toàn bộ các package và các package con của package chứa class chứa hàm `main`.

### Thay Đổi Phạm Vi Component Scan

Trong trường hợp bạn muốn tùy chỉnh cấu hình của Component Scan để chỉ tìm kiếm các bean trong một package nhất định, bạn có thể làm như sau:

#### 1.1 Sử dụng `@ComponentScan`

```java
@ComponentScan("com.duong.firstspringboot.others")
@SpringBootApplication
public class FirstSpringBootApplication { // ... }
```

#### 1.2 Sử dụng `scanBasePackages`

```java
@SpringBootApplication(scanBasePackages = "com.duong.firstspringboot.others")
public class App { // ... }
```

## 2. Lazy-Initialized Beans

- **Eager (háo hức)**: Bean được tạo ra ngay khi ứng dụng chạy.  
  Ví dụ: singleton scope.
  
- **Lazy (lười)**: Bean được tạo ra khi chương trình gọi đến nó.  
  Ví dụ: prototype scope hoặc sử dụng `@Lazy`.

## 3. Lifecycle của Bean

### `@PostConstruct` và `@PreDestroy`

- **`@PostConstruct`**: Là một annotation đánh dấu trên một method bên trong một bean. IoC Container hoặc ApplicationContext sẽ gọi method này sau khi bean đó được tạo ra và quản lý.
  
- **`@PreDestroy`**: Là một annotation đánh dấu trên một method bên trong một bean. IoC Container hoặc ApplicationContext sẽ gọi method này trước khi bean đó bị xóa hoặc không được quản lý nữa.