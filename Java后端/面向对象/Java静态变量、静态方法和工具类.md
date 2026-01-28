# Java静态变量、静态方法和工具类详解

## 一、静态变量（类变量）

### 1. 基本概念

**静态变量**是属于类的变量，不是属于对象的变量。所有对象共享同一个静态变量。

```
public class Student {
    // 实例变量 - 每个对象都有自己的副本
    private String name;
    private int age;
    
    // 静态变量 - 所有对象共享
    public static String school = "清华大学";
    private static int studentCount = 0;  // 统计学生数量
    
    // 常量（静态final变量）
    public static final double PI = 3.141592653589793;
    public static final String DEFAULT_SCHOOL = "清华大学";
    
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
        studentCount++;  // 每次创建对象，计数器+1
    }
}

// 使用
Student s1 = new Student("张三", 20);
Student s2 = new Student("李四", 21);

System.out.println(Student.school);  // "清华大学" - 通过类名访问
System.out.println(s1.school);       // "清华大学" - 通过对象访问（不推荐）
System.out.println(Student.DEFAULT_SCHOOL);  // "清华大学"

// 修改静态变量会影响所有对象
Student.school = "北京大学";
System.out.println(s2.school);  // "北京大学"
```

### 2. 静态变量初始化时机

```
class StaticTest {
    // 1. 直接初始化
    static int count = 0;
    
    // 2. 静态代码块初始化
    static {
        System.out.println("静态代码块执行");
        count = 100;
    }
    
    // 3. 常量（编译时常量）
    static final int MAX_VALUE = 100;
    
    // 4. 静态变量在类加载时初始化
    // 类加载时机：第一次使用时
}

// 执行顺序测试
class OrderTest {
    static String staticField = "静态变量";
    
    static {
        System.out.println("静态代码块1");
    }
    
    static {
        System.out.println("静态代码块2");
    }
    
    public static void print() {
        System.out.println("静态方法");
    }
}

// 测试
public class Main {
    public static void main(String[] args) {
        System.out.println("第一次访问：");
        System.out.println(OrderTest.staticField);
        OrderTest.print();
        
        System.out.println("\n第二次访问：");
        OrderTest.print();  // 静态代码块不会再次执行
    }
}
```

---

## 二、静态方法

### 1. 基本使用

```
public class MathUtils {
    // 静态方法
    public static int add(int a, int b) {
        return a + b;
    }
    
    public static double multiply(double a, double b) {
        return a * b;
    }
    
    public static boolean isEven(int number) {
        return number % 2 == 0;
    }
    
    // 工厂方法模式
    public static Date createDate(int year, int month, int day) {
        // 返回新对象
        return new Date(year, month, day);
    }
    
    // 工具方法
    public static String formatPhone(String phone) {
        if (phone == null || phone.length() != 11) {
            return phone;
        }
        return phone.substring(0, 3) + "-" 
             + phone.substring(3, 7) + "-" 
             + phone.substring(7);
    }
}

// 使用
int sum = MathUtils.add(5, 3);  // 8
double product = MathUtils.multiply(2.5, 4.0);  // 10.0
boolean even = MathUtils.isEven(10);  // true
String phone = MathUtils.formatPhone("13800138000");  // 138-0013-8000
```

### 2. 静态方法限制

```
class Example {
    private int instanceVar = 10;
    private static int staticVar = 20;
    
    // 实例方法
    public void instanceMethod() {
        System.out.println(instanceVar);  // ✓ 可以访问实例变量
        System.out.println(staticVar);    // ✓ 可以访问静态变量
        staticMethod();                   // ✓ 可以调用静态方法
    }
    
    // 静态方法
    public static void staticMethod() {
        // System.out.println(instanceVar);  // ✗ 不能访问实例变量
        System.out.println(staticVar);      // ✓ 可以访问静态变量
        // instanceMethod();                // ✗ 不能调用实例方法
    }
    
    // 静态方法中创建对象后可以访问实例成员
    public static void staticMethod2() {
        Example obj = new Example();
        System.out.println(obj.instanceVar);  // ✓ 通过对象访问
        obj.instanceMethod();                 // ✓ 通过对象调用
    }
}
```

