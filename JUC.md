# juc 并发编程

## 1. 什么是 juc?

juc(java.utils.concurrent)   并发编程包

## 2. 线程和进程?

什么是进程？

进程是资源分配的基本单位

什么是线程？

线程是独立调度的基本单位

## 3. Lock锁

**Lock接口** 是在`java.util.concurrent.locks`下面的一个接口

> ![image-20220623231115466](D:/ProgramFiles/typora/typora-images/image-20220623231115466.png)



**Lock接口** 下面已知的实现类:

![image-20220623231243683](D:/ProgramFiles/typora/typora-images/image-20220623231243683.png)



1. ReentrantLock(可重入锁)

   > 使用方式：
   >
   > ```java
   > class X { 
   >     private final ReentrantLock lock = new ReentrantLock(); 
   >     // ... 
   >     public void m() { 
   >     	lock.lock(); // block until condition holds 
   >             try { 
   >                 // ... method body 
   >             } finally {
   >                lock.unlock() 
   >             } 
   > 	} 
   > }
   > ```



## 4. 生产者和消费者问题

> Objcet类中的**wait()**方法使用注意事项:
>
> ```java
> synchronized (obj) {
>     while (<condition does not hold>) obj.wait(); ... // Perform action appropriate to condition 
> }
> ```



## 5. 8锁现象

锁只会锁两种: 1. 对象 2. Class

## 6. 集合类不安全

1. ArrayList<>()   线程不安全

   > ```java
   > public static void main(String[] args) {
   >     List<String> list = new ArrayList<>();
   >     for (int i = 0; i < 10; i++) {
   >         new Thread(()->{
   >             list.add(UUID.randomUUID().toString().substring(0,3));
   >             list.forEach(System.out::println);
   >         }, i + "").start();
   >     }
   > }
   > 
   > /**
   > 报错：
   > java.util.ConcurrentModificationException
   > */
   > ```
   >
   > 

   ```java
   // 解决办法：
   List<Integer> list = Collections.synchronizedList(new ArrayList<>());  
   
   List<Integer> list1 = new Vector<>();
   
   // 使用JUC包下的    
   List<String> list = new CopyOnWriteArrayList<>();
   
   // 为什么 CopyOnWriteArrayList线程安全？
   
   // 下面源码：
       /**
        * Appends the specified element to the end of this list.
        *
        * @param e element to be appended to this list
        * @return {@code true} (as specified by {@link Collection#add})
        */
       public boolean add(E e) {
           final ReentrantLock lock = this.lock;
           lock.lock();
           try {
               Object[] elements = getArray();
               int len = elements.length;
               Object[] newElements = Arrays.copyOf(elements, len + 1);
               newElements[len] = e;
               setArray(newElements);
               return true;
           } finally {
               lock.unlock();
           }
       }
   ```

2. HashSet<>() 线程不安全

   > ```java
   > public static void main(String[] args) {
   >     Set<String> set = new HashSet<>();
   >     for (int i = 0; i < 5; i++) {
   >         new Thread(()->{
   >             set.add(UUID.randomUUID().toString().substring(0,3));
   >             set.forEach(System.out::println);
   >         }, String.valueOf(i)).start();
   >     }
   > }
   > //报错信息：   java.util.ConcurrentModificationException
   > 
   > //解决办法同ArrayList<>();
   > 
   > //使用Set<String> set =new CopyOnWriteArraySet<>();
   > ```

## 7. Callable接口

简介：Callable也是实现线程的一个接口 

Callable接口有如下特点：

* 可以抛出异常
* 可以有返回值

> 线程的启动方式有new Thread().start; 而Thread() 中的参数没有Callable接口，
>
> 可以看到 Runnable接口下面有一个 **FutureTask** 实现类, 而FutureTask类有一个构造方法中可以传入Callable接口的实现类。
>
> ![image-20220624094353599](D:/ProgramFiles/typora/typora-images/image-20220624094353599.png)
>
> ![image-20220624094103925](D:/ProgramFiles/typora/typora-images/image-20220624094103925.png)
>
> ![image-20220624094635488](D:/ProgramFiles/typora/typora-images/image-20220624094635488.png)

**例子**

```java
//Thread -> Runnable -> FutureTask

public class CallableTest {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        FutureTask<Integer> task = new FutureTask<Integer>(new MyThread());
        new Thread(task).start();
        Integer integer = task.get();
        System.out.println(integer);

        new Thread(task).start();
    }
}
class MyThread implements Callable<Integer>{

    @Override
    public Integer call() throws Exception {
        System.out.println("call is ran");
        return 1;
    }
}
```



## 8. 常用的辅助类

