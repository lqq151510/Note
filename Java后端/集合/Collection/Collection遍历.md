# Java Collection 遍历方式详解

## 📋 遍历方式概览

|遍历方式|引入版本|特点|适用场景|
|---|---|---|---|
|迭代器遍历|Java 1.2|可删除元素，最原始|需要在遍历中删除元素|
|增强for循环|Java 5|语法简洁，易读|大多数遍历场景|
|Lambda遍历|Java 8|函数式编程，简洁|集合流式处理，并行处理|

---

## 🔄 迭代器遍历

### 1. 基本语法

```
// 获取迭代器
Iterator<元素类型> iterator = collection.iterator();

// 遍历
while (iterator.hasNext()) {
    元素类型 element = iterator.next();
    // 处理元素
}
```

### 2. 完整示例

```
import java.util.*;

public class IteratorTraversal {
    public static void main(String[] args) {
        // 创建迭代器对象
        Collection<String> fruits = new ArrayList<>();
        fruits.add("Apple");
        fruits.add("Banana");
        fruits.add("Cherry");
        fruits.add("Date");
        fruits.add("Elderberry");

        System.out.println("=== 迭代器遍历 ===");
        
        // 1. 基本迭代器遍历
        //获取迭代器对象
        Iterator<String> iterator = fruits.iterator();
        while (iterator.hasNext()) {
            String fruit = iterator.next();
            System.out.println("水果: " + fruit);
        }

        System.out.println("\n=== 遍历中删除元素 ===");
        // 2. 遍历时删除特定元素
        Collection<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
        Iterator<Integer> numIterator = numbers.iterator();
        while (numIterator.hasNext()) {
            Integer num = numIterator.next();
            if (num % 2 == 0) {  // 删除偶数
                numIterator.remove();
            }
        }
        System.out.println("删除偶数后: " + numbers);

        System.out.println("\n=== 双向遍历（List特有）===");
        // 3. 双向迭代器（仅List有）
        List<String> list = new ArrayList<>(fruits);
        ListIterator<String> listIterator = list.listIterator();
        
        // 正向遍历
        System.out.println("正向遍历:");
        while (listIterator.hasNext()) {
            int index = listIterator.nextIndex();
            String element = listIterator.next();
            System.out.println("索引 " + index + ": " + element);
        }
        
        // 反向遍历
        System.out.println("\n反向遍历:");
        while (listIterator.hasPrevious()) {
            int index = listIterator.previousIndex();
            String element = listIterator.previous();
            System.out.println("索引 " + index + ": " + element);
        }
    }
}
```

### 3. 迭代器方法详解

|方法|返回值|描述|
|---|---|---|
|`hasNext()`|boolean|是否还有下一个元素|
|`next()`|E|返回下一个元素|
|`remove()`|void|删除当前元素|
|`forEachRemaining(Consumer)`|void|Java 8+，剩余元素处理|

**注意**：

- 调用 `next()`前必须先调用 `hasNext()`检查
    
- 每个 `next()`调用前只能调用一次 `remove()`
    

### 4. 常见错误

```
Collection<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));
Iterator<String> it = list.iterator();

// 错误1：连续调用next()跳过元素
while (it.hasNext()) {
    System.out.println(it.next());  // 第一个
    System.out.println(it.next());  // 第二个，可能越界
}

// 错误2：修改集合后继续使用迭代器
while (it.hasNext()) {
    String item = it.next();
    if ("B".equals(item)) {
        list.remove(item); × // 抛出ConcurrentModificationException
        it.move()√
    }
}
```

---

## 🔄 增强for循环（foreach）

### 1. 基本语法

```
for (元素类型 element : collection) {
    // 处理元素
}
//只有单列集合和数组可以
```

### 2. 完整示例

