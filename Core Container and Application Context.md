
# Spring Framework: Core Container và Application Context

## 1. Core Container
**Core Container** là nền tảng của Spring Framework, cung cấp các tính năng cơ bản như Inversion of Control (IoC) và Dependency Injection (DI). Nó bao gồm một số mô-đun quản lý việc tạo ra, cấu hình và vòng đời của các bean.

### 1.1 Core Module
- **Vai trò**: Thành phần quan trọng nhất của Spring Framework.
- **Các tính năng chính**:
  - **Inversion of Control (IoC)**: Chuyển giao trách nhiệm quản lý vòng đời và cấu hình đối tượng từ mã ứng dụng sang Spring Container.
  - **Dependency Injection (DI)**: Một kỹ thuật để triển khai IoC, cho phép Spring Container tiêm các phụ thuộc vào đối tượng, giảm sự phụ thuộc chặt chẽ và cải thiện khả năng kiểm thử.
- **Ví dụ**: Khai báo một bean sử dụng XML hoặc chú thích `@Component` cho phép Spring quản lý việc tạo ra và tiêm phụ thuộc.

### 1.2 Bean Module
- **Vai trò**: Cung cấp **BeanFactory**, một triển khai mẫu thiết kế Factory tổng quát.
- **Các tính năng chính**:
  - **BeanFactory** chịu trách nhiệm tạo ra, cấu hình và quản lý các đối tượng (bean) trong ứng dụng.
  - Hỗ trợ cấu hình bean thông qua XML, Java Config, hoặc chú thích, và quản lý vòng đời của bean (tạo, khởi tạo, sử dụng, huỷ).
  - Là nền tảng cho việc triển khai IoC trong Spring.
- **Ví dụ**: Sử dụng `XmlBeanFactory` để tải các định nghĩa bean từ một tệp cấu hình XML.

### 1.3 Singleton và Prototype Scopes
- **Singleton Scope**:
  - **Định nghĩa**: Tạo một **instance duy nhất** của bean trong Spring Container, được chia sẻ trong tất cả các yêu cầu cho bean đó.
  - **Trường hợp sử dụng**: Thích hợp cho các đối tượng không trạng thái hoặc các dịch vụ chung, chẳng hạn như lớp service hoặc repository.
  - **Ví dụ**: `@Scope("singleton")` (mặc định).
- **Prototype Scope**:
  - **Định nghĩa**: Tạo một **instance mới** của bean cho mỗi yêu cầu.
  - **Trường hợp sử dụng**: Thích hợp cho các đối tượng có trạng thái hoặc đối tượng cần được tạo mới cho mỗi yêu cầu.
  - **Ví dụ**: `@Scope("prototype")`.

### 1.4 Context Module
- **Vai trò**: Cung cấp **ApplicationContext**, một phiên bản nâng cao của BeanFactory.
- **Các tính năng chính**:
  - Là Spring IoC Container, chịu trách nhiệm tạo ra, cấu hình và lắp ráp các bean.
  - Mở rộng BeanFactory với các tính năng bổ sung:
    - **Event Management**: Cho phép giao tiếp giữa các bean thông qua sự kiện.
    - **Resource Loading**: Hỗ trợ tải các tài nguyên như tệp cấu hình hoặc hình ảnh.
    - **Internationalization (i18n)**: Hỗ trợ đa ngôn ngữ.
    - **Environment Integration**: Hỗ trợ cấu hình dựa trên môi trường (ví dụ: dev, prod, test).
- **Ví dụ**: Sử dụng `ClassPathXmlApplicationContext` cho cấu hình dựa trên XML hoặc `AnnotationConfigApplicationContext` cho cấu hình dựa trên Java.

