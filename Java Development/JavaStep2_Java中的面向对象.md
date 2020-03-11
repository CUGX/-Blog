# JavaStep2__Java中的面向对象

## 1、类与对象

### 1.1、类

1. 把具有相同属性和相似行为的一类事物称为类；
2. 属性===》数据表示====》选择合适的数据类型；
3. 行为===》用方法/函数表示；
4. 相同属性称为数据抽象；相似行为称为过程抽象；
5. 属性和行为的先后没有影响，地位是并列的。

​		

### 1.2、对象

1. 类的实例就是对象（创建一个类实例，即创建一个对象）

2. 创建对象的方法：

   ```java
   //类名 对象(引用)名 = new ；类名();
   Person person = new Person();
   ```

   

### 1.3、成员：

1. 成员：包括成员变量和成员函数；

   成员变量：类的属性；

   成员函数：类的行为；成员函数可以访问其他成员；

2. 暂时而言：类中只能放成员，成员都是并列的，不分先后

   暂时而言：成员必须通过对象去访问，用“.”运算符

   ```java
   //这句话既不是成员函数也不是成员函数，直接写在类中是违法的
   System.out.println("Hello World!")
   ```

   

### 1.4、初始化

#### 1.4.1、变量默认值

​		成员变量有默认值: 0，0.0……；

#### 1.4.2、构造函数

> 对象创建成功后，就有初始值==>放在类中，有如下性质：

1. 是一个成员函数，函数类名与函数名相同;

2. 无任何返回类型，连void都没有;

3. 创建对象的时候自动调用，不用显示调用(即不用写调用语句)，并且创建一个对象就会调用一次;

4. 构造函数的作用给成员赋初始值;

5. 若一个类没有构造函数，jvm会给类自动添加一个无参的构造函数，称之为默认构造函数

   ```java
   //默认构造函数
   public 类名() {}
   //ps：一旦，人为写定构造函数，默认构造函数就不存在了
   ```

   

#### 1.4.3、函数重载

1. 重载函数是同一个类中的函数；

2. 这组函数，函数名称相同；

3. 各重载函数只有参数不同(参数类型，个数不同，类型个数不同)；

4. 与函数的返回值类型无关；

5. 函数调用时，根据函数的参数的个数和类型，自动匹配；

   ```java
   /*
   	但这种匹配，未必是精确的，符合就近原则(找最精确的匹配，不行找次精确的匹配，能够匹配上的不需要人为转换)
    	例：优先匹配int add(){};如不存在可调用double add(){} 
   */
   int add(int a,int b){} 
   double add(double d1,double d2){}
   add(10,10);
   ```

   

#### 1.4.4、构造函数重载



#### 1.4.5、定义初始化

1. 给成员变量直接赋值；当该类的所有对象的某个属性值都一样时使用；

2. 初始化顺序：创建对象的时候，构造函数、定义初始化都被执行，谁先，谁后?

   ```java
   /*
   1、定义初始化先执行，再执行构造函数；
   2、若存在多个定义初始化，那么，这些定义初始化从上到下依次执行，都在构造函数之前；
   3、成员变量值以最后执行的为最后结果。
   */
   public class Person {
       double height;
   	{ 	//定义初始化块：
           System.out.println("dingyi");
   		height = 175;
       }
   }
   ```

   

### 1.5、内存分配

1. 堆、栈、方法区

   - 方法区===》类的信息都存放在此；
   - 堆===》创建一个对象，分配了一个堆空间用来存放该对象的变量；
   - 栈===》在调用构造函数时，通过分配栈空间将变量初始化，初始化结束后，分配的栈空间释放；

2. 区分对象和引用：

   ```Java
   //p1引用了一个点的实例
   Point p1 = new Point(8,8);
   //p2指向（引用）了p1指向的实例对象
   Point p2 = p1;
   //p1和p2指向了同一个对象
   p2.x = 20；=====》p1.x = 20；
   ```

   

