# Java LinkedList 详解与示例

## 📋 LinkedList 概述

**LinkedList**​ 是 Java 集合框架中的一个双向链表实现，位于 `java.util`包中。它实现了 `List`和 `Deque`接口，既可以作为列表使用，也可以作为队列或双端队列使用。

### 核心特点：

- 基于**双向链表**实现
    
- 非连续内存存储
    
- 插入和删除操作效率高
    
- 随机访问效率低
    
- 实现了 `List`和 `Deque`接口
    
- 允许 null 元素
    
- 非线程安全
    

---

## 🔗 LinkedList 内部结构

### 1. 节点结构

```
// LinkedList 内部节点类
private static class Node<E> {
    E item;         // 存储的数据
    Node<E> next;   // 指向下一个节点
    Node<E> prev;   // 指向上一个节点
    
    Node(Node<E> prev, E element, Node<E> next) {
        this.item = element;
        this.next = next;
        this.prev = prev;
    }
}
```

### 2. 链表结构

```
first ↔ node1 ↔ node2 ↔ node3 ↔ last
  ↑        ↑        ↑        ↑
 头节点   节点1    节点2    尾节点
```

### 3. 核心字段

```
public class LinkedList<E> {
    transient int size = 0;          // 链表长度
    transient Node<E> first;         // 头节点
    transient Node<E> last;          // 尾节点
    
    // 构造方法
    public LinkedList() { }          // 空链表
    
    public LinkedList(Collection<? extends E> c) {
        this();
        addAll(c);
    }
}
```

---

## 🔧 创建 LinkedList

### 基本创建方式

```
import java.util.LinkedList;
import java.util.Arrays;
import java.util.List;

public class LinkedListCreation {
    public static void main(String[] args) {
        // 1. 空链表
        LinkedList<String> list1 = new LinkedList<>();
        
        // 2. 从现有集合创建
        List<String> arrayList = Arrays.asList("A", "B", "C");
        LinkedList<String> list2 = new LinkedList<>(arrayList);
        
        // 3. 通过List接口引用
        List<Integer> list3 = new LinkedList<>();
        
        // 4. 作为队列/双端队列
        LinkedList<String> deque = new LinkedList<>();
        
        System.out.println("list1: " + list1);  // []
        System.out.println("list2: " + list2);  // [A, B, C]
    }
}
```

---

## 📊 LinkedList 常用方法

### 1. 添加元素

```
import java.util.LinkedList;

public class LinkedListAdd {
    public static void main(String[] args) {
        LinkedList<String> list = new LinkedList<>();
        
        // 1. 末尾添加
        list.add("A");                    // [A]
        list.add("B");                    // [A, B]
        list.addLast("C");                // [A, B, C]
        list.offer("D");                  // [A, B, C, D] (队列方法)
        
        // 2. 头部添加
        list.addFirst("Start");           // [Start, A, B, C, D]
        list.offerFirst("First");         // [First, Start, A, B, C, D]
        
        // 3. 指定位置插入
        list.add(2, "Insert");            // [First, Start, Insert, A, B, C, D]
        
        // 4. 批量添加
        list.addAll(Arrays.asList("X", "Y", "Z"));  // 末尾添加
        list.addAll(1, Arrays.asList("1", "2"));    // 指定位置添加
        
        System.out.println("添加后: " + list);
    }
}
```

### 2. 获取元素

```
import java.util.LinkedList;

public class LinkedListGet {
    public static void main(String[] args) {
        LinkedList<String> list = new LinkedList<>(
            Arrays.asList("A", "B", "C", "D", "E")
        );
        
        // 1. 获取头元素
        String first = list.getFirst();       // A
        String peek = list.peek();            // A (不删除)
        String element = list.element();      // A (不删除)
        
        // 2. 获取尾元素
        String last = list.getLast();         // E
        String peekLast = list.peekLast();    // E (不删除)
        
        // 3. 通过索引获取（效率低）
        String third = list.get(2);           // C
        // 内部实现：从离索引最近的一端开始遍历
        
        // 4. 获取但不删除
        System.out.println("peek: " + list.peek());        // A
        System.out.println("peekLast: " + list.peekLast());// E
        
        // 列表不变：[A, B, C, D, E]
    }
}
```

### 3. 删除元素

