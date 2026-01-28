# StringBuilder基本操作详解

## 一、StringBuilder概述

### 为什么需要StringBuilder？

```
// 问题：字符串拼接性能差
String result = "";
for (int i = 0; i < 10000; i++) {
    result += i;  // 每次循环都创建新String对象
}

// 解决：使用StringBuilder
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 10000; i++) {
    sb.append(i);  // 在同一个对象上操作
}
String result2 = sb.toString();
```

### String vs StringBuilder vs StringBuffer

|特性|String|StringBuilder|StringBuffer|
|---|---|---|---|
|可变性|不可变|可变|可变|
|线程安全|是（不可变对象）|否|是|
|性能|低（修改时）|高|中等|
|使用场景|常量字符串|单线程字符串操作|多线程字符串操作|

---

## 二、创建StringBuilder

### 1. 基本创建方式

```
// 1. 默认构造器（初始容量16）
StringBuilder sb1 = new StringBuilder();

// 2. 指定初始容量
StringBuilder sb2 = new StringBuilder(100);  // 初始容量100

// 3. 用字符串初始化
StringBuilder sb3 = new StringBuilder("Hello");

// 4. 用CharSequence初始化
StringBuilder sb4 = new StringBuilder("Hello World");

// 验证
System.out.println("sb1容量: " + sb1.capacity());  // 16
System.out.println("sb2容量: " + sb2.capacity());  // 100
System.out.println("sb3内容: " + sb3.toString());  // Hello
System.out.println("sb3容量: " + sb3.capacity());  // 21 (16+5)
```

### 2. 查看状态信息

```
StringBuilder sb = new StringBuilder("Hello");

// 重要属性
System.out.println("内容: " + sb.toString());     // Hello
System.out.println("长度: " + sb.length());       // 5
System.out.println("容量: " + sb.capacity());     // 21
System.out.println("字符: " + sb.charAt(1));      // e
```

---

## 三、追加操作（append）

### 1. 追加各种数据类型

```
StringBuilder sb = new StringBuilder();

// 追加基本类型
sb.append("Hello ");           // 字符串
sb.append(123);               // int
sb.append(45.67);             // double
sb.append('A');               // char
sb.append(true);              // boolean

// 追加对象
sb.append(new Object());      // 调用对象的toString()
sb.append("\n");

// 追加字符数组
char[] chars = {'W', 'o', 'r', 'l', 'd'};
sb.append(chars);             // 整个数组
sb.append(chars, 1, 3);       // 数组的一部分: orl

// 追加StringBuffer
StringBuffer buffer = new StringBuffer("!");
sb.append(buffer);

System.out.println(sb.toString());
// 输出: Hello 12345.67Atrue
//       java.lang.Object@哈希值Worldorl!
```

### 2. 链式调用

```
// StringBuilder支持链式调用
String result = new StringBuilder()
    .append("姓名: ").append("张三").append("\n")
    .append("年龄: ").append(25).append("\n")
    .append("分数: ").append(89.5).append("\n")
    .append("通过: ").append(true)
    .toString();

System.out.println(result);
```

### 3. 格式化追加

```
StringBuilder sb = new StringBuilder();

// 使用String.format配合
sb.append(String.format("%-10s: %d\n", "年龄", 25));
sb.append(String.format("%-10s: %.2f\n", "工资", 5000.50));
sb.append(String.format("%-10s: %s\n", "姓名", "李四"));

// 或者创建格式化工具方法
public static StringBuilder appendFormat(StringBuilder sb, 
                                        String format, Object... args) {
    return sb.append(String.format(format, args));
}

// 使用
StringBuilder info = new StringBuilder();
appendFormat(info, "用户ID: %08d\n", 123);
appendFormat(info, "余额: ¥%.2f\n", 1234.56);
System.out.println(info.toString());
```

---

## 四、插入操作（insert）

### 1. 基本插入

```
StringBuilder sb = new StringBuilder("HelloWorld");

// 在指定位置插入
sb.insert(5, " ");        // 在索引5处插入空格 → "Hello World"
sb.insert(0, "Say: ");    // 在开头插入 → "Say: Hello World"
sb.insert(sb.length(), "!");  // 在末尾插入 → "Say: Hello World!"

System.out.println(sb);  // "Say: Hello World!"

// 插入各种数据类型
sb.insert(4, 123);       // 插入int → "Say:123 Hello World!"
sb.insert(0, 45.67);     // 插入double → "45.67Say:123 Hello World!"
sb.insert(5, 'X');       // 插入char → "45.67XSay:123 Hello World!"
sb.insert(0, true);      // 插入boolean → "true45.67XSay:123 Hello World!"
```