```
import java.util.*;

public class EnhancedForLoop {
    public static void main(String[] args) {
        // 创建不同类型的集合
        Collection<String> arrayList = new ArrayList<>(Arrays.asList("Java", "Python", "C++"));
        Collection<String> linkedList = new LinkedList<>(Arrays.asList("Red", "Green", "Blue"));
        Collection<String> hashSet = new HashSet<>(Arrays.asList("Apple", "Banana", "Cherry"));
        Collection<Integer> treeSet = new TreeSet<>(Arrays.asList(5, 2, 8, 1, 9));

        System.out.println("=== ArrayList遍历 ===");
        for (String language : arrayList) {
            System.out.println("编程语言: " + language);
        }

        System.out.println("\n=== LinkedList遍历 ===");
        for (String color : linkedList) {
            System.out.println("颜色: " + color);
        }

        System.out.println("\n=== HashSet遍历（无序）===");
        for (String fruit : hashSet) {
            System.out.println("水果: " + fruit);
        }

        System.out.println("\n=== TreeSet遍历（有序）===");
        for (Integer num : treeSet) {
            System.out.println("数字: " + num);
        }

        System.out.println("\n=== 遍历数组 ===");
        String[] array = {"张三", "李四", "王五"};
        for (String name : array) {
            System.out.println("姓名: " + name);
        }

        System.out.println("\n=== 遍历Map的Entry ===");
        Map<Integer, String> map = new HashMap<>();
        map.put(1, "Java");
        map.put(2, "Python");
        map.put(3, "C++");
        
        for (Map.Entry<Integer, String> entry : map.entrySet()) {
            System.out.println("Key: " + entry.getKey() + ", Value: " + entry.getValue());
        }

        System.out.println("\n=== 遍历Map的Key ===");
        for (Integer key : map.keySet()) {
            System.out.println("Key: " + key);
        }

        System.out.println("\n=== 遍历Map的Value ===");
        for (String value : map.values()) {
            System.out.println("Value: " + value);
        }
    }
}
```

### 3. 内部原理

```
// 增强for循环实际上会被编译器转换为迭代器
// 源代码：
for (String item : list) {
    System.out.println(item);
}

// 编译后等价于：
Iterator<String> iterator = list.iterator();
while (iterator.hasNext()) {
    String item = iterator.next();
    System.out.println(item);
}
```

### 4. 优缺点

**优点**：

- 语法简洁，易读
    
- 避免下标越界错误
    
- 适用于所有Iterable对象
    

**缺点**：

- 不能获取当前索引
    
- 不能修改集合结构（添加/删除）
    
- 不能反向遍历
    

### 5. 限制和注意事项

```
List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));

// 错误：在增强for循环中删除元素
for (String item : list) {
    if ("B".equals(item)) {
        list.remove(item);  // 抛出ConcurrentModificationException
    }
}

// 正确做法：使用迭代器或Java 8的removeIf
list.removeIf("B"::equals);
```

---

## 🔄 Lambda表达式遍历

### 1. 基本语法

```
// 使用forEach方法
collection.forEach(element -> {
    // 处理元素
});

// 或使用方法引用
collection.forEach(System.out::println);
```

### 2. 完整示例

```
import java.util.*;
import java.util.function.Consumer;

public class LambdaTraversal {
    public static void main(String[] args) {
        // 创建集合
        Collection<String> languages = new ArrayList<>(Arrays.asList(
            "Java", "Python", "JavaScript", "C++", "Go", "Rust"
        ));

        System.out.println("=== 1. 基本Lambda遍历 ===");
        // 1.1 使用Lambda表达式
        languages.forEach(lang -> {
            System.out.println("语言: " + lang);
        });

        // 1.2 使用方法引用
        System.out.println("\n=== 2. 方法引用遍历 ===");
        languages.forEach(System.out::println);

        System.out.println("\n=== 3. 带条件的遍历 ===");
        // 3. 结合流API进行过滤
        languages.stream()
                .filter(lang -> lang.startsWith("J"))
                .forEach(lang -> System.out.println("以J开头的语言: " + lang));

        System.out.println("\n=== 4. 并行遍历 ===");
        // 4. 并行遍历（顺序不保证）
        languages.parallelStream()
                .forEach(lang -> System.out.println("并行处理: " + lang));

        System.out.println("\n=== 5. 复杂操作 ===");
        // 5. 复杂操作：统计、转换、过滤
        List<String> result = new ArrayList<>();
        languages.stream()
                .map(String::toUpperCase)  // 转换为大写
                .sorted()                  // 排序
                .forEach(lang -> {
                    result.add(lang);
                    System.out.println("处理后的语言: " + lang);
                });

        System.out.println("\n最终结果: " + result);

        System.out.println("\n=== 6. Consumer接口实现 ===");
        // 6. 使用Consumer接口
        Consumer<String> printConsumer = lang -> System.out.println("消费者处理: " + lang);
        Consumer<String> upperCaseConsumer = lang -> System.out.println("大写: " + lang.toUpperCase());
        
        // 组合Consumer
        languages.forEach(printConsumer.andThen(upperCaseConsumer));
    }
}
```

### 3. Stream API 进阶示例