### 1.6、this的引用

1. 什么是this?

   当前对象（的引用）===》调用这个函数的对象（的引用）;

2. this用在什么地方?

   一个成员函数（非静态的）访问另外一个成员（非静态的），前面都省略了this；

   ```java
   //初始化时形式参数的变量名可以和成员一样；前提：成员前用this区分
   class point() {
       double x,y;
       point(double x,double y)
       {//起相同的名字，this不能省略
           this.x = x;
           this.y = y;
       }
   }
   ```

3. 函数的返回值类型为类名，那么返回的是该类的对象

   ```java
   //1、返回类型为类的对象
   public class Person {
       private int height;
       public Person getPerson() {
           height++;
           return this;
   	}
       public void print() {
       	System.out.println(height);
   	}
       
   }
   public class Demo1 {
       public static void main(String[] args) {
           Person p1 = new Person();
           //此语句合法；调用getPerson返回this即p1，后面都是，height一直+1
           p1.getPerson().getPerson().getPerson().getPerson().print();
       }
   }
   
   //2、形式参数为类的对象
   //主函数中
   Point p1 = new Point(8,8);
   Point other = new Point(10,10);
   p1.getDistence(other);
   
   //Point类中
   double getDistence(Point other) {
       //x都为this.x
       return Math.sqrt((x-other.x)*(x-other.x)+(y-other.y)*(y-other.y));
   }
   
   
   //3. this(参数)====》调用另外一个构造函数：class0.java必须放在第一句，并且只能调用一次
   circle(Point center,double r) {
       this.center = center;
       this.r = r;
   }
   circle(double x,double y,double r) {
   	/*
   	this.center = new Point(x,y);
   	this.r = r;  
   	*/
       //等价于下面这种方式：调用重载中的另外一个函数
       this(new Point(x,y),r);
   }
   
   
   ```

4. 以后有类名.this的用法（讲内部类的时候再说）；
   ps： 注意函数类的方法的调用，成员间的调用；调用格式的写法。



### 1.7、静态成员

1. 静态成员变量属于类，不属于某个对象；

2. 访问时可以用 类名.静态属性 直接访问，也可以用对象名.访问，后者不提倡；

3. 静态成员变量，在用到类的时候就会完成初始化加载；

   //静态定义初始化；静态定义初始化快（参照前面）；

   //初始化顺序：静态>定义初始化>构造函数；

4. 静态的成员方法//调用同静态成员变量：可用对象.访问不提倡，提倡类名.方法访问；

   - 静态成员方法只能访问静态成员变量；反之，普通成员函数能访问所有成员变量；

   - 为什么之前都将函数写成static？因为主函数本身是静态的，只能访问静态成员。

5. static变量不能写在构造函数里用this.static变量初始化。

   ```java
   //静态成员变量
   private static boolean diqiu;
   //合法
   person(double heig,boolean di) {
   	height = heig;
   	diqiu = di;
   }
   //不合法
   person(double height,boolean diqiu) {   
   	this.height = height;
   	this.diqui = diqiu;
   	//this.diqui = diqiu;//static变量不能写在构造函数里
   }
   
   //主类中：定义如下静态块，则不执行主函数就退出程序
   static {   
   	System.out.println("HelloWorld");
   	System.exit(0);//退出程序
   }
   public static void main(String[] args) {
       System.out.println("Hello World!");
   }
   ```

   

### 1.8、命令行参数

1. 运行时，给主函数的参数

   java 类名 空格 给主函数的参数

   如：java class2 hello world

   ```Java
   /*案例：
   人类：包括点和圆类（class0.java）
   	相同的属性===》身高、体重、年龄etc
   	相似的行为===》吃饭、走路、睡觉etc
   	通过封装行为构成函数。
   */
   class 类名{属性，行为} //=====》这个过程称为封装
   person p = new person(175,"wang");
   ```

   

## 2、面向对象的继承