---

## 三、工具类设计

### 1. 标准工具类示例

```
/**
 * 字符串工具类
 * 设计原则：
 * 1. 私有构造方法防止实例化
 * 2. 所有方法都是静态的
 * 3. 使用final类防止继承
 * 4. 添加详细的文档注释
 */
public final class StringUtils {
    
    // 私有构造方法，防止实例化
    private StringUtils() {
        throw new AssertionError("不能实例化工具类");
    }
    
    /**
     * 检查字符串是否为空
     * @param str 要检查的字符串
     * @return 如果字符串为null或空返回true
     */
    public static boolean isEmpty(CharSequence str) {
        return str == null || str.length() == 0;
    }
    
    /**
     * 检查字符串是否为空白
     * @param str 要检查的字符串
     * @return 如果字符串为null、空或只包含空白字符返回true
     */
    public static boolean isBlank(CharSequence str) {
        if (isEmpty(str)) {
            return true;
        }
        for (int i = 0; i < str.length(); i++) {
            if (!Character.isWhitespace(str.charAt(i))) {
                return false;
            }
        }
        return true;
    }
    
    /**
     * 反转字符串
     * @param str 要反转的字符串
     * @return 反转后的字符串，如果输入为null则返回null
     */
    public static String reverse(String str) {
        if (str == null) {
            return null;
        }
        return new StringBuilder(str).reverse().toString();
    }
    
    /**
     * 检查字符串是否是回文
     * @param str 要检查的字符串
     * @return 如果是回文返回true
     */
    public static boolean isPalindrome(String str) {
        if (str == null) {
            return false;
        }
        String cleaned = str.replaceAll("[^a-zA-Z0-9]", "").toLowerCase();
        return cleaned.equals(reverse(cleaned));
    }
    
    /**
     * 将字符串首字母大写
     * @param str 要转换的字符串
     * @return 首字母大写的字符串
     */
    public static String capitalize(String str) {
        if (isEmpty(str)) {
            return str;
        }
        return Character.toUpperCase(str.charAt(0)) + str.substring(1);
    }
    
    /**
     * 安全的字符串比较
     * @param str1 第一个字符串
     * @param str2 第二个字符串
     * @return 如果相等返回true
     */
    public static boolean equals(String str1, String str2) {
        if (str1 == str2) {
            return true;
        }
        if (str1 == null || str2 == null) {
            return false;
        }
        return str1.equals(str2);
    }
    
    /**
     * 生成指定长度的随机字符串
     * @param length 字符串长度
     * @return 随机字符串
     */
    public static String randomString(int length) {
        if (length <= 0) {
            return "";
        }
        String chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
        Random random = new Random();
        StringBuilder sb = new StringBuilder(length);
        for (int i = 0; i < length; i++) {
            sb.append(chars.charAt(random.nextInt(chars.length())));
        }
        return sb.toString();
    }
}
```

### 2. 更复杂的工具类示例

