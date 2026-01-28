# Map常用方法总结

## 一、Map核心特点回顾

- 键值对结构：`Key → Value`
    
- 键必须唯一，值可以重复
    
- 键不能重复，重复添加会覆盖
    

## 二、常用方法分类总结

### 1. **添加/修改操作**

```
// 添加键值对，key存在则覆盖
V put(K key, V value)

// 批量添加
void putAll(Map<? extends K, ? extends V> m)

// 如果key不存在才添加
V putIfAbsent(K key, V value)
```

### 2. **删除操作**

```
// 根据key删除
V remove(Object key)

// 根据key和value都匹配才删除
boolean remove(Object key, Object value)

// 清空所有元素
void clear()
```

### 3. **查询操作**

```
// 根据key获取value
V get(Object key)

// 获取key对应的value，不存在返回默认值
V getOrDefault(Object key, V defaultValue)

// 判断是否包含指定key
boolean containsKey(Object key)

// 判断是否包含指定value
boolean containsValue(Object value)

// 获取元素数量
int size()

// 判断是否为空
boolean isEmpty()
```

### 4. **遍历操作**

```
// 1. 获取所有key的Set集合
Set<K> keySet()

// 2. 获取所有value的Collection集合
Collection<V> values()

// 3. 获取所有键值对的Set集合
Set<Map.Entry<K, V>> entrySet()

// 4. Java 8+ Lambda遍历
void forEach(BiConsumer<? super K, ? super V> action)
```

### 5. **替换操作**

```
// 替换指定key的值
V replace(K key, V value)

// 当key和oldValue都匹配时才替换
boolean replace(K key, V oldValue, V newValue)

// 批量替换
void replaceAll(BiFunction<? super K, ? super V, ? extends V> function)
```

### 6. **比较和合并（Java 8+）**

```
// 如果key不存在，则计算value
V computeIfAbsent(K key, Function<? super K, ? extends V> mappingFunction)

// 如果key存在，则计算新value
V computeIfPresent(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction)

// 合并操作
V merge(K key, V value, BiFunction<? super V, ? super V, ? extends V> remappingFunction)
```

## 三、遍历方式对比

### 方式1：键找值（适合只需要key的场景）

```
for (K key : map.keySet()) {
    V value = map.get(key);
    // 处理逻辑
}
```

### 方式2：键值对遍历（最常用，效率高）

```
for (Map.Entry<K, V> entry : map.entrySet()) {
    K key = entry.getKey();
    V value = entry.getValue();
    // 处理逻辑
}
```

### 方式3：只遍历值

```
for (V value : map.values()) {
    // 处理逻辑
}
```

### 方式4：Lambda表达式（Java 8+）

```
map.forEach((key, value) -> {
    // 处理逻辑
});
```

### 方式5：迭代器

```
Iterator<Map.Entry<K, V>> iterator = map.entrySet().iterator();
while (iterator.hasNext()) {
    Map.Entry<K, V> entry = iterator.next();
    // 处理逻辑
    iterator.remove();  // 可以在遍历时安全删除
}
```

## 四、示例代码汇总

```
import java.util.*;

public class MapMethodsSummary {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        
        // 1. 添加操作
        map.put("Alice", 90);
        map.put("Bob", 85);
        map.put("Charlie", 92);
        
        // 2. 添加不存在的才添加
        map.putIfAbsent("Alice", 95);  // Alice已存在，不会改变
        map.putIfAbsent("David", 88);  // David不存在，会添加
        
        // 3. 获取操作
        System.out.println("Alice的分数: " + map.get("Alice"));
        System.out.println("不存在的key: " + map.get("Eve"));
        System.out.println("不存在的key(带默认值): " + 
                          map.getOrDefault("Eve", 60));
        
        // 4. 替换操作
        map.replace("Bob", 90);  // 改为90
        map.replace("Charlie", 92, 95);  // 只有原来是92才改为95
        
        // 5. 删除操作
        map.remove("David");  // 删除David
        map.remove("Bob", 85);  // 只有Bob=85才删除
        
        // 6. 批量操作
        Map<String, Integer> anotherMap = new HashMap<>();
        anotherMap.put("Eve", 78);
        anotherMap.put("Frank", 82);
        map.putAll(anotherMap);
        
        // 7. 遍历方式演示
        System.out.println("\n=== 遍历方式 ===");
        
        // 方式1：键找值
        System.out.println("键找值遍历:");
        for (String name : map.keySet()) {
            System.out.println(name + ": " + map.get(name));
        }
        
        // 方式2：键值对遍历
        System.out.println("\n键值对遍历:");
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }
        
        // 方式3：Lambda表达式
        System.out.println("\nLambda遍历:");
        map.forEach((name, score) -> 
            System.out.println(name + ": " + score));
        
        // 8. 判断操作
        System.out.println("\n=== 判断操作 ===");
        System.out.println("包含Alice? " + map.containsKey("Alice"));
        System.out.println("包含分数90? " + map.containsValue(90));
        System.out.println("元素数量: " + map.size());
        System.out.println("是否为空? " + map.isEmpty());
        
        // 9. Java 8+ 新特性
        System.out.println("\n=== Java 8+ 新特性 ===");
        
        // computeIfAbsent: 如果key不存在，则计算并添加
        map.computeIfAbsent("Grace", k -> 75);
        
        // computeIfPresent: 如果key存在，则重新计算
        map.computeIfPresent("Alice", (k, v) -> v + 5);
        
        // merge: 合并操作
        map.merge("Alice", 10, (oldValue, newValue) -> oldValue + newValue);
        
        // replaceAll: 批量替换
        map.replaceAll((name, score) -> score + 5);
        
        System.out.println("修改后:");
        map.forEach((k, v) -> System.out.println(k + ": " + v));
        
        // 10. 清空
        map.clear();
        System.out.println("清空后大小: " + map.size());
    }
}
```

## 五、使用建议

1. **选择合适遍历方式**
    
    - 需要key和value：用`entrySet()`或`forEach`
        
    - 只需要key：用`keySet()`
        
    - 只需要value：用`values()`
        
    - 需要修改集合：用迭代器
        
    
2. **线程安全**
    
    - 多线程环境用`ConcurrentHashMap`
        
    - 或`Collections.synchronizedMap()`
        
    
3. **性能考虑**
    
    - 预估数据量，设置合适初始容量
        
    - 自定义对象作为key时，正确重写`hashCode()`和`equals()`
        
    
4. **空值处理**
    
    - HashMap：允许null键和null值
        
    - TreeMap：不允许null键
        
    - ConcurrentHashMap：键值都不能为null