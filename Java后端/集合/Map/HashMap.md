# HashMap底层原理详解

## 一、HashMap数据结构演变

### 1. **JDK 1.7及以前：数组 + 链表**

```
索引位置: 数组
每个数组元素: 单向链表
```

### 2. **JDK 1.8及以后：数组 + 链表 + 红黑树**

```
索引位置: 数组
每个数组元素: 
  - 链表长度 ≤ 8: 单向链表
  - 链表长度 > 8 且 数组长度 ≥ 64: 红黑树
  - 链表长度 ≤ 6: 红黑树退化为链表
```

## 二、核心源码分析

### 1. **内部类定义**

```
// 1. 数组中的节点（链表节点）
static class Node<K,V> implements Map.Entry<K,V> {
    final int hash;          // 哈希值
    final K key;             // 键
    V value;                 // 值
    Node<K,V> next;          // 下一个节点
    
    // 构造方法...
    // hashCode()、equals()等方法...
}

// 2. 红黑树节点
static final class TreeNode<K,V> extends LinkedHashMap.Entry<K,V> {
    TreeNode<K,V> parent;    // 父节点
    TreeNode<K,V> left;      // 左子节点
    TreeNode<K,V> right;     // 右子节点
    TreeNode<K,V> prev;      // 前驱节点
    boolean red;            // 颜色
    
    // 树操作方法...
}
```

### 2. **关键字段**

```
public class HashMap<K,V> extends AbstractMap<K,V> 
    implements Map<K,V>, Cloneable, Serializable {
    
    // 默认初始容量：16
    static final int DEFAULT_INITIAL_CAPACITY = 1 << 4;
    
    // 最大容量：2^30
    static final int MAXIMUM_CAPACITY = 1 << 30;
    
    // 默认负载因子：0.75
    static final float DEFAULT_LOAD_FACTOR = 0.75f;
    
    // 链表转红黑树的阈值：8
    static final int TREEIFY_THRESHOLD = 8;
    
    // 红黑树转链表的阈值：6
    static final int UNTREEIFY_THRESHOLD = 6;
    
    // 最小树化容量：64
    static final int MIN_TREEIFY_CAPACITY = 64;
    
    // 存储元素的数组
    transient Node<K,V>[] table;
    
    // 键值对数量
    transient int size;
    
    // 修改次数（用于快速失败机制）
    transient int modCount;
    
    // 扩容阈值 = 容量 × 负载因子
    int threshold;
    
    // 负载因子
    final float loadFactor;
}
```

## 三、put操作详细流程

```
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}

final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
               boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    
    // 1. 如果数组为空，进行初始化
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
    
    // 2. 计算索引位置：(n-1) & hash
    if ((p = tab[i = (n - 1) & hash]) == null)
        // 3. 如果位置为空，直接创建新节点
        tab[i] = newNode(hash, key, value, null);
    else {
        Node<K,V> e; K k;
        // 4. 如果key已存在
        if (p.hash == hash && 
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
        // 5. 如果是红黑树节点
        else if (p instanceof TreeNode)
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        else {
            // 6. 遍历链表
            for (int binCount = 0; ; ++binCount) {
                if ((e = p.next) == null) {
                    // 7. 插入到链表末尾
                    p.next = newNode(hash, key, value, null);
                    // 8. 判断是否需要树化
                    if (binCount >= TREEIFY_THRESHOLD - 1)
                        treeifyBin(tab, hash);
                    break;
                }
                // 9. 找到相同key
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;
            }
        }
        
        // 10. 如果key已存在，更新value
        if (e != null) {
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;
            afterNodeAccess(e);
            return oldValue;
        }
    }
    
    ++modCount;
    // 11. 判断是否需要扩容
    if (++size > threshold)
        resize();
    afterNodeInsertion(evict);
    return null;
}
```

## 四、哈希计算和索引定位

### 1. **哈希扰动函数**

