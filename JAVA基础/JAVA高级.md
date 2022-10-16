# JAVA高级
## 1、反射
### 1.1、反射的概念
#### 1.1.1、反射（reflection）
1. 反射机制允许程序在运行时，通过反射API
* 获取或加载一个“类本身”的对象。
* 进而，获取类的内部信息。
* 进而，创建对象，操作对象的属性和方法。
2.这种运行时“看透类”的能力，动态获取的类的信息，创建对象，调用方法的功能称为反射。反射被视为动态语言的重要特点。
#### 1.1.2、动态语言
 1. 一般有这样的观点“程序运行时，允许改变程序结构或变量类型，这种语言称为动态语言”。从这个观点看，Perl,Python,Ruby是动态语言，C/C++,Java,C#不是动态语言。
 2. 但Java的反射使得其具有动态语言的特征。JAVA可以在运行时加载、探知、使用编译期间完全未知的类。
#### 1.1.3、反射的特点
1. 反射使代码更加灵活，极大提高了代码的运行时装配能力，强化了多态的特性。反射是构建框架技术的基础。
2. 但是反射的效率要比正常操作低，因为JVM难以对反射中的动态代码进行优化。当然反射的适量使用，性能不会是一个问题。
#### 1.1.4、Class类
1. 一个类被JVM加载，其.class文件被读入内存，JVM自动为之创建一个java.lang.Class对象，这个Class对象称为运行时类，Class对象代表着在内存中的“类本身”。
2. 通过Class对象可以访问该类的信息。
3. 一旦类被加载到JVM中，同一个类将不会被再次载入。
4. Class的对象代表加载入内存的类，Class是类的类，称为元类。
5. 一个类的Class对象代表着在内存中的这个类的“类本身”，和面向对象编程要先建立类一样，对某个类使用反射需要先得到这个类的Class对象。
6. 获得Class对象的4种方式
* 类名.class
* 对象.getClass()
* Class.forName("包名+类名")
* 使用类加载器ClassLoader.getSystemClassLoader().loadClass("包名+类名")

`package com.company.reflex;
public class Cat {
}`

`public class ReflexText {
    public static void main(String[] args) {
        Class<Cat> catClass = Cat.class;
        System.out.println(catClass.getName());

        Cat cat = new Cat();
        Class<? extends Cat> aClass = cat.getClass();
        System.out.println(aClass.getName());

        try {
            Class<?> aClass1 = Class.forName("com.company.reflex.Cat");
            System.out.println(aClass1.getName());
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }

        try {
            Class<?> aClass1 = ClassLoader.getSystemClassLoader().loadClass("com.company.reflex.Cat");
            System.out.println(aClass1.getName());
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}`