1. **java.util.concurrent.CountDownLatch**

   > 减法计数器。
   >
   > ```java
   > public static void main(String[] args) throws InterruptedException {
   >     CountDownLatch countDownLatch = new CountDownLatch(6);
   >     for (int i = 0; i < 6; i++) {
   >         new Thread(()->{
   >             System.out.println(Thread.currentThread().getName() +  "个学生出去");
   >             countDownLatch.countDown(); //使数量减一
   >         }, i + "").start();
   >     }
   >     /**
   >          * await()方法， 假设countDown()方法使计数器减至0那么这个方法就会结束，然后执行下面的代码
   >          */
   >     countDownLatch.await();
   >     System.out.println("结束");
   > }
   > ```

2. **java.util.concurrent.CyclicBarrier**

   > 加法计数器
   >
   > ```java
   > public static void main(String[] args) {
   >     CyclicBarrier cyclicBarrier = new CyclicBarrier(7, ()->{
   >         System.out.println("七剑合璧");
   >     }); // 当达到七个线程时，执行第二个参数(线程任务)
   >     for (int i = 0; i < 10; i++) {
   >         final int temp = i;
   >         new Thread(()->{
   >             System.out.println("第" + temp + "个剑收集完成");
   >             try {
   >                 cyclicBarrier.await();
   >             } catch (InterruptedException | BrokenBarrierException e) {
   >                 e.printStackTrace();
   >             }
   >         }).start();
   >     }
   > }
   > ```

3. **java.util.concurrent.Semaphore**

   > 假设有三个停车位，占满后不允许在进入停车位，只有离开后，其他车才能进入车位
   >
   > ```java
   > public static void main(String[] args) {
   >     Semaphore semaphore = new Semaphore(3);
   >     for (int i = 0; i < 10; i++) {
   >         new Thread(()->{
   >             try {
   >                 semaphore.acquire();
   >                 System.out.println(Thread.currentThread().getName() + "占了一个车位");
   >                 TimeUnit.SECONDS.sleep(3);// 占用停车位 2 秒
   >                 System.out.println(Thread.currentThread().getName() + "离开了车位");
   >             } catch (InterruptedException e) {
   >                 e.printStackTrace();
   >             }finally {
   >                 //离开停车位，释放
   >                 semaphore.release();
   >             }
   >         }, i + "").start();
   >     }
   > }
   > ```



## 9. 读写锁

> ![image-20220624095944243](D:/ProgramFiles/typora/typora-images/image-20220624095944243.png)

![image-20220624100258703](D:/ProgramFiles/typora/typora-images/image-20220624100258703.png)



**对于一个东西有两种操作，一种是写入操作，一种是读取操作，我们希望是写入操作时，只有一个线程进行写的操作，而读取操作时，可以有多个线程进行读取。**

```java
package com.biienu.rw;

import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

/**
 * @Author: biienu
 * ReadWriteLock 读写锁
 */
public class ReadWriteLockDemo {
    public static void main(String[] args) {
        MyCache2 myCache = new MyCache2();

        for (int i = 0; i < 1000; i++) {
            final String key = i + "";
            new Thread(()->{
                myCache.writeOperation(key, key + "");
            }, String.valueOf(i)).start();
        }
        for (int i = 0; i < 100; i++) {
            final String key = i + "";
            new Thread(()->{
                myCache.readOperation(key);
            }, String.valueOf(i)).start();
        }

    }
}
class MyCache2{

    private ReadWriteLock readWriteLock = new ReentrantReadWriteLock();
    private volatile Map<String, Object> map = new HashMap<>();
    //写操作
    public void writeOperation(String key, Object val){
        readWriteLock.writeLock().lock();
        try{
            System.out.println(Thread.currentThread().getName() + "写操作" + key);
            map.put(key, val);
            System.out.println(Thread.currentThread().getName() + "写操作结束");
        }finally {
            readWriteLock.writeLock().unlock();
        }
    }
    //读取操作
    public void readOperation(String key){
        readWriteLock.readLock().lock();
        try{
            System.out.println(Thread.currentThread().getName() + "读取" + key);
            map.get(key);
            System.out.println(Thread.currentThread().getName() + "读取操作结束");
        } finally {
            readWriteLock.readLock().unlock();
        }
    }
}

class MyCache{

    private volatile Map<String, Object> map = new HashMap<>();
    //写操作
    public void writeOperation(String key, Object val){
        System.out.println(Thread.currentThread().getName() + "写操作" + key);
        map.put(key, val);
        System.out.println(Thread.currentThread().getName() + "写操作结束");
    }
    //读取操作
    public void readOperation(String key){
        System.out.println(Thread.currentThread().getName() + "读取" + key);
        map.get(key);
        System.out.println(Thread.currentThread().getName() + "读取操作结束");
    }
}
```



## 10. 阻塞队列 



![image-20220625191150002](D:/ProgramFiles/typora/typora-images/image-20220625191150002.png)



**四组API**:

| 方式         | 抛出异常 | 有返回值，不抛出异常 | 阻塞等待 | 超时等待 |
| ------------ | -------- | -------------------- | -------- | -------- |
| 添加         | add      | offer                | put()    | offer    |
| 移除         | remove   | poll()               | take()   | poll()   |
| 检测队首元素 | element  | peek                 |          |          |

