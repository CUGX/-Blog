# Java线程、网络编程、GUI编程

## 1、Java线程

> 进程：计算机的运行线索；
> 线程：进程运行的线索，计算机运行的基本单位；

### 1.1、创建线程

1. 以实现了Runnable接口的类的实例作为创建Thread类对象的构造函数的参数

   ```java
   //主函数本身是一个线程，习惯称为主线程
   A a = new A();//省略了A类的实现
   Thread t1 = new Thread(a); 
   t1.start();	//start()方法启动线程，run()方法为线程要 执行的任务
   ```

2. 直接创建类继承Thread,重写 run()方法 

   ```java
   myThread mt = new myThread();//myThread类省略了，此时创建了对象就创建了线程
   mt.start();
   /*
   上述两种方法都行，建议使用第一种，更加面向对象，
       一个Runnalbe的实例，我们认是一个对象	
   */
   ```

3. 直接用匿名类实现

   ```java
   main(){
       //方法一：向父类传参，调用父类构造函数，重写run()方法
       new Thread(new Runnable(){
           public void run() {
               for(;;){
                   syso("subThread");
               }
           }
       }).start();
       
       //方法二：子类直接重写run()方法
       new Thread(){
           public void run() {
               for(;;){
                   syso("subThread");
               }
           }
       }.start();
   }
   ```

   

### 1.2、线程生命周期
> 线程状态转换 ：
> 	newBorn -->start()--> Runnable-->sleep() wait() 网络阻塞...<--> Pause -->stop()--> Dead 
>
> 多线程的程序，每次的运行结果可能不一样====》异步性

| 状态名   | 状态描述                           |
| -------- | ---------------------------------- |
| newBorn  | 新生状态                           |
| Pause    | 阻塞状态                           |
| Runnable | 可执行状态（有Running、Ready两种） |
| stop     | 暂停状态                           |
| Dead     | 不是stop--死不能复生               |



### 1.3、线程常用方法

```java
Thread t1 = new Thread();
t1.getName();//得到线程名
t1.setName();//设置线程名
Thread.currentThread().getName();//得到当前线程名
t1.getPriority();//得到线程的优先级，默认5级，优先级1到10直间，数字越大，优先级越高

//重要的一些方法：

/*1、线程睡眠，进入阻塞状态
	哪个线程调用sleep方法，那个线程阻塞，与前面的线程名无关。
	主函数运行到这句话 时，则为主函数阻塞，与t1无关，所以一般直接Thread.sleep();*/
Thread.sleep();
t1.sleep();

/*2、一个线程等待另一个线程运行结束,直接写，无参数
	写在主函数中，则主线程等待t1运行结束，
	join写在那个线程中就表示那个线程要等待线程t1运行结束*/
ThreadName.join();
t1.join();

/*3、主动放弃CPU，把机会让给别的线程，
	然后再参与竞争 Running--》ready
*/
Thread.yield();
//4、线程中断后就不再调用try/catch的语句，直接往下运行
Thread.interrupt();
```



### 1.4、线程的互斥操作

> 线程的互斥操作---->主要解决线程之间数据共享的问题

#### 1.4.1、 synchronized

1. 同步块 synchronized关键字：

   ```java
   /*
   java中任何对象都是一把锁，有且只有一把钥匙
   	出同步块就会释放钥匙 */
   synchronized(this){
       ……;
   }
   ```

2. 同步函数 synchronized关键字加在函数中：

   ```java
   /*同步函数中也有锁存在，---->普通方法加同步，锁就是当前对象
   	静态函数同步也有锁 ---->锁是当前类的类类型，Output.class 类名.class 	
   	*/
   public synchronized void run(){
       ……;
   }
   /* 
   注意： 
       1、多个线程共享数据要保证安全，一定要保证用的是同一个对象锁；
       2、死锁的问题 死锁程序就卡死，无法调试--->可以通过Java工具来检查死锁；
       3、StringBuidler 无synchronized
           StringBuffered类中有synchronized，线程安全
           Vector 是有synchronized线程安全
           ArrayList 无synchronized
   */
   ```