```
static final int hash(Object key) {
    int h;
    // 1. 获取key的hashCode
    // 2. 高16位与低16位异或，增加低位的随机性
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

**作用**：混合哈希码的高位和低位，增大低位的随机性，减少哈希冲突。

### 2. **计算索引位置**

```
int index = (table.length - 1) & hash;
```

**为什么用`(n-1) & hash`而不是`hash % n`？**

- 效率更高：位运算比取模快
    
- 要求n必须是2的幂：保证`n-1`的二进制是`111...1`
    

**示例**：

```
n = 16 (00010000)
n-1 = 15 (00001111)
hash = 25 (00011001)
index = 15 & 25 = 9 (00001001)
```

## 五、扩容机制（resize）

```
final Node<K,V>[] resize() {
    Node<K,V>[] oldTab = table;
    int oldCap = (oldTab == null) ? 0 : oldTab.length;
    int oldThr = threshold;
    int newCap, newThr = 0;
    
    // 1. 计算新容量和新阈值
    if (oldCap > 0) {
        if (oldCap >= MAXIMUM_CAPACITY) {
            threshold = Integer.MAX_VALUE;
            return oldTab;
        }
        // 容量扩大一倍
        else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                 oldCap >= DEFAULT_INITIAL_CAPACITY)
            newThr = oldThr << 1;  // 阈值扩大一倍
    }
    
    // 2. 创建新数组
    Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
    table = newTab;
    
    // 3. 迁移数据
    if (oldTab != null) {
        for (int j = 0; j < oldCap; ++j) {
            Node<K,V> e;
            if ((e = oldTab[j]) != null) {
                oldTab[j] = null;
                if (e.next == null)
                    // 单个节点，直接重新计算位置
                    newTab[e.hash & (newCap - 1)] = e;
                else if (e instanceof TreeNode)
                    // 红黑树节点
                    ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                else {
                    // 链表节点
                    Node<K,V> loHead = null, loTail = null;
                    Node<K,V> hiHead = null, hiTail = null;
                    Node<K,V> next;
                    do {
                        next = e.next;
                        // 关键：判断节点在新数组中的位置
                        if ((e.hash & oldCap) == 0) {
                            // 位置不变
                            if (loTail == null)
                                loHead = e;
                            else
                                loTail.next = e;
                            loTail = e;
                        } else {
                            // 位置 = 原位置 + oldCap
                            if (hiTail == null)
                                hiHead = e;
                            else
                                hiTail.next = e;
                            hiTail = e;
                        }
                    } while ((e = next) != null);
                    
                    if (loTail != null) {
                        loTail.next = null;
                        newTab[j] = loHead;
                    }
                    if (hiTail != null) {
                        hiTail.next = null;
                        newTab[j + oldCap] = hiHead;
                    }
                }
            }
        }
    }
    return newTab;
}
```

## 六、树化条件

### 链表转红黑树（treeifyBin）

```
final void treeifyBin(Node<K,V>[] tab, int hash) {
    int n, index; Node<K,V> e;
    // 1. 数组长度小于64，优先扩容而不是树化
    if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY)
        resize();
    // 2. 链表长度 >= 8 且 数组长度 >= 64，才进行树化
    else if ((e = tab[index = (n - 1) & hash]) != null) {
        TreeNode<K,V> hd = null, tl = null;
        do {
            TreeNode<K,V> p = replacementTreeNode(e, null);
            if (tl == null)
                hd = p;
            else {
                p.prev = tl;
                tl.next = p;
            }
            tl = p;
        } while ((e = e.next) != null);
        
        if ((tab[index] = hd) != null)
            hd.treeify(tab);
    }
}
```

## 七、关键点总结

### 1. **为什么容量必须是2的幂？**

- 方便通过位运算`(n-1) & hash`计算索引
    
- 扩容时元素迁移高效：元素要么在原位置，要么在原位置+原容量
    
- 判断条件：`(e.hash & oldCap) == 0`
    

### 2. **负载因子为什么是0.75？**

- 空间与时间的平衡：
    
    - 太小（如0.5）：空间利用率低，浪费内存
        
    - 太大（如1.0）：哈希冲突增加，链表变长，查询变慢
        
    
- 统计学计算得到的最优值
    

### 3. **树化阈值为什么是8？**

- 基于泊松分布：哈希冲突到8的概率约为0.00000006
    
- 链表长度达到8的概率极低，如果真的达到说明哈希函数有问题
    
- 此时转换为红黑树（O(log n)）比链表（O(n)）效率高
    

### 4. **退化阈值为什么是6？**

- 避免频繁的树化和退化
    
- 设置缓冲值2，防止在8附近频繁转换
    

## 八、性能分析

|操作|最好情况|最坏情况|平均情况|
|---|---|---|---|
|插入|O(1)|O(log n)|O(1)|
|查询|O(1)|O(log n)|O(1)|
|删除|O(1)|O(log n)|O(1)|

**注意**：最坏情况发生在所有元素都哈希冲突，形成链表或红黑树。

## 九、线程安全问题

### 1. **JDK 1.7的问题**

- 多线程扩容可能导致**死循环**（链表形成环）
    
- 数据丢失
    

### 2. **JDK 1.8的改进**

- 修复了死循环问题
    
- 但仍存在**数据覆盖**等线程安全问题
    
- 多线程环境应该用`ConcurrentHashMap`
    

## 十、使用建议

1. **设置合适初始容量**
    

```
// 预期存储100个元素
int initialCapacity = (int)(100 / 0.75) + 1;  // 134
Map<String, String> map = new HashMap<>(134);
```

1. **重写hashCode()和equals()**
    

```
class KeyObject {
    private int id;
    private String name;
    
    @Override
    public int hashCode() {
        return Objects.hash(id, name);
    }
    
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        KeyObject that = (KeyObject) obj;
        return id == that.id && Objects.equals(name, that.name);
    }
}
```

1. **选择合适的数据结构**
    
    - 单线程：HashMap
        
    - 多线程：ConcurrentHashMap
        
    - 有序：LinkedHashMap
        
    - 排序：TreeMap