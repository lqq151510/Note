# Java ArrayList 常用方法详解

## 📋 ArrayList 概述

**ArrayList**​ 是 Java 集合框架中最常用的动态数组实现，位于 `java.util`包中。它提供了可动态调整大小的数组功能，比传统数组更加灵活。

### 主要特点：

- 基于数组实现，支持随机访问
    
- 动态扩容（默认初始容量为10，扩容因子为1.5倍）
    
- 允许存储 null 值
    
- 非线程安全
    
- 保持了元素的插入顺序
    

---

## 📦 创建 ArrayList

### 1. 基本创建方式

```
import java.util.ArrayList;
import java.util.List;

public class ArrayListDemo {
    public static void main(String[] args) {
        // 方式1：不指定初始容量（默认10）
        ArrayList<String> list1 = new ArrayList<>();
        
        // 方式2：指定初始容量
        ArrayList<Integer> list2 = new ArrayList<>(20);
        
        // 方式3：通过List接口引用（推荐）
        List<String> list3 = new ArrayList<>();
        
        // 方式4：从其他集合创建
        List<String> existingList = Arrays.asList("A", "B", "C");
        ArrayList<String> list4 = new ArrayList<>(existingList);
    }
}
```

### 2. 泛型支持

```
// 指定元素类型
ArrayList<String> stringList = new ArrayList<>();    // 字符串类型
ArrayList<Integer> integerList = new ArrayList<>(); // 整数类型
ArrayList<Object> objectList = new ArrayList<>();    // 任意类型
```

---

## 🔧 常用方法详解

### 1. 添加元素

|方法|描述|返回值|时间复杂度|
|---|---|---|---|
|`add(E e)`|添加元素到列表末尾|boolean|O(1) 摊销|
|`add(int index, E element)`|在指定位置插入元素|void|O(n)|
|`addAll(Collection c)`|添加集合中所有元素|boolean|O(n)|
|`addAll(int index, Collection c)`|在指定位置插入集合|boolean|O(n+m)|

**示例**：

```
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class AddMethods {
    public static void main(String[] args) {
        // 创建ArrayList
        ArrayList<String> list = new ArrayList<>();
        
        // 1. add(E e) - 添加元素
        list.add("Apple");
        list.add("Banana");
        list.add("Cherry");
        System.out.println("添加元素后: " + list); // [Apple, Banana, Cherry]
        
        // 2. add(int index, E element) - 在指定位置插入
        list.add(1, "Blueberry"); // 在索引1处插入
        System.out.println("插入后: " + list); // [Apple, Blueberry, Banana, Cherry]
        
        // 3. addAll(Collection c) - 添加集合
        List<String> fruits = Arrays.asList("Durian", "Elderberry");
        list.addAll(fruits);
        System.out.println("添加集合后: " + list); 
        // [Apple, Blueberry, Banana, Cherry, Durian, Elderberry]
        
        // 4. addAll(int index, Collection c) - 在指定位置插入集合
        List<String> moreFruits = Arrays.asList("Fig", "Grape");
        list.addAll(2, moreFruits);
        System.out.println("指定位置插入集合后: " + list);
        // [Apple, Blueberry, Fig, Grape, Banana, Cherry, Durian, Elderberry]
    }
}
```

### 2. 获取元素

|方法|描述|返回值|时间复杂度|
|---|---|---|---|
|`get(int index)`|获取指定位置的元素|E|O(1)|
|`size()`|返回列表中的元素数量|int|O(1)|
|`isEmpty()`|判断列表是否为空|boolean|O(1)|
|`indexOf(Object o)`|返回元素第一次出现的索引|int|O(n)|
|`lastIndexOf(Object o)`|返回元素最后一次出现的索引|int|O(n)|
|`contains(Object o)`|判断是否包含指定元素|boolean|O(n)|

**示例**：