```
import java.util.*;
import java.util.stream.Collectors;

public class StreamAPIExamples {
    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
            new Person("张三", 25, "北京"),
            new Person("李四", 30, "上海"),
            new Person("王五", 28, "北京"),
            new Person("赵六", 35, "广州"),
            new Person("孙七", 22, "上海")
        );

        System.out.println("=== 1. 过滤和映射 ===");
        // 过滤年龄大于25的人，并提取名字
        List<String> names = people.stream()
                .filter(p -> p.getAge() > 25)
                .map(Person::getName)
                .collect(Collectors.toList());
        names.forEach(System.out::println);

        System.out.println("\n=== 2. 分组 ===");
        // 按城市分组
        Map<String, List<Person>> peopleByCity = people.stream()
                .collect(Collectors.groupingBy(Person::getCity));
        
        peopleByCity.forEach((city, persons) -> {
            System.out.println("城市: " + city);
            persons.forEach(p -> System.out.println("  - " + p.getName()));
        });

        System.out.println("\n=== 3. 统计 ===");
        // 统计信息
        IntSummaryStatistics ageStats = people.stream()
                .mapToInt(Person::getAge)
                .summaryStatistics();
        System.out.println("平均年龄: " + ageStats.getAverage());
        System.out.println("最大年龄: " + ageStats.getMax());
        System.out.println("最小年龄: " + ageStats.getMin());
        System.out.println("总人数: " + ageStats.getCount());

        System.out.println("\n=== 4. 排序 ===");
        // 多重排序：先按城市，再按年龄
        people.stream()
                .sorted(Comparator.comparing(Person::getCity)
                        .thenComparing(Person::getAge))
                .forEach(p -> System.out.println(p.getCity() + " - " + p.getName() + "(" + p.getAge() + ")"));

        System.out.println("\n=== 5. 查找和匹配 ===");
        // 查找第一个匹配的
        Optional<Person> firstBeijing = people.stream()
                .filter(p -> "北京".equals(p.getCity()))
                .findFirst();
        firstBeijing.ifPresent(p -> System.out.println("第一个北京人: " + p.getName()));

        // 检查是否所有人都大于20岁
        boolean allAbove20 = people.stream()
                .allMatch(p -> p.getAge() > 20);
        System.out.println("所有人都大于20岁吗? " + allAbove20);
    }
}

class Person {
    private String name;
    private int age;
    private String city;
    
    // 构造函数、getter、setter
    public Person(String name, int age, String city) {
        this.name = name;
        this.age = age;
        this.city = city;
    }
    
    public String getName() { return name; }
    public int getAge() { return age; }
    public String getCity() { return city; }
}
```

### 4. 并行流性能示例

```
import java.util.*;
import java.util.stream.IntStream;

public class ParallelStreamDemo {
    public static void main(String[] args) {
        // 创建大量数据
        List<Integer> numbers = new ArrayList<>();
        for (int i = 0; i < 1_000_000; i++) {
            numbers.add(i);
        }

        // 顺序流
        long startTime = System.currentTimeMillis();
        long sum1 = numbers.stream()
                .mapToLong(Integer::longValue)
                .sum();
        long endTime = System.currentTimeMillis();
        System.out.println("顺序流耗时: " + (endTime - startTime) + "ms, 和: " + sum1);

        // 并行流
        startTime = System.currentTimeMillis();
        long sum2 = numbers.parallelStream()
                .mapToLong(Integer::longValue)
                .sum();
        endTime = System.currentTimeMillis();
        System.out.println("并行流耗时: " + (endTime - startTime) + "ms, 和: " + sum2);
        
        // 注意：并行流不总是更快，小数据量时可能更慢
    }
}
```

---

## 📊 三种遍历方式对比

|特性|迭代器遍历|增强for循环|Lambda遍历|
|---|---|---|---|
|**语法简洁性**​|较低|高|高|
|**可读性**​|一般|好|好|
|**可删除元素**​|✅ 可以|❌ 不可以|✅ 可以（通过removeIf）|
|**可修改元素**​|✅ 可以|❌ 不可以|✅ 可以|
|**获取索引**​|❌ 不可以|❌ 不可以|❌ 不可以（Stream可以）|
|**并行处理**​|❌ 不可以|❌ 不可以|✅ 可以|
|**异常处理**​|容易|容易|复杂|
|**性能**​|中等|中等|高（并行时）|
|**链式操作**​|❌ 不可以|❌ 不可以|✅ 可以|
|**适用版本**​|Java 1.2+|Java 5+|Java 8+|

---

## 🎯 最佳实践建议

### 1. 选择遍历方式的原则

```
// 1. 简单遍历，不需要修改集合 → 增强for循环
for (String item : collection) {
    System.out.println(item);
}

// 2. 需要在遍历中删除元素 → 迭代器
Iterator<String> it = collection.iterator();
while (it.hasNext()) {
    if (it.next().equals("remove")) {
        it.remove();
    }
}

// 3. 复杂的过滤、转换、统计 → Lambda/Stream API
collection.stream()
        .filter(item -> item.length() > 3)
        .map(String::toUpperCase)
        .sorted()
        .forEach(System.out::println);

// 4. 大数据量并行处理 → 并行流
collection.parallelStream()
        .forEach(this::processItem);
```

