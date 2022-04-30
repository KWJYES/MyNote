# 一、开启一个子线程

## 法1：继承Thread类

```java
public class Main {
    public static void main(String[] args){
        T1 t1 = new T1();
        t1.start();
    }
}
class T1 extends Thread{
    @Override
    public void run() {
        System.out.print("run in T1");
    }
}
```

## 法2：实现Runnable接口

```java
public class Main {
    public static void main(String[] args){
        T1 t1 = new T1();
        Thread thread=new Thread(t1);
        thread.start();
    }
}
class T1 implements Runnable{
    @Override
    public void run() {
        System.out.print("run in T1");
    }
}
```

## 法3：匿名内部类

```java
public class Main {
    public static void main(String[] args){
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.print("run in T1");
            }
        }).start();
    }
}
```

# 二、Thread与Runnable的关系

Thread实现Runnable接口

![](E:\Study\MyNote\Java\image\Thread1.png)

run()方法是Runnable接口的方法

![Thread2](E:\Study\MyNote\Java\image\Thread2.png)

**所以，无论以哪种方式开启子线程都可以重写run()方法现实子线程逻辑**

# 三、Thread的执行机制

## start()方法

调用**Thread类**的**start()**方法后，Thread会调用**start0()**方法，**start0()**方法是真正开启子线程的方法

![Thread3](E:\Study\MyNote\Java\image\Thread3.png)

至于**start0()**是本地方法，是JVM调用，底层是C/C++去实现

![Thread4](E:\Study\MyNote\Java\image\Thread4.png)

start()方法调用start0()方法后，该线程并不一定会立马款行，只是将线程变成了可运行状态。具体什么时候执行，取决于CPU，由CPU统一调度

## 传入的Runnable接口对象

在开启子线程时要向**Thread**的**构造方法**传入一个Runnable接口对象

如：

```java
        T1 t1 = new T1();//class T1 implements Runnable{}
        Thread thread=new Thread(t1);
        thread.start();
```

**start()调用start0(),start0()会调用Thread的run()方法，在Thread的run()方法中调用传入Runnable接口对象的run方法**

![Thread5](E:\Study\MyNote\Java\image\Thread5.png)

# 四、Thread的生命周期

## 官方文档解释![Thread7](E:\Study\MyNote\Java\image\Thread7.png)

## 图解：![Thread6](E:\Study\MyNote\Java\image\Thread6.png)

# 五、锁

## 关键字：

```java
synchronized
```

## 作用：

可以起到一个线程安全的作用，比如：

> 当多个线程同时访问**同一量变**时，可能会对其进行同时操作
>
> 假设 int a=0;
>
> 当a=0时执行a++
>
> 若此时两个线程访问变量a，若两个线程访问a时，a都为0，两个线程都执行了a++，则a就变成了2，而不是递增1了
>
> 所以像这种情况就要对代码块
>
> ```
> {
> ....
> 	if(a==0)
> 		a++；//递增操作
> ....
> }
> ```
> 进行上锁，上了锁的部分一次只能被 一个线程访问，其它线程都要等待，直到拿到**锁**

**synchronized**可以给方法上锁也可以给代码块上锁，一般是给代码块上锁(程序运行效率高些)

## 用法(对代码块)

```
synchronized (obj){
            ....
			if(a==0)
				a++；//递增操作
			....
        }
```

其中**obj**是对象，也就是说这个锁是上在对象上的**对象锁**

**注意**：对象锁的对象必须是**同一对象**，若不是同一对象，则是不同的锁，起不到线程安全的作用

## 用例：

```java
public class Main {
    public static void main(String[] args){
        T1 t1 = new T1();
        Thread thread1=new Thread(t1);
        Thread thread2=new Thread(t1);
        //运行两个线程
        thread1.start();
        thread2.start();
    }
}
class T1 implements Runnable{
    private int a=0;
    @Override
    public void run() {
        synchronized (this){
            if(a==0){
                try {
                    Thread.sleep(50);//让两个线程访问a时，a为0
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                a++;//递增操作
            }
            System.out.print(a);
        }
    }
}

```

## 对方法上锁

### 非静态方法

### 静态方法

