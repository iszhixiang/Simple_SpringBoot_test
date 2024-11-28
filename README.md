# Simple_SpringBoot_Test

### 2024/11/26 晚
#### 之前没学好 Spring Boot，现在想着抽时间把这块知识补回来，希望还不晚。
#### 一点点学习，把基础内容都加到这个项目里面，等后期功能差不多做好之后再将代码放到仓库中。同时，对每一块内容做出详细的解释，做到即使是初学者也能理解。如果可以做到这一点，那我也真正掌握了。Keep going!

---

## `1. 了解 pom 文件`

`pom.xml` 文件是新建 Spring Boot 项目时最先接触到的配置文件。它的作用是 **管理项目依赖和构建配置**。在 `pom.xml` 文件中，我们可以添加项目需要的依赖，比如数据库驱动、Thymeleaf、日志框架等，Spring Boot 会自动帮我们拉取这些依赖包。

回想起之前做的大一课程设计，连接数据库时，每次进行增删改查操作都需要手动配置数据库连接信息，甚至需要将 JDK 依赖手动加入项目，这种方式非常麻烦，容易出错。而 Spring Boot 帮我们自动装配好这些配置，大大减少了开发工作量，真的很棒！

---

## `2. application.properties 配置`

`application.properties` 是 Spring Boot 项目的 **全局配置文件**，用于集中管理整个项目的属性配置。它可以用于设置数据库连接、端口号、日志级别、缓存配置等信息。

通过将配置信息与代码分离，可以方便地管理和修改配置，而无需更改代码。

**示例配置：**
```properties
# 数据库配置
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
spring.datasource.password=123456

# 服务器端口配置
server.port=8080

# 日志级别配置
logging.level.root=INFO
```

## `3. 控制层 (Controller)`
在项目中创建一个名为 controller 的包，用来存放控制器类。控制层的主要作用是 接收用户请求，并调用服务层来完成相应的业务逻辑。

控制层的注解是 @Controller，它标志着一个类是控制器。

我的理解是：控制层就像一个指挥官，根据用户的请求，决定调用哪个服务，并将结果返回给用户。比如：

用户访问某个页面时，使用 @GetMapping 处理请求并返回视图。
用户提交数据时，使用 @PostMapping 处理数据并执行相应操作。
示例：

@Controller
public class UserController {
```
    @GetMapping("/home")
    public String showHomePage() {
        return "home"; // 返回 home.html 页面
    }

    @PostMapping("/register")
    public String registerUser(User user) {
        // 处理注册逻辑
        return "success"; // 返回成功页面
    }
}
```
## `4. 服务层 (Service)`
服务层的注解是 @Service，它的作用是 编写和封装业务逻辑。

举个例子，当用户登录时，需要查询用户信息是否存在，具体的查询逻辑会在服务层实现。控制层会调用服务层的方法，而服务层会返回结果。

示例：
```
@Service
public class UserService {

    public boolean login(String username, String password) {
        // 假设通过数据库查询验证用户名和密码
        return username.equals("admin") && password.equals("123456");
    }
}
```
## `5. 持久层 (Repository)`
持久层的注解是 @Repository，它的作用是 与数据库交互，进行数据的增删改查。持久层一般使用 JPA 或 MyBatis 等持久化框架来完成数据库操作。

虽然目前对持久层还不太理解，但随着学习的深入，会逐渐掌握它的用法。
```
示例：

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    // JpaRepository 提供了常见的增删改查方法
    Optional<User> findByUsername(String username);
}
```
## `6. 实体类 (Entity)`
实体类是 数据库表的映射类，每个实体类的字段与数据库表的列一一对应。在对数据库操作时，实体类就像一个数据载体，使数据的调用更加方便。

实体类的注解：

@Entity：标记为一个实体类。
@Table(name = "table_name")：指定实体类对应的数据库表名。
```
示例：
@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true)
    private String username;

    @Column(nullable = false)
    private String password;

    // Getters and Setters
}
```
在这个例子中：

数据库中的表名是 users。
字段 username 和 password 分别对应数据库中的列。
7. templates 层
templates 是视图层，用来存放 HTML 模板文件。在 Spring Boot 中，借助 Thymeleaf 模板引擎，可以实现动态渲染页面。

之前学习的 HTML 页面大多是静态的，而 Thymeleaf 可以结合后端数据生成动态页面，使得前端展示更加灵活。
```
示例：
用户访问主页时：

@Controller
public class HomeController {

    @GetMapping("/")
    public String home(Model model) {
        model.addAttribute("message", "欢迎来到主页！");
        return "index"; // 返回 index.html
    }
}
```
```
对应的 Thymeleaf 模板文件（templates/index.html）：

<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>主页</title>
</head>
<body>
    <h1 th:text="${message}">欢迎！</h1>
</body>
</html>
```
继续加油！ 2024/11/28 晚