## 2、注解（Annotation）
1. 又称为标注，是JDK5.0引入的一种注释机制。与类、接口、枚举是在同一个层次。
2. 注解以“@注解名”进行使用，比如，方法重写中使用过的@Override。
3. 注解可以标注在包、类、字段、方法、局部变量、方法参数等的前面，用来对这些元素进行标注。通过分析工具和开发工具等，可以在编译、类的加载、运行时候，可以对这些注解进行某些处理。
### 2.1、编译检查注解
1. JDK中内置的三个基本注解
2. @Override用于重写方法，编译期会检查被注解的方法，确保该方法是重写了基类的方法，否则会有编译错误。
3. @Deprecated用来指示API已经过时了，不推荐使用。
4. @SuppressWarnings如果你确认程序中的警告没有问题，但是不想看到这些警告，可以用这个注解来抑制编译器警告。
### 2.2、自定义注解、注解处理器
1. 用@interface关键字来声名注解，注解也会生产.class文件。
2. 注解可以有成员变量，在使用注解的时候要给成员变量赋值。可以使用default来定义默认值。用以下方式来声名成员变量：
* 成员类型 成员名();
3. 成员类型可以为基本数据类型、String、Class、enum、Annotation，以及相应的数组。
4. 如果只有一个成员，成员名通常用value。
5. 使用时必须指定参数值，除非它有默认值。格式是“参数名=参数值”，如果只有一个参数成员，且名称为value，也可以直接写“参数值”。
6. 没有成员变量的注解也称为标记，有成员变量的注解称为元数据注解。
### 2.3、编写注解处理器解析注解
1. 使用注解主要目的就是为了可以读取注解并进行使用。
2. 在注解处理器中，可以使用反射机制来从使用注解的类中读取这个类的结构信息（属性，方法，构造器等），并可以获取注解的信息，进行动态处理（运行时这些信息进行处理）。  
```public class CustomAnnotationTest {

    public static void main(String[] args){

        //通过反射获取类的信息
        Class<CustomAnnotationTest> customAnnotationTestClass = CustomAnnotationTest.class;

        for (Method m:customAnnotationTestClass.getMethods()
             ) {
            MyAnnotation annotation = m.getAnnotation(MyAnnotation.class);
            if(annotation!=null){
                System.out.println(m.getName());
                System.out.println(annotation.name());
                System.out.println(annotation.age());
            }

        }
    }

    @MyAnnotation(name = "张三")
    public void test(){

    }

    @MyAnnotation(name = "李四",age = 22)
    public void test1(){

    }
}
//自定义注解
@Retention(RetentionPolicy.RUNTIME)
@interface MyAnnotation{
    String name();
    int age() default 18;
}
```
#### 2.4、元注解（meta-annotation）
元注解是对其他注解进行说明的注解。当自定义一个新的注解类型时，可以使用元注解对其进行说明。
常见的几个元注解
##### 2.4.1、@Target
1. 指明某个注解的作用目标。若不设置，则可以放在所有元素前。
2. 可以使用枚举ElementType的值来添加参数，设定具体的作用目标。
* TYPE:接口、类、枚举、注解
* FIELD：字段、枚举的常量
* METHOD：方法
* PARAMETER:方法参数
* CONSTRUCTOR：构造参数
## 3、多线程
### 3.1、并行与并发
并行：指多个任务同时进行，强调的是各个任务之间的独立性和实时性。计算机中指多个处理器同时执行多个任务。
并发：在一个系统内，当资源有限的情况下，通过并发设计，达到部分并行的效果。计算机中指一个处理器，按一定的设计，尽可能达到同时处理多个任务的效果。
### 3.2、进程与线程
进程：
1. 一个正在进行的程序。一个进程中可以使用多个线程来实现并发设计，从而达到并行的效果。
2. 一个进程有自己的内存资源。
3. 一个进程有自己的生命周期，包括创建、生存、消亡等。
线程：
1. 进程的基本单位。在JAVA程序中，程序从一个主线程（main线程）开始，主线程可以创建其他的线程。
2. 线程也有自己的生命周期。创建、生存、消亡等。
3. 线程是进程的一个组件，一个进程中的多个线程可以共享这个进程的内存等资源。
### 3.3、Java程序、JVM实例、用户线程（User Thread）、守护线程(Daemon Thread)
1. 以JAVA命令启动的JAVA程序，都会新增一个JVM实例。一旦程序结束，JVM实例也就销毁了。每个程序都会获得自己的JVM实例和对应的OS级进程，并且每个JVM实例相互独立。
2. 每个JAVA程序运行在单独JVM实例上，每一个JVM实例对应唯一的一个堆，所以一个JAVA程序中的多个线程也就运行同一个JVM实例之上，因此这些线程之间会共享这块堆内存，鉴于此，多线程在访问堆中数据时需要进行线程同步。
3. 守护线程是为其他的线程提供便利服务的，当用户线程结束时，守护线程也就没有存在的意义了，也会销毁。
4. mian线程就是一个用户线程，GC(垃圾回收器)就是一个守护向线程。GC随主线程从开始到结束。
### 3.4、JAVA中创建线程
`public class Test {
    public static void main(String[] args) throws Exception {
    
        //1、继承Thread类，重写run()方法，在run()方法里面写具体的逻辑。
        //注意用对象调用的时候是调start()方法启动线程，线程启动之后JVM会自动去再掉run()方法。
        //永远不要启动一个线程超过一次，即使这个线程启动完毕，也不要再启动该线程。
        //Thread类里面的run()方法只是一个方法，不能用于启动线程。源码中有标注。
        MyThread myThread=new MyThread();
        myThread.start();
        
        //2、实现Runnable接口，重写run()方法，在run()方法里面写具体的逻辑。还是通过start()方法启动线程。
         Thread runnableThread = new Thread(new MyRunnable());
         runnableThread.start();
         
        //还可以直接写成匿名内部类的方式：
         Thread thread = new Thread(new Runnable() {
            @Override
            public void run() {
                for(int i=0;i<50;i++){
                    System.out.println(Thread.currentThread().getName()+":"+i);
                }

            }
        });
        thread.start();
        
        //3、实现Callable<T>接口，重写call()方法，在call()方法里面写具体的逻辑。
        //特点：有返回值，返回值的类型就是实现该接口时传进去的泛型T。
        FutureTask<String> futureTask=new FutureTask<String>(new MyCallable1());
            Thread thread=new Thread(futureTask);
            thread.start();
            try {
                System.out.println("main线程调用get()");
                if(futureTask.isDone()){
                    String s = futureTask.get();
                    System.out.println(s);
                }else {
                    System.out.println("main:先干其他的事情了");
                    Thread.sleep(2000);
                    String s = futureTask.get();
                    System.out.println(s);
                }
            } catch (InterruptedException |ExecutionException e) {
                e.printStackTrace();
            }
        }
    }
}

class MyThread extends Thread{

    @Override
    public void run() {
        for(int i=0;i<100;i++){
            System.out.println("MyThread："+i);
        }
    }
}

class MyRunnable implements Runnable{

    @Override
    public void run() {
        for(int i=0;i<100;i++){
            System.out.println("MyRunAble："+i);
        }
    }
}

class MyCallable1 implements Callable<String>{

    @Override
    public String call(){
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("线程就绪并已经启动了");
        return "from MyCallable1";
    }
}
`
使用Callable<V>接口创建子线程
1. Runnable接口无返回值，也无法抛出异常，所以就设计了Callable接口
* Callable接口规定的方法是V call(),而Runnable接口的方法是run()。
* V call()方法可抛出异常，而run()方法不能抛出异常。
* V call()方法有返回值，而run()方法无返回值。
* Callable接口配合FutureTask对象一起，创建子线程，执行V call()方法，并取得执行结果。
2. 实现原理
* 实现Callable<V>接口，在其V call()方法里面写需要子线程执行的代码，并将返回值以V类型返回。
* Future接口提供了V get()方法，用于得到返回值。
* FutureTask类实现了RunnableFuture接口，这个接口继承自两个接口：Runnable和Future。在创建FutureTask对象的时候，可以把Callable接口的实现对象传入。这样FutureTask对象因为实现了Runnable接口，可以用于创建子线程，因为有了Callable接口的实现对象的引用，可以调用回调方法V call(),得到返回值V，因为其实现了Future接口，可以通过V get()方法提供返回值给外部。
* 这是一种异步回调V call()方法的方式。
### 3.5、线程中常用的方法
1. isAlive()判断线程是否存活
2. Thread.sleep(1000)静态方法，让当前线程休眠一段时间，处于阻塞状态。
3. Thread.currentThread().getName()得到当前线程的名字。
4. hread.yield();//静态方法，让出一次当前CPU的使用权,若让出之后其他线程抢不到，那还可以抢回来
示例代码：