```
import java.util.LinkedList;
import java.util.Arrays;

public class LinkedListRemove {
    public static void main(String[] args) {
        LinkedList<String> list = new LinkedList<>(
            Arrays.asList("A", "B", "C", "D", "E", "B", "F")
        );
        
        System.out.println("原始列表: " + list);
        
        // 1. 删除头元素
        String removedFirst = list.removeFirst();  // 删除并返回 A
        String poll = list.poll();                 // 删除并返回 B
        String pollFirst = list.pollFirst();       // 删除并返回 C
        
        // 2. 删除尾元素
        String removedLast = list.removeLast();    // 删除并返回 F
        String pollLast = list.pollLast();         // 删除并返回 B
        
        // 3. 删除指定元素
        boolean removed = list.remove("D");        // 删除第一个 D
        list.removeFirstOccurrence("B");           // 删除第一次出现的 B
        list.removeLastOccurrence("E");            // 删除最后一次出现的 E
        
        // 4. 按索引删除
        String removedByIndex = list.remove(0);    // 删除索引0的元素
        
        // 5. 清空列表
        list.clear();
        
        System.out.println("删除后列表: " + list);
    }
}
```

### 4. 队列操作

```
import java.util.LinkedList;
import java.util.Queue;

public class LinkedListQueue {
    public static void main(String[] args) {
        // 作为队列使用
        Queue<String> queue = new LinkedList<>();
        
        // 入队
        queue.offer("Task1");
        queue.offer("Task2");
        queue.offer("Task3");
        queue.add("Task4");  // 如果队列已满会抛出异常
        
        System.out.println("队列: " + queue);
        
        // 查看队头
        System.out.println("队头: " + queue.peek());  // Task1
        
        // 出队
        while (!queue.isEmpty()) {
            String task = queue.poll();
            System.out.println("处理: " + task);
        }
        
        // 队列为空时的行为
        System.out.println("空队列poll: " + queue.poll());  // null
        // System.out.println("空队列remove: " + queue.remove());  // 抛出异常
    }
}
```

### 5. 双端队列操作

```
import java.util.LinkedList;
import java.util.Deque;

public class LinkedListDeque {
    public static void main(String[] args) {
        // 作为双端队列使用
        Deque<String> deque = new LinkedList<>();
        
        // 头部操作
        deque.addFirst("First1");
        deque.offerFirst("First2");
        deque.push("First3");  // 等同于 addFirst
        
        // 尾部操作
        deque.addLast("Last1");
        deque.offerLast("Last2");
        deque.add("Last3");    // 等同于 addLast
        
        System.out.println("双端队列: " + deque);
        
        // 查看两端
        System.out.println("头部: " + deque.peekFirst());  // First3
        System.out.println("尾部: " + deque.peekLast());   // Last3
        
        // 从头部移除
        System.out.println("pop: " + deque.pop());         // First3
        System.out.println("pollFirst: " + deque.pollFirst()); // First2
        
        // 从尾部移除
        System.out.println("pollLast: " + deque.pollLast());   // Last3
        System.out.println("removeLast: " + deque.removeLast()); // Last2
        
        System.out.println("剩余: " + deque);
    }
}
```

### 6. 栈操作

```
import java.util.LinkedList;

public class LinkedListStack {
    public static void main(String[] args) {
        // 作为栈使用（LIFO）
        LinkedList<String> stack = new LinkedList<>();
        
        // 入栈
        stack.push("Item1");
        stack.push("Item2");
        stack.push("Item3");
        
        System.out.println("栈: " + stack);
        
        // 查看栈顶
        System.out.println("栈顶: " + stack.peek());  // Item3
        
        // 出栈
        while (!stack.isEmpty()) {
            String item = stack.pop();
            System.out.println("弹出: " + item);
        }
        
        // 栈为空时的行为
        System.out.println("空栈peek: " + stack.peek());  // null
        // System.out.println("空栈pop: " + stack.pop());  // 抛出异常
    }
}
```

### 7. 遍历操作