```
import java.util.ArrayList;
import java.util.Arrays;

public class GetMethods {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>(
            Arrays.asList("Apple", "Banana", "Cherry", "Banana", "Date")
        );
        
        // 1. get(int index) - 获取元素
        String first = list.get(0);
        System.out.println("第一个元素: " + first); // Apple
        
        // 2. size() - 获取大小
        int size = list.size();
        System.out.println("列表大小: " + size); // 5
        
        // 3. isEmpty() - 判断是否为空
        boolean empty = list.isEmpty();
        System.out.println("是否为空: " + empty); // false
        
        // 4. indexOf(Object o) - 第一次出现的索引
        int firstIndex = list.indexOf("Banana");
        System.out.println("Banana第一次出现的位置: " + firstIndex); // 1
        
        // 5. lastIndexOf(Object o) - 最后一次出现的索引
        int lastIndex = list.lastIndexOf("Banana");
        System.out.println("Banana最后一次出现的位置: " + lastIndex); // 3
        
        // 6. contains(Object o) - 是否包含
        boolean hasCherry = list.contains("Cherry");
        boolean hasFig = list.contains("Fig");
        System.out.println("包含Cherry吗? " + hasCherry); // true
        System.out.println("包含Fig吗? " + hasFig); // false
        
        // 7. 遍历元素
        System.out.println("\n遍历元素:");
        for (int i = 0; i < list.size(); i++) {
            System.out.println("索引 " + i + ": " + list.get(i));
        }
        
        // 8. 增强for循环
        System.out.println("\n增强for循环:");
        for (String fruit : list) {
            System.out.println(fruit);
        }
    }
}
```

### 3. 修改元素

|方法|描述|返回值|时间复杂度|
|---|---|---|---|
|`set(int index, E element)`|替换指定位置的元素|被替换的元素|O(1)|
|`replaceAll(UnaryOperator<E> operator)`|用给定操作的结果替换每个元素|void|O(n)|

**示例**：

```
import java.util.ArrayList;
import java.util.Arrays;

public class ModifyMethods {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>(
            Arrays.asList("Apple", "Banana", "Cherry", "Date")
        );
        System.out.println("原始列表: " + list);
        
        // 1. set(int index, E element) - 替换元素
        String oldValue = list.set(1, "Blueberry");
        System.out.println("替换索引1, 旧值: " + oldValue); // Banana
        System.out.println("替换后: " + list); // [Apple, Blueberry, Cherry, Date]
        
        // 2. replaceAll - 批量修改（Java 8+）
        ArrayList<String> numbers = new ArrayList<>(
            Arrays.asList("1", "2", "3", "4")
        );
        System.out.println("\n原始数字: " + numbers);
        
        // 给每个数字加上前缀
        numbers.replaceAll(s -> "No." + s);
        System.out.println("处理后: " + numbers); // [No.1, No.2, No.3, No.4]
        
        // 将所有字符串转为大写
        list.replaceAll(String::toUpperCase);
        System.out.println("转大写后: " + list); // [APPLE, BLUEBERRY, CHERRY, DATE]
    }
}
```

### 4. 删除元素

|方法|描述|返回值|时间复杂度|
|---|---|---|---|
|`remove(int index)`|删除指定位置的元素|被删除的元素|O(n)|
|`remove(Object o)`|删除第一次出现的指定元素|boolean|O(n)|
|`removeAll(Collection c)`|删除所有包含在指定集合中的元素|boolean|O(n²)|
|`retainAll(Collection c)`|仅保留指定集合中的元素|boolean|O(n²)|
|`clear()`|删除所有元素|void|O(n)|
|`removeIf(Predicate filter)`|删除满足条件的元素|boolean|O(n)|

**示例**：