`public class TestThread {
    public static void main(String[] args) throws InterruptedException {

        MyThread myThread1 = new MyThread("我的线程");
        myThread1.start();

        System.out.println(myThread1.isAlive());//isAlive()判断线程是否存活
        for(int i=0;i<100;i++){
            Thread.sleep(1000);//静态方法，让当前线程休眠一段时间，处于阻塞状态
            System.out.println(Thread.currentThread().getName()+":"+i);
        }

        System.out.println(myThread1.isAlive());
    }

    public static class MyThread extends Thread{

        public MyThread(String name) {
            super(name);
        }

        @Override
        public void run() {
           for(int i=0;i<100;i++){
               try {
                   if(i%10==0){
                       Thread.yield();//静态方法，让出当前CPU的使用权,若让出之后其他线程抢不到，那还可以抢回来
                   }
                   System.out.println(this.getName()+"休息一秒："+i);
                   Thread.sleep(1000);//线程休眠
               } catch (InterruptedException e) {
                   e.printStackTrace();
               }
           }
        }
    }
}`
5. join():A线程调用B线程的方法，则A线程阻塞，必须等到B线程执行完以后，A线程才会结束阻塞，继续执行。

`
/**
 * @Description: 启用两个子线程，分别求0-50和51-100的和，并返回给主线程，由主线程求出最终的和
 * @Author: yangyb
 * @Date:2022/5/23 11:58
 * Version: 1.0
 **/
public class TestSum {

    public static void main(String[] args){
        SumThread sumThread = new SumThread(1, 50);
        SumThread sumThread1 = new SumThread(51, 100);

        sumThread.start();
        sumThread1.start();

        try {
            sumThread.join();
            sumThread1.join();
            int sum=sumThread.get()+sumThread1.get();
            System.out.println(sum);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

class SumThread extends Thread {
    int num1,num2,sum;

    public int get(){
        return sum;
    }

    public SumThread(int num1,int num2){
        this.num1=num1;
        this.num2=num2;
    }

    @Override
    public void run() {
        add();
    }

    public void add(){

        for(int i=num1;i<=num2;i++){
            sum+=i;
        }
    }
}`

6. 中断线程：
过时的（deprecated）方法：stop()、suspend()、destroy()、resume()，避免使用，比如stop()方法，使得调用者可以立即终止一个线程，而这个线程可能在进行某些不适合立刻中断的操作，否则会引起比如，被迫释放锁，引起数据不一致，文件，数据库等资源未能关闭等。
interrupt():
A线程调用B线程的此方法，通知B线程，要中断B线程，B线程在自己的run方法里面，通过isInterrupt()或Interrupted()获知此信息，自行决断做何操作。
B线程并不会中断，这个方法只会给线程设置一个为true的中断标志，一旦线程正在调用wait(),join(),sleep()方法中的一种，会立刻抛出一个InterruptedException,B线程可以在catch块中进行进一步处理。
`public class TestInterrupt {

    public static void main(String[] args){

        MyInterruptThread myInterruptThread = new MyInterruptThread();
        myInterruptThread.start();

        try {
            Thread.sleep(1000);
            System.out.println(Thread.currentThread().getName()+"沉睡了一秒钟");
            System.out.println(Thread.currentThread().getName()+"通知子线程中断");
            myInterruptThread.interrupt();//中断子线程
            System.out.println(Thread.currentThread().getName()+"子线程中断完毕");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        myInterruptThread.interrupt();
       }
}

class MyInterruptThread extends Thread{

    @Override
    public void run() {
        try {
            System.out.println("子线程分配资源");
            sleep(5000);
            System.out.println(Thread.currentThread().getName()+"沉睡了五秒钟");
        } catch (InterruptedException e) {
            System.out.println("子线程释放资源");
        }
    }
}
`
### 3.6、线程的优先级
1. JAVA线程中的优先级有1到10共10个等级，等级数越大，优先级越高。有三个常量的等级值：MIN_PRIORITY = 1;NORM_PRIORITY = 5（默认）;MAX_PRIORITY = 10;
2. 可以通过getPriority()方法，得到该值；可以通过setPriority()设置该值。
3. 优先级的设置应该在启动该线程，即调用start()方法之前被设置。
4. 使用CPU资源的时候：同优先级，排队服务，先到先得；在资源有限的情况下，优先级的作用才能体现出来；优先级更多的是给操作系统调度器一个“hint”,至于如何处理，那要看操作系统的调度器实现；线程的高优先级不可以作为线程被优先执行的保证，不要依赖于优先级来写程序；一般对于所有线程使用默认优先级即可。
### 3.7、线程的同步机制
#### 3.7.1、举例
创建一个Cat类，包含一个属性age,一个增加年龄的方法addAge(),一个减少年龄的方法minusAge();