### 2. 性能优化建议

```
// 1. 小数据集使用顺序流
// 并行流有线程创建开销，小数据可能更慢
if (collection.size() < 1000) {
    collection.forEach(this::process);
} else {
    collection.parallelStream().forEach(this::process);
}

// 2. 避免在Stream中修改外部状态
// 不推荐
List<String> result = new ArrayList<>();
collection.forEach(item -> {
    if (item.startsWith("A")) {
        result.add(item);  // 修改外部状态
    }
});

// 推荐：使用collect收集结果
List<String> result = collection.stream()
        .filter(item -> item.startsWith("A"))
        .collect(Collectors.toList());

// 3. 重用Stream
// Stream只能使用一次，需要重用时应重新创建
Stream<String> stream = collection.stream();
stream.forEach(System.out::println);
// stream.forEach(...);  // 错误！Stream已关闭
```

### 3. 异常处理

```
// 1. Lambda中的异常处理
collection.forEach(item -> {
    try {
        processItem(item);
    } catch (Exception e) {
        System.err.println("处理失败: " + item);
    }
});

// 2. 使用包装方法
collection.forEach(this::safeProcess);

private void safeProcess(String item) {
    try {
        processItem(item);
    } catch (Exception e) {
        handleError(e, item);
    }
}

// 3. 使用Optional避免NullPointerException
Optional.ofNullable(collection)
        .orElse(Collections.emptyList())
        .forEach(System.out::println);
```

---

## 🔧 实用工具方法

### 1. 遍历工具类

```
import java.util.*;
import java.util.function.Consumer;
import java.util.function.Predicate;

public class CollectionUtils {
    
    /**
     * 安全的遍历方法（处理null集合）
     */
    public static <T> void safeForEach(Collection<T> collection, Consumer<T> action) {
        if (collection != null && action != null) {
            collection.forEach(action);
        }
    }
    
    /**
     * 带索引的遍历
     */
    public static <T> void forEachWithIndex(Collection<T> collection, IndexedConsumer<T> consumer) {
        if (collection == null || consumer == null) return;
        
        int index = 0;
        for (T item : collection) {
            consumer.accept(index++, item);
        }
    }
    
    /**
     * 过滤并收集结果
     */
    public static <T> List<T> filterAndCollect(Collection<T> collection, Predicate<T> predicate) {
        if (collection == null) return Collections.emptyList();
        
        return collection.stream()
                .filter(predicate)
                .collect(Collectors.toList());
    }
    
    /**
     * 并行处理并收集结果
     */
    public static <T, R> List<R> parallelProcess(Collection<T> collection, 
                                                 java.util.function.Function<T, R> mapper) {
        if (collection == null) return Collections.emptyList();
        
        return collection.parallelStream()
                .map(mapper)
                .collect(Collectors.toList());
    }
    
    @FunctionalInterface
    public interface IndexedConsumer<T> {
        void accept(int index, T item);
    }
}
```

### 2. 使用示例

```
public class UtilsDemo {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie", null, "David");
        
        // 安全遍历
        CollectionUtils.safeForEach(names, name -> {
            if (name != null) {
                System.out.println("Name: " + name);
            }
        });
        
        // 带索引的遍历
        CollectionUtils.forEachWithIndex(names, (index, name) -> {
            System.out.println("Index " + index + ": " + name);
        });
        
        // 过滤非null的名字
        List<String> nonNullNames = CollectionUtils.filterAndCollect(names, 
            name -> name != null && name.length() > 3);
        System.out.println("过滤结果: " + nonNullNames);
    }
}
```

---

## 📋 总结

### 1. 遍历方式选择指南

- **简单读取**：增强for循环
    
- **需要删除元素**：迭代器
    
- **复杂数据处理**：Stream API
    
- **大数据量并行处理**：并行流
    
- **需要向后兼容Java 8以下**：迭代器或增强for循环
    

### 2. 性能要点

1. 小数据集（<1000）用顺序操作
    
2. 大数据集考虑并行流
    
3. 避免在循环中创建大量临时对象
    
4. 重用Stream需要重新创建
    

### 3. 代码可读性

1. 方法引用（`System.out::println`）比Lambda更简洁
    
2. 链式调用提高可读性
    
3. 合理使用中间操作（filter, map, sorted）
    

### 4. 错误处理

1. 总是处理可能的null值
    
2. Lambda中的异常需要妥善处理
    
3. 并行流注意线程安全问题
    

根据具体场景选择合适的遍历方式，可以写出更高效、更易维护的Java代码。