```
import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
import java.time.format.DateTimeParseException;
import java.util.regex.Pattern;

/**
 * 日期时间工具类
 */
public final class DateUtils {
    
    private DateUtils() {
        throw new AssertionError("不能实例化工具类");
    }
    
    // 常用的日期格式
    public static final String DATE_FORMAT = "yyyy-MM-dd";
    public static final String TIME_FORMAT = "HH:mm:ss";
    public static final String DATETIME_FORMAT = "yyyy-MM-dd HH:mm:ss";
    
    // 预编译的正则表达式
    private static final Pattern DATE_PATTERN = 
        Pattern.compile("^\\d{4}-\\d{2}-\\d{2}$");
    private static final Pattern TIME_PATTERN = 
        Pattern.compile("^\\d{2}:\\d{2}:\\d{2}$");
    
    /**
     * 格式化当前日期
     */
    public static String getCurrentDate() {
        return LocalDate.now().format(DateTimeFormatter.ofPattern(DATE_FORMAT));
    }
    
    /**
     * 格式化日期
     */
    public static String formatDate(LocalDate date, String pattern) {
        if (date == null) {
            return "";
        }
        return date.format(DateTimeFormatter.ofPattern(pattern));
    }
    
    /**
     * 解析日期字符串
     */
    public static LocalDate parseDate(String dateStr, String pattern) {
        try {
            return LocalDate.parse(dateStr, DateTimeFormatter.ofPattern(pattern));
        } catch (DateTimeParseException e) {
            return null;
        }
    }
    
    /**
     * 计算两个日期之间的天数差
     */
    public static long daysBetween(LocalDate start, LocalDate end) {
        if (start == null || end == null) {
            return 0;
        }
        return Math.abs(java.time.temporal.ChronoUnit.DAYS.between(start, end));
    }
    
    /**
     * 检查是否是有效日期格式
     */
    public static boolean isValidDate(String dateStr) {
        if (dateStr == null) {
            return false;
        }
        if (!DATE_PATTERN.matcher(dateStr).matches()) {
            return false;
        }
        try {
            LocalDate.parse(dateStr, DateTimeFormatter.ofPattern(DATE_FORMAT));
            return true;
        } catch (DateTimeParseException e) {
            return false;
        }
    }
    
    /**
     * 获取某个月的最后一天
     */
    public static LocalDate getLastDayOfMonth(int year, int month) {
        LocalDate date = LocalDate.of(year, month, 1);
        return date.withDayOfMonth(date.lengthOfMonth());
    }
    
    /**
     * 计算年龄
     */
    public static int calculateAge(LocalDate birthDate) {
        if (birthDate == null) {
            return 0;
        }
        return (int) java.time.temporal.ChronoUnit.YEARS.between(
            birthDate, LocalDate.now());
    }
    
    /**
     * 工作日计算（简化版）
     */
    public static boolean isWorkday(LocalDate date) {
        if (date == null) {
            return false;
        }
        // 周一至周五是工作日
        return date.getDayOfWeek().getValue() <= 5;
    }
}
```

---

## 四、常见Java内置工具类

### 1. Collections工具类

```
import java.util.*;

public class CollectionsExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("Apple", "Banana", "Cherry");
        
        // 排序
        Collections.sort(list);
        System.out.println("排序后: " + list);
        
        // 反转
        Collections.reverse(list);
        System.out.println("反转后: " + list);
        
        // 查找
        int index = Collections.binarySearch(list, "Banana");
        System.out.println("Banana的索引: " + index);
        
        // 最大最小值
        String max = Collections.max(list);
        String min = Collections.min(list);
        System.out.println("最大值: " + max + ", 最小值: " + min);
        
        // 填充
        List<String> filledList = new ArrayList<>(Arrays.asList("A", "B", "C"));
        Collections.fill(filledList, "X");
        System.out.println("填充后: " + filledList);
        
        // 创建不可变集合
        List<String> immutableList = Collections.unmodifiableList(list);
        // immutableList.add("D");  // 抛出UnsupportedOperationException
        
        // 创建同步集合
        List<String> syncList = Collections.synchronizedList(new ArrayList<>());
    }
}
```

### 2. Arrays工具类

