# TreeSet集合底层原理详解

TreeSet是Java集合框架中一个基于**红黑树(Red-Black Tree)**实现的NavigableSet接口实现，它能够对元素进行**自然排序**或根据提供的Comparator进行排序。

## 一、核心数据结构

### 1.1 底层依赖TreeMap

```
public class TreeSet<E> extends AbstractSet<E>
    implements NavigableSet<E>, Cloneable, java.io.Serializable {
    
    // 底层使用TreeMap存储元素
    private transient NavigableMap<E, Object> m;
    
    // 虚拟的常量值，用于填充TreeMap的value
    private static final Object PRESENT = new Object();
    
    // 构造方法
    public TreeSet() {
        this(new TreeMap<E, Object>()); // 默认使用自然排序的TreeMap
    }
    
    // 添加元素的核心方法
    public boolean add(E e) {
        return m.put(e, PRESENT) == null;
    }
}
```

**核心结论**：TreeSet的底层实际上是一个**TreeMap**，所有元素作为TreeMap的key存储，value统一为PRESENT常量。

## 二、红黑树数据结构

### 2.1 红黑树特性

红黑树是一种自平衡的二叉查找树，具有以下特性：

1. **节点颜色**：每个节点是红色或黑色
    
2. **根节点**：根节点必须是黑色
    
3. **叶子节点**：所有叶子节点（NIL节点）都是黑色
    
4. **红色节点规则**：红色节点的子节点必须是黑色（不能有连续的红色节点）
    
5. **路径规则**：从任一节点到其每个叶子节点的所有路径都包含相同数目的黑色节点
    

### 2.2 TreeMap中的Entry结构

```
static final class Entry<K,V> implements Map.Entry<K,V> {
    K key;          // 存储的元素（TreeSet的value）
    V value;        // 固定的PRESENT值
    Entry<K,V> left;    // 左子节点
    Entry<K,V> right;   // 右子节点
    Entry<K,V> parent;  // 父节点
    boolean color = BLACK; // 节点颜色
    
    // 构造方法
    Entry(K key, V value, Entry<K,V> parent) {
        this.key = key;
        this.value = value;
        this.parent = parent;
    }
}
```

## 三、排序机制

### 3.1 两种排序方式

```
// 1. 自然排序：元素必须实现Comparable接口
TreeSet<String> naturalSet = new TreeSet<>();
naturalSet.add("apple");
naturalSet.add("banana");
naturalSet.add("cherry");

// 2. 定制排序：提供Comparator比较器
TreeSet<String> customSet = new TreeSet<>((a, b) -> b.compareTo(a)); // 逆序
customSet.add("apple");
customSet.add("banana");
```

### 3.2 排序规则的执行时机

当添加元素时，TreeSet通过比较器确定元素在红黑树中的位置：

```
public boolean add(E e) {
    return m.put(e, PRESENT) == null;
}

// TreeMap的put方法核心逻辑
final int compare(Object k1, Object k2) {
    return comparator == null ? 
        ((Comparable<? super K>)k1).compareTo((K)k2) : // 自然排序
        comparator.compare((K)k1, (K)k2);              // 定制排序
}
```

## 四、核心操作原理

### 4.1 添加元素流程

```
public boolean add(E e) {
    return m.put(e, PRESENT) == null;
}
```

**详细步骤**：

1. **比较定位**：从根节点开始，使用比较器比较新元素与当前节点
    
2. **查找位置**：
    
    - 如果新元素 < 当前节点，转向左子树
        
    - 如果新元素 > 当前节点，转向右子树
        
    - 如果相等，不插入（保证唯一性）
        
    
3. **插入节点**：找到合适位置后创建新节点（初始为红色）
    
4. **重新平衡**：通过旋转和变色操作维持红黑树特性
    

### 4.2 查找元素流程

```
public boolean contains(Object o) {
    return m.containsKey(o);
}
```

**查找过程**（二叉查找树原理）：

```
// 类似于二分查找，时间复杂度O(log n)
while (current != null) {
    int cmp = compare(o, current.key);
    if (cmp < 0) {
        current = current.left;    // 向左子树查找
    } else if (cmp > 0) {
        current = current.right;   // 向右子树查找
    } else {
        return true;               // 找到元素
    }
}
return false; // 未找到
```

### 4.3 删除元素流程

删除操作是红黑树中最复杂的操作，涉及多种情况的处理：

1. **查找节点**：定位要删除的节点
    
2. **替代节点**：找到中序遍历下的后继节点
    
3. **颜色调整**：根据节点颜色进行不同的平衡操作
    
4. **旋转修复**：通过左旋/右旋恢复红黑树性质
    

