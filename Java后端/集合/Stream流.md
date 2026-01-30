# Stream API 总结

## 什么是Stream API？

Stream API是Java 8引入的一个重要特性，它提供了一种高效且易于使用的处理数据集合的方式。Stream API允许你以声明式方式处理数据集合，类似于SQL查询语句。

## Stream的核心特点

1. **不存储数据**：Stream不是数据结构，它不存储数据，而是对数据源（如集合、数组）的视图。
2. **不可变性**：Stream操作不会修改源数据，而是返回一个新的Stream。
3. **惰性求值**：中间操作是惰性的，只有在终端操作被调用时才会执行。
4. **可链式调用**：Stream操作可以链式调用，形成流畅的API。

## Stream操作类型

### 1. 中间操作（Intermediate Operations）

中间操作返回一个新的Stream，可以继续进行链式调用。

- `filter(Predicate)`：筛选元素
- `map(Function)`：转换元素
- `sorted(Comparator)`：排序
- `distinct()`：去重
- `limit(long)`：限制元素数量
- `skip(long)`：跳过元素
[Integer.parseInt](obsidian://deepseek-ai-assistant?id=1769771240473 "Open plugin:deepseek-ai-assistant")
### 2. 终端操作（Terminal Operations）

终端操作会触发Stream的处理，并返回结果或产生副作用。

- `collect(Collector)`：收集结果到集合
- `forEach(Consumer)`：遍历元素
- `count()`：计算元素数量
- `reduce(BinaryOperator)`：归约操作
- `min(Comparator)`：找出最小值
- `max(Comparator)`：找出最大值
- `findFirst()`：找出第一个元素
- `findAny()`：找出任意元素

## 常用Collectors

- `Collectors.toList()`：收集到List
- `Collectors.toSet()`：收集到Set
- `Collectors.toMap(keyMapper, valueMapper)`：收集到Map
- `Collectors.groupingBy(classifier)`：分组
- `Collectors.partitioningBy(predicate)`：分区
- `Collectors.joining()`：连接字符串
- `Collectors.counting()`：计数
- `Collectors.averagingInt/Double/Long()`：计算平均值
- `Collectors.summingInt/Double/Long()`：求和

## Stream创建方式

1. **从集合创建**：
    
    ![](http://localhost:63342/markdownPreview/444912345/untitled1/src/Stream)
    
    `List<String> list = Arrays.asList("a", "b", "c"); Stream<String> stream = list.stream();`
    
2. **从数组创建**：
    
    ![](http://localhost:63342/markdownPreview/444912345/untitled1/src/Stream)
    
    `String[] array = {"a", "b", "c"}; Stream<String> stream = Arrays.stream(array);`
    
3. **使用Stream.of()**：
    
    ![](http://localhost:63342/markdownPreview/444912345/untitled1/src/Stream)
    
    `Stream<String> stream = Stream.of("a", "b", "c");`
    
4. **使用Stream.generate()**：
    
    ![](http://localhost:63342/markdownPreview/444912345/untitled1/src/Stream)
    
    `Stream<String> stream = Stream.generate(() -> "element").limit(10);`
    
5. **使用Stream.iterate()**：
    
    ![](http://localhost:63342/markdownPreview/444912345/untitled1/src/Stream)
    
    `Stream<Integer> stream = Stream.iterate(0, n -> n + 2).limit(10);`
    

## Stream最佳实践

1. **使用方法引用简化Lambda表达式**：
    
    ![](http://localhost:63342/markdownPreview/444912345/untitled1/src/Stream)
    
    `// 使用Lambda list.stream().map(s -> s.toUpperCase()).collect(Collectors.toList()); // 使用方法引用 list.stream().map(String::toUpperCase).collect(Collectors.toList());`
    
2. **避免修改外部状态**：
    
    ![](http://localhost:63342/markdownPreview/444912345/untitled1/src/Stream)
    
    `// 不推荐：修改外部状态 List<Integer> result = new ArrayList<>(); list.stream().forEach(item -> result.add(item * 2)); // 推荐：使用collect List<Integer> result = list.stream()     .map(item -> item * 2)     .collect(Collectors.toList());`
    
3. **使用并行流处理大数据集**：
    
    ![](http://localhost:63342/markdownPreview/444912345/untitled1/src/Stream)
    
    `// 顺序流 list.stream().filter(predicate).collect(Collectors.toList()); // 并行流 list.parallelStream().filter(predicate).collect(Collectors.toList());`
    
4. **注意空指针异常**：
    
    ![](http://localhost:63342/markdownPreview/444912345/untitled1/src/Stream)
    
    `// 可能抛出NullPointerException list.stream().map(item -> item.getProperty().getName()).collect(Collectors.toList()); // 使用Optional避免 list.stream()     .map(item -> Optional.ofNullable(item.getProperty()))     .filter(Optional::isPresent)     .map(Optional::get)     .map(Property::getName)     .collect(Collectors.toList());`
    

## Stream性能考虑

1. **小数据集**：对于小数据集，传统for循环可能更高效。
2. **大数据集**：对于大数据集，Stream API通常更简洁，并行流可能提供更好的性能。
3. **避免过度使用**：不是所有场景都适合使用Stream，简单操作可能直接使用集合方法更高效。

## 常见问题与解决方案

1. **Stream只能使用一次**：
    
    ![](http://localhost:63342/markdownPreview/444912345/untitled1/src/Stream)
    
    `Stream<String> stream = list.stream(); List<String> result1 = stream.collect(Collectors.toList()); // 下面这行会抛出IllegalStateException: stream has already been operated upon or closed List<String> result2 = stream.collect(Collectors.toList());`
    
2. **Collectors.toMap的键冲突**：
    
    ![](http://localhost:63342/markdownPreview/444912345/untitled1/src/Stream)
    
    `// 当键冲突时会抛出IllegalStateException Map<String, String> map = list.stream()     .collect(Collectors.toMap(Item::getKey, Item::getValue)); // 解决方案：指定合并策略 Map<String, String> map = list.stream()     .collect(Collectors.toMap(         Item::getKey,          Item::getValue,         (existingValue, newValue) -> existingValue    ));`
    
3. **空Stream处理**：
    
    ![](http://localhost:63342/markdownPreview/444912345/untitled1/src/Stream)
    
    `// 可能返回null Optional<String> result = list.stream()     .filter(predicate)     .findFirst(); // 提供默认值 String result = list.stream()     .filter(predicate)     .findFirst()     .orElse("default");`
    

## 总结

Stream API为Java开发者提供了强大的数据处理能力，使代码更加简洁、易读。通过合理使用Stream API，可以大大提高代码的可读性和维护性。但在使用时也需要注意性能影响和潜在问题，选择合适的场景使用Stream API。