`public class Cat {

    int age;
    
    void addAge(){
        System.out.println(Thread.currentThread().getName()+":"+age);
        age++;
        System.out.println(Thread.currentThread().getName()+":"+age);
    }

    void minusAge(){
        System.out.println(Thread.currentThread().getName()+":"+age);
        age--;
        System.out.println(Thread.currentThread().getName()+":"+age);
    }
    
}
`
创建一个测试类，里面创建两个线程，一个用来增加年龄，一个用来减少年龄，看是否能按照我们所想的输出：0110。

`public class Test {

    public static void main(String[] args){

        Cat cat=new Cat();
        //通过Runnable接口创建两个线程
        Thread thread=new Thread(new Runnable(){
            @Override
            public void run(){
                cat.addAge();
            }
        });

        Thread thread1=new Thread(new Runnable(){

            @Override
            public void run(){
                cat.minusAge();
            }
        });

        //一定要记得启动线程
        thread.start();
        thread1.start();
    }
}`
显然，通过运行程序我们发现与我们预期的结果不一样，发生了数据不一致的问题。
#### 3.7.2、解决方案
对于同一资源，有多个线程同时访问，导致出现数据不一致的问题，如果我们的程序出现了这种安全隐患问题，就需要进行限制，让同一资源在同一个时间只能被一个线程访问，这种线程访问的安全机制就是“线程同步机制”。
同步指的是让一个个线程排队执行，同指“协同”。
只要在多线程共同访问的方法前面加上synchronized(同步修饰符)，或者把共同运行的代码使用同步代码块synchronized(同步锁提供对象){}括起来，就可以实现同步访问。

1.synchronized(同步修饰符)
`public class Cat {

    int age;
    
    public synchronized void addAge(){
        System.out.println(Thread.currentThread().getName()+":"+age);
        age++;
        System.out.println(Thread.currentThread().getName()+":"+age);
    }

    public synchronized void minusAge(){
        System.out.println(Thread.currentThread().getName()+":"+age);
        age--;
        System.out.println(Thread.currentThread().getName()+":"+age);
    }
}`

2.同步代码块synchronized(同步锁提供对象){}
`public class Test {

    public static void main(String[] args){

        Cat cat=new Cat();
        //通过Runnable接口创建两个线程
        Thread thread=new Thread(new Runnable(){
            @Override
            public void run(){
                synchronized(cat){
                    cat.addAge();
                }
            }
        });

        Thread thread1=new Thread(new Runnable(){

            @Override
            public void run(){
                synchronized(cat){
                    cat.minusAge();
                }
            }
        });

        //一定要记得启动线程
        thread.start();
        thread1.start();
    }
}`

### 3.8、同步锁
1. JAVA中的每个对象都有一把锁，称为固有锁，监视器，同步锁，同步监视器，一般简称锁。
2. 如果一个线程想要独占的执行某个方法，或者某段代码，就需要获得一把锁，把这个方法，或者这段代码锁住，这段时间只允许自己访问。
3. 进入对象的非静态同步方法（synchronized）,默认会使用这个对象this的锁。如果是第一个进去，将自动获得这个锁，但是如果别的线程已经获得，那只能排队等待（同步阻塞）。注意，一个对象上的多个方法，如果都是synchronized修饰，那么他们共用这把锁，因为一个对象只有一个锁。
4. 关键字synchronized也可以用于声名类的静态方法，这时候提供锁的对象是类本身的对象，即类名.class。
5. 锁并不能阻止对象的非同步方法被多线程访问。
6. 进入同步块，则需要借某个对象的锁。如果几个线程想要使用同步一个或者几个代码块，那么他们要使用同一个对象的锁，至于这个对象是什么，并不重要。一般可以使用一个都可以访问到的对象，也可以创造一个Objected类型的对象，甚至可以用某个类的本身即类名.class，这也是一个对象。在用this作为同步锁对象的时候，要注意这个或者这些同步代码块的this是不是同一个对象。
7. synchronized代码块包含住操作共享资源的部分就可以，不要包含过多代码，否则会影响效率，或者影响其他线程使用被包住的非共享资源部分的代码。
8. synchronized方法只能同步方法的本身，即一个线程执行此方法的时候，其他线程不能执行此方法，并且不能执行此对象上的任何synchronized方法，因为这些方法用的是对象本身这同一把锁。但如果一个线程需要调用这个对象的几个synchronized方法完成一个复合的同步操作，这个中间会有间隔，就会释放锁，那么需要使用同步代码块来将几个方法一起锁住（同步）。
9. 因为同步方法在多线程同时访问的时候需要等待，同步方法的执行效率低于非同步方法。 
#### 3.8.1、Lock同步锁
1. 同步方法、同步代码块都是通过同步关键字synchronized来实现的,JDK5以后，提供了一种方式来实现同步访问，即Lock,在java.util.concurrent.locks里面。
2. Lock是接口，而synchronized是关键字，是通过内置语言来实现的。
3. ReentrantLock是Lock接口的实现类，是可重入锁，即如果已经有一个对象锁，那么当它调用同一对象的其他同步方法的时候，就不需要再次申请这个对象的锁。
4. ReentrantLock和synchronized都是可重入锁。
5. lock(),手动加锁
6. ReentrantLock(true)传入true,表示等待锁的线程，按先到先得的顺序公平使用锁，那就是公平锁；不传该参数，这个参数默认为false，就是非公平锁，所以ReenTrantLock默认就是非公平锁，无法保证等待的线程获取锁的顺序。
7. tryLock(1, TimeUnit.MILLISECONDS),尝试拿到锁，若成功则返回true,失败，会等待一段时间，再次尝试拿锁。
8. tryLock()尝试拿到锁，若成功则返回true,失败，返回false.
9. unlock(),手动解锁。若忘记释放锁，则有可能造成死锁，即使发生异常也要释放锁，为了避免这种情况的发生，一般写在finally块中解锁。
示例代码：

