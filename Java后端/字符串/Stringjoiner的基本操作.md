# StringJoiner基本操作详解

## 一、StringJoiner概述

### 什么是StringJoiner？

**StringJoiner**是Java 8引入的类，专门用于**构建由分隔符分隔的字符串序列**，可以指定前缀和后缀。

```
// 传统方式 vs StringJoiner
// 传统方式
String[] fruits = {"Apple", "Banana", "Orange"};
StringBuilder sb = new StringBuilder();
for (int i = 0; i < fruits.length; i++) {
    if (i > 0) sb.append(", ");
    sb.append(fruits[i]);
}
String result1 = sb.toString();  // "Apple, Banana, Orange"

// StringJoiner方式
StringJoiner sj = new StringJoiner(", ");
for (String fruit : fruits) {
    sj.add(fruit);
}
String result2 = sj.toString();  // "Apple, Banana, Orange"
```

---

## 二、创建StringJoiner

### 1. 基本创建方式

```
import java.util.StringJoiner;

// 1. 只指定分隔符
StringJoiner sj1 = new StringJoiner(", ");
System.out.println("sj1: " + sj1.toString());  // ""

// 2. 指定分隔符、前缀、后缀
StringJoiner sj2 = new StringJoiner(", ", "[", "]");
System.out.println("sj2: " + sj2.toString());  // "[]"

// 3. 使用空值处理
StringJoiner sj3 = new StringJoiner("-", "PREFIX_", "_SUFFIX");
System.out.println("sj3: " + sj3.toString());  // "PREFIX__SUFFIX"
```

### 2. 常用分隔符示例

```
// CSV格式
StringJoiner csv = new StringJoiner(",");

// SQL IN条件
StringJoiner sqlIn = new StringJoiner("', '", "'", "'");

// URL参数
StringJoiner urlParams = new StringJoiner("&");

// 文件路径
StringJoiner path = new StringJoiner("/");

// JSON数组
StringJoiner jsonArray = new StringJoiner(", ", "[", "]");

// HTML列表
StringJoiner htmlList = new StringJoiner("</li><li>", "<ul><li>", "</li></ul>");
```

---

## 三、基本操作

### 1. 添加元素

```
StringJoiner sj = new StringJoiner(", ");

// 添加单个元素
sj.add("Apple");
System.out.println("添加Apple后: " + sj);  // "Apple"

sj.add("Banana");
System.out.println("添加Banana后: " + sj);  // "Apple, Banana"

sj.add("Orange");
System.out.println("添加Orange后: " + sj);  // "Apple, Banana, Orange"

// 可以链式调用
StringJoiner sj2 = new StringJoiner(" - ")
    .add("Java")
    .add("Python")
    .add("JavaScript");
System.out.println("链式添加: " + sj2);  // "Java - Python - JavaScript"
```

### 2. 添加多个元素

```
StringJoiner sj = new StringJoiner(", ");

// 批量添加
String[] fruits = {"Apple", "Banana", "Cherry"};
for (String fruit : fruits) {
    sj.add(fruit);
}
System.out.println("批量添加: " + sj);  // "Apple, Banana, Cherry"

// 使用Stream添加
StringJoiner sj2 = new StringJoiner(" | ");
java.util.Arrays.asList("One", "Two", "Three")
    .forEach(sj2::add);
System.out.println("Stream添加: " + sj2);  // "One | Two | Three"
```

### 3. 设置空值处理

```
// 创建时没有元素
StringJoiner sj1 = new StringJoiner(", ", "[", "]");
System.out.println("空joiner: " + sj1.toString());  // "[]"

// 添加null值
StringJoiner sj2 = new StringJoiner(", ");
sj2.add("Apple");
sj2.add(null);  // 可以添加null
sj2.add("Orange");
System.out.println("包含null: " + sj2);  // "Apple, null, Orange"

// 设置空值默认文本
StringJoiner sj3 = new StringJoiner(", ", "【", "】");
sj3.setEmptyValue("【暂无数据】");
System.out.println("空值默认: " + sj3);  // "【暂无数据】"

sj3.add("数据1");
System.out.println("有数据后: " + sj3);  // "【数据1】"
```

---

## 四、前缀和后缀

### 1. 基本使用