#### 1.4.2、Lock

```java
/*
synchronized较繁琐，比较不好理解，而且不面向对象，
    Java5版本开始，做了改进，有了Lock类
*/
class Output{
    //创建锁对象
    Lock lock = new ReentrantLock();
    public void print(String name ) {

        lock.lock();
        try{
            for(){
                syso(name.charAt(i));
            }
        }finally{
            lock.unlock();//解锁后其他对象才能进来
        }
    }
}
//扩展：读写锁CacheDemo
```



### 1.5、线程的通讯

#### 1.5.1、管道

​		通过管道流的方式进行简单的通讯，性能操作比较差，只能进行简单数据交互。

#### 1.5.2、Thread.yield()方法

​		生产者 消费者模型。
​		效率也比较差，原因：yield是主动放弃CPU的使用权，但是还是会参与CPU竞争。

#### 1.5.3、wait/notify

- 任何一个对象都拥有一个线程等待池；

- 挂在同一个对象的线程等待池中的线程之间可以相互唤醒；

- 属于Object类；

- wait方法的使用必须放入synchronized同步块中，

  先学搭建模型 waitDemo生产者 ，消费者模型

  ```java
  /*
  案例：
      A线程运行10次，然后B线程运行20次，这样反复运行50次
  理解：
      A线程生产者，生产一次食物需要运行10次；
      B线程消费者，消费一次食物需要运行20次；
   拓展：
      A线程运行10次，然后B线程运行20次，然后C线程运行30次，这样反复运行50次
  */
  ```

  

#### 1.5.4、锁机制

- Java5版本开始的新方法，不使用wait/notify，使用锁机制

  ```java
  /*
  java.util.concurrent
  java.util.concurrent.locks------->包中
  	使用condition.await()方法进入等待状态，condition.singal()方法唤醒其他线程
  */
  private Lock lock = new ReentrantLock;
  private Condition condition = lock.newCondition();
  //(阻塞队列中使用较多，在线程异步情况下使用)
  ```



### 1.6、线程内数据共享

#### 1.6.1、HashMap<Thread,,object>



#### 1.6.2、ThreadLocal

> java中提供了ThreadLocal这个类，已经实现了类似的功能，可以直接使用tl.set()放数据，tl.get()得到数据

```java
//泛型为value的类型，key默认为当前线程
ThreadLocal<> tl = new ThreadLocal<>();
```



#### 1.6.3、单独类实现

​		写一个类，使得该类在创建对象时，创建完之后，直接就是同一个线程，同一个对象，不同线程，对象不同；

> 注意：

- 不管是单例模式，还是这种情况，我们都使得 构造函数private，但是对反射无效，即通过反射依然可以通过构造函数新建对象；

- 线程间的通讯 与 线程范围内数据共享的区别：

  线程间的通讯涉及到多个线程在不同模块中使用相同的数据；

  线程范围的数据共享指单个线程访问不同模块时，访问到的数据是一致的。

  

### 1.7、Java线程池

> 线程并不是越多越好，无限制的创建线程，那么线程的创建、销毁都有很大消耗；
>
> 希望不管执行多少任务，都用固定的线程数来执行，，可以让一个线程池来执行多个任务

```java
/*
如何创建线程池：
1. ExecutorService类......可用来创建线程池,
	Executors类*/
newFixedThreadPool();//创建一个固定线程数的线程池
newSingleThreadExecutor();//创建单个线程的线程池
newCachedThreadPool();//创建一个可根据需要创建新线程的线程池
/*2、ExecutorService类
	ScheduledExecutorService类
	线程在time时间后执行command命令，每过周期时间执行一次*/
newScheduledThreadPool();	
scheduledAtFixedRate(command,time,周期,周期的单位);

//3、创建一个固定线程数的线程池
ExecutorService threadPool = Executors.newFixedThreadPool(3);
threadPool.shutdown();//关闭线程池
```



### 1.8、Callable&Future