```
import java.util.Arrays;

public class ArraysExample {
    public static void main(String[] args) {
        int[] numbers = {5, 2, 8, 1, 9};
        
        // 排序
        Arrays.sort(numbers);
        System.out.println("排序后: " + Arrays.toString(numbers));
        
        // 二分查找
        int index = Arrays.binarySearch(numbers, 8);
        System.out.println("8的索引: " + index);
        
        // 填充
        int[] filled = new int[5];
        Arrays.fill(filled, 10);
        System.out.println("填充后: " + Arrays.toString(filled));
        
        // 复制
        int[] copy = Arrays.copyOf(numbers, 3);
        System.out.println("复制前3个: " + Arrays.toString(copy));
        
        // 比较
        int[] arr1 = {1, 2, 3};
        int[] arr2 = {1, 2, 3};
        System.out.println("数组相等: " + Arrays.equals(arr1, arr2));
        
        // 转换为List
        List<String> list = Arrays.asList("A", "B", "C");
        System.out.println("数组转List: " + list);
        
        // 并行排序（大数据量）
        int[] largeArray = new int[10000];
        // Arrays.parallelSort(largeArray);
    }
}
```

### 3. Objects工具类

```
import java.util.Objects;

public class ObjectsExample {
    public static void main(String[] args) {
        String str1 = null;
        String str2 = "Hello";
        
        // 安全的equals
        boolean equal = Objects.equals(str1, str2);  // false
        System.out.println("相等: " + equal);
        
        // 深度相等比较
        int[] arr1 = {1, 2, 3};
        int[] arr2 = {1, 2, 3};
        boolean deepEqual = Objects.deepEquals(arr1, arr2);  // true
        System.out.println("深度相等: " + deepEqual);
        
        // 安全的hashCode
        int hashCode = Objects.hashCode(str1);  // 0
        System.out.println("hashCode: " + hashCode);
        
        // 多参数hash
        int multiHash = Objects.hash("John", 25, "Engineer");
        System.out.println("多参数hash: " + multiHash);
        
        // 非空检查
        String name = "John";
        String result = Objects.requireNonNull(name, "名字不能为null");
        System.out.println("非空检查: " + result);
        
        // 默认值
        String value = Objects.toString(str1, "默认值");
        System.out.println("默认值: " + value);
        
        // 比较
        int compare = Objects.compare("A", "B", String::compareTo);
        System.out.println("比较结果: " + compare);
    }
}
```

---

## 五、设计工具类的最佳实践

### 1. 完整工具类示例

```
import java.util.*;
import java.util.function.Predicate;
import java.util.stream.Collectors;

/**
 * 集合工具类
 * 设计模式：工具类
 */
public final class CollectionUtils {
    
    private CollectionUtils() {
        // 防止工具类被实例化
        throw new IllegalStateException("工具类不能实例化");
    }
    
    /**
     * 检查集合是否为空
     */
    public static boolean isEmpty(Collection<?> collection) {
        return collection == null || collection.isEmpty();
    }
    
    /**
     * 检查集合是否不为空
     */
    public static boolean isNotEmpty(Collection<?> collection) {
        return !isEmpty(collection);
    }
    
    /**
     * 安全地获取集合大小
     */
    public static int size(Collection<?> collection) {
        return collection == null ? 0 : collection.size();
    }
    
    /**
     * 过滤集合
     */
    public static <T> List<T> filter(Collection<T> collection, Predicate<T> predicate) {
        if (isEmpty(collection)) {
            return new ArrayList<>();
        }
        return collection.stream()
                .filter(predicate)
                .collect(Collectors.toList());
    }
    
    /**
     * 转换集合元素
     */
    public static <T, R> List<R> map(Collection<T> collection, 
                                     java.util.function.Function<T, R> mapper) {
        if (isEmpty(collection)) {
            return new ArrayList<>();
        }
        return collection.stream()
                .map(mapper)
                .collect(Collectors.toList());
    }
    
    /**
     * 去重
     */
    public static <T> List<T> distinct(Collection<T> collection) {
        if (isEmpty(collection)) {
            return new ArrayList<>();
        }
        return collection.stream()
                .distinct()
                .collect(Collectors.toList());
    }
    
    /**
     * 合并多个集合
     */
    @SafeVarargs
    public static <T> List<T> merge(Collection<T>... collections) {
        List<T> result = new ArrayList<>();
        for (Collection<T> collection : collections) {
            if (isNotEmpty(collection)) {
                result.addAll(collection);
            }
        }
        return result;
    }
    
    /**
     * 集合分页
     */
    public static <T> List<T> page(Collection<T> collection, int page, int size) {
        if (isEmpty(collection)) {
            return new ArrayList<>();
        }
        
        int start = (page - 1) * size;
        int end = Math.min(start + size, collection.size());
        
        if (start >= collection.size()) {
            return new ArrayList<>();
        }
        
        List<T> list = collection instanceof List ? 
                      (List<T>) collection : new ArrayList<>(collection);
        
        return list.subList(start, end);
    }
    
    /**
     * 集合分组
     */
    public static <T, K> Map<K, List<T>> groupBy(Collection<T> collection, 
                                                 java.util.function.Function<T, K> classifier) {
        if (isEmpty(collection)) {
            return new HashMap<>();
        }
        return collection.stream()
                .collect(Collectors.groupingBy(classifier));
    }
    
    /**
     * 创建不可变集合
     */
    public static <T> List<T> unmodifiableList(Collection<T> collection) {
        if (collection == null) {
            return Collections.emptyList();
        }
        return Collections.unmodifiableList(new ArrayList<>(collection));
    }
    
    /**
     * 集合转字符串
     */
    public static String toString(Collection<?> collection, String delimiter) {
        if (isEmpty(collection)) {
            return "";
        }
        return collection.stream()
                .map(String::valueOf)
                .collect(Collectors.joining(delimiter));
    }
}
```