```
// 创建带前缀后缀的StringJoiner
StringJoiner sj = new StringJoiner(", ", "[", "]");

sj.add("A");
System.out.println("添加A后: " + sj);  // "[A]"

sj.add("B");
System.out.println("添加B后: " + sj);  // "[A, B]"

sj.add("C");
System.out.println("添加C后: " + sj);  // "[A, B, C]"

// 没有元素时
StringJoiner sj2 = new StringJoiner(", ", "{", "}");
System.out.println("空时: " + sj2);  // "{}"

// 设置空值文本
sj2.setEmptyValue("空集合");
System.out.println("设置空值后: " + sj2);  // "空集合"
```

### 2. 实际应用场景

```
// 1. JSON数组
StringJoiner jsonArray = new StringJoiner(", ", "[", "]");
jsonArray.add("\"Apple\"");
jsonArray.add("\"Banana\"");
jsonArray.add("\"Orange\"");
System.out.println("JSON数组: " + jsonArray);  // ["Apple","Banana","Orange"]

// 2. SQL IN语句
StringJoiner sqlIn = new StringJoiner("', '", "('", "')");
sqlIn.add("John");
sqlIn.add("Jane");
sqlIn.add("Bob");
String sql = "SELECT * FROM users WHERE name IN " + sqlIn;
System.out.println("SQL: " + sql);  // SELECT * FROM users WHERE name IN ('John', 'Jane', 'Bob')

// 3. HTML列表
StringJoiner htmlList = new StringJoiner("</li>\n<li>", "<ul>\n<li>", "</li>\n</ul>");
htmlList.add("首页");
htmlList.add("产品");
htmlList.add("关于我们");
System.out.println("HTML列表:\n" + htmlList);

// 4. 方法参数
StringJoiner params = new StringJoiner(", ", "(", ")");
params.add("String name");
params.add("int age");
params.add("double score");
System.out.println("方法参数: " + params);  // (String name, int age, double score)
```

---

## 五、合并StringJoiner

### 1. merge方法

```
// 创建两个StringJoiner
StringJoiner sj1 = new StringJoiner(", ", "[", "]");
sj1.add("A").add("B");

StringJoiner sj2 = new StringJoiner(" - ", "{", "}");
sj2.add("X").add("Y");

// 合并sj2到sj1
sj1.merge(sj2);
System.out.println("合并后sj1: " + sj1);  // "[A, B, X - Y]"
System.out.println("sj2不变: " + sj2);    // "{X - Y}"

// 注意：前缀后缀以第一个为准
StringJoiner sj3 = new StringJoiner(", ", "【", "】");
StringJoiner sj4 = new StringJoiner("|", "[[", "]]");
sj3.add("1").add("2");
sj4.add("a").add("b");

sj3.merge(sj4);
System.out.println("合并前缀后缀: " + sj3);  // "【1, 2, a|b】"
```

### 2. 合并规则详解

```
// 合并示例1：空joiner合并
StringJoiner sj1 = new StringJoiner(", ");
StringJoiner sj2 = new StringJoiner("|");
sj2.add("X").add("Y");

sj1.merge(sj2);
System.out.println("空合并有: " + sj1);  // "X|Y"

// 合并示例2：有内容joiner合并空joiner
StringJoiner sj3 = new StringJoiner(", ");
StringJoiner sj4 = new StringJoiner("|");
sj3.add("A").add("B");

sj3.merge(sj4);
System.out.println("有合并空: " + sj3);  // "A, B"

// 合并示例3：合并后的分隔符处理
StringJoiner sj5 = new StringJoiner("@");
StringJoiner sj6 = new StringJoiner("#");
sj5.add("1").add("2");
sj6.add("a").add("b");

sj5.merge(sj6);
System.out.println("分隔符合并: " + sj5);  // "1@2@a#b"
```

---

## 六、与Stream配合使用

### 1. Collectors.joining()