1. **add、remove测试**

   > ```java
   > public static void test01(){
   >     ArrayBlockingQueue arrayBlockingQueue = new ArrayBlockingQueue<>(3);
   >     System.out.println(arrayBlockingQueue.add(1));
   >     System.out.println(arrayBlockingQueue.add(2));
   >     System.out.println(arrayBlockingQueue.add(3));
   >     System.out.println(arrayBlockingQueue.add(4)); 
   > }
   > //运行结果
   > true
   > true
   > true
   > Exception in thread "main" java.lang.IllegalStateException: Queue full
   > 	at java.util.AbstractQueue.add(AbstractQueue.java:98)
   > 	at java.util.concurrent.ArrayBlockingQueue.add(ArrayBlockingQueue.java:312)
   > 	at com.biienu.blockqueue.BlockQueueDemo.test01(BlockQueueDemo.java:17)
   > 	at com.biienu.blockqueue.BlockQueueDemo.main(BlockQueueDemo.java:10)
   >     
   > =================================================================================
   >     public static void test01(){
   >     ArrayBlockingQueue arrayBlockingQueue = new ArrayBlockingQueue<>(3);
   >     System.out.println(arrayBlockingQueue.add(1));
   >     System.out.println(arrayBlockingQueue.add(2));
   >     System.out.println(arrayBlockingQueue.add(3));
   > 
   >     System.out.println("===============");
   >     System.out.println(arrayBlockingQueue.remove());
   >     System.out.println(arrayBlockingQueue.remove());
   >     System.out.println(arrayBlockingQueue.remove());
   >     System.out.println(arrayBlockingQueue.remove());
   > }
   > 
   > //运行结果
   > true
   > true
   > true
   > ===============
   > 1
   > 2
   > 3
   > Exception in thread "main" java.util.NoSuchElementException
   > 	at java.util.AbstractQueue.remove(AbstractQueue.java:117)
   > 	at com.biienu.blockqueue.BlockQueueDemo.test01(BlockQueueDemo.java:23)
   > 	at com.biienu.blockqueue.BlockQueueDemo.main(BlockQueueDemo.java:10)
   > ```

2. **offer、poll**

   > ```java
   > public static void test01(){
   >     ArrayBlockingQueue arrayBlockingQueue = new ArrayBlockingQueue<>(3);
   >     System.out.println(arrayBlockingQueue.offer(2));
   >     System.out.println(arrayBlockingQueue.offer(2));
   >     System.out.println(arrayBlockingQueue.offer(2));
   >     System.out.println(arrayBlockingQueue.offer(2));
   >     System.out.println("===============");
   >     System.out.println(arrayBlockingQueue.poll());
   >     System.out.println(arrayBlockingQueue.poll());
   >     System.out.println(arrayBlockingQueue.poll());
   >     System.out.println(arrayBlockingQueue.poll());
   > }
   > 
   > //运行结果
   > true
   > true
   > false
   > ===============
   > 2
   > 2
   > 2
   > null
   > ```

3. **put、take**

   > ```java
   > public static void test01() throws InterruptedException {
   >     ArrayBlockingQueue arrayBlockingQueue = new ArrayBlockingQueue<>(3);
   >     arrayBlockingQueue.put(2);
   >     arrayBlockingQueue.put(2);
   >     arrayBlockingQueue.put(2);
   >     arrayBlockingQueue.put(2);
   > 
   > }
   > //由于队列大小只有 3 ，所以在put(2)最后一个2时，会进入一直阻塞状态
   > 
   > ===============================================================================
   >     public static void test01() throws InterruptedException {
   >     ArrayBlockingQueue arrayBlockingQueue = new ArrayBlockingQueue<>(3);
   >     arrayBlockingQueue.put(2);
   >     arrayBlockingQueue.put(2);
   >     arrayBlockingQueue.put(2);
   > 
   >     System.out.println(arrayBlockingQueue.take());
   >     System.out.println(arrayBlockingQueue.take());
   >     System.out.println(arrayBlockingQueue.take());
   >     System.out.println(arrayBlockingQueue.take());
   > } 
   > //由于队列中只有三个元素，在第四次take()时，队列为空，会进入一直阻塞状态
   > ```

4. **offer、poll**

   > ```java
   > public static void test01() throws InterruptedException {
   >     ArrayBlockingQueue arrayBlockingQueue = new ArrayBlockingQueue<>(3);
   >     arrayBlockingQueue.offer(2);
   >     arrayBlockingQueue.offer(2);
   >     arrayBlockingQueue.offer(2);
   >     arrayBlockingQueue.offer(2, 10, TimeUnit.SECONDS);
   > }
   > //程序10秒后解除阻塞
   > ========================================================================
   >     public static void test01() throws InterruptedException {
   >     ArrayBlockingQueue arrayBlockingQueue = new ArrayBlockingQueue<>(3);
   >     arrayBlockingQueue.offer(2);
   >     arrayBlockingQueue.offer(2);
   >     arrayBlockingQueue.offer(2);
   > 
   >     arrayBlockingQueue.poll();
   >     arrayBlockingQueue.poll();
   >     arrayBlockingQueue.poll();
   >     arrayBlockingQueue.poll(10, TimeUnit.SECONDS);
   > }
   > 
   > ```

