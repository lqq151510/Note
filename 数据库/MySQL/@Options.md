MyBatis 的 `@Options`注解就像给 SQL 语句加装的一个“控制面板”，让你能精细调控语句执行的方方面面，特别是处理**逻辑外键**时，它能帮你自动获取并填充数据库生成的主键（例如自增ID），从而在应用层面维持数据关联的完整性。

下面这个表格汇总了 `@Options`注解的核心参数，能帮你快速了解其功能。

|参数|类型|默认值|核心作用描述|
|---|---|---|---|
|**`useGeneratedKeys`**​|`boolean`|`false`|**核心：是否使用JDBC的getGeneratedKeys方法获取数据库自动生成的主键**。|
|**`keyProperty`**​|`String`|`"id"`|**核心：将生成的主键值赋值给参数对象的哪个属性**。支持点分隔的嵌套属性（如`user.id`）。|
|**`keyColumn`**​|`String`|`""`|当数据库主键列名与Java对象属性名不一致时，指定数据库表中的主键列名。|
|**`flushCache`**​|`boolean`|`false`|是否清空本地缓存和二级缓存。**对增删改语句有效，查询语句无效**。|
|**`useCache`**​|`boolean`|`true`|是否将本次查询结果存入二级缓存。**仅对查询语句有效**。|
|**`timeout`**​|`int`|`-1`|设置SQL语句执行超时时间，单位毫秒。`-1`表示使用驱动默认值。|
|**`fetchSize`**​|`int`|`-1`|给JDBC驱动的一个提示，每次从数据库读取的行数，可用于优化大结果集查询。|

### 💡 主要作用

`@Options`注解的核心价值在于，它允许你在方法级别直接定义一些SQL执行的细节配置，而无需在XML配置文件中进行繁琐的设置。具体作用包括：

1. **主键回填**：这是`@Options`最常用的功能。通过设置`useGeneratedKeys=true`和正确的`keyProperty`，在执行插入操作后，数据库自动生成的主键值会自动填充到参数对象的指定属性中。这使得你在代码中能直接通过对象获取该ID，无需再次查询数据库。
    
2. **缓存控制**：你可以通过`useCache`和`flushCache`参数来控制当前语句是否使用缓存或刷新缓存，这对于需要实时性的操作非常有用。
    
3. **性能调优**：通过`timeout`设置超时时间，防止慢查询长时间阻塞。通过`fetchSize`优化大数据量查询的内存占用。
    

### 🛠️ 如何使用与示例

将`@Options`注解直接放在Mapper接口的相应方法上即可，通常是`@Insert`、`@Update`等注解的下方。

**1. 插入数据并返回自增主键（最常用场景）**

假设有一个`User`实体，其自增主键为`id`。插入用户后，我们希望`user`对象的`id`属性能被自动赋值。

```
@Insert("INSERT INTO user (name, age) VALUES (#{name}, #{age})")
@Options(useGeneratedKeys = true, keyProperty = "id")
int insertUser(User user); // 调用后，数据库生成的id会设置到user对象的id属性中
```

调用该方法后，数据库自动生成的ID会直接赋给参数`user`对象的`id`属性。

```
User user = new User("张三", 25);
userMapper.insertUser(user); // 插入成功后
System.out.println("新用户的ID是：" + user.getId()); // 直接获取ID，无需再次查询
```

**2. 禁用二级缓存，确保获取实时数据**

对于需要实时性的查询，可以禁用缓存。

```
@Select("SELECT * FROM product WHERE stock > 0")
@Options(useCache = false)
List<Product> getAvailableProducts();
```

**3. 设置查询超时时间**

为防止查询长时间运行，可以设置超时。

```
@Select("SELECT * FROM orders WHERE user_id = #{userId}")
@Options(timeout = 5000) // 5秒超时
List<Order> getOrdersByUserId(Long userId);
```

### ⚠️ 使用注意事项

- **主键回填失效排查**：如果发现主键没有成功回填，请检查：1）数据库表的主键是否确实设置为自增；2）`keyProperty`指定的属性名是否与实体类中的属性名完全一致。
    
- **`keyProperty`支持嵌套属性**：如果主键需要设置到一个嵌套对象的属性中，可以使用点操作符，例如`@Options(useGeneratedKeys = true, keyProperty = "order.user.id")`。
    
- **批量插入的主键回填**：从MyBatis 3.3.1版本开始，批量插入也支持主键回填，但需要数据库驱动支持（如MySQL）。
    
- **非自增主键的生成**：对于Oracle这样使用序列（Sequence）的数据库，获取主键通常需要使用`@SelectKey`注解，而不是`@Options`的`useGeneratedKeys`属性。
    

希望这些解释和示例能帮助你更好地理解和使用MyBatis的`@Options`注解。