```
import java.util.ArrayList;
import java.util.Arrays;
import java.util.function.Predicate;

public class RemoveMethods {
    public static void main(String[] args) {
        // 创建测试列表
        ArrayList<String> fruits = new ArrayList<>(
            Arrays.asList("Apple", "Banana", "Cherry", "Date", "Elderberry", "Fig")
        );
        System.out.println("原始列表: " + fruits);
        
        // 1. remove(int index) - 按索引删除
        String removed = fruits.remove(2);
        System.out.println("删除索引2的元素: " + removed); // Cherry
        System.out.println("删除后: " + fruits); // [Apple, Banana, Date, Elderberry, Fig]
        
        // 2. remove(Object o) - 按值删除
        boolean isRemoved = fruits.remove("Banana");
        System.out.println("删除Banana成功吗? " + isRemoved); // true
        System.out.println("删除后: " + fruits); // [Apple, Date, Elderberry, Fig]
        
        // 删除不存在的元素
        boolean notFound = fruits.remove("Grape");
        System.out.println("删除Grape成功吗? " + notFound); // false
        
        // 3. removeAll(Collection c) - 删除集合中的所有元素
        ArrayList<String> toRemove = new ArrayList<>(
            Arrays.asList("Apple", "Date", "Fig")
        );
        fruits.removeAll(toRemove);
        System.out.println("删除集合后: " + fruits); // [Elderberry]
        
        // 4. retainAll(Collection c) - 仅保留指定集合中的元素
        fruits.addAll(Arrays.asList("Apple", "Banana", "Cherry", "Date", "Fig"));
        System.out.println("\n新列表: " + fruits);
        
        ArrayList<String> toRetain = new ArrayList<>(
            Arrays.asList("Apple", "Banana", "Cherry")
        );
        fruits.retainAll(toRetain);
        System.out.println("仅保留指定集合后: " + fruits); // [Apple, Banana, Cherry]
        
        // 5. removeIf(Predicate filter) - 条件删除（Java 8+）
        // 删除长度大于5的字符串
        fruits.removeIf(s -> s.length() > 5);
        System.out.println("删除长度>5的元素后: " + fruits); // [Apple]
        
        // 6. clear() - 清空列表
        fruits.clear();
        System.out.println("清空后: " + fruits); // []
        System.out.println("清空后大小: " + fruits.size()); // 0
        System.out.println("是否为空: " + fruits.isEmpty()); // true
    }
}
```

### 5. 遍历和转换

|方法|描述|返回值|时间复杂度|
|---|---|---|---|
|`iterator()`|返回迭代器|Iterator<E>|O(1)|
|`listIterator()`|返回列表迭代器|ListIterator<E>|O(1)|
|`toArray()`|转换为数组|Object[]|O(n)|
|`toArray(T[] a)`|转换为指定类型的数组|T[]|O(n)|
|`subList(int fromIndex, int toIndex)`|返回子列表|List<E>|O(1)|

**示例**：

```
import java.util.*;

public class IterationMethods {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>(
            Arrays.asList("Apple", "Banana", "Cherry", "Date", "Elderberry")
        );
        
        System.out.println("原始列表: " + list);
        
        // 1. 使用迭代器遍历
        System.out.println("\n使用迭代器遍历:");
        Iterator<String> iterator = list.iterator();
        while (iterator.hasNext()) {
            String fruit = iterator.next();
            System.out.println(fruit);
            // 可以在遍历时删除元素
            if ("Banana".equals(fruit)) {
                iterator.remove();
            }
        }
        System.out.println("删除Banana后: " + list);
        
        // 2. 使用列表迭代器（可以双向遍历）
        System.out.println("\n使用列表迭代器:");
        ListIterator<String> listIterator = list.listIterator();
        while (listIterator.hasNext()) {
            int index = listIterator.nextIndex();
            String fruit = listIterator.next();
            System.out.println("索引 " + index + ": " + fruit);
        }
        
        // 反向遍历
        System.out.println("\n反向遍历:");
        while (listIterator.hasPrevious()) {
            int index = listIterator.previousIndex();
            String fruit = listIterator.previous();
            System.out.println("索引 " + index + ": " + fruit);
        }
        
        // 3. 转换为数组
        System.out.println("\n转换为数组:");
        // 转换为Object数组
        Object[] objectArray = list.toArray();
        System.out.println("Object数组: " + Arrays.toString(objectArray));
        
        // 转换为指定类型数组
        String[] stringArray = list.toArray(new String[0]);
        System.out.println("String数组: " + Arrays.toString(stringArray));
        
        // 4. 获取子列表
        System.out.println("\n获取子列表:");
        List<String> subList = list.subList(1, 3); // 包含1，不包含3
        System.out.println("子列表(1,3): " + subList);
        
        // 5. 使用forEach（Java 8+）
        System.out.println("\n使用forEach遍历:");
        list.forEach(fruit -> System.out.println("水果: " + fruit));
        
        // 6. 使用Stream API（Java 8+）
        System.out.println("\n使用Stream API:");
        list.stream()
            .filter(f -> f.length() > 4)
            .map(String::toUpperCase)
            .forEach(System.out::println);
    }
}
```

### 6. 其他实用方法

