通过实现 `Runnable`接口来创建多线程，是 Java 中一种非常常用且推荐的方式。它提供了一种将 **“任务”**​ 与 **“执行线程”**​ 分离的清晰模型，带来了更好的灵活性和设计上的优势。

下面这个表格能帮你快速抓住它的核心实现步骤。

|步骤|核心操作|代码示例|
|---|---|---|
|**1. 实现接口**​|创建一个类实现 `Runnable`接口。|`class MyTask implements Runnable`|
|**2. 重写 run()**​|在该类中重写 `run()`方法，编写线程要执行的任务代码。|`public void run() { ... }`|
|**3. 创建线程对象**​|实例化你的 `Runnable`实现类，并将其作为 `target`传递给 `Thread`类的构造方法。|`Thread t = new Thread(new MyTask());`|
|**4. 启动线程**​|调用 `Thread`对象的 `start()`方法（而非 `run()`方法）。|`t.start();`|

### 💡 完整代码示例

让我们通过一个具体的例子来串联上述步骤。这个程序将创建两个线程，它们会交替打印数字。

```
// 1. 创建一个实现Runnable接口的类
class MyTask implements Runnable {
    private String threadName;
    
    public MyTask(String name) {
        this.threadName = name;
    }
    
    // 2. 重写run方法，定义线程要执行的任务
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(threadName + " 正在运行: " + i);
            try {
                // 让线程休眠一段时间，使交替效果更明显
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println(threadName + " 执行完毕。");
    }
}

public class RunnableDemo {
    public static void main(String[] args) {
        System.out.println("主线程开始: " + Thread.currentThread().getName());
        
        // 3. 创建Runnable实现类的实例
        MyTask task1 = new MyTask("线程-A");
        MyTask task2 = new MyTask("线程-B");
        
        // 4. 创建Thread对象，并将Runnable实例作为参数传入
        Thread thread1 = new Thread(task1);
        Thread thread2 = new Thread(task2);
        
        // 5. 启动线程（注意是调用start()，不是run()！）
        thread1.start();
        thread2.start();
        
        // 主线程继续执行自己的任务
        System.out.println("主线程结束。");
    }
}
```

**可能的输出结果（每次运行可能不同，这正是并发特性的体现）：**

```
主线程开始: main
主线程结束。
线程-A 正在运行: 0
线程-B 正在运行: 0
线程-A 正在运行: 1
线程-B 正在运行: 1
线程-A 正在运行: 2
...
线程-B 执行完毕。
线程-A 执行完毕。
```

### ⚔️ 核心优势：为何更推荐 Runnable？

与继承 `Thread`类的方式相比，实现 `Runnable`接口具有以下显著优势，这也是它在实际开发中更受青睐的原因：

|特性|实现 `Runnable`接口|继承 `Thread`类|
|---|---|---|
|**继承灵活性**​|**可以继承其他类**，不受Java单继承的限制。|由于Java是单继承，继承了`Thread`后不能再继承其他类。|
|**资源共享能力**​|**天然支持**。多个线程可以共享同一个`Runnable`实例（即同一个`target`），非常适合处理如售票系统等需要共享资源的场景。|资源独立。每个线程对象有其独立的资源，共享相对复杂。|
|**设计原则**​|更符合**面向对象**设计思想。将“任务”（`Runnable`）与“线程执行器”（`Thread`）**解耦**，职责更清晰。|任务和线程机制绑定在一起，耦合性较高。|
|**线程池支持**​|现代的线程池（如`ExecutorService`）直接接受`Runnable`（或`Callable`）任务，与`Thread`子类无关，**是使用线程池的必要方式**。|需要额外适配才能很好地融入线程池框架。|

**资源共享示例：模拟售票系统**

这个例子清晰地展示了为什么 `Runnable`更适合需要共享资源的场景。

```
// 售票任务，实现了Runnable接口
class SellTicketTask implements Runnable {
    private int tickets = 10; // 总票数，是所有线程共享的资源
    
    @Override
    public void run() {
        while (tickets > 0) {
            System.out.println(Thread.currentThread().getName() + " 卖出一张票，剩余: " + (--tickets));
            try {
                Thread.sleep(100); // 模拟出票时间
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

public class TicketSystem {
    public static void main(String[] args) {
        // 只创建一个票务任务实例
        SellTicketTask task = new SellTicketTask();
        
        // 三个售票窗口（三个线程）共享同一个票务任务，即共享tickets变量
        new Thread(task, "窗口1").start();
        new Thread(task, "窗口2").start();
        new Thread(task, "窗口3").start();
    }
}
```

在这个例子中，三个线程操作的是**同一个**​ `tickets`变量，从而实现了票数的共享和递减。如果使用继承 `Thread`类的方式，每个线程都会有自己独立的 `tickets`变量，无法实现共享售票。

### 🚀 现代简化写法

自从 Java 8 引入 Lambda 表达式后，创建 `Runnable`实例变得异常简洁。

```
// 传统匿名内部类写法
new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("传统方式");
    }
}).start();

// 使用Lambda表达式简化（因为Runnable是函数式接口，只有一个run方法）
new Thread(() -> {
    System.out.println("使用Lambda表达式，非常简洁！");
}).start();
```

### 💎 总结

- **核心机制**：实现 `Runnable`接口来定义任务，再交由 `Thread`对象去执行。
    
- **关键区别**：调用 `start()`会启动新线程来执行 `run()`方法；直接调用 `run()`则只是在当前线程执行普通方法。
    
- **首选方案**：由于更好的**灵活性**、**设计解耦**和对**资源共享**的天然支持，实现 `Runnable`接口是比继承 `Thread`类**更受推荐的多线程实现方式**。
    

希望这份详细的解释能帮助你透彻理解如何通过 `Runnable`接口实现多线程。如果你对线程安全（`synchronized`）或更现代的 `ExecutorService`线程池感兴趣，我们可以继续深入。