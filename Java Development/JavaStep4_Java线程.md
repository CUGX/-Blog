# Java线程、网络编程、GUI编程

## 1、Java线程

> 进程：计算机的运行线索；
> 线程：进程运行的线索，计算机运行的基本单位；

### 1.1、创建线程

### 1.2、线程生命周期

### 1.3、线程常用方法

### 1.4、线程的互斥操作

### 1.5、线程的通讯

### 1.6、线程内数据共享

### 1.7、Java线程池

### 1.8、Callable&Future





## 2、Java网络编程

### 2.1、java.net.InetAdress







### 2.2、java.net.ServerSocket









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



##  3、JavaCore GUI

> java.awt.\*;	创建用户界面和绘制 图形图像的所有类
>     	java.awt.event.\*;	事件
> 		javax.swing.\*;	创建用户界面的组件 
> 					（不建议使用awt包下的组件  swing包下的组件多数都继承了awt包）
> 					swing使每个界面在不同平台显示相同界面
> 		java.awt.geom.*;	Java中的2D图形类

### 3.1、组件和布局





### 3.2、Java时间机制







### 3.3、画图操作



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