`public class Testlock {

    public static void main(String[] args){

        Cat cat = new Cat();

        Lock lock=new ReentrantLock();//创建一个可重入锁
        new Thread(new Runnable(){

            @Override
            public void run() {
                try {
                    if (lock.tryLock(1, TimeUnit.MILLISECONDS)) {
                        try {
                            lock.lock();//加锁
                            System.out.println(Thread.currentThread().getName() + ":" + cat.getAge());
                            cat.addAge();
                            System.out.println(Thread.currentThread().getName() + ":" + cat.getAge());
                        } finally {
                            lock.unlock();//释放锁
                        }
                    }
                } catch (InterruptedException e) {
                    System.out.println(Thread.currentThread().getName() + "没有拿到锁");
                }
            }
        }).start();

        new Thread(new Runnable(){

            @Override
            public void run(){
                try {
                    if (lock.tryLock(1, TimeUnit.MILLISECONDS)) {
                        try {
                            lock.lock();//加锁
                            System.out.println(Thread.currentThread().getName() + ":" + cat.getAge());
                            cat.minusAge();
                            System.out.println(Thread.currentThread().getName() + ":" + cat.getAge());
                        }finally{
                            lock.unlock();//释放锁
                        }
                    }
                } catch (InterruptedException e) {
                    System.out.println(Thread.currentThread().getName() + "没有拿到锁");
                }
            }
        }).start();
    }
}
`
`public class Cat {

        int age;
        void addAge(){
            age++;
        }

    void minusAge(){
        age--;
    }

    public int getAge(){
        return age;
    }
}`

#### 3.8.2、死锁
两个或多个线程，在各自使用一把锁的时候，为了完成当前的执行，需要使用对方正在使用的锁，这样一来，双方都在等待对方释放手中的锁，就造成循环等待，发生了死锁。死锁发生后，并不会发生异常，难以调试。
`public class DeadLock {

    public static void main(String[] args){

        Object o1=new Object();
        Object o2=new Object();

        new Thread(new Runnable(){

            @Override
            public void run(){
                synchronized(o1){
                    try {
                        Thread.sleep(1000);
                    }catch (InterruptedException e){
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName()+"等待o2的锁");

                    synchronized(o2){
                        System.out.println(Thread.currentThread().getName()+"执行了代码");
                    }
                }
            }
        }).start();

        new Thread(new Runnable(){

            @Override
            public void run(){
                synchronized(o2){
                    try {
                        Thread.sleep(1000);
                    }catch (InterruptedException e){
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName()+"等待o1的锁");

                    synchronized(o1){
                        System.out.println(Thread.currentThread().getName()+"执行了代码");
                    }
                }
            }
        }).start();
    }
}`
避免死锁：
1. 嵌套锁，即已经锁住了 一个资源，同时再锁住另一个资源。在多线程的情况下，完全避免一个线程使用多个锁来完成任务，并不现实。所以理清锁的顺序，避免交互加嵌套锁造成死锁。
2. 可以通过Lock回避死锁。比如使用tryLock()回避死锁问题。 
### 3.9、wait-notify机制
1. 每个对象从Object类继承下来，就拥有了一把锁（监视器），wait(),notify(),notifyAll()方法，一个锁池，一个等待池。
2. 线程可以在synchronized的同步方法，和同步块里面使用一个对象锁来锁定资源，也可以调用对象的这几个方法实现wait和notify。
3. 注意，对象的wait(),notify(),notifyAll()方法只能在对象自己锁定的同步代码块或者同步方法中被调用。
4. 如果一个线程想要获取锁，若果锁已经被占用了，则此线程进入锁池，称为阻塞(blocked)。
5. 如果线程t已经拥有锁，在需要某些条件，暂时不执行了，则可以调用锁对象.wait()，释放锁，进入等待池，等待条件满足了，别的线程调用锁对象.notify()后，线程t从等待池再次进入锁池，等到线程t竞争到锁后，再从wait()方法处向后执行。
6. 也可以调用锁对象.wait(时间)，如果没有等到notify，等待的时间到，则进入锁池。
7. 锁对象.notify()，从等待池中随机选一个线程，放入锁池。
8. 锁对象.notifyAll()，将所有等待池中的线程，放入锁池。
9. 两个或者多个线程，可以使用同一把锁的wait(),和notify()方法相互协作，实现线程间的通讯。

`public class WaitNotify {

    public static void main(String[] args){

        Object o = new Object();

        new Thread(new Runnable() {
            @Override
            public void run() {
                synchronized (o) {
                    System.out.println("2、子线程通知主线程，主线程满足条件，可以进入该对象的锁池！");
                    o.notify();
                }
                System.out.println("3、子线程释放锁！");
            }
        }).start();

        synchronized (o){
            try {
                System.out.println("1、主线程不满足条件，进入该对象的等待池！");
                o.wait();
                System.out.println("4、主线程满足，从等待池释放进入该对象的锁池！，并且拿到了锁，继续执行");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}`
