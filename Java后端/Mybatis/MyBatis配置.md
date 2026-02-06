MyBatis 是一款优秀的 **Java 持久层框架**，它主要解决了传统 JDBC 编程中的繁琐操作（如手动设置参数、处理结果集等），让你能更专注于 SQL 语句本身。

### 🧩 核心特性与优势

MyBatis 通过以下几个核心特性，在简化开发和提高灵活性之间取得了很好的平衡：

|特性|说明|
|---|---|
|**SQL 与代码分离**​|SQL 语句写在 XML 配置文件或通过注解定义，与 Java 代码解耦，便于统一管理和优化。|
|**强大的映射能力**​|能轻松地将数据库中的记录与 Java 对象（POJO）进行相互转换，并支持复杂关系映射。|
|**动态 SQL**​|提供了灵活的标签（如 `<if>`, `<where>`, `<foreach>`），可以根据条件动态生成 SQL 语句，避免了繁琐的字符串拼接。|
|**轻量级与灵活性**​|框架本身小巧且无太多依赖。它允许你编写原生的 SQL，对数据库设计和应用架构不会强加约束，被称为**半自动化的 ORM 框架**。|

与 JDBC 和 Hibernate 等全自动 ORM 框架相比，MyBatis 在灵活性和控制力上找到了一个平衡点，让你既能享受对象映射的便利，又能对 SQL 进行精细优化。

### ⚙️ 配置流程详解

配置 MyBatis 主要涉及几个核心步骤，其核心组件协作关系如下：

```
flowchart TD
    A[项目pom.xml] --> B[添加MyBatis<br>及数据库驱动依赖]
    B --> C[创建MyBatis核心配置文件<br>mybatis-config.xml]
    C --> D[配置数据源/连接池<br>事务管理器等]
    D --> E[创建Mapper接口<br>定义数据库操作方法]
    E --> F[创建对应的Mapper.xml文件<br>编写SQL语句]
    F --> G[在核心配置文件中<br>注册Mapper.xml]
    G --> H[编写代码<br>通过SqlSession执行操作]
```

#### 1. 添加项目依赖

在 Maven 项目的 `pom.xml`中添加 MyBatis 和数据库驱动依赖。

```
<dependencies>
    <!-- MyBatis 核心 -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.7</version>
    </dependency>
    <!-- MySQL 驱动 -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.27</version>
    </dependency>
</dependencies>
```

#### 2. 创建核心配置文件

在项目的资源目录（如 `src/main/resources`）下创建 `mybatis-config.xml`文件。这个文件用于配置数据源、事务管理器、以及指定 SQL 映射文件的位置等。

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <!-- 使用 JDBC 事务管理 -->
            <transactionManager type="JDBC"/>
            <!-- 配置数据源，使用内置的连接池 -->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/你的数据库名?serverTimezone=UTC"/>
                <property name="username" value="你的用户名"/>
                <property name="password" value="你的密码"/>
            </dataSource>
        </environment>
    </environments>
    
    <!-- 告诉 MyBatis 去哪里找 SQL 映射文件（Mapper.xml） -->
    <mappers>
        <mapper resource="com/example/mapper/UserMapper.xml"/>
    </mappers>
</configuration>
```

#### 3. 创建 Mapper 接口与 XML 文件

Mapper 接口定义了数据访问的方法，而对应的 XML 文件则实现了具体的 SQL 语句。

- **Mapper 接口示例**​ (`UserMapper.java`)：
    

```
public interface UserMapper {
    User selectUserById(int id);
}
```

- **对应的 XML 映射文件**​ (`UserMapper.xml`)：此文件需与接口在同一包下，且名称相同。
    

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- namespace 属性对应 Mapper 接口的全限定名 -->
<mapper namespace="com.example.mapper.UserMapper">
    <!-- id 对应接口中的方法名，resultType 指定返回的实体类 -->
    <select id="selectUserById" resultType="com.example.entity.User">
        SELECT * FROM user WHERE id = #{id}
    </select>
</mapper>
```

#### 4. 在代码中使用 MyBatis

可以通过 `SqlSessionFactory`和 `SqlSession`来执行数据库操作。

```
// 1. 加载核心配置文件
String resource = "mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
// 2. 创建 SqlSessionFactory
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
// 3. 获取 SqlSession
try (SqlSession session = sqlSessionFactory.openSession()) {
    // 4. 获取 Mapper 接口的代理对象并调用方法
    UserMapper mapper = session.getMapper(UserMapper.class);
    User user = mapper.selectUserById(1);
    System.out.println(user);
}
```

### 💡 进阶配置与建议

- **使用外部属性文件**：可以将数据库连接信息单独放在 `db.properties`文件中，然后在 `mybatis-config.xml`中引用，提高配置灵活性。
    
- **配置类型别名**：在核心配置文件中定义类型别名，可以避免在 Mapper.xml 文件中重复书写冗长的 Java 类全名。
    
- **集成连接池**：对于生产环境，建议使用性能更好的连接池（如 HikariCP）来代替 MyBatis 内置的 POOLED 数据源。
    

### 💎 总结

总的来说，MyBatis 的核心价值在于它让你能以**声明式**的方法管理数据访问，既保留了 SQL 的灵活性，又极大地减少了模板代码。现在许多项目会选择将 MyBatis 与 Spring Boot 集成，这会进一步简化配置。如果你对这个方向感兴趣，我们可以继续探讨。

希望这些信息能帮助你快速上手 MyBatis。