### 10.1 同步队列

> ```java
> SynchronousQueue<Object> objects = new SynchronousQueue<>();
> 
> //一次只能放入一个元素，只能这个元素被取出来后才能放另一个元素
> 
> 
> 
> ```



## 11. 线程池

> 线程池：三大方法、七大参数、四种拒绝策略

**池化技术**：

程序的运行，本质：占用系统资源，优化资源的使用==》 池化技术。

线程池、连接池、内存池、......创建、销毁过程十分浪费资源

**池化技术**：事先准备好一些资源，有人要用，就来池中拿，用完之后再放进去



**线程池的好处**：

1. 降低资源的消耗
2. 提高响应速度
3. 方便管理

<span style="color:red">线程复用，可以控制最大并发数，管理线程</span>

```java
    public static void main(String[] args) {
//        ExecutorService executorService = Executors.newSingleThreadExecutor();
//        for (int i = 0; i < 10; i++) {
//            executorService.execute(()->{
//                System.out.println(Thread.currentThread().getName()  + " ooo");
//            });
//        }
       
//运行结果显示：只有一个线程运行       
pool-1-thread-1 ooo
pool-1-thread-1 ooo
pool-1-thread-1 ooo
pool-1-thread-1 ooo
pool-1-thread-1 ooo
pool-1-thread-1 ooo
pool-1-thread-1 ooo
pool-1-thread-1 ooo
pool-1-thread-1 ooo
pool-1-thread-1 ooo
    
=====================================================================================

//        ExecutorService executorService = Executors.newFixedThreadPool(4);
//        for (int i = 0; i < 10; i++) {
//            executorService.execute(()->{
//                System.out.println(Thread.currentThread().getName()  + " ooo");
//            });
//        }
    运行结果显示：有四个进程运行
pool-1-thread-1 ooo
pool-1-thread-1 ooo
pool-1-thread-1 ooo
pool-1-thread-1 ooo
pool-1-thread-1 ooo
pool-1-thread-1 ooo
pool-1-thread-1 ooo
pool-1-thread-2 ooo
pool-1-thread-3 ooo
pool-1-thread-4 ooo

=====================================================================================

//        ExecutorService executorService = Executors.newCachedThreadPool();
//        for (int i = 0; i < 10; i++) {
//            executorService.execute(()->{
//                System.out.println(Thread.currentThread().getName()  + " ooo");
//            });
//        }
    运行结果显示：有十个线程在运行
pool-1-thread-1 ooo
pool-1-thread-2 ooo
pool-1-thread-3 ooo
pool-1-thread-4 ooo
pool-1-thread-5 ooo
pool-1-thread-6 ooo
pool-1-thread-7 ooo
pool-1-thread-8 ooo
pool-1-thread-9 ooo
pool-1-thread-10 ooo 
    }
```

### 11.1 七大参数及自定义线程池

> **四个柜台就是最大线程数（maximumPoolSize），柜台1和柜台2两个可以办公（corePoolSize），柜台3和柜台4休息，四个等候区即阻塞队列（workQueue），超时时间（keepAliveTime）线程在这个时间内没有工作自动释放, 拒绝策略（RejectedExecutionHandler）是指四个柜台和四个等候区都有人占着，后面还有人进入银行，对这个人的处理策略**

![image-20220626145543992](D:/ProgramFiles/typora/typora-images/image-20220626145543992.png)

> **七大参数:**
>
> ```java
> // cpu密集型：就是电脑的核数，maximunPoolSize为cpu密集型，可以通过 Runtime.getRuntime().availableProcessors()获取电脑的cpu核数。
> // io密集型：十分消耗io设备的任务
> public ThreadPoolExecutor(int corePoolSize,  //核心线程池大小
>                           int maximumPoolSize,//最大线程个数
>                           long keepAliveTime,//超时时间
>                           TimeUnit unit,//超时单位
>                           BlockingQueue<Runnable> workQueue, //阻塞队列
>                           ThreadFactory threadFactory,  //线程工厂
>                           RejectedExecutionHandler handler) {//拒绝策略
>     //......
> }
> 
> ```
>
> > **四种拒绝策略**：
> >
> > ![image-20220626144149446](D:/ProgramFiles/typora/typora-images/image-20220626144149446.png)
>
> 1. `AbortPolicy`抛出如下异常
>
>    > java.util.concurrent.RejectedExecutionException
>
> 2. `CallerRunsPolicy`
>
>    > 从哪里来回哪里去
>
> 3. `DiscardPolicy`
>
>    > 队列满了不抛出异常
>
> 4. `DiscardOldestPolicy`
>
>    > 和排在最前面的竞争，如果最前面的处理完成了，就可以进入。