|方法|描述|返回值|时间复杂度|
|---|---|---|---|
|`sort(Comparator c)`|排序列表|void|O(n log n)|
|`trimToSize()`|将容量调整为当前大小|void|O(n)|
|`ensureCapacity(int minCapacity)`|确保最小容量|void|O(1)|
|`clone()`|创建浅拷贝副本|Object|O(n)|

**示例**：

```
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Comparator;
import java.util.Collections;

public class UtilityMethods {
    public static void main(String[] args) {
        ArrayList<Integer> numbers = new ArrayList<>(
            Arrays.asList(5, 2, 8, 1, 9, 3, 7, 4, 6)
        );
        System.out.println("原始列表: " + numbers);
        
        // 1. sort(Comparator c) - 排序
        // 自然顺序排序（升序）
        numbers.sort(Comparator.naturalOrder());
        System.out.println("升序排序: " + numbers);
        
        // 降序排序
        numbers.sort(Comparator.reverseOrder());
        System.out.println("降序排序: " + numbers);
        
        // 自定义排序规则
        ArrayList<String> words = new ArrayList<>(
            Arrays.asList("apple", "banana", "cherry", "date", "elderberry")
        );
        // 按字符串长度排序
        words.sort(Comparator.comparingInt(String::length));
        System.out.println("\n按长度排序: " + words);
        
        // 2. Collections.sort() - 传统排序方式
        Collections.sort(numbers);
        System.out.println("\nCollections.sort升序: " + numbers);
        
        Collections.sort(words, Collections.reverseOrder());
        System.out.println("Collections.sort降序: " + words);
        
        // 3. clone() - 克隆列表
        @SuppressWarnings("unchecked")
        ArrayList<Integer> clonedList = (ArrayList<Integer>) numbers.clone();
        System.out.println("\n克隆的列表: " + clonedList);
        System.out.println("是同一个对象吗? " + (numbers == clonedList)); // false
        System.out.println("内容相同吗? " + numbers.equals(clonedList)); // true
        
        // 4. trimToSize() - 调整容量
        // 添加大量元素
        ArrayList<Integer> largeList = new ArrayList<>();
        for (int i = 0; i < 1000; i++) {
            largeList.add(i);
        }
        System.out.println("\n添加1000个元素后:");
        System.out.println("大小: " + largeList.size());
        
        // 删除大部分元素
        largeList.subList(100, 1000).clear();
        System.out.println("删除900个元素后:");
        System.out.println("大小: " + largeList.size());
        
        // 调整容量
        largeList.trimToSize();
        System.out.println("trimToSize后: 容量已优化");
        
        // 5. ensureCapacity() - 确保容量
        ArrayList<Integer> capacityList = new ArrayList<>();
        capacityList.ensureCapacity(1000); // 预先分配容量
        System.out.println("\n预先分配1000容量，避免多次扩容");
        
        // 6. 批量操作示例
        ArrayList<Integer> list1 = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
        ArrayList<Integer> list2 = new ArrayList<>(Arrays.asList(4, 5, 6, 7, 8));
        
        // 并集
        ArrayList<Integer> union = new ArrayList<>(list1);
        union.addAll(list2);
        System.out.println("\n并集: " + union);
        
        // 交集
        ArrayList<Integer> intersection = new ArrayList<>(list1);
        intersection.retainAll(list2);
        System.out.println("交集: " + intersection);
        
        // 差集 (list1 - list2)
        ArrayList<Integer> difference = new ArrayList<>(list1);
        difference.removeAll(list2);
        System.out.println("差集 (list1 - list2): " + difference);
    }
}
```

---

## 💡 性能考虑

### 时间复杂度总结

|操作|平均时间复杂度|最坏时间复杂度|说明|
|---|---|---|---|
|添加（末尾）|O(1) 摊销|O(n)|扩容时需要复制数组|
|添加（指定位置）|O(n)|O(n)|需要移动元素|
|获取|O(1)|O(1)|随机访问|
|修改|O(1)|O(1)|直接替换|
|删除（末尾）|O(1)|O(1)|直接删除|
|删除（指定位置）|O(n)|O(n)|需要移动元素|
|查找|O(n)|O(n)|需要遍历|
|排序|O(n log n)|O(n log n)|快速排序/归并排序|

### 容量管理