### 3.10、park和unpark

1. park()和unpark()不是线程的方法，也不是锁对象的方法，而是LockSupport类中的两个静态方法，这个类位于java.util.concurrent.locks包中。
2. 用法：
* 在线程t1中调用LockSupport.park()，用于暂停当前线程t1的运行。
* 在另外的线程调用LockSupport.unpark(t1)，恢复线程t1的运行。
* 比如：t1先park,过段时间t2中再unpark(t1),t1继续从park处执行；或者t2先提前unpark(t1),过段时间t1再park,t1会继续执行，注意，unpark不能累加。这两种顺序，代码都可以执行。
* 注意，park和unpark虽然也可以实现线程间通讯，但是与wait-notify不同，不需要在synchronized同步代码块中进行。
* 也可以执行park的时间，如果时间到了，没有等到unpark，也会恢复线程运行。parkNanos(纳秒)，1纳秒=十亿分之一秒；parkUntil(截止时间)，截止时间为从Epoch(1970年1月1午夜)开始的毫秒数。

`public class ParkAndUnpark {

    public static void main(String[] args){

        Thread t1 = new Thread(new Runnable() {

            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName()+":t1开始执行");
                park();
                System.out.println(Thread.currentThread().getName()+":t1执行结束");
            }
        });

        Thread t2 = new Thread(new Runnable() {

            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName()+":t2开始执行");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                unpark(t1);
                System.out.println(Thread.currentThread().getName()+":t2执行结束");
            }
        });

        t1.start();
        t2.start();
    }
}`
### 3.10、比较sleep和wait
1. 两个方法都是使用当前线程释放CPU，进入睡眠或等待。
2. sleep()是线程的静态方法，通过Thread.sleep()来调用；而wait()是锁对象的方法。
3. sleep()不强制要求和synchronized配合使用，而wait()方法需要和synchronized一起使用。
4. 使用Thread.sleep()使得当前线程进入睡眠，当前线程如果持有锁，并不会释放锁；而锁对象.wait()使得当前线程进入当前锁对象的等待池，会释放锁。
### 3.11、比较park-unpark和wait-notify
1. 两者都可以实现线程间通讯。
2. wait-notify是属于锁对象的，需要在锁的synchronized同步代码中被锁对象调用；park和unpark与synchronized同步代码无关，是属于LockSupport的静态方法，直接就可以执行。
3. unpark可以具体唤醒某个线程，而notify只能随机唤醒一个等待的线程，notifyALL是唤醒所有等待线程。
4. park和unpark可以先unpark,这个unpark依然可以对后面的park起作用；而wait-notify如果先notify，则不会对后面的wait起作用。
### 3.12、回调（Callback）
1. 回调方法是指一个方法当一个事件发生的时候被调用，通常可以使用接口来定义回调方法，再把一个接口的实现传递到负责触发这个事件的系统中去。
2. 简单的说:回调是指把一段代码，传递给另外一段代码，让第二段代码来执行第一段代码。
### 3.13、同步调用和异步调用
1. 同步调用是基本的调用方式，调用者要等待（被阻塞）被调用者返回，才会继续执行。一般的方法调用，就是同步调用。比如：线程t1中调用类B的方法b()，一直等待b()方法执行完毕，t1才继续往下走。
2. 异步调用，调用者不需要等待（非阻塞）被调用者返回，调用者在调用被调用者后，会立即继续执行。比如：线程t1通过新启动线程的方式调用另外的代码，t1的代码会接着往下执行。
3. 这两种调用方式的最大区别，就是同步调用需要等待被调用者返回，而异步调用不需要等待。使用多线程的异步调用方式，可以在被调用线程执行号是任务的时候，调用线程可以继续执行代码，这是一种非阻塞的处理。
### 3.14、线程池
1. 对于大量使用线程，并且线程执行时间比较短，就会需要频繁创建和销毁线程，线程的创建和销毁比较耗费计算机资源，也提高了软件编写中对于线程调度的要求。
2. 使用线程池，可以维护多个线程供直接使用，复用，并可以获得线程池提供的对于线程的管理和调度。
* 节约资源：线程创建和销毁造成比较耗费资源，重复使用现成的线程可以节约资源。
* 提高响应：可以立刻拿现成的线程使用，不需要等待线程创建。
* 可管理性：可以对线程进行统一的分配，管理，和优化。
3. 线程池原理
* 线程池里面使用了一个生产者-消费者模型：任务管理和线程管理。
* 用户将任务，即业务逻辑写在Runnable接口实现类或者Callable接口实现类里面，然后提交给线程池。
* 线程池会给任务分配线程，让线程运行任务。
4. 线程池的创建
 * 使用Executors工具类来创建线程池对象ThreadPoolExecutor，以ExecutorService接口返回。
 * 通过ExecutorService接口来执行任务。
 * 使用完线程池，使用ExcutorService要关闭shuntdown()。
 * 可以通过ThreadPoolExecutor可以设置线程池配置参数，一般不需要设置，Executors创建线程池的时候已经默认设置。
 