> **自定义线程**
>
> ```java
> public static void main(String[] args) {
>     ThreadPoolExecutor threadPoolExecutor = new ThreadPoolExecutor(
>         2,
>         5,
>         2,
>         TimeUnit.SECONDS,
>         new ArrayBlockingQueue<>(3),
>         Executors.defaultThreadFactory(),
>         new ThreadPoolExecutor.DiscardOldestPolicy()
>     );
> 
>     for (int i = 0; i < 10; i++) {
>         threadPoolExecutor.execute(()->{
>             System.out.println(Thread.currentThread().getName() + "  hello  ");
>         });
>     }
> }
> ```
>
> 



## 12. 四大函数式接口

**Lambada表达式、链式编程、函数式接口、Stream流式计算**

> **函数式接口：只有一个方法的接口**
>
> 如：
>
> > ```java
> > @FunctionalInterface
> > public interface Runnable {
> >     /**
> >      * When an object implementing interface <code>Runnable</code> is used
> >      * to create a thread, starting the thread causes the object's
> >      * <code>run</code> method to be called in that separately executing
> >      * thread.
> >      * <p>
> >      * The general contract of the method <code>run</code> is that it may
> >      * take any action whatsoever.
> >      *
> >      * @see     java.lang.Thread#run()
> >      */
> >     public abstract void run();
> > }
> > ```
> >
> > 

代码测试：

**Function** 函数式接口

```java
public static void main(String[] args) {
    
    // Function<par1, par2>  par1 是传入apply(par1 p)的参数类型, par2 是apply()函数的返回值类型
    //        Function<String, String> function = new Function<String, String>() {
    //            @Override
    //            public String apply(String s) {
    //                return s.toUpperCase();
    //            }
    //        };
    Function<String, String> function = (str)->{return str.toUpperCase();};//()->{}; 只有一个参数时可以 parameter->{};
    System.out.println(function.apply("jfkd"));
}
```

**Predicate** 断定型接口

```java
    public static void main(String[] args) {
        //Predicate<String> String是传入test()参数的类型,test()方法返回值为true/false
//        Predicate<String> predicate = new Predicate<String>() {
//            @Override
//            public boolean test(String s) {
//                return s.isEmpty();
//            }
//        };
        Predicate<String> predicate = (str)->{return str.isEmpty();};
        System.out.println(predicate.test("jfdk"));
        System.out.println(predicate.test(""));
    }        
```

**Consumer**消费型接口

```java
    public static void main(String[] args) {
//        Consumer<String> consumer = new Consumer<String>() {
//            @Override
//            public void accept(String s) {
//                System.out.println(s);
//            }
//        };

        Consumer<String> consumer = (str)->{
            System.out.println(str);
        };
        consumer.accept("jksdfla");
    }
```



**Supplier** 提供型接口

```java
    public static void main(String[] args) {
//        Supplier<Integer> supplier = new Supplier<Integer>() {
//            @Override
//            public Integer get() {
//                return 123;
//            }
//        };
        Supplier<Integer> supplier = ()->{
            return 123;
        };
        System.out.println(supplier.get());
    }
```



## 13. Stream流式计算

```java
package com.biienu.functioninterface;

import java.util.Arrays;
import java.util.Comparator;
import java.util.List;

/**
 * @Author: biienu
 * 流式计算
 * 需求：
 * 用一行代码实现
 * 现在有5个用户，进行筛选
 * 1。 id必须是偶数，
 * 2。年龄必须是大于23
 * 3。用户名转为大写字母，
 * 4。用户名字母倒着排序
 * 5。只能输出一个用户
 */
public class StreamDemo {
    public static void main(String[] args) {
        User u1 = new User(1,"a",21);
        User u2 = new User(2,"b",22);
        User u3 = new User(3,"c",23);
        User u4 = new User(4,"d",24);
        User u5 = new User(5,"e",25);
        List<User> users = Arrays.asList(u1, u2, u3, u4, u5);
        users.stream().filter(u->{return u.getId() % 2 == 0;})
                .filter(u-> {return u.getAge() > 23;})
                .map(u->{return u.getName().toUpperCase();})
                .sorted((o1,o2)->{return o2.compareTo(o1);})
                .limit(1)
                .forEach(System.out::println);
    }
}
class User{
    private int id;
    private String name;
    private int age;

    public User(int id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}

```

## 14.ForkJoin的使用

**简介**：

```tex
ForkJoin线程池是java8引入的产物。forkJoin的运作原理， 本质上有点像归并计算。他会将提交大任务按照一定规则拆解（fork）成多个小任务当任务小到一定程度时，就会执行计算执行完成时会和其他的小任务进行合并（join）， 逐步将所有小结果合成一个大结果。
```