### 2. 配置文件工具类

```
import java.io.*;
import java.util.Properties;

/**
 * 配置文件工具类
 */
public final class ConfigUtils {
    
    private static final Properties props = new Properties();
    private static final String CONFIG_FILE = "config.properties";
    
    // 静态代码块加载配置
    static {
        loadConfig();
    }
    
    private ConfigUtils() {
        throw new AssertionError("不能实例化工具类");
    }
    
    /**
     * 加载配置文件
     */
    private static void loadConfig() {
        try (InputStream input = ConfigUtils.class.getClassLoader()
                .getResourceAsStream(CONFIG_FILE)) {
            if (input != null) {
                props.load(input);
            } else {
                System.err.println("配置文件未找到: " + CONFIG_FILE);
            }
        } catch (IOException e) {
            System.err.println("加载配置文件失败: " + e.getMessage());
        }
    }
    
    /**
     * 获取字符串配置
     */
    public static String getString(String key) {
        return getString(key, "");
    }
    
    /**
     * 获取字符串配置（带默认值）
     */
    public static String getString(String key, String defaultValue) {
        return props.getProperty(key, defaultValue);
    }
    
    /**
     * 获取整数配置
     */
    public static int getInt(String key) {
        return getInt(key, 0);
    }
    
    /**
     * 获取整数配置（带默认值）
     */
    public static int getInt(String key, int defaultValue) {
        try {
            return Integer.parseInt(props.getProperty(key));
        } catch (NumberFormatException e) {
            return defaultValue;
        }
    }
    
    /**
     * 获取布尔配置
     */
    public static boolean getBoolean(String key) {
        return getBoolean(key, false);
    }
    
    /**
     * 获取布尔配置（带默认值）
     */
    public static boolean getBoolean(String key, boolean defaultValue) {
        String value = props.getProperty(key);
        if (value == null) {
            return defaultValue;
        }
        return Boolean.parseBoolean(value);
    }
    
    /**
     * 重新加载配置
     */
    public static void reload() {
        props.clear();
        loadConfig();
    }
    
    /**
     * 获取所有配置
     */
    public static Properties getAllProperties() {
        Properties copy = new Properties();
        copy.putAll(props);
        return copy;
    }
}
```

---

## 六、注意事项和陷阱

### 1. 静态变量线程安全问题