```
import java.util.stream.Collectors;
import java.util.Arrays;
import java.util.List;

List<String> fruits = Arrays.asList("Apple", "Banana", "Cherry", "Date");

// 1. 基本joining
String result1 = fruits.stream()
    .collect(Collectors.joining());
System.out.println("无分隔符: " + result1);  // "AppleBananaCherryDate"

// 2. 带分隔符
String result2 = fruits.stream()
    .collect(Collectors.joining(", "));
System.out.println("有分隔符: " + result2);  // "Apple, Banana, Cherry, Date"

// 3. 带前缀后缀
String result3 = fruits.stream()
    .collect(Collectors.joining(", ", "[", "]"));
System.out.println("全都有: " + result3);  // "[Apple, Banana, Cherry, Date]"

// 4. 过滤后joining
String result4 = fruits.stream()
    .filter(f -> f.length() > 5)
    .collect(Collectors.joining(" | "));
System.out.println("过滤后: " + result4);  // "Banana | Cherry"

// 5. 映射后joining
String result5 = fruits.stream()
    .map(String::toUpperCase)
    .collect(Collectors.joining("; "));
System.out.println("大写后: " + result5);  // "APPLE; BANANA; CHERRY; DATE"
```

### 2. 自定义收集器

```
// 使用StringJoiner作为收集器
String result = fruits.stream()
    .collect(
        () -> new StringJoiner(", ", "[", "]"),  // 供应者
        (joiner, item) -> joiner.add(item),      // 累加器
        (joiner1, joiner2) -> joiner1.merge(joiner2)  // 组合器
    )
    .toString();

System.out.println("自定义收集: " + result);
```

### 3. 复杂Stream操作

```
List<Person> people = Arrays.asList(
    new Person("Alice", 25),
    new Person("Bob", 30),
    new Person("Charlie", 35)
);

// 提取属性并连接
String names = people.stream()
    .map(Person::getName)
    .collect(Collectors.joining(", ", "Names: ", ""));
System.out.println(names);  // "Names: Alice, Bob, Charlie"

// 条件连接
String adults = people.stream()
    .filter(p -> p.getAge() >= 18)
    .map(p -> p.getName() + "(" + p.getAge() + ")")
    .collect(Collectors.joining("; "));
System.out.println(adults);  // "Alice(25); Bob(30); Charlie(35)"

class Person {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public String getName() { return name; }
    public int getAge() { return age; }
}
```

---

## 七、实际应用示例

### 示例1：SQL查询构建器

```
public class SQLQueryBuilder {
    public static void main(String[] args) {
        // 构建SELECT语句
        String table = "users";
        StringJoiner columns = new StringJoiner(", ");
        columns.add("id");
        columns.add("name");
        columns.add("email");
        
        // 构建WHERE条件
        StringJoiner conditions = new StringJoiner(" AND ");
        conditions.add("age > 18");
        conditions.add("status = 'active'");
        
        // 构建ORDER BY
        StringJoiner orderBy = new StringJoiner(", ");
        orderBy.add("created_at DESC");
        orderBy.add("name ASC");
        
        // 组合SQL
        StringBuilder sql = new StringBuilder();
        sql.append("SELECT ").append(columns)
           .append(" FROM ").append(table);
        
        if (conditions.length() > 0) {
            sql.append(" WHERE ").append(conditions);
        }
        
        if (orderBy.length() > 0) {
            sql.append(" ORDER BY ").append(orderBy);
        }
        
        System.out.println("生成的SQL:");
        System.out.println(sql.toString());
        // SELECT id, name, email FROM users WHERE age > 18 AND status = 'active' ORDER BY created_at DESC, name ASC
    }
}
```

### 示例2：CSV文件生成

```
public class CSVGenerator {
    public static void main(String[] args) {
        List<String[]> data = Arrays.asList(
            new String[]{"1", "Alice", "25", "Engineer"},
            new String[]{"2", "Bob", "30", "Designer"},
            new String[]{"3", "Charlie", "35", "Manager"}
        );
        
        // 表头
        StringJoiner header = new StringJoiner(",");
        header.add("ID").add("Name").add("Age").add("Position");
        
        // 数据行
        List<String> rows = new ArrayList<>();
        for (String[] row : data) {
            StringJoiner rowJoiner = new StringJoiner(",");
            for (String cell : row) {
                // 处理可能包含逗号的内容
                String escaped = cell.contains(",") ? "\"" + cell + "\"" : cell;
                rowJoiner.add(escaped);
            }
            rows.add(rowJoiner.toString());
        }
        
        // 合并所有行
        StringJoiner csv = new StringJoiner("\n");
        csv.add(header.toString());
        rows.forEach(csv::add);
        
        System.out.println("生成的CSV:");
        System.out.println(csv.toString());
        
        // 或者用Stream一行搞定
        String csv2 = Stream.concat(
            Stream.of(header.toString()),
            data.stream().map(row -> String.join(",", row))
        ).collect(Collectors.joining("\n"));
        
        System.out.println("\nStream方式生成的CSV:");
        System.out.println(csv2);
    }
}
```