```java
package com.biienu.forkjoin;

import java.util.concurrent.ExecutionException;
import java.util.concurrent.ForkJoinPool;
import java.util.concurrent.ForkJoinTask;
import java.util.concurrent.RecursiveTask;
import java.util.stream.IntStream;
import java.util.stream.LongStream;

/**
 * @Author: biienu
 * ForkJoin
 *
 * test1() ：1 - 10_10000_0000的和500000000500000000  执行用时487
 * forkJoin:               结果为:500000000500000000执行用时:4003
 * LongStream:          结果为:500000000500000000执行用时:884
 */


public class ForkJoinDemo extends RecursiveTask<Long> {

    private Long start;
    private Long end;
    private Long temp = 10000L;
    public ForkJoinDemo(Long start, Long end) {
        this.start = start;
        this.end = end;
    }

    public ForkJoinDemo() {
    }
    @Override
    protected Long compute() {
        if(end - start < temp){
            Long sum = 0L;
            for (long i = start; i <= end; i++){
                sum += i;
            }
            return sum;
        } else {
            long mid = end + start >> 1;
            ForkJoinDemo task1 = new ForkJoinDemo(start, mid);
            task1.fork();
            ForkJoinDemo task2 = new ForkJoinDemo(mid + 1, end);
            task2.fork();
            return task1.join() + task2.join();
        }
    }
}
class Test{
    public static void main(String[] args) throws ExecutionException, InterruptedException {
//        test1();
//        test2();
        test3();
    }
    static void test1(){
        long sum = 0;
        long start = System.currentTimeMillis();
        for(int i = 1; i <= 10_0000_0000; i++){
            sum += i;
        }
        long end = System.currentTimeMillis();
        System.out.println("1 - 10_10000_0000的和" + sum +"  执行用时" + (end - start));
    }
    static void test2() throws ExecutionException, InterruptedException {
        long start = System.currentTimeMillis();
        ForkJoinDemo forkJoinDemo = new ForkJoinDemo(0L, 10_0000_0000L);
        ForkJoinPool forkJoinPool = new ForkJoinPool();
        ForkJoinTask<Long> submit = forkJoinPool.submit(forkJoinDemo);
        Long sum = submit.get();
        long end = System.currentTimeMillis();
        System.out.println("结果为:"+sum +"执行用时:" + (end - start));
    }
    static void test3(){

        long start = System.currentTimeMillis();
        long sum = LongStream.rangeClosed(0, 10_0000_0000).parallel().reduce(0, Long::sum);
        long end = System.currentTimeMillis();
        System.out.println("结果为:"+sum +"执行用时:" + (end - start));
    }
}

```



## 15. 异步回调

如ajax。

```java
// 没有返回值的异步回调runAsync

// 先输出22222 再输出 ".....ok"
"CompletableFuture.runAsync(()->{
    try {
        TimeUnit.SECONDS.sleep(2);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    System.out.println(Thread.currentThread().getName() +" ok ");
});
System.out.println("22222");





=====================================================================================
    // 有返回值的异步回调
        CompletableFuture<Integer> completableFuture = CompletableFuture.supplyAsync(() -> {
            System.out.println(Thread.currentThread() + "supplyAsync=>Integer");
            int i = 10 / 0;
            return 123;
        });
        System.out.println(completableFuture.whenComplete((t, u) -> {
            System.out.println("t=>" + t);  // 成功执行时 t=>123   失败时  t=>null
            System.out.println("u=>" + u);  // 成功执行时 u=>null  失败时 u=>java.util.concurrent.CompletionException: java.lang.ArithmeticException: / by zero
        }).exceptionally(e -> {
            System.out.println("e=>" + e);  // e=>java.util.concurrent.CompletionException: java.lang.ArithmeticException: / by zero
            return 555;
        }).get());

//执行结果:
/**
Thread[Thread-0,5,main]supplyAsync=>Integer
t=>null
u=>java.util.concurrent.CompletionException: java.lang.ArithmeticException: / by zero
e=>java.util.concurrent.CompletionException: java.lang.ArithmeticException: / by zero
555
*
```



## 16. JMM

> 谈谈对 Volatile 的理解 

Volatile 是java虚拟机提供的**轻量级的同步机制**

1. 保证可见性
2. <span style="color:red ">不保证原子性</span>
3. 禁止指令重排



![image-20220627214940946](D:/ProgramFiles/typora/typora-images/image-20220627214940946.png)