​		前面执行任务使用的 都是run方法，并且没有返回值，在多线程运行时，不知道线程什么时候结束；如果线执行完返回一个值，就认为线程运行结束，java5中提供了线程执行任务，任务执行返回具体值。

```java
//泛型的具体类型就是返回值的类型，也是Future泛型类型
ExecutorService threadPool1 = Executors.newSingleThreadExecutor();
Future<Integer> fu = threadPool1.submit(new Callable<Integer>(){
    //实现Callable接口，需要重写call方法
    public Integer call() throws Exception{
        Thread.sleep(3000);
        return 10;
    }
});
int value = future.get();	//得到线程返回值
syso(value);
```



## 2、Java网络编程

### 2.1、java.net.InetAdress

```java
InetAdress ad = InetAdress.getByName("www.163.com");
syso(ad.getHostAddress());//打印网址的IP，电脑连上网才能 打印出来	

//获取本机IP地址，默认127.0.0.1 
InetAdress ad = InetAdress.getByName(null);
syso(ad.getHostAddress());
```



### 2.2、java.net.ServerSocket、java.net.Socket

1. 端口：计算机上的服务  都会运行在一个端口上，端口用整数表示，计算机最多有65536个端口，1024以下的端口基本被占用，我们一般使用1024以上的；
2. 在计算机上开启一个服务，需要占用一个端口；
3. 如何开启一个服务？在Java中使用ServerSocket类；

```java
//服务器端，
ServerSocket s = new ServerSocket(9000);//9000端口
//等待客户访问，没有客户访问，阻塞
Socket socket = s.accept();

//客户端：
Socket socket = new Socket(ip,port);//IP和端口

Socket socket = new Socket("127.0.0.1",9000);

//socket的两个重要方法：
socket.getInputStream
socket.getOutputStream
//客户端和服务器端的In和Out构成一对IO类
```



### 2.3、聊天室

```java
/*
群聊信息格式：消息
	私聊消息格式：IP/消息
	客户端：a. 随时接收服务器端发来的数据，进行读操作
			b. 随时从键盘读取数据发送给服务器
			所以客户端连上服务器后，需要开启两个线程，
			分别执行ab任务
	
	服务器端：
		a. 随时读取客户端发来的数据，再往外写
		b. 保存产生的所有Socket对象，并且耨遍历和查找
	群聊：遍历所有的socket，进行写操作
	私聊：截取ip地址
*/	
```



### 2.4、在网络中传输对象

1. 必须实现序列化接口；Serializable；
2. 传输对象必须使用ObjectIn/OutputStream。



##  3、JavaCore GUI

> java.awt.\*;	创建用户界面和绘制 图形图像的所有类
>     	java.awt.event.\*;	事件
> 		javax.swing.\*;	创建用户界面的组件 
> 					（不建议使用awt包下的组件  swing包下的组件多数都继承了awt包）
> 					swing使每个界面在不同平台显示相同界面
> 		java.awt.geom.*;	Java中的2D图形类

### 3.1、组件和布局

1. 常用的简单组件和布局 java.swing.*

   JFrame、JButton、JPanel、JTextField、JCheckBox、JTextArea...

2. 布局：

   | 布局方法              | 组件布局规则            |
   | --------------------- | ----------------------- |
   | FlowLayout 水平布局   | 从上往下放组件          |
   | BorderLayout 边界布局 | 将界面分成东西南北中    |
   | GridLayout 网格布局   | x*y的网格区（x行，y列） |

   

3. javax.swing.JPanle  面板 

   - 比窗体丰富的操作(比如画图等操作)
   - 面板是没有边界的窗体
   - 习惯在窗体上布局面板，面板布局组件




### 3.2、Java事件机制

> Java中已经将事件都分类了（与异常原理类似）；一个事件就产生了时间类的对象给某个组件加事件操作，就是给这个组件添加一个监听器

| 事件类型    | 监听器类型     | 加监听器方式                      |
| ----------- | -------------- | --------------------------------- |
| ActionEvent | ActionListener | addActionListener(ActionListener) |
| XXXEvent    | XXXListener    | addXXXListener(XXXActionListener) |