### 示例3：HTML生成器

```
public class HTMLGenerator {
    public static void main(String[] args) {
        // 生成无序列表
        List<String> items = Arrays.asList("首页", "产品介绍", "关于我们", "联系我们");
        
        StringJoiner listItems = new StringJoiner("</li>\n    <li>", "    <li>", "</li>");
        items.forEach(listItems::add);
        
        String html = "<!DOCTYPE html>\n" +
                     "<html>\n" +
                     "<head>\n" +
                     "    <title>我的网站</title>\n" +
                     "</head>\n" +
                     "<body>\n" +
                     "    <h1>导航菜单</h1>\n" +
                     "    <ul>\n" +
                     listItems.toString() + "\n" +
                     "    </ul>\n" +
                     "</body>\n" +
                     "</html>";
        
        System.out.println(html);
        
        // 生成表格
        List<List<String>> tableData = Arrays.asList(
            Arrays.asList("张三", "25", "工程师"),
            Arrays.asList("李四", "30", "设计师"),
            Arrays.asList("王五", "28", "产品经理")
        );
        
        StringJoiner tableRows = new StringJoiner("\n        ");
        for (List<String> row : tableData) {
            StringJoiner cells = new StringJoiner("</td><td>", "        <tr><td>", "</td></tr>");
            row.forEach(cells::add);
            tableRows.add(cells.toString());
        }
        
        String tableHTML = "<table border='1'>\n" +
                          "    <tr><th>姓名</th><th>年龄</th><th>职位</th></tr>\n" +
                          tableRows.toString() + "\n" +
                          "</table>";
        
        System.out.println("\n生成的表格:");
        System.out.println(tableHTML);
    }
}
```

### 示例4：URL参数构建

```
public class URLBuilder {
    public static void main(String[] args) {
        Map<String, Object> params = new HashMap<>();
        params.put("q", "java programming");
        params.put("page", 1);
        params.put("limit", 20);
        params.put("sort", "date");
        params.put("order", "desc");
        
        // 构建查询字符串
        StringJoiner queryString = new StringJoiner("&");
        params.forEach((key, value) -> {
            try {
                String encodedKey = java.net.URLEncoder.encode(key, "UTF-8");
                String encodedValue = java.net.URLEncoder.encode(value.toString(), "UTF-8");
                queryString.add(encodedKey + "=" + encodedValue);
            } catch (Exception e) {
                // 忽略编码错误
            }
        });
        
        String url = "https://api.example.com/search?" + queryString;
        System.out.println("生成的URL:");
        System.out.println(url);
        
        // 构建RESTful路径参数
        String userId = "123";
        String postId = "456";
        StringJoiner path = new StringJoiner("/");
        path.add("api").add("v1").add("users").add(userId).add("posts").add(postId);
        
        String apiUrl = "https://api.example.com/" + path;
        System.out.println("\nAPI路径:");
        System.out.println(apiUrl);
    }
}
```

### 示例5：日志消息格式化

```
public class LogFormatter {
    public static void main(String[] args) {
        // 格式化日志消息
        String timestamp = "2024-01-15 10:30:00";
        String level = "INFO";
        String thread = "main";
        String className = "com.example.MyClass";
        String message = "Processing completed successfully";
        
        StringJoiner logEntry = new StringJoiner(" ", "[", "]");
        logEntry.add(timestamp)
                .add(level)
                .add("[" + thread + "]")
                .add(className + ":")
                .add(message);
        
        System.out.println("日志消息:");
        System.out.println(logEntry.toString());
        
        // 带上下文的日志
        Map<String, String> context = new HashMap<>();
        context.put("userId", "12345");
        context.put("sessionId", "abc123def");
        context.put("requestId", "req_789");
        
        StringJoiner contextStr = new StringJoiner(", ", "{", "}");
        context.forEach((k, v) -> contextStr.add(k + "=" + v));
        
        String detailedLog = String.format("[%s] [%s] %s - %s %s", 
            timestamp, level, thread, message, contextStr);
        
        System.out.println("\n详细日志:");
        System.out.println(detailedLog);
    }
}
```

---