## 五、性能特点

### 5.1 时间复杂度对比

|操作|TreeSet|HashSet|LinkedHashSet|
|---|---|---|---|
|添加(add)|O(log n)|O(1)|O(1)|
|删除(remove)|O(log n)|O(1)|O(1)|
|查找(contains)|O(log n)|O(1)|O(1)|
|遍历迭代|O(n)|O(n)|O(n)|
|获取首尾元素|O(log n)|不支持|不支持|

### 5.2 空间复杂度

- **平均情况**：O(n)
    
- **每个节点**需要存储左右子节点指针、父节点指针和颜色标志
    

## 六、特殊功能方法

由于实现了NavigableSet接口，TreeSet提供了丰富的导航方法：

```
TreeSet<Integer> set = new TreeSet<>();
set.addAll(Arrays.asList(10, 20, 30, 40, 50));

// 范围查询
System.out.println(set.headSet(30));        // [10, 20]       小于30的元素
System.out.println(set.tailSet(30));        // [30, 40, 50]   大于等于30的元素
System.out.println(set.subSet(20, 40));    // [20, 30]       20到40之间的元素

// 导航方法
System.out.println(set.ceiling(25));        // 30     大于等于25的最小元素
System.out.println(set.floor(25));          // 20     小于等于25的最大元素
System.out.println(set.higher(30));         // 40     严格大于30的最小元素
System.out.println(set.lower(30));          // 20     严格小于30的最大元素

// 首尾元素
System.out.println(set.first());            // 10
System.out.println(set.last());             // 50
```

## 七、使用注意事项

### 7.1 元素必须可比较

```
// 错误示例：未实现Comparable接口的类
class Person {
    String name;
    int age;
}

TreeSet<Person> set = new TreeSet<>(); // 运行时抛出ClassCastException
set.add(new Person("Alice", 25));

// 正确方式1：实现Comparable接口
class Person implements Comparable<Person> {
    String name;
    int age;
    
    @Override
    public int compareTo(Person other) {
        return Integer.compare(this.age, other.age);
    }
}

// 正确方式2：提供Comparator
TreeSet<Person> set = new TreeSet<>(
    Comparator.comparing(Person::getName).thenComparingInt(Person::getAge)
);
```

### 7.2 排序一致性

**重要原则**：compareTo或compare方法必须与equals方法保持一致

```
class Product implements Comparable<Product> {
    String id;
    String name;
    
    // 错误：排序逻辑与equals不一致
    @Override
    public int compareTo(Product other) {
        return this.name.compareTo(other.name); // 只按name排序
    }
    
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (!(obj instanceof Product)) return false;
        Product other = (Product) obj;
        return Objects.equals(id, other.id) && Objects.equals(name, other.name);
    }
    
    // 会导致TreeSet认为两个不同id但同name的对象"相等"，从而拒绝插入
}
```

## 八、与HashSet的对比选择

### 8.1 选择依据

|场景|推荐选择|理由|
|---|---|---|
|需要自动排序|**TreeSet**​|提供自然顺序或定制顺序|
|需要最高性能的查找|**HashSet**​|O(1)时间复杂度|
|需要插入顺序|**LinkedHashSet**​|维护插入迭代顺序|
|需要范围查询|**TreeSet**​|提供headSet、tailSet等方法|
|内存敏感|**HashSet**​|TreeSet每个节点需要额外指针|

### 8.2 性能测试示例

```
// 大量数据插入性能对比
Set<Integer> hashSet = new HashSet<>();
Set<Integer> treeSet = new TreeSet<>();

long start = System.nanoTime();
for (int i = 0; i < 1000000; i++) {
    hashSet.add(i);
}
long hashTime = System.nanoTime() - start;

start = System.nanoTime();
for (int i = 0; i < 1000000; i++) {
    treeSet.add(i);
}
long treeTime = System.nanoTime() - start;

System.out.println("HashSet插入时间: " + hashTime / 1000000 + "ms");
System.out.println("TreeSet插入时间: " + treeTime / 1000000 + "ms");
```

## 总结

TreeSet的核心特点是：

1. **底层结构**：基于红黑树实现的自平衡二叉查找树
    
2. **排序特性**：元素自动排序（自然排序或定制排序）
    
3. **性能特点**：所有操作的时间复杂度为O(log n)
    
4. **特殊功能**：提供丰富的导航和范围查询方法
    
5. **使用约束**：元素必须可比较，排序逻辑需与equals一致
    

TreeSet在需要有序存储、范围查询或导航操作的场景下非常有用，但在纯查找性能要求极高的场景下，HashSet可能是更好的选择。