### 2. 插入数组和子串

```
StringBuilder sb = new StringBuilder("ABCDEF");

// 插入字符数组
char[] chars = {'1', '2', '3'};
sb.insert(3, chars);          // 在索引3处插入 → "ABC123DEF"
sb.insert(5, chars, 1, 2);    // 插入数组的一部分 → "ABC1232DEF"

// 插入子字符串
sb.insert(0, "Start: ");      // 开头插入 → "Start: ABC1232DEF"
sb.insert(sb.length(), " End");  // 结尾插入 → "Start: ABC1232DEF End"

// 插入StringBuilder
StringBuilder insertSb = new StringBuilder("INSERT");
sb.insert(7, insertSb);       // → "Start: INSERT ABC1232DEF End"
```

---

## 五、删除操作

### 1. 删除单个字符

```
StringBuilder sb = new StringBuilder("Hello World!");

// 删除指定位置的字符
sb.deleteCharAt(5);  // 删除索引5处的字符（空格）
System.out.println(sb);  // "HelloWorld!"

// 删除最后一个字符
sb.deleteCharAt(sb.length() - 1);
System.out.println(sb);  // "HelloWorld"
```

### 2. 删除子序列

```
StringBuilder sb = new StringBuilder("Hello Beautiful World!");

// 删除指定范围的字符
sb.delete(6, 16);  // 删除索引6-15的字符（包含6，不包含16）
System.out.println(sb);  // "Hello World!"

// 删除开头
sb.delete(0, 6);  // 删除前6个字符
System.out.println(sb);  // "World!"

// 删除结尾
sb.delete(sb.length() - 1, sb.length());
System.out.println(sb);  // "World"

// 清空StringBuilder
sb.delete(0, sb.length());
System.out.println("清空后长度: " + sb.length());  // 0
System.out.println("是否为空: " + (sb.length() == 0));  // true
```

---

## 六、替换操作

### 1. 替换子串

```
StringBuilder sb = new StringBuilder("Hello Java World!");

// 替换指定范围的字符
sb.replace(6, 10, "Python");  // 将索引6-9替换为Python
System.out.println(sb);  // "Hello Python World!"

// 替换单个字符
sb.setCharAt(7, 'Y');  // 将索引7处的字符改为Y
System.out.println(sb);  // "Hello PYthon World!"

// 模拟字符串的replace方法
String text = sb.toString().replace("World", "Universe");
sb = new StringBuilder(text);
System.out.println(sb);  // "Hello PYthon Universe!"
```

### 2. 实现replaceAll功能

```
public class StringBuilderUtils {
    
    // 为StringBuilder添加replaceAll方法
    public static void replaceAll(StringBuilder sb, 
                                 String target, String replacement) {
        int index = sb.indexOf(target);
        while (index != -1) {
            sb.replace(index, index + target.length(), replacement);
            index = sb.indexOf(target, index + replacement.length());
        }
    }
    
    // 使用
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder("aa bb aa cc aa dd");
        replaceAll(sb, "aa", "XX");
        System.out.println(sb);  // "XX bb XX cc XX dd"
    }
}
```

---

## 七、查找和截取

### 1. 查找操作

```
StringBuilder sb = new StringBuilder("Hello Java, Hello World!");

// 查找子串位置
int firstIndex = sb.indexOf("Hello");     // 0
int lastIndex = sb.lastIndexOf("Hello");  // 12
int notFound = sb.indexOf("Python");      // -1

// 从指定位置开始查找
int indexFrom = sb.indexOf("Hello", 5);   // 12

// 查找字符
int charIndex = sb.indexOf("J");         // 6

System.out.println("第一次出现Hello的位置: " + firstIndex);
System.out.println("最后出现Hello的位置: " + lastIndex);
System.out.println("从位置5开始查找Hello: " + indexFrom);
```

### 2. 截取操作

```
StringBuilder sb = new StringBuilder("Hello World!");

// 获取子串（返回String，不修改原StringBuilder）
String sub1 = sb.substring(6);      // 从索引6到结尾 → "World!"
String sub2 = sb.substring(0, 5);   // 索引0-4 → "Hello"
String sub3 = sb.substring(6, 11);  // 索引6-10 → "World"

// 注意：substring不改变原StringBuilder
System.out.println("原字符串: " + sb);  // 仍然是"Hello World!"
System.out.println("子串1: " + sub1);
System.out.println("子串2: " + sub2);
System.out.println("子串3: " + sub3);
```