```
// 1. 设置初始容量（如果知道大概大小）
ArrayList<String> list = new ArrayList<>(1000);

// 2. 预先分配容量
list.ensureCapacity(2000);

// 3. 优化容量（删除大量元素后）
list.trimToSize();
```

---

## 🎯 最佳实践

### 1. 选择正确的接口类型

```
// 推荐：使用接口类型声明
List<String> list = new ArrayList<>();

// 不推荐：使用具体类声明
ArrayList<String> list = new ArrayList<>(); // 不够灵活
```

### 2. 预估初始容量

```
// 如果知道大概元素数量，设置初始容量
List<String> list = new ArrayList<>(expectedSize);
```

### 3. 遍历时删除元素

```
// 正确：使用迭代器的remove方法
List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));
Iterator<String> iterator = list.iterator();
while (iterator.hasNext()) {
    String item = iterator.next();
    if ("B".equals(item)) {
        iterator.remove(); // 安全删除
    }
}

// 错误：在for循环中删除
for (int i = 0; i < list.size(); i++) {
    if ("B".equals(list.get(i))) {
        list.remove(i); // 可能导致ConcurrentModificationException
        i--; // 需要调整索引
    }
}
```

### 4. 批量操作

```
// 批量添加
list.addAll(Arrays.asList("A", "B", "C"));

// 批量删除
list.removeAll(Arrays.asList("A", "B"));

// 批量修改
list.replaceAll(String::toUpperCase);
```

---

## 📊 完整示例

```
import java.util.*;

public class CompleteArrayListExample {
    public static void main(String[] args) {
        // 1. 创建和初始化
        List<String> fruits = new ArrayList<>();
        fruits.add("Apple");
        fruits.add("Banana");
        fruits.add("Orange");
        
        // 2. 从数组初始化
        List<String> moreFruits = new ArrayList<>(
            Arrays.asList("Mango", "Grape", "Pineapple")
        );
        fruits.addAll(moreFruits);
        
        // 3. 访问元素
        System.out.println("第一个水果: " + fruits.get(0));
        System.out.println("列表大小: " + fruits.size());
        
        // 4. 遍历
        System.out.println("\n所有水果:");
        for (String fruit : fruits) {
            System.out.println("- " + fruit);
        }
        
        // 5. 修改
        fruits.set(1, "Blueberry");
        System.out.println("\n修改后: " + fruits);
        
        // 6. 删除
        fruits.remove("Orange");
        fruits.remove(0);
        System.out.println("删除后: " + fruits);
        
        // 7. 检查
        System.out.println("包含Mango吗? " + fruits.contains("Mango"));
        System.out.println("索引位置: " + fruits.indexOf("Grape"));
        
        // 8. 排序
        fruits.sort(String::compareToIgnoreCase);
        System.out.println("排序后: " + fruits);
        
        // 9. 转换为数组
        String[] fruitArray = fruits.toArray(new String[0]);
        System.out.println("\n转换为数组: " + Arrays.toString(fruitArray));
        
        // 10. 清空
        fruits.clear();
        System.out.println("清空后是否为空? " + fruits.isEmpty());
    }
}
```

---

## 📋 总结

ArrayList 是 Java 集合框架中最常用的数据结构之一，具有以下特点：

### 优点：

1. **随机访问快**：通过索引访问元素的时间复杂度为 O(1)
    
2. **内存连续**：数据在内存中连续存储，缓存友好
    
3. **简单易用**：API 设计直观，使用方便
    
4. **动态扩容**：自动处理容量增长
    

### 缺点：

1. **插入/删除慢**：在中间位置插入或删除需要移动元素
    
2. **内存浪费**：容量增长策略可能导致内存浪费
    
3. **非线程安全**：多线程环境下需要额外同步
    

### 适用场景：

- 需要频繁随机访问元素
    
- 数据量不大，或主要在末尾添加/删除元素
    
- 不需要频繁在中间位置插入/删除元素
    
- 不需要线程安全特性
    

### 替代选择：

- **LinkedList**：频繁在中间插入/删除元素
    
- **Vector**：需要线程安全的动态数组
    
- **CopyOnWriteArrayList**：读多写少的并发场景
    
- **ArrayDeque**：需要双端队列功能
    

掌握 ArrayList 的常用方法和特性，能够帮助你在实际开发中选择合适的数据结构，编写出更高效的 Java 代码。