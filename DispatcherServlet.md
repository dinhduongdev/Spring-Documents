# Giải thích sơ đồ và ví dụ minh họa luồng xử lý trong Spring MVC

## 1. Giải thích sơ đồ luồng xử lý trong Spring MVC

Sơ đồ sau mô tả luồng xử lý một HTTP Request trong Spring MVC:

### Sơ đồ:
- **Tiêu đề**: "DispatcherServlet xử lý các HTTP Request và Response, nó quyết định chương trình nào của controller sẽ được thực thi khi nhận Request."
- **Cấu trúc**:
  - **HTTP Request/Response**: Đầu vào và đầu ra của hệ thống.
  - **Filter**: Bộ lọc trước khi request đến DispatcherServlet.
  - **DispatcherServlet**: Thành phần trung tâm điều phối request.
  - **HandlerMapping**: Ánh xạ request đến Controller.
  - **Controller**: Xử lý logic nghiệp vụ.
  - **ModelAndView**: Đối tượng chứa dữ liệu và tên view.
  - **ViewResolver**: Chuyển đổi tên view thành view cụ thể.
  - **View**: Render giao diện người dùng.

### Giải thích chi tiết từng thành phần:
1. **HTTP Request/Response**: Đây là yêu cầu từ client (trình duyệt) gửi đến server và phản hồi trả về sau khi xử lý.
   
2. **Filter**: 
   - Trước khi request đến DispatcherServlet, nó đi qua các Filter (bộ lọc). Filter được sử dụng để xử lý các tác vụ như kiểm tra bảo mật, logging, hoặc mã hóa dữ liệu. Filter không thuộc DispatcherServlet mà thuộc Servlet Container (như Tomcat).

3. **DispatcherServlet**: 
   - Đây là thành phần trung tâm trong Spring MVC, đóng vai trò "Front Controller". Nó nhận tất cả HTTP Request và điều phối (dispatch) chúng đến các thành phần phù hợp để xử lý.
   - DispatcherServlet quyết định chương trình nào (controller) sẽ xử lý request dựa trên cấu hình.

4. **HandlerMapping**: 
   - DispatcherServlet sử dụng HandlerMapping để ánh xạ (map) request đến đúng Controller dựa trên URL hoặc các tiêu chí khác. Ví dụ, URL "/home" có thể được ánh xạ đến HomeController.

5. **Controller**: 
   - Controller nhận request từ DispatcherServlet, xử lý logic nghiệp vụ (có thể tương tác với Model để lấy dữ liệu), và trả về một ViewName (tên của view) cùng với Model (dữ liệu để hiển thị).

6. **ModelAndView**: 
   - Sau khi Controller xử lý, nó trả về một đối tượng ModelAndView chứa dữ liệu (Model) và tên view (ViewName) để hiển thị.

7. **ViewResolver**: 
   - ViewResolver chuyển đổi ViewName thành một View cụ thể. Ví dụ, ViewName "home" có thể được ánh xạ đến file "home.jsp" hoặc một template khác (như Thymeleaf).

8. **View**: 
   - View là thành phần cuối cùng, chịu trách nhiệm render giao diện người dùng (HTML, JSP, Thymeleaf, v.v.) dựa trên dữ liệu từ Model, sau đó trả về HTTP Response cho client.

### Luồng tổng thể:
- Client gửi HTTP Request → Filter xử lý trước → DispatcherServlet nhận request → HandlerMapping tìm Controller → Controller xử lý và trả về ModelAndView → ViewResolver xác định View → View render và trả về HTTP Response.

---

## 2. Ví dụ minh họa bằng mã nguồn

Dưới đây là một ứng dụng Spring MVC đơn giản hiển thị trang "Hello World", minh họa luồng xử lý theo sơ đồ trên.

### 2.1. Cấu hình dự án (pom.xml)

Tạo một dự án Spring Boot với file `pom.xml`:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>spring-mvc-demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.2.5</version>
    </parent>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
    </dependencies>
</project>
```

### 2.2. Cấu hình DispatcherServlet

Trong Spring Boot, `DispatcherServlet` được tự động cấu hình khi thêm dependency `spring-boot-starter-web`. Bạn không cần cấu hình thêm.

### 2.3. Tạo Filter

Tạo một Filter để ghi log request:

**File: `LogFilter.java`**

```java
import jakarta.servlet.*;
import java.io.IOException;

public class LogFilter implements Filter {
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) 
            throws IOException, ServletException {
        System.out.println("Request is passing through LogFilter");
        chain.doFilter(request, response); // Chuyển request đến DispatcherServlet
    }
}
```

Đăng ký Filter trong cấu hình:

**File: `FilterConfig.java`**

```java
import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class FilterConfig {
    @Bean
    public FilterRegistrationBean<LogFilter> loggingFilter() {
        FilterRegistrationBean<LogFilter> registrationBean = new FilterRegistrationBean<>();
        registrationBean.setFilter(new LogFilter());
        registrationBean.addUrlPatterns("/*");
        return registrationBean;
    }
}
```

### 2.4. Tạo Controller

Tạo một Controller để xử lý request:

**File: `HelloController.java`**

```java
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HelloController {

    @GetMapping("/hello")
    public String sayHello(Model model) {
        model.addAttribute("message", "Hello World from Spring MVC!");
        return "hello"; // ViewName
    }
}
```

- `@GetMapping("/hello")`: Ánh xạ URL `/hello` đến phương thức này.
- `Model`: Chứa dữ liệu để gửi đến View.
- `"hello"`: Tên view sẽ được ViewResolver xử lý.

### 2.5. Tạo View (Sử dụng Thymeleaf)

Tạo file `hello.html` trong thư mục `src/main/resources/templates`:

**File: `hello.html`**

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Hello World</title>
</head>
<body>
    <h1 th:text="${message}"></h1>
</body>
</html>
```

- ViewResolver sẽ ánh xạ ViewName `"hello"` thành file `hello.html`.

### 2.6. Cấu hình ViewResolver

Spring Boot tự động cấu hình `ViewResolver` cho Thymeleaf khi bạn thêm dependency `spring-boot-starter-thymeleaf`. Không cần cấu hình thêm.

### 2.7. Tạo lớp khởi động Spring Boot

**File: `SpringMvcDemoApplication.java`**

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SpringMvcDemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(SpringMvcDemoApplication.class, args);
    }
}
```

### 2.8. Chạy và kiểm tra

- Chạy ứng dụng và truy cập: `http://localhost:8080/hello`.
- Kết quả: Trang hiển thị "Hello World from Spring MVC!".

### 2.9. Luồng hoạt động (theo sơ đồ)

1. **HTTP Request**: Client truy cập `/hello`.
2. **Filter**: `LogFilter` ghi log và chuyển request đến `DispatcherServlet`.
3. **DispatcherServlet**: Nhận request và dùng `HandlerMapping` để ánh xạ `/hello` đến `HelloController`.
4. **Controller**: `sayHello` xử lý, thêm dữ liệu vào `Model` và trả về ViewName `"hello"`.
5. **ViewResolver**: Ánh xạ `"hello"` thành `hello.html`.
6. **View**: Render `hello.html` với dữ liệu từ `Model`.
7. **HTTP Response**: Trả về trang HTML cho trình duyệt.