```
import java.util.LinkedList;
import java.util.Iterator;
import java.util.ListIterator;
import java.util.Arrays;

public class LinkedListTraversal {
    public static void main(String[] args) {
        LinkedList<String> list = new LinkedList<>(
            Arrays.asList("A", "B", "C", "D", "E")
        );
        
        System.out.println("=== 1. 普通for循环（不推荐，效率低）===");
        for (int i = 0; i < list.size(); i++) {
            // 每次get(i)都是O(n)操作
            System.out.println("索引 " + i + ": " + list.get(i));
        }
        
        System.out.println("\n=== 2. 增强for循环（推荐）===");
        for (String item : list) {
            System.out.println("元素: " + item);
        }
        
        System.out.println("\n=== 3. 迭代器遍历 ===");
        Iterator<String> iterator = list.iterator();
        while (iterator.hasNext()) {
            String item = iterator.next();
            System.out.println("迭代器: " + item);
            if ("C".equals(item)) {
                iterator.remove();  // 安全删除
            }
        }
        
        System.out.println("删除C后: " + list);
        
        System.out.println("\n=== 4. ListIterator 双向遍历 ===");
        ListIterator<String> listIterator = list.listIterator();
        
        // 向前遍历
        System.out.println("向前遍历:");
        while (listIterator.hasNext()) {
            String item = listIterator.next();
            System.out.println("下一个: " + item);
        }
        
        // 向后遍历
        System.out.println("\n向后遍历:");
        while (listIterator.hasPrevious()) {
            String item = listIterator.previous();
            System.out.println("上一个: " + item);
        }
        
        System.out.println("\n=== 5. forEach（Java 8+）===");
        list.forEach(item -> System.out.println("forEach: " + item));
        
        System.out.println("\n=== 6. 并行流遍历 ===");
        list.parallelStream().forEach(item -> 
            System.out.println("并行: " + item + " - " + Thread.currentThread().getName())
        );
    }
}
```

### 8. 查找和检查

```
import java.util.LinkedList;
import java.util.Arrays;

public class LinkedListSearch {
    public static void main(String[] args) {
        LinkedList<String> list = new LinkedList<>(
            Arrays.asList("A", "B", "C", "D", "E", "A", "B")
        );
        
        // 1. 检查包含
        boolean hasA = list.contains("A");          // true
        boolean hasZ = list.contains("Z");          // false
        
        // 2. 查找索引
        int firstA = list.indexOf("A");            // 0
        int lastA = list.lastIndexOf("A");         // 5
        int firstZ = list.indexOf("Z");            // -1（不存在）
        
        // 3. 检查空
        boolean isEmpty = list.isEmpty();          // false
        int size = list.size();                    // 7
        
        // 4. 转换为数组
        Object[] array1 = list.toArray();
        String[] array2 = list.toArray(new String[0]);
        String[] array3 = list.toArray(new String[10]);  // 指定大小
        
        // 5. 子列表
        LinkedList<String> subList = new LinkedList<>(list.subList(1, 4));
        // 注意：subList返回的是视图，修改会影响原列表
        
        System.out.println("包含A吗? " + hasA);
        System.out.println("第一个A的索引: " + firstA);
        System.out.println("最后一个A的索引: " + lastA);
        System.out.println("列表大小: " + size);
    }
}
```

### 9. 批量操作

```
import java.util.LinkedList;
import java.util.Arrays;

public class LinkedListBatch {
    public static void main(String[] args) {
        LinkedList<String> list1 = new LinkedList<>(
            Arrays.asList("A", "B", "C", "D")
        );
        LinkedList<String> list2 = new LinkedList<>(
            Arrays.asList("C", "D", "E", "F")
        );
        
        // 1. 保留交集
        list1.retainAll(list2);
        System.out.println("交集: " + list1);  // [C, D]
        
        // 重置
        list1 = new LinkedList<>(Arrays.asList("A", "B", "C", "D"));
        
        // 2. 删除交集
        list1.removeAll(list2);
        System.out.println("差集(list1 - list2): " + list1);  // [A, B]
        
        // 3. 添加所有
        list1.addAll(list2);
        System.out.println("并集: " + list1);  // [A, B, C, D, E, F]
        
        // 4. 包含所有
        boolean containsAll = list1.containsAll(Arrays.asList("A", "B"));
        System.out.println("包含A和B吗? " + containsAll);  // true
    }
}
```

---

## 🎯 实际应用示例

### 示例1：LRU缓存实现

```
import java.util.LinkedHashMap;
import java.util.LinkedList;
import java.util.Map;

class LRUCache<K, V> {
    private final int capacity;
    private final LinkedList<K> accessOrder;
    private final Map<K, V> cache;
    
    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.accessOrder = new LinkedList<>();
        this.cache = new LinkedHashMap<>();
    }
    
    public V get(K key) {
        if (cache.containsKey(key)) {
            // 移动到链表头部
            accessOrder.remove(key);
            accessOrder.addFirst(key);
            return cache.get(key);
        }
        return null;
    }
    
    public void put(K key, V value) {
        if (cache.size() >= capacity && !cache.containsKey(key)) {
            // 移除最近最少使用的
            K lruKey = accessOrder.removeLast();
            cache.remove(lruKey);
        }
        
        if (cache.containsKey(key)) {
            accessOrder.remove(key);
        }
        
        accessOrder.addFirst(key);
        cache.put(key, value);
    }
    
    public void display() {
        System.out.println("访问顺序: " + accessOrder);
        System.out.println("缓存内容: " + cache);
    }
}

public class LRUCacheExample {
    public static void main(String[] args) {
        LRUCache<String, String> cache = new LRUCache<>(3);
        
        cache.put("1", "A");
        cache.put("2", "B");
        cache.put("3", "C");
        cache.display();
        
        cache.get("1");  // 访问1
        cache.display();
        
        cache.put("4", "D");  // 容量已满，移除2
        cache.display();
    }
}
```