`public class PoolTest {

    public static void main(String[] args){

        ExecutorService executorService=Executors.newFixedThreadPool(10);
        executorService.execute(new MyRun());
        executorService.shutdown();

    }
}

class MyRun implements Runnable{

    @Override
    public void run(){

        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(Thread.currentThread().getName()+":执行了任务！");
    }
}
`
### 3.15 线程池的拒绝策略
线程池中的线程已经用完了，无法继续为新任务服务，同时，等待队列也已经排满了，再也
塞不下新任务了。这时候我们就需要拒绝策略机制合理的处理这个问题。
JDK 内置的拒绝策略如下：
1. AbortPolicy ： 直接抛出异常，阻止系统正常运行。
2. CallerRunsPolicy ： 只要线程池未关闭，该策略直接在调用者线程中，运行当前被丢弃的
任务。显然这样做不会真的丢弃任务，但是，任务提交线程的性能极有可能会急剧下降。
3. DiscardOldestPolicy ： 丢弃最老的一个请求，也就是即将被执行的一个任务，并尝试再
次提交当前任务。
4. DiscardPolicy ： 该策略默默地丢弃无法处理的任务，不予任何处理。如果允许任务丢
失，这是最好的一种方案。
以上内置拒绝策略均实现了 RejectedExecutionHandler 接口，若以上策略仍无法满足实际
需要，完全可以自己扩展 RejectedExecutionHandler 接口。
### 3.16、线程的生命周期
1. Thread.State
* NEW(新建)：还没有调用start();
* RUNNABLE(可运行的)：可以运行的状态，进一步分为就绪(READY，等待CPU使用权)与运行(RUNNING,正在运行)。
* BLOCKED(阻塞)：现在在等待监视器锁，即synchronized设定的对象内置锁。
* WAITING（等待）：线程无期限等待，等待其他线程执行一个特定动作。
* TIMED_WAITING(限时等待)：线程等待其他线程执行一个特定动作，最多等待设定的时间。
* TERMINATED(终止)：线程生命周期结束。
## 4、JVM
### 4.1、内存模型
![](vx_images/317815120226235.png =702x)
1. 程序计数器（线程私有）：计数器记录的是虚拟机字节码指令集的地址（当前指令的地址）
2. 虚拟机栈（线程私有）：每个方法在执行的时候也会创建一个栈帧，存储了局部变量，操作数，动态链接，方法返回地址。
3. 本地方法栈：主要为虚拟机使用到的Navite方法服务。
4. 堆：被所有线程共享的一块区域，在虚拟机启动的时候，用于存放对象实例。
5. 方法区：存储已经被虚拟机加载的类信息，常量，静态变量等。
### 4.2、GC算法
#### 4.2.1、标记清除算法
通过可达性分析法，标记出引用计数为0的可以被垃圾回收机器回收的对象，清除阶段回收这些被标记的对象所占的空间。
![](vx_images/378381921248675.png =727x)A
优点：简单；缺点：内存碎片化。
#### 4.2.2、复制算法
按内存容量将内存划分为等大小的两块。每次只使用其中一块，当这一块内存满后将尚存活的对象复制到另一块上去，把已使用的内存清掉。
![](vx_images/122342021237357.png =471x)
优点：吞吐量高、无碎片化。缺点：空间利用率低。
#### 4.2.3、标记整理算法
标记后不是清理对象，而是将存活对象向内存的一端，然后清除端边界的对象。
![](vx_images/313262021230491.png =419x)
优点：兼顾了空间和时间的问题；缺点：效率非最优。
#### 4.2.3、分代回收算法
分代收集是目前大部分JVM所采用的方法，其核心思想是根据对象存活的不同生命周期将内存划分为不同的域，一般情况下将GC堆划分为老生代（Tenured/Old Generation）和新生代（Young Generation）。老生代的特点是每次垃圾回收时只有少量对象需要被回收，新生代的特点是每次垃圾回收时都有大量的垃圾被回收，因此可以根据不同区域选择不同的算法。
### 4.3、双亲委派机制
双亲委派的指的是，类加载由最底下的自定义加载器开始，将加载请求委派给上级，如果上级已经加载过该类，则直接加载完成，如果没有则继续递归至顶级Bootstrap加载器，然后父类判断如果该类不属于自己的加载范畴则委派子类加载。继续往下递归。
双亲委派的目的是，防止加载同一个.class。通过委托去询问上级是否已经加载过该.class，如果加载过了，则不需要重新加载。
通过委托的方式，保证核心.class不被篡改，即使被篡改也不会被加载，即使被加载也不会是同一个class对象，因为不同的加载器加载同一个.class也不是同一个Class对象。
这样则保证了Class的执行安全
## 5、Git
git pull origin master --allow-unrelated-histories (该选项可以合并两个独立启动仓库的历史)
git push -u origin master
## 6、函数式编程
### 6.1、函数式编程的概念
函数式编程主要关注的是对数据进行什么操作。
### 6.2、函数式编程的优点
代码简洁、开发快速。
接近自然语言，易于理解。
易于“并发编程”。
## 7、JAVA缓存
### 7.1、缓存分类
#### 7.1.1、单机缓存
如ehcache、guava cache
##### 7.1.1.1、ehcache

