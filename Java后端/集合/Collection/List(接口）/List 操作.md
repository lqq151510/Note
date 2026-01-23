# Java List 操作与遍历方式详解

## 📋 知识点小结

### 一、List 类知识点

**List 基本操作**：

1. **创建 List**：`List<String> list = new ArrayList<>()`
    
2. **添加元素**：
    
    - 末尾添加：`add("a")`
        
    - 指定位置添加：`add(1, "f")`
        
    
3. **删除元素**：`remove(1)`按索引删除
    
4. **修改元素**：`set(0, "g")`按索引修改
    
5. **输出列表**：直接打印会调用 `toString()`方法
    

---

## 🔄 五种遍历方式对比

### 1. 迭代器遍历 (Iterator)

```
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    String s = it.next();
    System.out.println(s);
}
```

**优点**：

- ✅ 可以在遍历中安全删除元素（使用 `it.remove()`）
    
- ✅ 适用于所有 Collection 接口的实现类
    
- ✅ 统一的遍历方式
    
- ✅ 线程安全（单线程环境下）
    

**缺点**：

- ❌ 只能向前遍历
    
- ❌ 不能修改元素（除非删除）
    
- ❌ 代码相对冗长
    
- ❌ 需要手动控制迭代
    

**适用场景**：

- 需要在遍历中删除元素
    
- 需要兼容旧版本代码
    
- 遍历多种集合类型
    

---

### 2. 增强 for 循环 (foreach)

```
for(String s: list){
    System.out.println(s);
}
```

**优点**：

- ✅ 语法简洁，可读性好
    
- ✅ 避免索引越界错误
    
- ✅ 编译器自动处理迭代
    
- ✅ 适用于数组和集合
    

**缺点**：

- ❌ 不能获取当前索引
    
- ❌ 不能在遍历中修改集合结构
    
- ❌ 不能反向遍历
    
- ❌ 无法删除元素（会抛出 ConcurrentModificationException）
    

**适用场景**：

- 只读遍历
    
- 简单的集合处理
    
- 代码简洁性优先的场景
    

---

### 3. Lambda 表达式遍历

```
list.forEach(System.out::println);
// 或
list.forEach(s -> System.out.println(s));
```

**优点**：

- ✅ 代码最简洁
    
- ✅ 支持并行处理（`parallelStream()`）
    
- ✅ 支持链式操作
    
- ✅ 支持方法引用
    

**缺点**：

- ❌ 不能在遍历中修改集合
    
- ❌ 异常处理复杂
    
- ❌ 不能使用 break/continue
    
- ❌ Java 8+ 才支持
    

**适用场景**：

- Java 8+ 项目
    
- 函数式编程风格
    
- 需要链式处理
    
- 并行处理大量数据
    

---

### 4. 普通 for 循环

```
for(int i = 0; i < list.size(); i++){
    System.out.println(list.get(i));
}
```

**优点**：

- ✅ 可以获取索引
    
- ✅ 可以修改元素
    
- ✅ 控制灵活（break/continue）
    
- ✅ 性能较高（ArrayList）
    
- ✅ 兼容所有Java版本
    

**缺点**：

- ❌ LinkedList 性能差（get() 是 O(n)）
    
- ❌ 容易索引越界
    
- ❌ 代码相对冗长
    
- ❌ 不适用于所有集合类型
    

**适用场景**：

- 需要索引操作
    
- 需要反向遍历
    
- 需要复杂的循环控制
    
- ArrayList 等随机访问集合
    

---

### 5. 列表迭代器 (ListIterator)

```
ListIterator<String> listIterator = list.listIterator();
while (listIterator.hasNext()){
    String s = listIterator.next();
    if("b".equals(s)){
        listIterator.add("f");  // 添加元素
    }
    System.out.println(s);
}
```

**优点**：

- ✅ 可以双向遍历（向前/向后）
    
- ✅ 可以添加、删除、修改元素
    
- ✅ 可以获取当前索引
    
- ✅ 功能最强大
    

**缺点**：

- ❌ 代码最复杂
    
- ❌ 只适用于 List 接口
    
- ❌ 需要更多内存
    
- ❌ 使用相对较少
    

**适用场景**：

- 需要双向遍历
    
- 需要在遍历中修改列表
    
- 需要复杂的列表操作
    
- List 专用操作
    

---

## 📊 遍历方式对比总结

|特性|迭代器|增强for|Lambda|普通for|ListIterator|
|---|---|---|---|---|---|
|**语法简洁性**​|中|高|高|中|低|
|**可读性**​|中|高|高|中|低|
|**删除元素**​|✅ 可以|❌ 不可以|❌ 不可以|✅ 可以|✅ 可以|
|**添加元素**​|❌ 不可以|❌ 不可以|❌ 不可以|✅ 可以|✅ 可以|
|**修改元素**​|❌ 不可以|❌ 不可以|❌ 不可以|✅ 可以|✅ 可以|
|**获取索引**​|❌ 不可以|❌ 不可以|❌ 不可以|✅ 可以|✅ 可以|
|**双向遍历**​|❌ 不可以|❌ 不可以|❌ 不可以|❌ 不可以|✅ 可以|
|**性能**​|中|中|高（并行）|高（ArrayList）|中|
|**异常安全**​|高|高|中|低|高|
|**版本要求**​|Java 1.2+|Java 5+|Java 8+|所有版本|Java 1.2+|
|**适用集合**​|所有Collection|数组+集合|所有Collection|数组+List|仅List|

---

## 🎯 性能对比测试

```
import java.util.*;

public class TraversalPerformance {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < 1000000; i++) {
            list.add(i);
        }
        
        // 1. 普通for循环
        long start = System.currentTimeMillis();
        for (int i = 0; i < list.size(); i++) {
            int val = list.get(i);
        }
        long time1 = System.currentTimeMillis() - start;
        
        // 2. 增强for循环
        start = System.currentTimeMillis();
        for (int val : list) {
            // 空操作
        }
        long time2 = System.currentTimeMillis() - start;
        
        // 3. 迭代器
        start = System.currentTimeMillis();
        Iterator<Integer> it = list.iterator();
        while (it.hasNext()) {
            int val = it.next();
        }
        long time3 = System.currentTimeMillis() - start;
        
        // 4. Lambda
        start = System.currentTimeMillis();
        list.forEach(val -> {});
        long time4 = System.currentTimeMillis() - start;
        
        // 5. 并行流
        start = System.currentTimeMillis();
        list.parallelStream().forEach(val -> {});
        long time5 = System.currentTimeMillis() - start;
        
        System.out.println("普通for循环: " + time1 + "ms");
        System.out.println("增强for循环: " + time2 + "ms");
        System.out.println("迭代器: " + time3 + "ms");
        System.out.println("Lambda: " + time4 + "ms");
        System.out.println("并行流: " + time5 + "ms");
    }
}
```

**结果分析**（100万数据）：

- ArrayList：普通for循环最快，Lambda稍慢
    
- LinkedList：迭代器和增强for循环最快
    
- 大数据量：并行流有明显优势
    

---

## 💡 最佳实践建议

### 1. 选择策略

```
// 场景1：只读遍历 → 增强for循环
for (String item : list) {
    process(item);
}

// 场景2：需要索引 → 普通for循环
for (int i = 0; i < list.size(); i++) {
    list.set(i, process(list.get(i)));
}

// 场景3：需要删除元素 → 迭代器
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    if (shouldRemove(it.next())) {
        it.remove();
    }
}

// 场景4：Java 8+ 简洁遍历 → Lambda
list.forEach(System.out::println);

// 场景5：复杂列表操作 → ListIterator
ListIterator<String> lit = list.listIterator();
while (lit.hasNext()) {
    String item = lit.next();
    if (condition) {
        lit.add("new");
        lit.set("modified");
    }
}
```

### 2. 性能优化

```
// 1. ArrayList 使用普通for循环
// 2. LinkedList 使用迭代器或增强for循环
// 3. 大数据量考虑并行流
if (list.size() > 10000) {
    list.parallelStream().forEach(this::process);
} else {
    list.forEach(this::process);
}
```

### 3. 代码示例模板

```
import java.util.*;

public class TraversalExamples {
    
    // 模板1：安全删除元素
    public static void removeElements(List<String> list) {
        // 方法1：使用迭代器
        Iterator<String> iterator = list.iterator();
        while (iterator.hasNext()) {
            if (shouldRemove(iterator.next())) {
                iterator.remove();
            }
        }
        
        // 方法2：Java 8+ 使用 removeIf
        list.removeIf(this::shouldRemove);
        
        // 方法3：从后向前遍历
        for (int i = list.size() - 1; i >= 0; i--) {
            if (shouldRemove(list.get(i))) {
                list.remove(i);
            }
        }
    }
    
    // 模板2：带索引的遍历
    public static void processWithIndex(List<String> list) {
        // 方法1：普通for循环
        for (int i = 0; i < list.size(); i++) {
            System.out.println(i + ": " + list.get(i));
        }
        
        // 方法2：ListIterator
        ListIterator<String> lit = list.listIterator();
        while (lit.hasNext()) {
            int index = lit.nextIndex();
            String item = lit.next();
            System.out.println(index + ": " + item);
        }
        
        // 方法3：自定义计数器
        int index = 0;
        for (String item : list) {
            System.out.println(index++ + ": " + item);
        }
    }
    
    // 模板3：并行处理
    public static void parallelProcessing(List<String> list) {
        // 并行流处理
        list.parallelStream()
            .filter(s -> s != null)
            .map(String::toUpperCase)
            .forEach(System.out::println);
    }
    
    private static boolean shouldRemove(String s) {
        return s == null || s.isEmpty();
    }
}
```

---

## ⚠️ 注意事项

### 1. 并发修改异常

```
// 错误示例
for (String item : list) {
    if ("delete".equals(item)) {
        list.remove(item);  // ConcurrentModificationException
    }
}

// 正确做法
list.removeIf("delete"::equals);
```

### 2. 性能陷阱

```
// LinkedList 不要用普通for循环
for (int i = 0; i < linkedList.size(); i++) {
    linkedList.get(i);  // 每次get都是O(n)操作
}

// 应该用迭代器
for (String item : linkedList) {
    // O(1) 操作
}
```

### 3. 空指针处理

```
// 安全遍历
if (list != null) {
    for (String item : list) {
        if (item != null) {
            process(item);
        }
    }
}

// 或使用 Optional
Optional.ofNullable(list)
        .orElse(Collections.emptyList())
        .forEach(this::process);
```

---

## 📋 总结

### 1. 选择建议

- **简单只读**​ → 增强for循环
    
- **需要索引**​ → 普通for循环
    
- **需要删除**​ → 迭代器
    
- **现代代码**​ → Lambda表达式
    
- **复杂操作**​ → ListIterator
    
- **大数据量**​ → 并行流
    

### 2. 核心要点

1. 理解每种遍历方式的适用场景
    
2. 注意集合类型对性能的影响
    
3. 避免在遍历中修改集合（除非使用迭代器）
    
4. 考虑代码的可读性和维护性
    
5. 根据数据量选择合适的方式
    

### 3. 一句话建议

**"读用增强for，改要索引用普通for，删用迭代器，现代用Lambda，复杂用ListIterator。"**

掌握这五种遍历方式，能够让你在各种场景下选择最合适的方法，写出高效、可维护的Java代码。