```
class Counter {
    // 不安全的静态变量
    public static int unsafeCount = 0;
    
    // 安全的静态变量
    private static int safeCount = 0;
    
    public static void incrementUnsafe() {
        unsafeCount++;  // 非原子操作，线程不安全
    }
    
    public static synchronized void incrementSafe() {
        safeCount++;  // 同步方法，线程安全
    }
    
    // 使用AtomicInteger更佳
    private static java.util.concurrent.atomic.AtomicInteger atomicCount = 
        new java.util.concurrent.atomic.AtomicInteger(0);
    
    public static void incrementAtomic() {
        atomicCount.incrementAndGet();
    }
}
```

### 2. 静态代码块异常处理

```
class StaticBlockExample {
    // 静态变量
    static int value;
    
    // 静态代码块
    static {
        try {
            value = initializeValue();
        } catch (Exception e) {
            // 必须处理异常，否则类加载失败
            value = 0;
            System.err.println("初始化失败: " + e.getMessage());
        }
    }
    
    private static int initializeValue() throws Exception {
        // 可能抛出异常的操作
        return 10 / 0;  // 故意除零
    }
}
```

### 3. 静态方法的重写问题

```
class Parent {
    public static void staticMethod() {
        System.out.println("Parent static method");
    }
    
    public void instanceMethod() {
        System.out.println("Parent instance method");
    }
}

class Child extends Parent {
    // 这不是重写，是隐藏
    public static void staticMethod() {
        System.out.println("Child static method");
    }
    
    // 这是重写
    @Override
    public void instanceMethod() {
        System.out.println("Child instance method");
    }
}

public class Test {
    public static void main(String[] args) {
        Parent p = new Child();
        p.staticMethod();   // 输出: Parent static method（静态绑定）
        p.instanceMethod(); // 输出: Child instance method（动态绑定）
        
        Child c = new Child();
        c.staticMethod();   // 输出: Child static method
    }
}
```

### 4. 工具类设计陷阱

```
// 不好的设计
class BadUtils {
    // 没有私有构造方法，可以实例化
    public BadUtils() {}
    
    public static void doSomething() {
        // ...
    }
}

// 客户端可能错误地实例化
BadUtils utils = new BadUtils();  // 不应该被允许

// 好的设计
class GoodUtils {
    // 私有构造方法
    private GoodUtils() {
        throw new AssertionError("不能实例化工具类");
    }
    
    public static void doSomething() {
        // ...
    }
}

// GoodUtils utils = new GoodUtils();  // 编译错误
```

---

## 七、性能优化

### 1. 静态变量初始化优化

```
class PerformanceExample {
    // 不好的写法 - 每次调用都重新计算
    public static String getTimestamp() {
        return new java.util.Date().toString();
    }
    
    // 好的写法 - 使用缓存
    private static volatile String cachedTimestamp;
    private static final Object lock = new Object();
    
    public static String getCachedTimestamp() {
        if (cachedTimestamp == null) {
            synchronized (lock) {
                if (cachedTimestamp == null) {
                    cachedTimestamp = new java.util.Date().toString();
                }
            }
        }
        return cachedTimestamp;
    }
    
    // 使用懒加载模式
    private static class LazyHolder {
        static final String INSTANCE = new java.util.Date().toString();
    }
    
    public static String getLazyTimestamp() {
        return LazyHolder.INSTANCE;
    }
}
```

### 2. 避免不必要的静态初始化

```
class ResourceIntensive {
    // 静态变量在类加载时初始化
    static ExpensiveObject expensive = new ExpensiveObject();  // 立即加载
    
    // 懒加载模式
    private static class Holder {
        static final ExpensiveObject INSTANCE = new ExpensiveObject();
    }
    
    public static ExpensiveObject getExpensiveObject() {
        return Holder.INSTANCE;  // 第一次访问时才加载
    }
}
```

---

## 八、实际应用案例

