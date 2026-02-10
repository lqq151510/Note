MyBatis的动态SQL功能允许你在XML映射文件中，根据特定条件灵活地构建SQL语句，这极大地避免了在Java代码中手动拼接SQL字符串的烦恼。

下面这个表格汇总了最常用的几个动态SQL标签及其核心作用。

|标签|主要作用|关键点说明|
|---|---|---|
|**`<if>`**​|条件判断|根据`test`属性的OGNL表达式结果，决定是否包含其中的SQL片段。|
|**`<where>`**​|封装WHERE子句|1. 只有子元素有内容时才会插入`WHERE`关键字。  <br>2. 自动去除子句开头多余的`AND`或`OR`。|
|**`<set>`**​|封装SET子句|1. 用于UPDATE语句。  <br>2. 动态前置SET关键字，并删除结尾无关的逗号。|
|**`<foreach>`**​|遍历集合|常用于`IN`条件或批量操作（如插入、更新）。需指定集合(`collection`)、项(`item`)等属性。|
|**`<choose>`、`<when>`、`<otherwise>`**​|多路选择|类似Java的`switch-case-default`结构，从多个条件中选择一个执行。|
|**`<trim>`**​|自定义字符串修剪|功能更底层，可通过属性定制SQL片段的**前缀**、**后缀**及其覆盖规则，可模拟`<where>`和`<set>`的功能。|
|**`<sql>`和 `<include>`**​|代码复用|`<sql>`定义可重用的SQL片段，`<include>`在需要的地方引用，提高代码维护性。|

### 💡 核心标签详解与示例

- **`<if>`标签**：这是最基础的标签。它的`test`属性里写的是判断条件。
    
    ```
    <select id="findActiveBlog" resultType="Blog">
      SELECT * FROM BLOG WHERE state = 'ACTIVE'
      <if test="title != null">
        AND title like #{title}
      </if>
    </select>
    ```
    
    如果传入的`title`参数不为空，`AND title like #{title}`才会被拼接到SQL中。
    
- **`<where>`标签**：它解决了使用`<if>`时可能产生的语法错误。比如，如果所有`<if>`条件都不满足，SQL会变成`SELECT * FROM BLOG WHERE`，而`<where>`标签会在至少一个子条件成立时才插入`WHERE`，并智能地去除掉第一个条件前的`AND`或`OR`。
    
- **`<foreach>`标签**：在需要对集合进行迭代时非常有用，比如根据ID列表查询。
    
    ```
    <select id="selectPostIn" resultType="domain.blog.Post">
      SELECT * FROM POST P WHERE ID in
      <foreach collection="list" item="item" open="(" separator="," close=")">
        #{item}
      </foreach>
    </select>
    ```
    
    关键属性包括：`collection`（集合参数名）、`item`（每次迭代的别名）、`open`/`close`（包裹符号，如括号）、`separator`（分隔符）。
    

### ⚠️ 实用技巧与注意事项

1. **优先使用 `<where>`和 `<set>`**：它们比手动写`WHERE 1=1`或处理逗号更安全、更简洁。
    
2. **`<trim>`标签的灵活性**：你可以用`<trim>`实现`<where>`和`<set>`的功能。例如，`<trim prefix="WHERE" prefixOverrides="AND |OR ">`等价于`<where>`标签。
    
3. **避免条件陷阱**：在`<if>`的`test`条件中，应使用`and`进行逻辑与操作，而不是`&&`。判断字符串是否为空时，常用`null != and != ''`。
    
4. **批量操作**：`<foreach>`标签除了用于查询中的`IN`条件，还可实现**批量插入**。
    
    ```
    <insert id="insertUsers">
        insert into user (name, email) values
        <foreach collection="list" item="user" separator=",">
            (#{user.name}, #{user.email})
        </foreach>
    </insert>
    ```
    

### 💎 总结

MyBatis的动态SQL标签让SQL的拼接变得直观且安全。核心思路是根据不同场景选择合适的标签：**条件判断用`<if>`，组合条件用`<where>`，更新字段用`<set>`，遍历集合用`<foreach>`，多选一用`<choose>`**。熟练掌握它们能显著提升开发效率并减少错误。

希望这些信息能帮助你更好地运用MyBatis动态SQL。如果你有具体的应用场景或遇到其他问题，我很乐意继续为你解答。