### 示例2：任务调度器

```
import java.util.LinkedList;
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.TimeUnit;

class TaskScheduler {
    private final LinkedList<Runnable> taskQueue = new LinkedList<>();
    private final ScheduledExecutorService scheduler = 
        Executors.newScheduledThreadPool(1);
    private boolean isRunning = false;
    
    public void addTask(Runnable task) {
        synchronized (taskQueue) {
            taskQueue.addLast(task);
            if (!isRunning) {
                isRunning = true;
                scheduleNextTask();
            }
        }
    }
    
    private void scheduleNextTask() {
        scheduler.schedule(() -> {
            Runnable task = null;
            synchronized (taskQueue) {
                if (!taskQueue.isEmpty()) {
                    task = taskQueue.removeFirst();
                } else {
                    isRunning = false;
                }
            }
            
            if (task != null) {
                try {
                    task.run();
                } finally {
                    scheduleNextTask();
                }
            }
        }, 1, TimeUnit.SECONDS);
    }
    
    public void shutdown() {
        scheduler.shutdown();
    }
}

public class TaskSchedulerExample {
    public static void main(String[] args) throws InterruptedException {
        TaskScheduler scheduler = new TaskScheduler();
        
        for (int i = 1; i <= 5; i++) {
            final int taskId = i;
            scheduler.addTask(() -> {
                System.out.println("执行任务 " + taskId + " - " + 
                    Thread.currentThread().getName());
            });
        }
        
        Thread.sleep(10000);
        scheduler.shutdown();
    }
}
```

### 示例3：浏览器历史记录

```
import java.util.LinkedList;

class BrowserHistory {
    private final LinkedList<String> history = new LinkedList<>();
    private int currentIndex = -1;
    private final int maxSize = 50;
    
    public void visit(String url) {
        // 移除当前索引之后的历史记录
        while (history.size() > currentIndex + 1) {
            history.removeLast();
        }
        
        history.add(url);
        currentIndex++;
        
        // 限制历史记录大小
        if (history.size() > maxSize) {
            history.removeFirst();
            currentIndex--;
        }
    }
    
    public String back() {
        if (currentIndex > 0) {
            currentIndex--;
            return history.get(currentIndex);
        }
        return null;
    }
    
    public String forward() {
        if (currentIndex < history.size() - 1) {
            currentIndex++;
            return history.get(currentIndex);
        }
        return null;
    }
    
    public void displayHistory() {
        System.out.println("浏览历史:");
        for (int i = 0; i < history.size(); i++) {
            String prefix = (i == currentIndex) ? "-> " : "   ";
            System.out.println(prefix + i + ": " + history.get(i));
        }
    }
}

public class BrowserHistoryExample {
    public static void main(String[] args) {
        BrowserHistory browser = new BrowserHistory();
        
        browser.visit("google.com");
        browser.visit("github.com");
        browser.visit("stackoverflow.com");
        browser.displayHistory();
        
        System.out.println("\n后退: " + browser.back());
        browser.displayHistory();
        
        System.out.println("\n前进: " + browser.forward());
        browser.displayHistory();
        
        browser.visit("leetcode.com");  // 创建新分支
        browser.displayHistory();
    }
}
```

### 示例4：多项式相加