### 案例1：验证工具类

```
import java.util.regex.Pattern;

/**
 * 验证工具类
 */
public final class Validator {
    
    private Validator() {
        throw new AssertionError("不能实例化工具类");
    }
    
    // 预编译的正则表达式
    private static final Pattern EMAIL_PATTERN = 
        Pattern.compile("^[A-Za-z0-9+_.-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,}$");
    
    private static final Pattern PHONE_PATTERN = 
        Pattern.compile("^1[3-9]\\d{9}$");
    
    private static final Pattern ID_CARD_PATTERN = 
        Pattern.compile("^\\d{17}[0-9X]$");
    
    /**
     * 验证邮箱
     */
    public static boolean isEmail(String email) {
        return email != null && EMAIL_PATTERN.matcher(email).matches();
    }
    
    /**
     * 验证手机号
     */
    public static boolean isPhone(String phone) {
        return phone != null && PHONE_PATTERN.matcher(phone).matches();
    }
    
    /**
     * 验证身份证
     */
    public static boolean isIdCard(String idCard) {
        if (idCard == null || !ID_CARD_PATTERN.matcher(idCard).matches()) {
            return false;
        }
        return validateIdCard(idCard);
    }
    
    /**
     * 验证身份证校验码
     */
    private static boolean validateIdCard(String idCard) {
        int[] weight = {7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2};
        char[] validate = {'1', '0', 'X', '9', '8', '7', '6', '5', '4', '3', '2'};
        
        int sum = 0;
        for (int i = 0; i < 17; i++) {
            sum += (idCard.charAt(i) - '0') * weight[i];
        }
        
        char checkCode = validate[sum % 11];
        return idCard.charAt(17) == checkCode;
    }
    
    /**
     * 验证密码强度
     */
    public static PasswordStrength checkPasswordStrength(String password) {
        if (password == null || password.length() < 8) {
            return PasswordStrength.WEAK;
        }
        
        boolean hasUpper = false;
        boolean hasLower = false;
        boolean hasDigit = false;
        boolean hasSpecial = false;
        
        for (char c : password.toCharArray()) {
            if (Character.isUpperCase(c)) hasUpper = true;
            else if (Character.isLowerCase(c)) hasLower = true;
            else if (Character.isDigit(c)) hasDigit = true;
            else hasSpecial = true;
        }
        
        int score = 0;
        if (hasUpper) score++;
        if (hasLower) score++;
        if (hasDigit) score++;
        if (hasSpecial) score++;
        if (password.length() >= 12) score++;
        
        if (score >= 5) return PasswordStrength.STRONG;
        if (score >= 3) return PasswordStrength.MEDIUM;
        return PasswordStrength.WEAK;
    }
    
    public enum PasswordStrength {
        WEAK, MEDIUM, STRONG
    }
}
```

### 案例2：加密工具类

```
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.Base64;

/**
 * 加密工具类
 */
public final class EncryptionUtils {
    
    private EncryptionUtils() {
        throw new AssertionError("不能实例化工具类");
    }
    
    /**
     * MD5加密
     */
    public static String md5(String input) {
        if (input == null) {
            return null;
        }
        try {
            MessageDigest md = MessageDigest.getInstance("MD5");
            byte[] digest = md.digest(input.getBytes());
            return bytesToHex(digest);
        } catch (NoSuchAlgorithmException e) {
            throw new RuntimeException(e);
        }
    }
    
    /**
     * SHA-256加密
     */
    public static String sha256(String input) {
        if (input == null) {
            return null;
        }
        try {
            MessageDigest md = MessageDigest.getInstance("SHA-256");
            byte[] digest = md.digest(input.getBytes());
            return bytesToHex(digest);
        } catch (NoSuchAlgorithmException e) {
            throw new RuntimeException(e);
        }
    }
    
    /**
     * Base64编码
     */
    public static String base64Encode(String input) {
        if (input == null) {
            return null;
        }
        return Base64.getEncoder().encodeToString(input.getBytes());
    }
    
    /**
     * Base64解码
     */
    public static String base64Decode(String input) {
        if (input == null) {
            return null;
        }
        byte[] decoded = Base64.getDecoder().decode(input);
        return new String(decoded);
    }
    
    /**
     * 字节数组转16进制字符串
     */
    private static String bytesToHex(byte[] bytes) {
        StringBuilder sb = new StringBuilder();
        for (byte b : bytes) {
            sb.append(String.format("%02x", b));
        }
        return sb.toString();
    }
}
```

