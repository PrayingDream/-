### 多线程

##### １、进程 与 线程

进程是:一个应用程序(1个进程是一个软件)。

线程是:一个进程中的执行场景/执行单元。

一个进程可以启动多个线程

##### ２、进程个线程是什么关系

进程:可以看作是现实生活当中的公司

线程:可以看作是公司当中的某个员工

进程A和进程B的**内存独立不共享**

##### ３、线程与线程之间的关系

线程Ａ和线程Ｂ，堆内存和方法区内存共享。但是栈内存独立，一个线内存一个栈。

##### ４、线程对象的生命周期

１.新建状态　２.就绪状态　３.运行状态　４.阻塞状态　５.死亡状态

![线程的生命周期](https://s2.loli.net/2024/07/16/ei3tucqEwxObpXH.png)

#### 线程构造方法

| 构造方法名                          | 备注           |
| ----------------------------------- | -------------- |
| Thread()                            |                |
| Thread(String name)                 | name为线程名字 |
| Thread(Runnable target)             |                |
| Thread(Runnable Target,String name) | name为线程名字 |

#### 实现线程的两种方法

##### 第一种方式:

编写一个类，直接继承java.lang.Thread，重写run方法。

1.创建线程对象的方法：new继承线程的类

2.启动线程：调用线程对象的start()方法。

eg.

```java
public class ThreadTest02 {
    public static void main(String[] args) {
        MyThread t = new MyThread();
        // 启动线程
        //t.run(); // 不会启动线程，不会分配新的分支栈。（这种方式就是单线程。）
        t.start();
        // 这里的代码还是运行在主线程中。
        for(int i = 0; i < 1000; i++){
            System.out.println("主线程--->" + i);
        }
    }
}

class MyThread extends Thread {
    @Override
    public void run() {
        // 编写程序，这段程序运行在分支线程中（分支栈）。
        for(int i = 0; i < 1000; i++){
            System.out.println("分支线程--->" + i);
        }
    }
}
```

###### 注意：

t.run() 不会启动线程，只是普通的调用方法而已。不会分配新的分支栈。（这种方式就是单线程。）

t.start() 方法的作用是：启动一个分支线程，在JVM中开辟一个新的栈空间，这段代码任务完成之后，瞬间就结束了。这段代码的任务只是为了开启一个新的栈空间，只要新的栈空间开出来，start()方法就结束了。线程就启动成功了。启动成功的线程会自动调用run方法，并且run方法在分支栈的栈底部（压栈）。run方法在分支栈的栈底部，main方法在主栈的栈底部。run和main是平级的。

**调用run()方法内存图：**

![run()方法内存图](https://s2.loli.net/2024/07/16/8cn492tPeLD56Wm.png)

**调用start()方法内存图：**

![start()方法内存图](https://s2.loli.net/2024/07/16/bvp54BMCNaKqfWQ.png)

##### 第二种方式:

编写一个类，实现**java.lang.Runnable**接口，实现**run方法**。

1.创建线程对象:new线程类传入可运行的类/接口。

2.启动线程:调用线程对象的start()方法。

eg.

```java
public class ThreadTest03 {
    public static void main(String[] args) {
        Thread t = new Thread(new MyRunnable()); 
        // 启动线程
        t.start();
        
        for(int i = 0; i < 100; i++){
            System.out.println("主线程--->" + i);
        }
    }
}

// 这并不是一个线程类，是一个可运行的类。它还不是一个线程。
class MyRunnable implements Runnable {
    @Override
    public void run() {
        for(int i = 0; i < 100; i++){
            System.out.println("分支线程--->" + i);
        }
    }
}

```

#### **采用匿名内部类创建：**

```java
public class ThreadTest04 {
    public static void main(String[] args) {
        // 创建线程对象，采用匿名内部类方式。
        Thread t = new Thread(new Runnable(){
            @Override
            public void run() {
                for(int i = 0; i < 100; i++){
                    System.out.println("t线程---> " + i);
                }
            }
        });

        // 启动线程
        t.start();

        for(int i = 0; i < 100; i++){
            System.out.println("main线程---> " + i);
        }
    }
}

```

**注意：**
**第二种方式**实现接口比较常用，因为一个类实现了接口，它还可以去继承其它的类，更灵活。

#### 获取当前线程对象、获取线程对象名字、修改线程对象名字

| 方法名                        | 作用             |
| ----------------------------- | ---------------- |
| static Thread currentThread() | 获取当前线程对象 |
| String getName()              | 获取线程对象名字 |
| void setName(String name)     | 修改线程对象名字 |

当线程没有设置名字的时候，默认的名字是

Thread-0 Thread-1 Thread-2 Thread-3 ......

eg.

```java
class MyThread2 extends Thread {
    public void run(){
        for(int i = 0; i < 100; i++){
            // currentThread就是当前线程对象。当前线程是谁呢？
            // 当t1线程执行run方法，那么这个当前线程就是t1
            // 当t2线程执行run方法，那么这个当前线程就是t2
            Thread currentThread = Thread.currentThread();
            System.out.println(currentThread.getName() + "-->" + i);

            //System.out.println(super.getName() + "-->" + i);
            //System.out.println(this.getName() + "-->" + i);
        }
    }
}
```

#### 关于线程的sleep方法

| 方法名                         | 作用                   |
| ------------------------------ | ---------------------- |
| static void sleep(long millis) | 让当前线程休眠millis秒 |

1.静态方法:Thread.sleep(1000);
2.参数是**毫秒**
3.**作用**:让当前线程进入休眠，进入“阻塞状态”，**放弃占有CPU时间片**，让给其他线程使用。出现在哪个线程中就会让那个线程休眠。
4.通过Thread.sleep()方法，可以使特定代码，每个多久执行一次。

eg.

```java
public class ThreadTest06 {
    public static void main(String[] args) {
    	//每打印一个数字睡1s
        for(int i = 0; i < 10; i++){
            System.out.println(Thread.currentThread().getName() + "--->" + i);

            // 睡眠1秒
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

#### 关于线程终端sleep()的方法

| 方法名           | 作用           |
| ---------------- | -------------- |
| void interrupt() | 终止线程的休眠 |

eg.

```java
public class ThreadTest08 {
    public static void main(String[] args) {
        Thread t = new Thread(new MyRunnable2());
        t.setName("t");
        t.start();

        // 希望5秒之后，t线程醒来（5秒之后主线程手里的活儿干完了。）
        try {
            Thread.sleep(1000 * 5);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        // 终断t线程的睡眠（这种终断睡眠的方式依靠了java的异常处理机制。）
        t.interrupt();
    }
}

class MyRunnable2 implements Runnable {
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + "---> begin");
        try {
            // 睡眠1年
            Thread.sleep(1000 * 60 * 60 * 24 * 365);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        //1年之后才会执行这里
        System.out.println(Thread.currentThread().getName() + "---> end");
}
```

##### run()方法的小知识

![run()方法抛异常](https://s2.loli.net/2024/07/16/f4wKxPhyrXzkRVU.png)

###### 为什么run()方法只能try...catch...不能throws？

因为run()方法在父类中没有抛出任何异常，子类不能比父类抛出更多的异常。

#### java终止线程(不推荐使用)

```java
public class ThreadTest09 {
    public static void main(String[] args) {
        Thread t = new Thread(new MyRunnable3());
        t.setName("t");
        t.start();

        // 模拟5秒
        try {
            Thread.sleep(1000 * 5);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        // 5秒之后强行终止t线程
        t.stop(); // 已过时（不建议使用。）
    }
}

class MyRunnable3 implements Runnable {

    @Override
    public void run() {
        for(int i = 0; i < 10; i++){
            System.out.println(Thread.currentThread().getName() + "--->" + i);
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

#### java中合理结束一个进程的执行(常用)

eg.

```java
public class ThreadTest10 {
    public static void main(String[] args) {
        MyRunable4 r = new MyRunable4();
        Thread t = new Thread(r);
        t.setName("t");
        t.start();

        // 模拟5秒
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        // 终止线程
        // 你想要什么时候终止t的执行，那么你把标记修改为false，就结束了。
        r.run = false;
    }
}

class MyRunable4 implements Runnable {

    // 打一个布尔标记
    boolean run = true;

    @Override
    public void run() {
        for (int i = 0; i < 10; i++){
            if(run){
                System.out.println(Thread.currentThread().getName() + "--->" + i);
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }else{
                // return就结束了，你在结束之前还有什么没保存的。
                // 在这里可以保存呀。
                //save....

                //终止当前线程
                return;
            }
        }
    }
}
```

### 线程调度

##### 1.常见的线程调度模型有哪些？

- **抢占式**调度模型：
  那个线程的优先级比较高，抢到的CPU时间片的概率就高一些/多一些。
  **java采用的就是抢占式调度模型**。
- **均分式**调度模型：
  平均分配CPU时间片。每个线程占有的CPU时间片时间长度一样。
  平均分配，一切平等。
  有一些编程语言，线程调度模型采用的是这种方式。

#### 2.java中提供了哪些方法是和咸亨调度有关系的呢？

##### 2.1实例方法：

| 方法名                            | 作用           |
| --------------------------------- | -------------- |
| int getPriority()                 | 获取线程优先级 |
| void setPriority(int newPriority) | 设置线程优先级 |

- 最低优先级1
- 默认优先级是5
- 最高优先级10

**优先级比较高的获取CPU时间片可能会多一些**。（但也不完全是，大概率是多的。）

##### 2.2静态方法：

| 方法名              | 作用                                                 |
| ------------------- | ---------------------------------------------------- |
| static void yield() | 让位方法，当前线程暂停，回到就绪状态，让给其他线程。 |

yield()方法不是阻塞方法。让当前线程让位，让给其它线程使用。

yield()方法的执行会让当前线程从“**运行状态**”回到“**就绪状态**”。

注意：在回到就绪之后，**有可能还会再次抢到**。

##### 2.3实例方法：

| 方法名      | 作用                                                         |
| ----------- | ------------------------------------------------------------ |
| void join() | 将一个线程合并到当前线程中，当前线程受阻塞，加入的线程执行知道结束 |

eg.

```java
class MyThread1 extends Thread {
	public void doSome(){
		MyThread2 t = new MyThread2();
		t.join(); // 当前线程进入阻塞，t线程执行，直到t线程结束。当前线程才可以继续。
	}
}

class MyThread2 extends Thread{

}
```

### java进程的优先级

##### 常量：

| 常量名                   | 备注           |
| ------------------------ | -------------- |
| static int MAX_PRIORITY  | 最高优先级(10) |
| static int MIN_PRIORITY  | 最低优先级(1)  |
| static int NORM_PRIORITY | 默认优先级(5)  |

##### 方法：

| 方法名                            | 作用           |
| --------------------------------- | -------------- |
| int getPriority()                 | 获得线程优先级 |
| void setPriority(int newPriority) | 设置线程优先级 |

eg.

```java
public class ThreadTest11 {
    public static void main(String[] args) {
        System.out.println("最高优先级：" + Thread.MAX_PRIORITY);//最高优先级：10
        System.out.println("最低优先级:" + Thread.MIN_PRIORITY);//最低优先级:1
        System.out.println("默认优先级:" + Thread.NORM_PRIORITY);//默认优先级:5
        
        // main线程的默认优先级是：5
        System.out.println(hread.currentThread().getName() + "线程的默认优先级是：" + currentThread.getPriority());

        Thread t = new Thread(new MyRunnable5());
        t.setPriority(10);
        t.setName("t");
        t.start();

        // 优先级较高的，只是抢到的CPU时间片相对多一些。
        // 大概率方向更偏向于优先级比较高的。
        for(int i = 0; i < 10000; i++){
            System.out.println(Thread.currentThread().getName() + "-->" + i);
        }
    }
}

class MyRunnable5 implements Runnable {
    @Override
    public void run() {
        for(int i = 0; i < 10000; i++){
            System.out.println(Thread.currentThread().getName() + "-->" + i);
        }
    }
}
```

##### 注意：

main线程的默认优先级是:5
优先级较高的，只是前后感到CPU时间片相对多一些。大概率方向更偏向于优先级比较高的。

### 关于线程的yiled()方法

| 方法名              | 作用                                             |
| ------------------- | ------------------------------------------------ |
| static void yield() | 让位，当前线程暂停，回到就绪状态，让给其它线程。 |

eg.

```java
public class ThreadTest12 {
    public static void main(String[] args) {
        Thread t = new Thread(new MyRunnable6());
        t.setName("t");
        t.start();

        for(int i = 1; i <= 10000; i++) {
            System.out.println(Thread.currentThread().getName() + "--->" + i);
        }
    }
}

class MyRunnable6 implements Runnable {

    @Override
    public void run() {
        for(int i = 1; i <= 10000; i++) {
            //每100个让位一次。
            if(i % 100 == 0){
                Thread.yield(); // 当前线程暂停一下，让给主线程。
            }
            System.out.println(Thread.currentThread().getName() + "--->" + i);
        }
    }
}

```

注意：并不是每次都让成功的，有可能它又抢到时间片了。

### 关于线程的join()方法

| 方法名                            | 作用                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| void join()                       | 将一个线程合并到当前线程中，当前线程受阻塞，加入的线程执行直到结束 |
| void join(long millis)            | 接上条，等待该线程终止的事件最长为millis毫秒                 |
| void join(long millis, int nanos) | 接第一条，等待该线程终止的事件最长为millis毫秒 + nanos纳秒   |

eg.

```java
public class ThreadTest13 {
    public static void main(String[] args) {
        System.out.println("main begin");

        Thread t = new Thread(new MyRunnable7());
        t.setName("t");
        t.start();

        //合并线程
        try {
            t.join(); // t合并到当前线程中，当前线程受阻塞，t线程执行直到结束。
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("main over");
    }
}

class MyRunnable7 implements Runnable {

    @Override
    public void run() {
        for(int i = 0; i < 10000; i++){
            System.out.println(Thread.currentThread().getName() + "--->" + i);
        }
    }
}
```

注意:一个线程.join(),当前线程会进入“阻塞状态”。等待加入线程执行完！

### 数据的安全

#### 1.什么时候会出现数据的安全问题

条件1：多线程并发
条件2：有共享数据
条件3：共享数据有修改行为
满足以上三个条件之后，就会存在线程安全问题。

#### 2.怎么解决线程安全问题

线程排队执行。(不能并发)。用排队执行解决线程安全问题。

这种机制被称为：线程同步机制。

****

线程同步就是线程排队了，线程排队了就会**牺牲一部分效率**，没办法，数据安全第一位，只有数据安全了，我们才可以谈效率。数据不安全，没有效率的事儿。

****

#### 3.两个专业术语：

##### 异步编辑模型：

线程t1和线程t2，各自执行各自的，t1不管t2，t2不管t1，谁也不需要等谁，这种变成模型叫做：异步编程模型。其实就是：多线程并发(效率较高)。
**异步就是并发**

##### 同步编辑模型：

线程t1和线程t2，在线程t1执行的时候，必须等待t2线程执行结束，或者说在t2线程执行的时候，必须等待t1线程执行结束，两个线程之间发生了等待关系，这就是同步编程模型。效率较低，线程排队执行。
**同步就是排队**

### 线程安全

#### synchronized-线程同步

线程同步机制的**语法**是：

```java
synchronized(){
	// 线程同步代码块。
}
```

**重点：**

synchronized后面的小括号()中传的这个”数据“是相当关键的。这个数据必须是**多线程共享**的数据。才能达到多线程排队。

##### （）中写什么

假设t1、t2、t3、t4、t5，有5个线程，你只希望t1 t2 t3排队，t4 t5不需要排队。怎么办？

你一定要在()中写一个t1 t2 t3共享的对象。而这个对象对于t4 t5来说不是共享的。

这里的共享对象是：账户对象。
账户对象是共享的，那么this就是账户对象！！！
()不一定是this，这里只要是多线程共享的那个对象就行。

##### 代码执行原理

1、假设t1和t2线程并发，开始执行以下代码的时候，肯定有一个先一个后。

2、假设t1先执行了，遇到了synchronized，这个时候自动找“后面共享对象”的对象锁，
找到之后，并占有这把锁，然后执行同步代码块中的程序，在程序执行过程中一直都是
占有这把锁的。直到同步代码块代码结束，这把锁才会释放。

3、假设t1已经占有这把锁，此时t2也遇到synchronized关键字，也会去占有后面
共享对象的这把锁，结果这把锁被t1占有，t2只能在同步代码块外面等待t1的结束，
直到t1把同步代码块执行结束了，t1会归还这把锁，此时t2终于等到这把锁，然后
t2占有这把锁之后，进入同步代码块执行程序。

4、这样就达到了线程排队执行。

**重中之重：**

这个共享对象一定要选好了。这个共享对象一定是你需要排队执行的这些线程对象所共享的。

```java
class Account {
    private String actno;
    private double balance; //实例变量。

    //对象
    Object o= new Object(); // 实例变量。（Account对象是多线程共享的，Account对象中的实例变量obj也是共享的。）

    public Account() {
    }

    public Account(String actno, double balance) {
        this.actno = actno;
        this.balance = balance;
    }

    public String getActno() {
        return actno;
    }

    public void setActno(String actno) {
        this.actno = actno;
    }

    public double getBalance() {
        return balance;
    }

    public void setBalance(double balance) {
        this.balance = balance;
    }

    //取款的方法
    public void withdraw(double money){
        /**
         * 以下可以共享,金额不会出错
         * 以下这几行代码必须是线程排队的，不能并发。
         * 一个线程把这里的代码全部执行结束之后，另一个线程才能进来。
         */
        synchronized(this) {
        //synchronized(actno) {
        //synchronized(o) {
        
        /**
         * 以下不共享，金额会出错
         */
		  /*Object obj = new Object();
	        synchronized(obj) { // 这样编写就不安全了。因为obj2不是共享对象。
	        synchronized(null) {//编译不通过
	        String s = null;
	        synchronized(s) {//java.lang.NullPointerException*/
            double before = this.getBalance();
            double after = before - money;
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            this.setBalance(after);
        //}
    }
}

class AccountThread extends Thread {
    // 两个线程必须共享同一个账户对象。
    private Account act;

    // 通过构造方法传递过来账户对象
    public AccountThread(Account act) {
        this.act = act;
    }

    public void run(){
        double money = 5000;
        act.withdraw(money);
        System.out.println(Thread.currentThread().getName() + "对"+act.getActno()+"取款"+money+"成功，余额" + act.getBalance());
    }
}

public class Test {
    public static void main(String[] args) {
        // 创建账户对象（只创建1个）
        Account act = new Account("act-001", 10000);
        // 创建两个线程，共享同一个对象
        Thread t1 = new AccountThread(act);
        Thread t2 = new AccountThread(act);

        t1.setName("t1");
        t2.setName("t2");
        t1.start();
        t2.start();
    }
}
```

###### 在实例方法上可以使用synchronized

synchronized出现在实例方法上，一定锁的是 **`this`**。没得挑。只能是this。不能是其他的对象了。所以这种方式**不灵活**。

###### 缺点

synchronized出现在实例方法上，表示**整个方法体都需要同步**，可能会无故**扩大同步的范围**，导致程序的**执行效率降低**。所以这种方式**不常用**。

###### 优点

代码写的少了。节俭了。

###### 总结

如果共享的对象就是**this**，并且需要**同步的代码块是整个方法体**，建议使用这种方法。

eg.

```java
    public synchronized void withdraw(double money){
        double before = this.getBalance();
        double after = before - money;
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        this.setBalance(after);
    }
```

#### 在方法调用处使用synchronized

eg.

```java
    public void run(){
        double money = 5000;
        // 取款
        // 多线程并发执行这个方法。
        //synchronized (this) { //这里的this是AccountThread对象，这个对象不共享！
        synchronized (act) { // 这种方式也可以，只不过扩大了同步的范围，效率更低了。
            act.withdraw(money);
        }

        System.out.println(Thread.currentThread().getName() + "对"+act.getActno()+"取款"+money+"成功，余额" + act.getBalance());
    }

```

这种方式也可以，只不过**扩大了同步的范围**，效率更低了。

#### java中有三大变量

- **实例**变量：在**堆**中。
- **静态**变量：在**方法区**。
- **局部**变量：在**栈**中。

以上三大变量中：

**局部变量永远都不会存在线程安全问题**

- 因为局部变量不共享。（一个线程一个栈。）
- 局部变量在**栈**中。所以局部变量永远都不会共享。

****

1.视力变量在堆中，堆只有1个。

2.静态变量在方法区中，方法区只有1个。

**堆和方法区都是多线程共享的，所以可能存在线程安全问题。**

**总结：**

- **局部变量+常量**：不会有线程安全问题。
- **成员变量（实例+静态）**：可能会有线程安全问题

#### 以后线程安全和非线程安全的类如何选择？

**如果使用局部变量的话：**

建议使用：StringBuilder。

因为局部变量不存在线程安全问题。选择StringBuilder。

StringBuffer效率比较低。

**反之：**

使用StringBuffer。

- ArrayList是非线程安全的。
- Vector是线程安全的。
- HashMap HashSet是非线程安全的。
- Hashtable是线程安全的。

#### 总结synchronized

##### synchronized有三种写法：

##### 第一种：同步代码块

###### 灵活

```java
synchronized(线程共享对象){
	同步代码块;
}
```

##### 第二种：在实例方法上使用synchronized

表示**共享对象**一定是this并且同步代码块是**整个方法体**。

##### 第三种：在静态方法上使用synchronized

表示找**类锁。**类锁永远只有一把。
就算创建了100个对象，那类锁也只有1把。

#### 在开发中应该怎么解决线程安全问题？

是一上来就选择线程同步吗？synchronized

不是，synchronized会让程序的执行效率降低，用户体验不好。
系统的用户吞吐量降低。用户体验差。在不得已的情况下再选择线程同步机制。

第一种方案：尽量使用局部变量 代替 “实例变量和静态变量”。

第二种方案：如果必须是实例变量，那么可以考虑创建多个对象，这样实例变量的内存就不共享了。（一个线程对应1个对象，100个线程对应100个对象，对象不共享，就没有数据安全问题了。）

第三种方案：如果不能使用局部变量，对象也不能创建多个，这个时候就只能选择synchronized了。线程同步机制。

#### 死锁

![死锁](https://s2.loli.net/2024/07/16/YziU8LxvBFRwtDM.png)

```java
/**
 * 比如：t1想先穿衣服在穿裤子
 *       t2想先穿裤子在传衣服
 * 此时：t1拿到衣服，t2拿到裤子；
 * 由于t1拿了衣服，t2找不到衣服；t2拿了裤子，t1找不到裤子
 * 就会导致死锁的发生！
 */
public class Thread_DeadLock {
    public static void main(String[] args) {
        Dress dress = new Dress();
        Trousers trousers = new Trousers();
        //t1、t2共享dress和trousers。
        Thread t1 = new Thread(new MyRunnable1(dress, trousers), "t1");
        Thread t2 = new Thread(new MyRunnable2(dress, trousers), "t2");
        t1.start();
        t2.start();
    }
}

class MyRunnable1 implements Runnable{
    Dress dress;
    Trousers trousers;

    public MyRunnable1() {
    }

    public MyRunnable1(Dress dress, Trousers trousers) {
        this.dress = dress;
        this.trousers = trousers;
    }

    @Override
    public void run() {
        synchronized(dress){
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            synchronized (trousers){
                System.out.println("--------------");
            }
        }
    }
}

class MyRunnable2 implements Runnable{
    Dress dress;
    Trousers trousers;

    public MyRunnable2() {
    }

    public MyRunnable2(Dress dress, Trousers trousers) {
        this.dress = dress;
        this.trousers = trousers;
    }

    @Override
    public void run() {
        synchronized(trousers){
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            synchronized (dress){
                System.out.println("。。。。。。。。。。。。。。");
            }
        }
    }
}

class Dress{

}

class Trousers{

}
```

#### 守护线程

##### java语言中线程分为两大类:

一类是：用户线程
一类是：守护线程（后台线程）
其中具有代表性的就是：垃圾回收线程（守护线程）。

##### 守护线程的特点：

一般守护线程是一个死循环，所有的用户线程只要结束，守护线程自动结束。

注意：主线程main方法是一个用户线程。

##### 守护线程用在什么地方呢？

每天00:00的时候系统数据自动备份。
这个需要使用到定时器，并且我们可以将定时器设置为守护线程。
一直在那里看着，没到00:00的时候就备份一次。所有的用户线程如果结束了，守护线程自动退出，没有必要进行数据备份了。

##### 方法

| 方法名                     | 作用                             |
| -------------------------- | -------------------------------- |
| void setDaemon(boolean on) | on为true表示吧线程设置为守护线程 |

eg.

```java
public class ThreadTest14 {
    public static void main(String[] args) {
        Thread t = new BakDataThread();
        t.setName("备份数据的线程");

        // 启动线程之前，将线程设置为守护线程
        t.setDaemon(true);

        t.start();

        // 主线程：主线程是用户线程
        for(int i = 0; i < 10; i++){
            System.out.println(Thread.currentThread().getName() + "--->" + i);
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

class BakDataThread extends Thread {
    public void run(){
        int i = 0;
        // 即使是死循环，但由于该线程是守护者，当用户线程结束，守护线程自动终止。
        while(true){
            System.out.println(Thread.currentThread().getName() + "--->" + (++i));
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

```

#### 定时器

##### 定时器的作用

间隔特定的事件，执行特定的程序。

eg.

每周要进行银行账户的总账操作。
每天要进行数据的备份操作。

****

在实际的开发中，每隔多久执行一段特定的程序，这种需求是很常见的，那么在java中其实可以采用多种方式实现：

可以使用sleep方法，睡眠，设置睡眠时间，没到这个时间点醒来，执行任务。这种方式是最原始的定时器。（比较low）

在java的类库中已经写好了一个定时器：java.util.Timer，可以直接拿来用。
不过，这种方式在目前的开发中也很少用，因为现在有很多高级框架都是支持定时任务的。

在实际的开发中，目前使用较多的是Spring框架中提供的SpringTask框架，这个框架只要进行简单的配置，就可以完成定时器的任务。

##### 构造方法

| 构造方法名                           | 备注                             |
| ------------------------------------ | -------------------------------- |
| Timer()                              | 创建一个定时器                   |
| Timer(boolean isDaemon)              | isDaemon为true为守护线程定时器   |
| Timer(String name)                   | 创建一个定时器，其线程名字为name |
| Timer(String name, boolean isDaemon) | 结合2、3                         |

##### 方法

| 方法名                                                     | 作用                                                 |
| ---------------------------------------------------------- | ---------------------------------------------------- |
| void schedule(TimerTask task, Date firstTime, long period) | 安排指定的任务在指定的时间开始进行重复的固定延迟执行 |
| void cancel()                                              | 终止定时器                                           |

#### 使用定时器实现日志备份

**正常方式：**

```java
class TimerTest01{
    public static void main(String[] args) {
        Timer timer = new Timer();
//        Timer timer = new Timer(true);//守护线程
        String firstTimeStr = "2021-05-09 17:27:00";
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        try {
            Date firstTime = sdf.parse(firstTimeStr);
            timer.schedule(new MyTimerTask(), firstTime, 1000 * 5);//每5s执行一次
        } catch (ParseException e) {
            e.printStackTrace();
        }
    }
}

class MyTimerTask extends TimerTask{
    @Override
    public void run() {
        Date d = new Date();
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String time = sdf.format(d);
        System.out.println(time + ":备份日志一次！");
    }
}

```

**匿名内部类方法：**

```java
class TimerTest02{
    public static void main(String[] args) {
        Timer timer = new Timer();
        String firstTimeStr = "2021-05-09 17:56:00";
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        try {
            Date firstTime = sdf.parse(firstTimeStr);
            timer.schedule(new TimerTask() {
                @Override
                public void run() {
                    Date d = new Date();
                    SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
                    String time = sdf.format(d);
                    System.out.println(time + ":备份日志一次！");
                }
            }, firstTime, 1000 * 5);
        } catch (ParseException e) {
            e.printStackTrace();
        }
    }
}

```

#### 实现线程的第三种方式：实现Callable接口(JDK8新特性)

这种方式实现的线程**可以获取线程的返回值**。

##### 优点

可以获取到线程的执行结果。

##### 缺点

**效率较低**，在获取t线程执行结果的时候，当前线程受阻塞，效率降低。

eg.

```java
public class ThreadTest15 {
    public static void main(String[] args) throws Exception {

        // 第一步：创建一个“未来任务类”对象。
        // 参数非常重要，需要给一个Callable接口实现类对象。
        FutureTask task = new FutureTask(new Callable() {
            @Override
            public Object call() throws Exception { // call()方法就相当于run方法。只不过这个有返回值
                // 线程执行一个任务，执行之后可能会有一个执行结果
                // 模拟执行
                System.out.println("call method begin");
                Thread.sleep(1000 * 10);
                System.out.println("call method end!");
                int a = 100;
                int b = 200;
                return a + b; //自动装箱(300结果变成Integer)
            }
        });

        // 创建线程对象
        Thread t = new Thread(task);

        // 启动线程
        t.start();

        // 这里是main方法，这是在主线程中。
        // 在主线程中，怎么获取t线程的返回结果？
        // get()方法的执行会导致“当前线程阻塞”
        Object obj = task.get();
        System.out.println("线程执行结果:" + obj);

        // main方法这里的程序要想执行必须等待get()方法的结束
        // 而get()方法可能需要很久。因为get()方法是为了拿另一个线程的执行结果
        // 另一个线程执行是需要时间的。
        System.out.println("hello world!");
    }
}

```

#### 关于Object类的wait()、notify()、notifyAll()方法

##### 方法

| 方法名           | 作用                                                     |
| ---------------- | -------------------------------------------------------- |
| void wait()      | 让活动在当前对象的线程无限等待（释放之前占有的锁）       |
| void notify()    | 唤醒当前对象正在等待的线程（只提示唤醒，不会释放锁）     |
| void notifyAll() | 唤醒当前对象全部正在等待的线程（只提示唤醒，不会释放锁） |

##### 方法详解

**第一**：wait和notify方法**不是线程对象的方法**，是java中任何一个java对象都有的方法，因为这两个方法是 **`Object类中自带`** 的。

**第二：wait()方法作用**？

```java
Object o = new Object();

o.wait();
```

**表示：**
**让正在o对象上活动的线程进入等待状态，无期限等待，直到被唤醒为止。**

**第三：notify()方法作用?**

```java
Object o = new Object();

o.notify();

```

**表示：**
**唤醒正在o对象上等待的线程。**

**第四**：**notifyAll()** 方法 **作用**？

```java
Object o = new Object();

o.notifyAll();

```

**表示：**
这个方法是唤醒o对象上处于等待的**所有线程**。

![在这里插入图片描述](https://s2.loli.net/2024/07/16/N8bjBevoTVM1flp.png)

##### 总结

1、wait和notify方法不是线程对象的方法，是普通java对象都有的方法。

2、wait方法和notify方法建立在 线程同步 的基础之上。因为多线程要同时操作一个仓库。有线程安全问题。

3、wait方法作用：o.wait() 让正在o对象上活动的线程t进入等待状态，并且释放掉t线程之前占有的o对象的锁。

4、notify方法作用：o.notify() 让正在o对象上等待的线程唤醒，只是通知，不会释放o对象上之前占有的锁。

#### 生产者和消费者模式(wait()和notify())

##### 什么是”生产者和消费者模式“?

- 生产线程负责生产，消费线程负责消费。
- 生产线程和消费线程要达到均衡。
- 这是一种特殊的业务需求，在这种特殊的情况下需要使用**wait方法和notify方法**。

##### 模拟一个业务需求

模拟这样一个需求：

- 仓库我们采用**List**集合。
- **List**集合中假设只能存储**1个**元素。
- **1**个元素就表示仓库**满**了。
- 如果**List**集合中元素个数是**0**，就表示仓库**空**了。
- 保证**List**集合中永远都是最多存储**1**个元素。
- 必须做到这种效果：**生产1个消费1个**。

![在这里插入图片描述](https://s2.loli.net/2024/07/16/Z1sdCirPySQUwTf.png)

eg.

```java
public class ThreadTest16 {
    public static void main(String[] args) {
        // 创建1个仓库对象，共享的。
        List list = new ArrayList();
        // 创建两个线程对象
        // 生产者线程
        Thread t1 = new Thread(new Producer(list));
        // 消费者线程
        Thread t2 = new Thread(new Consumer(list));

        t1.setName("生产者线程");
        t2.setName("消费者线程");

        t1.start();
        t2.start();
    }
}

// 生产线程
class Producer implements Runnable {
    // 仓库
    private List list;

    public Producer(List list) {
        this.list = list;
    }
    @Override
    public void run() {
        // 一直生产（使用死循环来模拟一直生产）
        while(true){
            // 给仓库对象list加锁。
            synchronized (list){
                if(list.size() > 0){ // 大于0，说明仓库中已经有1个元素了。
                    try {
                        // 当前线程进入等待状态，并且释放Producer之前占有的list集合的锁。
                        list.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
                // 程序能够执行到这里说明仓库是空的，可以生产
                Object obj = new Object();
                list.add(obj);
                System.out.println(Thread.currentThread().getName() + "--->" + obj);
                // 唤醒消费者进行消费
                list.notifyAll();
            }
        }
    }
}

// 消费线程
class Consumer implements Runnable {
    // 仓库
    private List list;

    public Consumer(List list) {
        this.list = list;
    }

    @Override
    public void run() {
        // 一直消费
        while(true){
            synchronized (list) {
                if(list.size() == 0){
                    try {
                        // 仓库已经空了。
                        // 消费者线程等待，释放掉list集合的锁
                        list.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
                // 程序能够执行到此处说明仓库中有数据，进行消费。
                Object obj = list.remove(0);
                System.out.println(Thread.currentThread().getName() + "--->" + obj);
                // 唤醒生产者生产。
                list.notifyAll();
            }
        }
    }
}

```