---

## 八、反转和修改

### 1. 反转字符串

```
StringBuilder sb = new StringBuilder("Hello World!");

// 反转整个字符串
sb.reverse();
System.out.println("反转后: " + sb);  // "!dlroW olleH"

// 再反转回来
sb.reverse();
System.out.println("恢复: " + sb);  // "Hello World!"

// 判断回文
public static boolean isPalindrome(String str) {
    StringBuilder sb = new StringBuilder(str);
    return str.equals(sb.reverse().toString());
}

System.out.println(isPalindrome("abcba"));  // true
System.out.println(isPalindrome("hello"));  // false
```

### 2. 设置长度

```
StringBuilder sb = new StringBuilder("Hello World!");

// 设置长度（小于当前长度会截断）
sb.setLength(5);
System.out.println("截断后: " + sb);  // "Hello"

// 设置长度（大于当前长度会用null字符填充）
sb.setLength(10);
System.out.println("扩展后: " + sb);  // "Hello\0\0\0\0\0"
System.out.println("长度: " + sb.length());  // 10

// 恢复
sb = new StringBuilder("Hello World!");
```

### 3. 获取和设置字符

```
StringBuilder sb = new StringBuilder("Hello");

// 获取指定位置字符
char ch = sb.charAt(1);  // 'e'

// 设置指定位置字符
sb.setCharAt(1, 'a');    // 修改索引1的字符
System.out.println(sb);  // "Hallo"

// 获取字符数组
char[] chars = new char[sb.length()];
sb.getChars(0, sb.length(), chars, 0);
System.out.println("字符数组: " + new String(chars));  // "Hallo"
```

---

## 九、容量管理

### 1. 容量和长度

```
StringBuilder sb = new StringBuilder();  // 默认容量16
System.out.println("初始容量: " + sb.capacity());  // 16
System.out.println("初始长度: " + sb.length());    // 0

// 追加数据
sb.append("1234567890123456");  // 追加16个字符
System.out.println("追加16字符后:");
System.out.println("容量: " + sb.capacity());  // 16
System.out.println("长度: " + sb.length());    // 16

// 再追加1个字符（触发扩容）
sb.append("7");
System.out.println("再追加1字符后:");
System.out.println("容量: " + sb.capacity());  // 34 (16 * 2+2)
System.out.println("长度: " + sb.length());    // 17
```

### 2. 手动扩容

```
StringBuilder sb = new StringBuilder(10);

System.out.println("初始容量: " + sb.capacity());  // 10

// 确保最小容量
sb.ensureCapacity(100);
System.out.println("ensureCapacity(100)后: " + sb.capacity());  // 100

// 如果已经够大，不会缩小
sb.ensureCapacity(50);
System.out.println("ensureCapacity(50)后: " + sb.capacity());  // 100

// 压缩容量（去掉未使用的空间）
sb.append("Hello");
sb.trimToSize();
System.out.println("trimToSize后:");
System.out.println("容量: " + sb.capacity());  // 5
System.out.println("长度: " + sb.length());    // 5
```

### 3. 扩容策略

```
public class CapacityTest {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder();
        
        System.out.println("初始容量: " + sb.capacity());
        
        for (int i = 1; i <= 100; i++) {
            sb.append("a");
            if (sb.length() == sb.capacity()) {
                int oldCapacity = sb.capacity();
                sb.append("a");  // 触发扩容
                int newCapacity = sb.capacity();
                System.out.printf("扩容: %d -> %d (长度: %d)\n", 
                                oldCapacity, newCapacity, sb.length());
            }
        }
        
        // 扩容公式: newCapacity = (oldCapacity * 2) + 2
    }
}
```

---

## 十、实际应用示例

### 示例1：SQL构建器