1. 监听器都是接口，实现接口的方法，就是事件要做的事件；

2. 按钮事件 ActionEvent；

3. 鼠标事件 MouseEvent

   - addMouseListener：需要重写5个方法，移入，移出，按下释放点击
   - MouseAdapter： 适配器实现了这个监听器，利用适配器不需要重写所有的方法，只需要按需要重写方法即可；
   - MouseMotionListener：鼠标的移动和拖拽方法。

4. 键盘事件：KetEvent KeyListener	addKKeyListener

   给组件添加键盘事件，是指该组件获得焦点时，操作键盘，产生事件



### 3.3、画图操作

> 重写paintComponent(Graphics g){}方法 

```java
g.drawLine(10,10,100,100);//直线由起始点和结束点坐标组成
g.drawRect()//矩形由左上角和宽度/高度	决定
g.drawString("hello",100,100);//画字符串 

//Graphics2D的重要方法-->g2.draw(shape);

//也可直接画图
Line2D line = new Line2D.Double(10,10,100,100);
Rectangle2D rect = new Rectangle2D.Double(10,10,100,100);
//转换画笔
Graphics2D g2 = (Graphics)g;
g2.draw(line);
g2.draw(rect);

/* 注意：
1、如果要调用paintComponent只需要调用repaint方法即可；
2、画图和事件相结合。
*/
```



### 3.4、贪吃蛇游戏

```java
/*
描述：
	大家都有所体验，在此不赘述
	
思路：
1. 类和方法：
    实体类
    视图类
    控制类
    a. 蛇		Snake
        Move()
        EatFood()
        ChangeDirection()
        DrawSelf()//画出蛇身
    	蛇是否碰到自己、食物、障碍物...
    b. 食物	Food
        isEatBySanke(Snake snake)
        DrawSelf()
    c. 障碍物	Obstacle
        isHitObstacle(Snake snake)
        DrawSelf()
    d. 面板类	GamePanel extends Jpanel
        Display()//显示蛇、食物和障碍物的位置
        repaint() paintComponent...

    e. 控制器类：
        1. 控制蛇的移动、食物的位置、墙、游戏界面
        2. 键盘控制其移动，通过继承KeyAdapter，重写keyPressed事件
        3. 蛇的每次移动都要判断：（接口）SnakeListener
            1). 是否碰到自己
            2). 是否碰到墙
            3). 是否吃到食物
            
2. 蛇类中需要有添加监听器的方法，接收监听器的方法---》给蛇类添加SnakeListener成员addSnakeListener

3. 监听器中也要有蛇的监听器，能够监听蛇是否碰到自己、食物、障碍物；
    ===》控制器实现SnakeListener接口

4. 组件游戏，主类&主方法
    a. 创建类的对象
    b. 控制器可以实现创建新游戏
   		newGame()
    c. Snake中添加start()方法，使游戏开始运行
        start()方法中需要启动一个线程使蛇不断的移动
        ===》添加一个SnakeDriver内部类，实现Runnable接口，不断移动

5. 具体实现
    a. 用什么存储蛇身：采用去尾加头的方式移动===》采用LinkedList对象
    	用点来表示蛇身的Point
    b. 蛇身就是被分割成一个一个的格子，长宽均为15，界面宽度和高度均为20个，
        可以添加一个辅助类Global
        ===》坐标表示：
        （x,y）==》（x*格子宽度，y*格子高度）
    c. 初始蛇身
        区域中间三个格子作为蛇身，
        构造函数进行初始化，
        再用DrawSelf方法画出蛇身
    d. 蛇的Move方法
        去尾加头
        完成ChangeDirection方法
        修正反方向的问题
        边界问题
    e. 添加食物
        食物就是一个格子
        位置由左上角坐标决定
        可以让其继承Point类
        设置食物出现的位置
        控制器而言，游戏开始时，让蛇移动并产生食物
    f. 吃食物：
        将去掉的尾巴加上即可
        什么时候吃食物======》碰到的时候，将食物吃掉
    g. 完成障碍物
*/
```

