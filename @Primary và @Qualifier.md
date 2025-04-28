# Hướng Dẫn về Bean Name, @Primary và @Qualifier trong Spring Boot

## 1. Bean Name

Bạn có thể lấy danh sách tên của các bean trong Spring Context bằng cách sử dụng:  
```java
context.getBeanDefinitionNames();
```

## 2. `@Primary`

`@Primary` đánh dấu một bean là ưu tiên và sẽ luôn được ưu tiên chọn trong trường hợp có nhiều bean cùng loại trong context.

## 3. `@Qualifier`

`@Qualifier` xác định tên của một bean mà bạn muốn chỉ định để inject.