```
import java.util.LinkedList;

class Polynomial {
    static class Term {
        int coefficient;
        int exponent;
        
        Term(int coefficient, int exponent) {
            this.coefficient = coefficient;
            this.exponent = exponent;
        }
    }
    
    private LinkedList<Term> terms = new LinkedList<>();
    
    public void addTerm(int coefficient, int exponent) {
        terms.add(new Term(coefficient, exponent));
    }
    
    public Polynomial add(Polynomial other) {
        Polynomial result = new Polynomial();
        LinkedList<Term> list1 = new LinkedList<>(this.terms);
        LinkedList<Term> list2 = new LinkedList<>(other.terms);
        
        while (!list1.isEmpty() && !list2.isEmpty()) {
            Term t1 = list1.peek();
            Term t2 = list2.peek();
            
            if (t1.exponent > t2.exponent) {
                result.terms.add(list1.poll());
            } else if (t1.exponent < t2.exponent) {
                result.terms.add(list2.poll());
            } else {
                int sumCoeff = t1.coefficient + t2.coefficient;
                if (sumCoeff != 0) {
                    result.terms.add(new Term(sumCoeff, t1.exponent));
                }
                list1.poll();
                list2.poll();
            }
        }
        
        result.terms.addAll(list1);
        result.terms.addAll(list2);
        
        return result;
    }
    
    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder();
        for (Term term : terms) {
            if (sb.length() > 0 && term.coefficient > 0) {
                sb.append("+");
            }
            sb.append(term.coefficient)
              .append("x^")
              .append(term.exponent)
              .append(" ");
        }
        return sb.toString();
    }
}

public class PolynomialExample {
    public static void main(String[] args) {
        Polynomial p1 = new Polynomial();
        p1.addTerm(3, 2);
        p1.addTerm(2, 1);
        p1.addTerm(5, 0);
        
        Polynomial p2 = new Polynomial();
        p2.addTerm(2, 3);
        p2.addTerm(-2, 1);
        p2.addTerm(3, 0);
        
        System.out.println("多项式1: " + p1);
        System.out.println("多项式2: " + p2);
        
        Polynomial sum = p1.add(p2);
        System.out.println("相加结果: " + sum);
    }
}
```

---

## ⚡ 性能优化建议

### 1. 避免使用 get(index)

```
// 错误：频繁随机访问
for (int i = 0; i < linkedList.size(); i++) {
    String item = linkedList.get(i);  // 每次都是O(n)
}

// 正确：使用迭代器
for (String item : linkedList) {  // 每次O(1)
    // 处理
}

// 或使用ListIterator
ListIterator<String> it = linkedList.listIterator();
while (it.hasNext()) {
    String item = it.next();
    // 处理
}
```

### 2. 批量操作优化

```
// 如果需要频繁在中间插入
// 考虑收集所有要插入的元素，然后一次性插入
LinkedList<String> list = new LinkedList<>();
List<String> toInsert = new ArrayList<>();

// 收集要插入的元素
for (int i = 0; i < 100; i++) {
    toInsert.add("Item" + i);
}

// 一次性插入
list.addAll(5, toInsert);  // 比多次add(index, element)高效
```

### 3. 选择合适的遍历方式

```
LinkedList<String> list = new LinkedList<>();

// 场景1：只需要顺序访问
for (String item : list) {  // 最佳
    // 处理
}

// 场景2：需要索引
int index = 0;
for (String item : list) {  // 手动维护索引
    System.out.println(index++ + ": " + item);
}

// 场景3：需要双向遍历
ListIterator<String> it = list.listIterator();
// 向前遍历
while (it.hasNext()) { /* ... */ }
// 向后遍历
while (it.hasPrevious()) { /* ... */ }
```

---

## 📊 LinkedList vs ArrayList 总结

|操作|LinkedList|ArrayList|建议|
|---|---|---|---|
|随机访问|O(n)|O(1)|随机访问多用 ArrayList|
|头部插入|O(1)|O(n)|头部操作多用 LinkedList|
|尾部插入|O(1)|O(1) 摊销|两者都合适|
|中间插入|O(n) + O(1)|O(n)|LinkedList 稍好|
|内存占用|大（指针开销）|小|内存敏感用 ArrayList|
|缓存友好|差|好|遍历多用 ArrayList|
|实现队列|原生支持|需要额外处理|队列用 LinkedList|

### 使用场景总结

- **使用 LinkedList**：频繁头部操作、队列/双端队列、栈、中间插入（如果已持有节点引用）
    
- **使用 ArrayList**：频繁随机访问、遍历、内存敏感、尾部操作
    
- **默认选择**：大多数情况下 ArrayList 性能更好
    

---

## 💡 最佳实践

1. **明确需求**：根据操作类型选择数据结构
    
2. **接口编程**：使用 `List<String> list = new LinkedList<>()`
    
3. **避免随机访问**：LinkedList 不要用 get(index)
    
4. **利用双端特性**：需要队列/栈功能时使用 LinkedList
    
5. **注意线程安全**：多线程环境需要同步或使用并发集合
    

掌握 LinkedList 的特性和适用场景，能够帮助你在合适的情况下选择它，写出更高效的代码。