```
public class SQLBuilder {
    private StringBuilder sql = new StringBuilder();
    private boolean hasWhere = false;
    
    public SQLBuilder select(String columns) {
        sql.append("SELECT ").append(columns);
        return this;
    }
    
    public SQLBuilder from(String table) {
        sql.append(" FROM ").append(table);
        return this;
    }
    
    public SQLBuilder where(String condition) {
        if (!hasWhere) {
            sql.append(" WHERE ");
            hasWhere = true;
        } else {
            sql.append(" AND ");
        }
        sql.append(condition);
        return this;
    }
    
    public SQLBuilder orWhere(String condition) {
        sql.append(" OR ").append(condition);
        return this;
    }
    
    public SQLBuilder orderBy(String column, String direction) {
        sql.append(" ORDER BY ").append(column).append(" ").append(direction);
        return this;
    }
    
    public SQLBuilder limit(int count) {
        sql.append(" LIMIT ").append(count);
        return this;
    }
    
    public String build() {
        return sql.toString();
    }
    
    // 使用示例
    public static void main(String[] args) {
        String sql = new SQLBuilder()
            .select("id, name, email")
            .from("users")
            .where("age > 18")
            .where("status = 'active'")
            .orWhere("vip = true")
            .orderBy("created_at", "DESC")
            .limit(10)
            .build();
        
        System.out.println(sql);
        // SELECT id, name, email FROM users WHERE age > 18 AND status = 'active' OR vip = true ORDER BY created_at DESC LIMIT 10
    }
}
```

### 示例2：HTML生成器

```
public class HTMLBuilder {
    private StringBuilder html = new StringBuilder();
    private int indentLevel = 0;
    
    private String getIndent() {
        return "  ".repeat(indentLevel);
    }
    
    public HTMLBuilder startTag(String tagName, String... attributes) {
        html.append(getIndent()).append("<").append(tagName);
        
        for (int i = 0; i < attributes.length; i += 2) {
            if (i + 1 < attributes.length) {
                html.append(" ")
                    .append(attributes[i])
                    .append("=\"")
                    .append(attributes[i + 1])
                    .append("\"");
            }
        }
        
        html.append(">\n");
        indentLevel++;
        return this;
    }
    
    public HTMLBuilder endTag(String tagName) {
        indentLevel--;
        html.append(getIndent()).append("</").append(tagName).append(">\n");
        return this;
    }
    
    public HTMLBuilder addText(String text) {
        html.append(getIndent()).append(text).append("\n");
        return this;
    }
    
    public HTMLBuilder addElement(String tagName, String text, String... attributes) {
        startTag(tagName, attributes);
        addText(text);
        endTag(tagName);
        return this;
    }
    
    public String build() {
        return html.toString();
    }
    
    public static void main(String[] args) {
        HTMLBuilder html = new HTMLBuilder();
        
        html.startTag("html")
            .startTag("head")
                .addElement("title", "我的网页")
            .endTag("head")
            .startTag("body")
                .addElement("h1", "欢迎来到我的网站", "class", "title")
                .startTag("div", "class", "content")
                    .addElement("p", "这是一个段落。")
                    .startTag("ul")
                        .addElement("li", "项目1")
                        .addElement("li", "项目2")
                        .addElement("li", "项目3")
                    .endTag("ul")
                .endTag("div")
            .endTag("body")
            .endTag("html");
        
        System.out.println(html.build());
    }
}
```

### 示例3：CSV导出

```
public class CSVBuilder {
    private StringBuilder csv = new StringBuilder();
    private boolean isFirstRow = true;
    private int columnCount = 0;
    
    public CSVBuilder addHeader(String... headers) {
        columnCount = headers.length;
        for (int i = 0; i < headers.length; i++) {
            csv.append(escapeCSV(headers[i]));
            if (i < headers.length - 1) {
                csv.append(",");
            }
        }
        csv.append("\n");
        isFirstRow = false;
        return this;
    }
    
    public CSVBuilder addRow(Object... values) {
        if (isFirstRow) {
            columnCount = values.length;
            isFirstRow = false;
        }
        
        for (int i = 0; i < values.length; i++) {
            String value = values[i] == null ? "" : values[i].toString();
            csv.append(escapeCSV(value));
            if (i < values.length - 1) {
                csv.append(",");
            }
        }
        csv.append("\n");
        return this;
    }
    
    private String escapeCSV(String value) {
        if (value.contains(",") || value.contains("\"") || value.contains("\n")) {
            return "\"" + value.replace("\"", "\"\"") + "\"";
        }
        return value;
    }
    
    public String build() {
        return csv.toString();
    }
    
    public static void main(String[] args) {
        CSVBuilder csv = new CSVBuilder();
        csv.addHeader("ID", "姓名", "年龄", "邮箱")
            .addRow(1, "张三", 25, "zhangsan@example.com")
            .addRow(2, "李四", 30, "lisi@example.com")
            .addRow(3, "王五, Jr.", 28, "wangwu@test.com")
            .addRow(4, "赵六", 35, "zhaoliu@demo.com");
        
        System.out.println(csv.build());
    }
}
```