#### 7.1.2、分布式缓存
如redis
## 8、JUC
### 8.1、Future接口
#### 1.1、优点：
Future接口（FutureTask实现类）定义了操作异步任务执行的一些方法，如获取异步任务的执行结果、取消任务的执行、判断任务是否被取消、判断任务执行是否完毕等。
Future接口可以为主线程开一个分支任务，专门为主线程处理耗时和费力的复杂业务。

 1. 处理三个任务，只用一个main线程

```java
import java.util.concurrent.TimeUnit;

/**
 * @Description: 主线程
 * @Author: yangyb
 * @Date:2022/9/18 23:47
 * Version: 1.0
 **/
public class FutureThreadPoolDemo {

    public static void main(String[] args){

        //3个任务目前只有一个main来处理？会耗时多长时间
        long startTime = System.currentTimeMillis();
        try {
            TimeUnit.MILLISECONDS.sleep(300);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        try {
            TimeUnit.MILLISECONDS.sleep(500);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        try {
            TimeUnit.MILLISECONDS.sleep(200);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        long endTime = System.currentTimeMillis();

        System.out.println("共用"+(endTime-startTime)+"毫秒");
    }

}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/45e3f9a11a484673afda02ca64c1dd20.png)
 2. 处理三个任务，用三个异步任务

```java
package com.company.async;

import java.util.concurrent.*;

/**
 * @Description: FutureTask
 * @Author: yangyb
 * @Date:2022/9/14 22:06
 * Version: 1.0
 **/
public class TestOne {
    public static void main(String[] args) throws ExecutionException, InterruptedException {

        ExecutorService threadPool = Executors.newFixedThreadPool(3);

        long startTime = System.currentTimeMillis();
        FutureTask<String> futureTask1 = new FutureTask<>(() -> {
            TimeUnit.MILLISECONDS.sleep(500);
            return "task1 over";
        });
        threadPool.submit(futureTask1);
        System.out.println(futureTask1.get());

        FutureTask<String> futureTask2 = new FutureTask<>(() -> {
            TimeUnit.MILLISECONDS.sleep(500);
            return "task2 over";
        });
        threadPool.submit(futureTask2);
        System.out.println(futureTask2.get());

        FutureTask<String> futureTask3 = new FutureTask<>(() -> {
            TimeUnit.MILLISECONDS.sleep(500);
            return "task3 over";
        });
        threadPool.submit(futureTask3);
        System.out.println(futureTask3.get());
        long endTime = System.currentTimeMillis();
        System.out.println("共用" + (endTime - startTime) + "毫秒");
        threadPool.shutdown();
    }

}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/db1b18ff71514c72a4be062ea7c2876a.png)
## 1.2、缺点
1、get()方法容易引起阻塞。

```java
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;
import java.util.concurrent.TimeUnit;

/**
 * @Description: 阻塞
 * @Author: yangyb
 * @Date:2022/9/19 22:13
 * Version: 1.0
 **/
public class FutureAPIDemo {

    public static void main(String[] args) throws InterruptedException, ExecutionException {
        FutureTask<String> futureTask = new FutureTask<>(() -> {
            System.out.println("futureTask 启动！");
            long startTime = System.currentTimeMillis();
            TimeUnit.MILLISECONDS.sleep(5000);
            long endTime = System.currentTimeMillis();
            System.out.println(endTime-startTime);
            return "futureTask over!";
        });

        Thread thread = new Thread(futureTask, "t1");
        //启动线程
        thread.start();
        System.out.println(futureTask.get());
        System.out.println("主线程还可以做其它的事儿吗？不好意思，做不了，futureTask没结束，main线程只能等它结束！");
    }

}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/fd4151b1e4044130b02bb14a77eaf81d.png)
2、isDone()轮询
轮询的方式会耗费无所谓的CPU资源，而且也不见得能及时地得到计算结果，如果想要异步获取结果，通常都会以轮询的方式去获取结果，尽量不要阻塞。

```java
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;
import java.util.concurrent.TimeUnit;

/**
 * @Description: 轮询
 * @Author: yangyb
 * @Date:2022/9/19 22:13
 * Version: 1.0
 **/
public class FutureAPIDemo {

    public static void main(String[] args) {
        FutureTask<String> futureTask = new FutureTask<>(() -> {
            System.out.println("futureTask 启动！");
            long startTime = System.currentTimeMillis();
            TimeUnit.SECONDS.sleep(5);
            long endTime = System.currentTimeMillis();
            System.out.println(endTime - startTime);
            return "futureTask over!";
        });

        Thread thread = new Thread(futureTask, "t1");
        //启动线程
        thread.start();
        System.out.println("主线程先去做其他的事儿！");
        while (true) {
            if (futureTask.isDone()) {
                try {
                    System.out.println(futureTask.get());
                } catch (InterruptedException | ExecutionException e) {
                    e.printStackTrace();
                }
                break;
            } else {
                try {
                    TimeUnit.MILLISECONDS.sleep(500);
                    System.out.println("正在执行。。。。");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

            }

        }


    }

}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/ac3b0aa291754a4f82252c38cf098f38.png)
## 1.3、结论
Future对于结果的获取不是很友好，只能通过阻塞或轮询的方式得到任务的结果。
## 9、设计模式
### 9.1、单例模式
。。。。。。。。。。。。。
### 9.2、工厂模式
。。。。。。。。。。。。。。
### 9.3、策略模式
。。。。。。。。。。。。
### 9.4、适配器模式
。。。。。。。。。。。。
### 9.5、代理模式
。。。。。。。。。。。。。。
### 9.6、模板模式
