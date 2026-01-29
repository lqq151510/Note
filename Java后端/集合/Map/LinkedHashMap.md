`LinkedHashMap`是 `HashMap`的子类，在保持 HashMap 高效查找的同时，**维护了一个双向链表来记录元素的顺序**。以下是其核心原理：

---

## 一、数据结构：数组 + 链表/红黑树 + 双向链表

```
// 继承 HashMap
class LinkedHashMap<K,V> extends HashMap<K,V> {
    
    // 双向链表的头尾节点
    transient LinkedHashMap.Entry<K,V> head; // 最老的节点
    transient LinkedHashMap.Entry<K,V> tail; // 最新的节点
    
    // Entry 节点继承自 HashMap.Node，并增加前后指针
    static class Entry<K,V> extends HashMap.Node<K,V> {
        Entry<K,V> before, after; // 双向链表指针
    }
}
```

---

## 二、两种顺序模式

### 1. **插入顺序（默认）**

- 按元素第一次插入的顺序排序
    
- `accessOrder = false`
    

### 2. **访问顺序（LRU 基础）**

- 每次访问（get/put）都会将元素移到链表末尾
    
- `accessOrder = true`
    
- 适合实现 LRU 缓存
    

---

## 三、关键方法实现

### 1. **节点创建**

```
// 覆盖 HashMap 的 newNode 方法
Node<K,V> newNode(int hash, K key, V value, Node<K,V> e) {
    LinkedHashMap.Entry<K,V> p = new Entry<>(hash, key, value, e);
    // 将新节点链接到链表末尾
    linkNodeLast(p);
    return p;
}
```

### 2. **访问回调**

```
// 在访问后调用（get/put 时）
void afterNodeAccess(Node<K,V> e) {
    if (accessOrder) { // 如果是访问顺序模式
        LinkedHashMap.Entry<K,V> last = tail;
        if (last != e) { // 如果当前节点不是尾节点
            // 从链表中移除 e，并放到链表末尾
            removeNodeFromLinkedList(e);
            linkNodeLast(e);
        }
    }
}
```

### 3. **删除回调**

```
// 在删除节点后调用
void afterNodeRemoval(Node<K,V> e) {
    // 从双向链表中断开 e 的前后连接
    LinkedHashMap.Entry<K,V> p = (Entry<K,V>)e;
    LinkedHashMap.Entry<K,V> b = p.before;
    LinkedHashMap.Entry<K,V> a = p.after;
    p.before = p.after = null;
    if (b == null) head = a;
    else b.after = a;
    if (a == null) tail = b;
    else a.before = b;
}
```

---

## 四、LRU 缓存实现示例

```
class LRUCache<K, V> extends LinkedHashMap<K, V> {
    private final int capacity;
    
    public LRUCache(int capacity) {
        super(capacity, 0.75f, true); // accessOrder = true
        this.capacity = capacity;
    }
    
    @Override
    protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
        return size() > capacity; // 超过容量时移除最老的节点
    }
}
```

---

## 五、与 HashMap 对比

|特性|HashMap|LinkedHashMap|
|---|---|---|
|顺序|无保证|插入顺序/访问顺序|
|数据结构|数组+链表/红黑树|数组+链表/红黑树+双向链表|
|迭代性能|随机|按链表顺序（O(n)）|
|内存占用|较低|较高（多两个指针）|

---

## 六、使用场景

1. **需要保持插入顺序**：如配置项读取
    
2. **实现 LRU 缓存**：通过 `accessOrder=true`+ `removeEldestEntry`
    
3. **需要可预测的迭代顺序**：避免 HashMap 的随机顺序
    

---

## 关键点总结

- **继承自 HashMap**，复用哈希表逻辑
    
- **通过双向链表维护顺序**，支持两种排序模式
    
- **访问顺序模式**可实现 LRU 缓存
    
- **线程不安全**（同 HashMap）
    
- **迭代性能稳定**，与容量无关（直接遍历链表）
    

LinkedHashMap 在需要顺序的 Map 场景中提供了 HashMap 的性能和可预测的迭代顺序。