### 示例4：日志消息构建

```
public class LogMessageBuilder {
    private StringBuilder log = new StringBuilder();
    
    public LogMessageBuilder timestamp() {
        log.append("[")
           .append(java.time.LocalDateTime.now())
           .append("] ");
        return this;
    }
    
    public LogMessageBuilder level(String level) {
        log.append("[")
           .append(level.toUpperCase())
           .append("] ");
        return this;
    }
    
    public LogMessageBuilder thread() {
        log.append("[Thread-")
           .append(Thread.currentThread().getId())
           .append("] ");
        return this;
    }
    
    public LogMessageBuilder message(String format, Object... args) {
        if (args.length > 0) {
            log.append(String.format(format, args));
        } else {
            log.append(format);
        }
        return this;
    }
    
    public LogMessageBuilder exception(Throwable e) {
        log.append(" | Exception: ")
           .append(e.getClass().getName())
           .append(": ")
           .append(e.getMessage());
        return this;
    }
    
    public String build() {
        return log.toString();
    }
    
    public static void main(String[] args) {
        try {
            int result = 10 / 0;
        } catch (Exception e) {
            String log = new LogMessageBuilder()
                .timestamp()
                .level("ERROR")
                .thread()
                .message("计算失败: 除数不能为零")
                .exception(e)
                .build();
            
            System.out.println(log);
        }
    }
}
```

---

## 十一、性能优化技巧

### 1. 预分配容量

```
// 不好的做法
StringBuilder sb1 = new StringBuilder();
for (int i = 0; i < 10000; i++) {
    sb1.append("data" + i);  // 多次扩容
}

// 好的做法
int estimatedSize = 10000 * 10;  // 预估总大小
StringBuilder sb2 = new StringBuilder(estimatedSize);
for (int i = 0; i < 10000; i++) {
    sb2.append("data").append(i);  // 一次性分配，避免扩容
}
```

### 2. 链式vs分开

```
// 链式调用（推荐）
StringBuilder sb1 = new StringBuilder();
String result1 = sb1.append("a").append("b").append("c").toString();

// 分开调用
StringBuilder sb2 = new StringBuilder();
sb2.append("a");
sb2.append("b");
sb2.append("c");
String result2 = sb2.toString();
// 两种方式性能相同，链式更简洁
```

### 3. 避免不必要的toString()

```
// 不好的做法
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    String temp = sb.toString();  // 每次循环都创建新String
    // ... 处理temp
    sb.setLength(0);  // 清空
}

// 好的做法
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    // 直接在StringBuilder上操作
    // ... 使用sb
    sb.setLength(0);  // 清空重用
}
```

### 4. 重用StringBuilder

```
// 线程局部变量重用StringBuilder
class StringBuilderPool {
    private static final ThreadLocal<StringBuilder> threadLocalBuilder =
        ThreadLocal.withInitial(() -> new StringBuilder(1024));
    
    public static StringBuilder get() {
        StringBuilder sb = threadLocalBuilder.get();
        sb.setLength(0);  // 清空以便重用
        return sb;
    }
    
    public static void main(String[] args) {
        StringBuilder sb = StringBuilderPool.get();
        sb.append("Hello");
        // 使用后不需要手动清理，下次get时会自动清空
    }
}
```

---

## 十二、常见问题解答

### 1. StringBuilder是线程安全的吗？

```
// 不是线程安全的
StringBuilder sb = new StringBuilder();

// 多线程操作会出现问题
Runnable task = () -> {
    for (int i = 0; i < 1000; i++) {
        sb.append("a");
    }
};

Thread t1 = new Thread(task);
Thread t2 = new Thread(task);
t1.start();
t2.start();

// 使用StringBuffer代替
StringBuffer buffer = new StringBuffer();
```

### 2. 如何清空StringBuilder？

```
StringBuilder sb = new StringBuilder("Hello World");

// 方法1：setLength(0) - 推荐
sb.setLength(0);
System.out.println("清空后: " + sb.toString());  // ""
System.out.println("长度: " + sb.length());      // 0
System.out.println("容量: " + sb.capacity());    // 27 (保持原容量)

// 方法2：delete
sb.append("Hello World");
sb.delete(0, sb.length());

// 方法3：new新的（不推荐，浪费内存）
sb = new StringBuilder();
```