CSDN 关于JMM详解：[(13条消息) 详解什么是JMM！_阿里官方架构师的博客-CSDN博客_jmm](https://blog.csdn.net/gd_yuzhe/article/details/119031820)



## 17. Volatile 

验证Volatile的三个特性：

1. 保证可见性

   > ```java
   > package com.biienu.jmm;
   > 
   > import java.util.concurrent.TimeUnit;
   > 
   > /**
   >  * @Author: biienu
   >  * JMM java memory model
   >  */
   > public class JmmDemo {
   > 
   >     // 不加 volatile 就会进入死循环
   >     private static volatile int num = 0;
   >     public static void main(String[] args) {
   >         new Thread(()->{
   >             while (num == 0);
   >         }).start();
   >         try {
   >             TimeUnit.SECONDS.sleep(2);
   >         } catch (InterruptedException e) {
   >             e.printStackTrace();
   >         }
   >         num = 1;
   >         System.out.println(num);
   >     }
   > }
   > ```

2. 不保证原子性

   > ```java
   > package com.biienu.jmm;
   > 
   > import java.util.concurrent.atomic.AtomicInteger;
   > 
   > /**
   >  * @Author: biienu
   >  * 验证 volatile的不保证原子性
   >  */
   > public class JmmDemo2 {
   > 
   > //    private static int num = 0;
   >     //使用原子性的包装类Atomic
   >     private static AtomicInteger num = new AtomicInteger(0);
   >     public static void add() {
   >         int andIncrement = num.getAndIncrement();
   >     }
   >     public static void main(String[] args) {
   >         for (int i = 0; i < 20; i++) {
   >             new Thread(()->{
   >                 for (int j = 0; j < 1000000; j++) {
   >                     add();
   >                 }
   >             }).start();
   >         }
   >         while (Thread.activeCount() > 2){
   >             Thread.yield();  //线程让步：当前线程进入就绪状态，和其他线程竞争cpu
   >         }
   >         System.out.println(num);
   >     }
   > }
   > ```

3. 禁止指令重排

   > 保证代码的执行顺序



## 18. 单例模式

1. 饿汉式

   > ```java
   > package com.biienu.singlePattern;
   > 
   > /**
   >  * @Author: biienu
   >  * 单例模式--饿汉模式
   >  * 线程安全
   >  */
   > public class HungryPattern {
   >     private static HungryPattern hungryPattern = new HungryPattern();
   >     private HungryPattern(){
   >         System.out.println(Thread.currentThread().getName() + " ok");
   >     }
   > 
   >     private static HungryPattern getInstance(){
   > 
   >         return hungryPattern;
   >     }
   > 
   >     public static void main(String[] args) {
   >         for (int i = 0; i < 20; i++) {
   >             new Thread(()->{
   >                 
   >             }).start();
   >         }
   >     }
   > 
   > }
   > 
   > ```

2. 懒汉式(双重检验锁)(DCL)  double-checked locking

   > ```java
   > package com.biienu.singlePattern;
   > 
   > /**
   >  * @Author: biienu
   >  * 单例模式--懒汉式
   >  *
   >  */
   > public class LazyPattern {
   >     private volatile static LazyPattern lazyPattern;
   > 
   >     private LazyPattern(){
   >         System.out.println(Thread.currentThread().getName() + "  ok");
   >     }
   >     public static LazyPattern getInstance(){
   >         if(lazyPattern == null){
   >             synchronized (LazyPattern.class){
   >                 if(lazyPattern == null){
   >                     lazyPattern = new LazyPattern();
   >                     // new LazyPattern();执行步骤：
   >                     //1。先开辟一块内存
   >                     //2。执行构造方法，创建对象
   >                     //3。变量lazyPattern 指向内存
   >                     // 由于cpu会重排指令，所以可能执行顺序是 1,3,2也有可能是1,2,3, 假设执行顺序是1,3,2
   >                     // A线程执行到3，还没执行2，此时lazyPattern已经指向了内存，所以lazyPattern不为null,而此时来了一个线程
   >                     // B, B线程判断到lazyPattern 不为null, 会直接返回lazyPattern, 而这个时候A线程还没有执行 2。执行构造方法，创建对象。
   >                     //所以解决这个问题就是禁止指令重排，使用 volatile关键字修饰lazyPattern
   >                 }
   >             }
   >         }
   >         return lazyPattern;
   >     }
   > 
   >     public static void main(String[] args) {
   >         for (int i = 0; i < 10; i++) {
   >             new Thread(()->{
   >                 getInstance();
   >             }).start();
   >         }
   >     }
   > }
   > 
   > ```
   >
   > 



## 19.CAS

```java
package com.biienu.cas;

import java.util.concurrent.atomic.AtomicInteger;

/**
 * @Author: biienu
 */
public class CASDemo {
    public static void main(String[] args) {
        //CAS   compareAndSet();
        AtomicInteger atomicInteger = new AtomicInteger(1998);
        System.out.println(atomicInteger.compareAndSet(1998, 2022));//true
        System.out.println(atomicInteger.get());//2022

        System.out.println(atomicInteger.compareAndSet(2022, 1998));//true
        System.out.println(atomicInteger.get());//1998
    }
}

```

### 19.1 ABA问题

![](D:/ProgramFiles/typora/typora-images/image-20220708102050300.png)

> **ABA问题类似于mysql的乐观锁**

## 20、原子引用（解决ABA问题）

```java
package com.biienu.cas;

import java.util.concurrent.TimeUnit;
import java.util.concurrent.atomic.AtomicStampedReference;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

/**
 * @Author: biienu
 */
public class ABADemo {

    public static void main(String[] args) {
        AtomicStampedReference<Integer> reference = new AtomicStampedReference<>(1,1);
        new Thread(()->{
            int stamp = reference.getStamp();
            System.out.println("a1=>" + stamp);
            reference.compareAndSet(1,2,reference.getStamp(),reference.getStamp() + 1);
            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            int stamp2 = reference.getStamp();
            System.out.println("a2=>" + stamp2);

            //将reference  1->2
            System.out.println("a线程 2->1"+reference.compareAndSet(2, 1, reference.getStamp(), reference.getStamp() + 1));
            int stamp3 = reference.getStamp();
            System.out.println("a3=>" + stamp3);
        },"a").start();

        new Thread(()->{
            int stamp = reference.getStamp();
            System.out.println("b1=>"+stamp);
            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("b线程 1->7"+reference.compareAndSet(1, 7, stamp, stamp + 1));
            System.out.println("b2=>"+reference.getStamp());

        },"b").start();
    }
}
```





## 21. 各种锁的理解

### 1、公平锁、非公平锁

公平锁：非常公平，不能够插队，必须先来后到；

非公平锁：非常不公平，可以插队，（默认是非公平的）

```java
public ReentrantLock() {
    sync = new NonfairSync();
}

public ReentrantLock(boolean fair) {
    sync = fair ? new FairSync() : new NonfairSync();
}
```



### 2、可重入锁

**拿到外面的锁后，可以自动获得里面的锁**

```java
package reentrantLock;

/**
 * @Author: biienu
 * 可重入锁   synchronized
 */
public class ReentrantLockDemo {
    public static void main(String[] args) {
        Phone phone = new Phone();
        new Thread(()->{
            phone.sms();
        },"a   ").start();

        new Thread(()->{
            phone.sms();
        },"b   ").start();
    }
    /**
    运行结果：
    a   sms
    a   call
    b   sms
    b   call
    */
}
class Phone{
    public synchronized void sms(){
        System.out.println(Thread.currentThread().getName() + "sms");
        call();
    }
    public synchronized void call(){
        System.out.println(Thread.currentThread().getName() + "call");
    }
}
```



**Lock锁必须 成对出现，否则死锁**

```
package reentrantLock;

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

/**
 * @Author: biienu
 * 可重入锁   synchronized
 */
public class ReentrantLockDemo2 {
    public static void main(String[] args) {
        Phone2 phone = new Phone2();
        new Thread(()->{
            phone.sms();
        },"a   ").start();

        new Thread(()->{
            phone.sms();
        },"b   ").start();
    }
}

class Phone2{
    Lock lock = new ReentrantLock();
    public void sms(){
        lock.lock();
        try {
            System.out.println(Thread.currentThread().getName() + "sms");
            call();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }
    public synchronized void call(){
        lock.lock();
        try {
            System.out.println(Thread.currentThread().getName() + "call");
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }
}
```

```java
package reentrantLock;

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

/**
 * @Author: biienu
 * 可重入锁   lock
 */
public class ReentrantLockDemo2 {
    public static void main(String[] args) {
        Phone2 phone = new Phone2();
        new Thread(()->{
            phone.sms();
        },"a   ").start();

        new Thread(()->{
            phone.sms();
        },"b   ").start();
    }
}

class Phone2{
    Lock lock = new ReentrantLock();
    public void sms(){
        lock.lock();
        try {
            System.out.println(Thread.currentThread().getName() + "sms");
            call();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }
    public synchronized void call(){
        lock.lock();
        try {
            System.out.println(Thread.currentThread().getName() + "call");
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }
}
```

### 3. 自旋锁



![image-20220708105817220](D:/ProgramFiles/typora/typora-images/image-20220708105817220.png)





```java
package com.biienu.spin;

import java.util.concurrent.TimeUnit;
import java.util.concurrent.atomic.AtomicReference;

/**
 * @Author: biienu
 * 自旋锁
 */
public class SpinLock {
    AtomicReference<Thread> atomicReference = new AtomicReference<>();
    public void myLock(){
        Thread thread = Thread.currentThread();
        System.out.println(thread.getName() + " myLock");
        while(!atomicReference.compareAndSet(null, thread));
    }

    public void myUnlock(){
        Thread thread = Thread.currentThread();
        System.out.println(thread.getName() + " myUnlock");
        atomicReference.compareAndSet(thread, null);
    }
}
class SpinLockTest{
    public static void main(String[] args) {

        for (int i = 0; i < 1000; i++) {
            new Thread(()->{
                SpinLock spinLock = new SpinLock();
                spinLock.myLock();//加锁
                spinLock.myUnlock();

            }, String.valueOf(i)).start();
        }
    }
}
```





### 4、死锁

解决死锁的方法：

1. 使用java自带的`jsp -l`命令，可以查看当前所有进程号
2. 使用`jstack 进程号`找到死锁问题



