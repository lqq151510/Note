通过继承 `Thread`类来实现多线程是 Java 中最基础直接的方式，其核心在于创建一个自定义类来继承 `Thread`类，并重写其 `run()`方法以定义线程要执行的任务。

下面这个表格能帮你快速把握这种方法的全貌。

|特性|说明|
|---|---|
|**核心机制**​|自定义类继承 `Thread`类，并重写 `run()`方法作为线程执行体。|
|**启动方式**​|创建自定义线程类的实例，调用其 `start()`方法（而非 `run()`方法）来启动新线程。|
|**主要优点**​|**简单直观**，适合初学者理解和快速实现多线程。|
|**主要缺点**​|由于 Java 的单继承限制，自定义类无法再继承其他类，**扩展性受限**。线程任务与线程对象绑定，**不利于资源共享**。|

### 🛠️ 实现步骤

1. **定义子类**：创建一个类（例如 `MyThread`），让它继承 `java.lang.Thread`类。
    
2. **重写 run() 方法**：在子类中重写 `run()`方法，将需要并发执行的代码逻辑编写在该方法内。这个方法就是线程的入口点。
    
3. **创建线程对象**：实例化你的自定义线程类（如 `new MyThread()`）。
    
4. **启动线程**：调用线程对象的 `start()`方法。JVM 会自动调用 `run()`方法，并在新的线程中执行。
    

### 🧩 代码示例

```
/**
 * 自定义线程类，继承Thread
 */
class MyThread extends Thread {
    private String threadName;
    
    // 构造方法，可用于设置线程名字
    public MyThread(String name) {
        this.threadName = name;
    }
    
    /**
     * 重写run方法，定义线程执行体
     */
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            // Thread.currentThread().getName() 可以获取当前执行线程的名称
            System.out.println(threadName + " 正在运行，计数: " + i + " | 当前线程名: " + Thread.currentThread().getName());
            try {
                // 让线程睡眠一段时间，模拟任务执行，使交互更清晰
                Thread.sleep(500);
            } catch (InterruptedException e) {
                System.out.println("线程被中断");
            }
        }
        System.out.println(threadName + " 执行完毕。");
    }
}

/**
 * 测试类
 */
public class ThreadExample {
    public static void main(String[] args) {
        System.out.println("主线程开始执行: " + Thread.currentThread().getName());
        
        // 1. 创建自定义线程对象
        MyThread threadA = new MyThread("线程-A");
        MyThread threadB = new MyThread("线程-B");
        
        // 2. 启动线程，注意是调用 start()，不是 run()
        threadA.start();
        threadB.start();
        
        // 主线程继续执行自己的任务
        for (int i = 0; i < 3; i++) {
            System.out.println("主线程打印: " + i);
            try {
                Thread.sleep(300);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        
        System.out.println("主线程任务结束。");
        // 注意：主线程结束后，子线程会继续执行直到完成
    }
}
```

### ⚠️ 关键要点

- **`start()`vs `run()`**：这是最关键的区别。直接调用 `run()`方法只会像普通方法一样在当前线程（如主线程）中顺序执行，**不会创建新线程**。只有调用 `start()`方法，JVM 才会为其分配新的执行路径，并异步调用 `run()`方法。
    
- **线程名称**：可以通过 `setName()`方法或在构造时为线程命名，使用 `getName()`获取。若不设置，JVM 会为其分配默认名称（如 `Thread-0`, `Thread-1`）。
    
- **执行顺序不可控**：线程启动后，具体的执行顺序（交替运行）由线程调度器决定，每次运行结果可能不同。这正是多线程并发特性的体现。
    

### ⚖️ 对比与选择

为了让你更清楚地了解何时该选择继承 `Thread`类，这里将它与其他主流方式做个对比。

|实现方式|优点|缺点|适用场景|
|---|---|---|---|
|**继承 Thread 类**​|编码简单直接|受限于单继承，扩展性差|简单的演示或任务，不需要继承其他父类|
|**实现 Runnable 接口**​|灵活性高，可继承其他类；**多个线程可共享同一Runnable实例**，方便共享资源|代码结构稍复杂|**实际开发中更常用**，尤其需要资源共享时|
|**实现 Callable 接口**​|线程执行后有**返回值**；可以抛出异常|需使用 `FutureTask`封装，稍显繁琐|需要获取线程执行结果的场景|

### 💎 总结与建议

继承 `Thread`类是实现多线程最易上手的方式，非常适合理解多线程的基本概念。

然而，由于其单继承的限制和不利于资源共享的特性，在现代 Java 开发中，**实现 `Runnable`接口或使用基于 `Runnable`的线程池通常是更受推荐和更为灵活的做法**。当你需要获取线程执行结果时，可以考虑使用 `Callable`接口。

希望这些解释和示例能帮助你清晰地掌握通过继承 `Thread`类实现多线程的方法。如果你对 `Runnable`或线程池等其他方式感兴趣，我们可以继续深入探讨。