## 八、性能考虑

### 1. 与StringBuilder性能对比

```
public class PerformanceComparison {
    public static void main(String[] args) {
        int iterations = 100000;
        String delimiter = ", ";
        
        // 测试StringJoiner
        long start1 = System.nanoTime();
        StringJoiner sj = new StringJoiner(delimiter);
        for (int i = 0; i < iterations; i++) {
            sj.add("item" + i);
        }
        String result1 = sj.toString();
        long time1 = System.nanoTime() - start1;
        
        // 测试StringBuilder
        long start2 = System.nanoTime();
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < iterations; i++) {
            if (i > 0) sb.append(delimiter);
            sb.append("item" + i);
        }
        String result2 = sb.toString();
        long time2 = System.nanoTime() - start2;
        
        // 测试StringJoiner（带前缀后缀）
        long start3 = System.nanoTime();
        StringJoiner sj2 = new StringJoiner(delimiter, "[", "]");
        for (int i = 0; i < iterations; i++) {
            sj2.add("item" + i);
        }
        String result3 = sj2.toString();
        long time3 = System.nanoTime() - start3;
        
        System.out.printf("StringJoiner: %.3f ms%n", time1 / 1_000_000.0);
        System.out.printf("StringBuilder: %.3f ms%n", time2 / 1_000_000.0);
        System.out.printf("StringJoiner(带括号): %.3f ms%n", time3 / 1_000_000.0);
        
        // 结论：StringJoiner在需要分隔符的场景下更简洁，性能相近
    }
}
```

### 2. 内存使用

```
// StringJoiner内部使用StringBuilder，所以内存使用相似
// 但StringJoiner多了分隔符、前缀、后缀的存储

StringJoiner sj = new StringJoiner(", ", "[", "]");
// 相当于：
StringBuilder sb = new StringBuilder();
String delimiter = ", ";
String prefix = "[";
String suffix = "]";
```

---

## 九、常见问题解答

### 1. StringJoiner是线程安全的吗？

```
// 不，StringJoiner不是线程安全的
// 和StringBuilder一样，只能在单线程中使用

// 如果需要线程安全，可以使用StringBuffer或同步
StringJoiner sj = new StringJoiner(", ");
synchronized(sj) {
    sj.add("item1");
    sj.add("item2");
}
```

### 2. 如何清空StringJoiner？

```
StringJoiner sj = new StringJoiner(", ", "[", "]");
sj.add("A").add("B").add("C");
System.out.println("原内容: " + sj);  // "[A, B, C]"

// StringJoiner没有clear()方法，需要创建新的
sj = new StringJoiner(", ", "[", "]");
System.out.println("清空后: " + sj);  // "[]"

// 或者使用setEmptyValue
sj.add("X").add("Y");
System.out.println("添加后: " + sj);  // "[X, Y]"
```

### 3. 如何处理空值或空字符串？

```
StringJoiner sj = new StringJoiner(", ");

// 添加空字符串
sj.add("");  // 会添加空字符串
sj.add("Apple");
System.out.println("有空字符串: " + sj);  // ", Apple"

// 过滤空值
List<String> items = Arrays.asList("", "Apple", null, "Banana", "");
String result = items.stream()
    .filter(item -> item != null && !item.isEmpty())
    .collect(Collectors.joining(", "));
System.out.println("过滤后: " + result);  // "Apple, Banana"
```

### 4. 如何获取当前长度？

```
StringJoiner sj = new StringJoiner(", ", "[", "]");
sj.add("Hello");
sj.add("World");

int length = sj.length();
System.out.println("长度: " + length);  // 13 (包括前缀后缀和分隔符)
System.out.println("字符串: " + sj);    // "[Hello, World]"

// toString()和length()方法
System.out.println("toString(): " + sj.toString());
System.out.println("length(): " + sj.length());
```

---

## 十、高级技巧

### 1. 自定义空值处理