### 1.5 SpEL (Spring Expression Language)
- **Vai trò**: Một ngôn ngữ biểu thức mạnh mẽ để truy vấn và thao tác với các đối tượng tại runtime.
- **Các tính năng chính**:
  - Hỗ trợ tham chiếu các bean, thuộc tính, hoặc phương thức, thực hiện các phép toán logic/toán học và cấu hình giá trị động.
  - Cú pháp: Giống như biểu thức trong các ngôn ngữ lập trình, ví dụ: `#{beanName.property}` hoặc `#{1 + 2}`.
- **Ví dụ**:
  - XML: `<property name="value" value="#{systemProperties['user.name']}" />`
  - Annotation: `@Value("#{someBean.someProperty}")`

## 2. Application Context
**ApplicationContext** là một giao diện trung tâm trong Spring, mở rộng từ BeanFactory, cung cấp các tính năng nâng cao để quản lý các bean trong các môi trường khác nhau, bao gồm cả ứng dụng độc lập và ứng dụng web.

### 2.1 ClassPathXmlApplicationContext
- **Định nghĩa**: Tải cấu hình từ các tệp XML nằm trong **classpath** (ví dụ: `src/main/resources` hoặc các tệp JAR).
- **Trường hợp sử dụng**: Phù hợp cho các ứng dụng độc lập hoặc môi trường phát triển/kiểm tra.
- **Ví dụ**:
  ```java
  ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
  MyBean myBean = context.getBean(MyBean.class);
  ```

### 2.2 FileSystemXmlApplicationContext
- **Định nghĩa**: Tải cấu hình từ các tệp XML được chỉ định bởi **đường dẫn hệ thống tệp tuyệt đối hoặc tương đối**.
- **Trường hợp sử dụng**: Hữu ích khi các tệp cấu hình nằm ngoài ứng dụng hoặc ở các vị trí đĩa cụ thể.
- **Ví dụ**:
  ```java
  ApplicationContext context = new FileSystemXmlApplicationContext("C:/config/applicationContext.xml");
  MyBean myBean = context.getBean(MyBean.class);
  ```

### 2.3 XmlWebApplicationContext
- **Định nghĩa**: Tải cấu hình từ các tệp XML trong một **ứng dụng web**, thường nằm tại `/WEB-INF/applicationContext.xml`.
- **Trường hợp sử dụng**: Dành cho các ứng dụng web, tích hợp với ServletContext trong Spring MVC.
- **Ví dụ** (trong `web.xml`):
  ```xml
  <context-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>/WEB-INF/applicationContext.xml</param-value>
  </context-param>
  <listener>
      <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
  ```

### 2.4 AnnotationConfigApplicationContext
- **Định nghĩa**: Tải cấu hình từ **các lớp Java** được chú thích bằng `@Configuration`, loại bỏ sự cần thiết của XML.
- **Trường hợp sử dụng**: Ưu tiên trong các ứng dụng độc lập hiện đại sử dụng cấu hình Java.
- **Ví dụ**:
  ```java
  @Configuration
  public class AppConfig {
      @Bean
      public MyBean myBean() {
          return new MyBean();
      }
  }

  ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
  MyBean myBean = context.getBean(MyBean.class);
  ```

### 2.5 AnnotationConfigWebApplicationContext
- **Định nghĩa**: Tải cấu hình từ **các lớp Java** được chú thích bằng `@Configuration` cho **ứng dụng web**.
- **Trường hợp sử dụng**: Sử dụng trong các ứng dụng Spring MVC hoặc Spring Boot ưu tiên cấu hình Java thay vì XML.
- **Ví dụ** (trong `web.xml`):
  ```xml
  <context-param>
      <param-name>contextClass</param-name>
      <param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext</param-value>
  </context-param>
  <context-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>com.example.AppConfig</param-value>
  </context-param>
  <listener>
      <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
  ```

## Tóm tắt
- **Core Container**: Cung cấp nền tảng cho IoC và DI với các module như Core, Bean, Context, và SpEL.
- **Application Context**: Cung cấp quản lý bean nâng cao với các triển khai cho ứng dụng độc lập và web, hỗ trợ cả cấu hình XML và Java.