---

## 九、最佳实践总结

### 1. 工具类设计原则

```
/**
 * 1. 使用final类防止继承
 * 2. 私有构造方法防止实例化
 * 3. 方法都是静态的
 * 4. 提供详细的文档注释
 * 5. 处理null值输入
 * 6. 保持方法的原子性
 * 7. 考虑线程安全性
 * 8. 使用有意义的异常
 */
public final class BestPracticeUtils {
    
    // 1. 私有构造方法
    private BestPracticeUtils() {
        throw new AssertionError("不能实例化工具类");
    }
    
    // 2. 静态常量（大写+下划线）
    public static final int MAX_RETRY_COUNT = 3;
    private static final String DEFAULT_VALUE = "default";
    
    // 3. 静态代码块用于初始化
    private static final Map<String, String> CACHE = new HashMap<>();
    static {
        CACHE.put("key1", "value1");
        CACHE.put("key2", "value2");
    }
    
    /**
     * 4. 方法要有明确的功能
     */
    public static String process(String input) {
        // 5. 处理null输入
        if (input == null) {
            return DEFAULT_VALUE;
        }
        // 6. 业务逻辑
        return input.trim().toUpperCase();
    }
    
    /**
     * 7. 重载方法提供灵活性
     */
    public static String process(String input, String defaultValue) {
        if (input == null) {
            return defaultValue;
        }
        return process(input);
    }
    
    /**
     * 8. 使用枚举提高可读性
     */
    public static String format(FormatType type, String input) {
        switch (type) {
            case UPPER: return input.toUpperCase();
            case LOWER: return input.toLowerCase();
            case CAPITALIZE: return capitalize(input);
            default: return input;
        }
    }
    
    public enum FormatType {
        UPPER, LOWER, CAPITALIZE
    }
    
    private static String capitalize(String str) {
        if (str == null || str.isEmpty()) {
            return str;
        }
        return str.substring(0, 1).toUpperCase() + str.substring(1).toLowerCase();
    }
}
```

### 2. 使用建议

1. **优先使用Java内置工具类**（Collections, Arrays, Objects等）
    
2. **静态方法应该是无状态的**
    
3. **避免在静态方法中修改共享状态**
    
4. **考虑使用依赖注入替代静态方法**
    
5. **大型项目考虑使用单例模式替代工具类**
    
6. **工具类方法应该小而专注**
    

---

## 十、常见面试题

### 1. 静态方法和实例方法的区别？

```
/**
 * 静态方法：
 * 1. 属于类，用static修饰
 * 2. 通过类名调用
 * 3. 不能访问实例变量和方法
 * 4. 不能使用this和super
 * 5. 在类加载时初始化
 * 
 * 实例方法：
 * 1. 属于对象
 * 2. 通过对象调用
 * 3. 可以访问静态和实例成员
 * 4. 可以使用this和super
 * 5. 在对象创建时初始化
 */
```

### 2. 什么时候用静态方法？

- 工具类方法
    
- 工厂方法
    
- 数学计算
    
- 不需要对象状态的辅助方法
    
- 单例模式的getInstance()
    

### 3. 静态变量和实例变量的区别？

- 静态变量：类级别，所有对象共享
    
- 实例变量：对象级别，每个对象独立
    
- 静态变量在类加载时初始化
    
- 实例变量在对象创建时初始化