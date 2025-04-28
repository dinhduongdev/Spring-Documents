## Spring Bean Là Gì?

Spring Framework documentation: *In Spring, the objects that form the backbone of your application and that are managed by the Spring IoC container are called beans. A bean is an object that is instantiated, assembled, and otherwise managed by a Spring IoC container.*

=> Những object mà được tạo và quản lý bởi Spring IoC Container thì được gọi là **Bean**.

### Cách Tạo Bean
1. **Dùng `@Component`**: Sử dụng các annotation như `@Repository`, `@Service`, `@Controller`.
2. **Dùng `@Bean` trong class có annotation `@Configuration`**:
   - **`@Configuration`**:
     - Đánh dấu trên một class, cho biết class đó chứa các thông tin cấu hình của ứng dụng.
     - Spring Boot sẽ tìm và quét các class được đánh dấu bởi `@Configuration` để tạo và quản lý các beans.
   - **`@Bean`**: Đánh dấu trên một method trong class được đánh dấu bởi `@Configuration`. Nó cho biết rằng method đó tạo và trả về một bean, và Spring Boot nên quản lý bean đó trong ứng dụng.

### Bean Scope
- **SINGLETON**: Container chỉ khởi tạo 1 instance của bean và trả về chính nó nếu có yêu cầu.
- **PROTOTYPE**: Mỗi khi có yêu cầu, container sẽ tạo ra một instance mới và trả về.
- **REQUEST**: Khởi tạo instance cho mỗi HTTP Request.
- **SESSION**: Khởi tạo instance cho mỗi HTTP Session của ServletContext.
- **WEBSOCKET**: Khởi tạo instance cho mỗi Websocket Session.

=> Dùng `@Scope` để chỉ định scope, ví dụ: `@Scope("singleton")`.