MyBatis 的 XML 映射文件是其核心组成部分，它负责定义 SQL 语句、结果映射、动态 SQL 等，实现了 SQL 与 Java 代码的分离。下面这个表格汇总了 XML 文件的主要构成部分及其核心作用，可以帮你快速建立整体认知。

|组成部分|核心标签/属性|主要作用与说明|
|---|---|---|
|**文件根元素**​|`<mapper namespace="...">`|**定义命名空间**，其值必须对应 Mapper 接口的全限定名，这是 XML 与接口关联的桥梁 。|
|**SQL 语句定义**​|`<select>`, `<insert>`, `<update>`, `<delete>`|分别对应查、增、改、删操作。`id`属性必须与接口方法名一致，`resultType`或 `resultMap`定义返回类型 。|
|**高级结果映射**​|`<resultMap>`, `<result>`, `<association>`, `<collection>`|当数据库字段名与 Java 对象属性名不一致或存在复杂关联（一对一、一对多）时，用于自定义映射规则 。|
|**动态 SQL**​|`<if>`, `<where>`, `<foreach>`, `<choose>`|根据参数条件动态拼接 SQL，避免繁琐的字符串拼接，有效解决 SQL 拼接难题 。|
|**可重用 SQL 片段**​|`<sql>`, `<include>`|通过 `<sql>`定义可重用的 SQL 代码块，再通过 `<include>`引入，提升代码复用性和整洁度 。|
|**缓存配置**​|`<cache>`|配置 Mapper 级别的**二级缓存**，可以跨 SqlSession 共享数据，提升查询性能 。|

---

### 💡 核心元素详解与示例

#### 1. 基本结构与 CRUD

一个标准的 XML 映射文件以 `<mapper>`为根标签，并通过 `namespace`与 Mapper 接口绑定 。

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
                        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.mapper.UserMapper">
    
    <!-- 查询用户 By ID -->
    <select id="selectUserById" parameterType="int" resultType="com.example.model.User">
        SELECT * FROM user WHERE id = #{id}
    </select>
    
    <!-- 插入用户，并返回自增主键 -->
    <insert id="insertUser" parameterType="com.example.model.User" useGeneratedKeys="true" keyProperty="id">
        INSERT INTO user (name, age) VALUES (#{name}, #{age})
    </insert>
    
    <!-- 更新用户信息 -->
    <update id="updateUser" parameterType="com.example.model.User">
        UPDATE user SET name=#{name}, age=#{age} WHERE id=#{id}
    </update>
    
    <!-- 根据ID删除用户 -->
    <delete id="deleteUserById" parameterType="int">
        DELETE FROM user WHERE id = #{id}
    </delete>
</mapper>
```

#### 2. 动态 SQL

动态 SQL 是 MyBatis 最强大的特性之一，让你能构建灵活且强大的查询 。

```
<!-- 一个综合运用了 <if>, <where>, <foreach> 的动态查询示例 -->
<select id="selectByCondition" resultType="com.example.model.User">
    SELECT * FROM user
    <where>
        <!-- 如果传入了 name 参数，则拼接 AND name 条件 -->
        <if test="name != null and name != ''">
            AND name LIKE CONCAT('%', #{name}, '%')
        </if>
        <!-- 如果传入了 minAge 参数，则拼接 AND age 条件 -->
        <if test="minAge != null">
            AND age >= #{minAge}
        </if>
        <!-- 遍历传入的 id 列表，进行批量查询 -->
        <if test="ids != null and ids.size() > 0">
            AND id IN
            <foreach collection="ids" item="id" open="(" separator="," close=")">
                #{id}
            </foreach>
        </if>
    </where>
    ORDER BY id DESC
</select>
```

- **`<where>`标签**：它会智能地处理 `WHERE`关键字。如果其内部嵌套的标签没有生成任何内容，`WHERE`关键字会被忽略；如果内部内容以 `AND`或 `OR`开头，它会被自动移除 。
    
- **`<if>`标签**：通过 `test`属性进行条件判断，为 `true`时才会拼接其内部的 SQL 片段。
    
- **`<foreach>`标签**：常用于 `IN`语句，遍历集合参数。`collection`指定集合，`item`是每次遍历的变量名，`open`和 `close`是循环开始和结束时拼接的符号，`separator`是元素间的分隔符 。
    

#### 3. 高级结果映射 (`<resultMap>`)

当字段名（如 `user_name`）与属性名（如 `userName`）不一致，或查询包含关联对象时，需要使用 `<resultMap>`。

```
<!-- 定义名为 UserResultMap 的结果映射 -->
<resultMap id="UserResultMap" type="com.example.model.User">
    <!-- 主键字段映射 -->
    <id property="id" column="id"/>
    <!-- 普通字段映射 -->
    <result property="username" column="user_name"/>
    <result property="age" column="age"/>
    
    <!-- 一对一关联：一个用户对应一个部门 -->
    <association property="department" javaType="com.example.model.Department">
        <id property="id" column="dept_id"/>
        <result property="name" column="dept_name"/>
    </association>
    
    <!-- 一对多关联：一个用户对应多个订单 -->
    <collection property="orders" ofType="com.example.model.Order">
        <id property="orderId" column="order_id"/>
        <result property="orderNo" column="order_no"/>
    </collection>
</resultMap>

<!-- 在查询语句中引用定义好的 resultMap -->
<select id="selectUserWithAssociations" resultMap="UserResultMap">
    SELECT 
        u.id, 
        u.name as user_name, 
        u.age,
        d.id as dept_id,
        d.name as dept_name,
        o.id as order_id,
        o.order_no
    FROM user u
    LEFT JOIN department d ON u.dept_id = d.id
    LEFT JOIN order o ON u.id = o.user_id
    WHERE u.id = #{userId}
</select>
```

---

### ⚠️ 配置文件要点与最佳实践

1. **命名与位置规范**：XML 文件名称应与 Mapper 接口名相同（如 `UserMapper.java`对应 `UserMapper.xml`），并且放置在相同的包结构下。在 Maven 项目中，通常将 XML 文件放在 `src/main/resources`下对应的包路径中 。
    
2. **`useGeneratedKeys`**：在 `<insert>`标签中设置 `useGeneratedKeys="true"`和 `keyProperty="id"`，可以让 MyBatis 获取数据库自动生成的主键值，并直接赋给传入对象的 `id`属性 。
    
3. **SQL 注入防范**：**始终使用 `#{}`参数占位符**，而不是 `${}`进行字符串拼接。`#{}`会使用预编译语句，安全地处理参数，有效防止 SQL 注入攻击。`${}`仅用于动态指定表名、列名等非参数部分 。
    

希望这份详细的解释能帮助你更好地理解和使用 MyBatis 的 XML 映射文件！如果你对动态 SQL 的更多标签（如 `<set>`, `<choose>`）或缓存的具体配置有进一步兴趣，我可以继续为你介绍。