### 3. 如何从StringBuilder获取子串？

```
StringBuilder sb = new StringBuilder("Hello World");

// 获取子串（不修改原StringBuilder）
String sub1 = sb.substring(6);      // "World"
String sub2 = sb.substring(0, 5);   // "Hello"

// 如果要修改原StringBuilder，使用delete
sb.delete(5, sb.length());  // 删除" World"
System.out.println(sb);  // "Hello"
```

### 4. 性能对比测试

```
public class PerformanceTest {
    public static void main(String[] args) {
        int iterations = 100000;
        
        // 测试1: String拼接
        long start1 = System.currentTimeMillis();
        String str = "";
        for (int i = 0; i < iterations; i++) {
            str += "a";
        }
        long time1 = System.currentTimeMillis() - start1;
        
        // 测试2: StringBuilder
        long start2 = System.currentTimeMillis();
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < iterations; i++) {
            sb.append("a");
        }
        String result2 = sb.toString();
        long time2 = System.currentTimeMillis() - start2;
        
        // 测试3: 预分配容量的StringBuilder
        long start3 = System.currentTimeMillis();
        StringBuilder sb2 = new StringBuilder(iterations);
        for (int i = 0; i < iterations; i++) {
            sb2.append("a");
        }
        String result3 = sb2.toString();
        long time3 = System.currentTimeMillis() - start3;
        
        System.out.println("String拼接: " + time1 + "ms");
        System.out.println("StringBuilder: " + time2 + "ms");
        System.out.println("预分配StringBuilder: " + time3 + "ms");
    }
}
```

---

## 十三、练习题

### 练习1：实现字符串反转

```
// 编写一个方法，使用StringBuilder反转字符串
public static String reverseString(String str) {
    // TODO: 实现
}

// 测试
System.out.println(reverseString("Hello"));  // "olleH"
System.out.println(reverseString("12345"));  // "54321"
```

### 练习2：删除重复字符

```
// 编写一个方法，删除字符串中的重复字符
public static String removeDuplicates(String str) {
    // TODO: 使用StringBuilder实现
}

// 测试
System.out.println(removeDuplicates("aabbcc"));  // "abc"
System.out.println(removeDuplicates("hello"));   // "helo"
```

### 练习3：字符串压缩

```
// 实现字符串压缩：aabcccccaaa → a2b1c5a3
// 如果压缩后没有变短，返回原字符串
public static String compressString(String str) {
    // TODO: 使用StringBuilder实现
}
```

### 练习4：XML标签生成器

```
// 创建一个XML生成器类
class XMLBuilder {
    private StringBuilder xml = new StringBuilder();
    
    // TODO: 实现startTag、endTag、addAttribute、addText等方法
    // TODO: 实现缩进功能
}

// 使用示例
XMLBuilder xml = new XMLBuilder();
xml.startTag("book")
   .addAttribute("id", "1")
   .addText("Java编程")
   .endTag();
System.out.println(xml.build());
```

---

## 十四、最佳实践总结

1. **创建时预估容量**：避免频繁扩容
    
    ```
    // 不好
    StringBuilder sb = new StringBuilder();
    
    // 好
    StringBuilder sb = new StringBuilder(estimatedSize);
    ```
    
2. **使用链式调用**：代码更简洁
    
    ```
    // 推荐
    String result = new StringBuilder()
        .append("a").append("b").append("c")
        .toString();
    ```
    
3. **避免中间转换**：减少toString()调用
    
    ```
    // 不好
    for (int i = 0; i < 1000; i++) {
        String temp = sb.toString();  // 频繁转换
    }
    
    // 好
    // 直接在StringBuilder上操作
    ```
    
4. **清空重用**：使用setLength(0)
    
    ```
    StringBuilder sb = new StringBuilder();
    // ... 使用
    sb.setLength(0);  // 清空重用
    ```
    
5. **多线程用StringBuffer**
    
    ```
    // 单线程
    StringBuilder sb = new StringBuilder();
    
    // 多线程
    StringBuffer buffer = new StringBuffer();
    ```
    
6. **优先使用append**：避免字符串拼接
    
    ```
    // 不好
    sb.append("a" + "b" + "c");  // 先创建临时String
    
    // 好
    sb.append("a").append("b").append("c");
    ```
    

记住：**StringBuilder是处理可变字符串的最佳工具**，在大多数字符串操作场景中都应该优先使用它。