```
class SmartStringJoiner {
    private StringJoiner joiner;
    private String emptyValue;
    
    public SmartStringJoiner(CharSequence delimiter) {
        this.joiner = new StringJoiner(delimiter);
        this.emptyValue = "";
    }
    
    public SmartStringJoiner(CharSequence delimiter, 
                            CharSequence prefix, 
                            CharSequence suffix) {
        this.joiner = new StringJoiner(delimiter, prefix, suffix);
        this.emptyValue = prefix.toString() + suffix.toString();
    }
    
    public SmartStringJoiner addIfNotNull(Object obj) {
        if (obj != null) {
            joiner.add(obj.toString());
        }
        return this;
    }
    
    public SmartStringJoiner addIfNotEmpty(String str) {
        if (str != null && !str.trim().isEmpty()) {
            joiner.add(str);
        }
        return this;
    }
    
    public SmartStringJoiner addWithDefault(Object obj, String defaultValue) {
        String value = (obj == null) ? defaultValue : obj.toString();
        joiner.add(value);
        return this;
    }
    
    @Override
    public String toString() {
        String result = joiner.toString();
        return result.equals(emptyValue) ? "（无内容）" : result;
    }
}

// 使用示例
SmartStringJoiner ssj = new SmartStringJoiner(", ", "[", "]");
ssj.addIfNotNull("Apple")
   .addIfNotNull(null)
   .addIfNotEmpty("")
   .addIfNotEmpty("  ")
   .addIfNotEmpty("Banana")
   .addWithDefault(null, "未知");

System.out.println(ssj);  // "[Apple, Banana, 未知]"
```

### 2. 嵌套StringJoiner

```
// 创建嵌套结构
StringJoiner outer = new StringJoiner("; ", "【", "】");

// 内层joiner 1
StringJoiner inner1 = new StringJoiner(", ", "(", ")");
inner1.add("A").add("B").add("C");
outer.add(inner1.toString());

// 内层joiner 2
StringJoiner inner2 = new StringJoiner(" | ", "{", "}");
inner2.add("X").add("Y").add("Z");
outer.add(inner2.toString());

// 内层joiner 3
StringJoiner inner3 = new StringJoiner(" - ");
inner3.add("1").add("2").add("3");
outer.add("[" + inner3 + "]");

System.out.println("嵌套结果: " + outer);
// 输出: 【(A, B, C); {X | Y | Z}; [1 - 2 - 3]】
```

### 3. 格式化添加

```
class FormattedStringJoiner {
    private StringJoiner joiner;
    
    public FormattedStringJoiner(CharSequence delimiter) {
        this.joiner = new StringJoiner(delimiter);
    }
    
    public FormattedStringJoiner(CharSequence delimiter,
                                CharSequence prefix,
                                CharSequence suffix) {
        this.joiner = new StringJoiner(delimiter, prefix, suffix);
    }
    
    public FormattedStringJoiner addFormatted(String format, Object... args) {
        joiner.add(String.format(format, args));
        return this;
    }
    
    public FormattedStringJoiner addPadded(Object obj, int width) {
        String padded = String.format("%-" + width + "s", obj);
        joiner.add(padded);
        return this;
    }
    
    public FormattedStringJoiner addNumber(Number num) {
        if (num instanceof Integer || num instanceof Long) {
            joiner.add(String.format("%,d", num));
        } else if (num instanceof Double || num instanceof Float) {
            joiner.add(String.format("%,.2f", num));
        } else {
            joiner.add(num.toString());
        }
        return this;
    }
    
    @Override
    public String toString() {
        return joiner.toString();
    }
}

// 使用示例
FormattedStringJoiner fsj = new FormattedStringJoiner(" | ");
fsj.addFormatted("姓名: %s", "张三")
   .addFormatted("年龄: %d岁", 25)
   .addPadded("职业", 8)
   .addNumber(1234567.89)
   .addNumber(1000000);

System.out.println(fsj);
// 输出: 姓名: 张三 | 年龄: 25岁 | 职业       | 1,234,567.89 | 1,000,000
```

---

## 十一、练习题

### 练习1：实现路径构建

```
// 编写一个方法，使用StringJoiner构建文件路径
public static String buildPath(String... parts) {
    // TODO: 使用StringJoiner实现
    return "";
}

// 测试
System.out.println(buildPath("home", "user", "docs", "file.txt"));
// 应该输出: home/user/docs/file.txt
```

### 练习2：构建SQL IN语句

```
// 编写一个方法，使用StringJoiner构建SQL IN语句
public static String buildSQLIn(String column, List<String> values) {
    // TODO: 实现
    return "";
}

// 测试
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
System.out.println(buildSQLIn("name", names));
// 应该输出: name IN ('Alice', 'Bob', 'Charlie')
```
