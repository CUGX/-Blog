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

### 2.1、继承

1. 主要是代码的复用，以后在设计中意义更大；

   关键字：extends；

2. Java中继承是全部继承，除了构造函数以外都继承

   ```java
   //Circle称为Shape的子类/派基类，Shape称为Circle的父类/基类
   Class Circle extends Shape{}
   ```

   

### 2.2、继承的构造函数

1. 子类必须调用父类的构造函数（构造函数是不被继承的），如果没写，那么子类的构造函数默认调用父类无参的构造函数（默认在子类构造函数的第一句）===》super();

   ```java
   public ClassName(int len, ……) {
   	super();
       this.len = len;
       .....    
   }
   //若要显示调用，要用super关键字，并且必须在子类构造函数的第一句
   super(参数列表)
   ```

   

2. 有子类、父类的构造函数的初始化顺序：

   - 先父类静态定义初始化块，然后子类静态定义初始化块；

   - 父类的定义初始化，父类的构造函数；

   - 子类的定义初始化，子类的构造函数；

     

3. 方法的重写

   - 父类有某个方法，子类有重新实现
   
   - 访问权限必须是非private，子类中还可以放大，不能缩小；要方法重写，父类中必须不能 是private，private子类无法访问；
   
   - super.方法()==》调用父类被重写的方法;不重写直接调用即可；
   
     ```java
     /*	总结：super关键字：
     super(参数列表)==》调用父类构造函数；必须在子类构造函数第一句
     super.函数()=====》在子类重写父类方法里，调用父类的被重写函数java
     */
     ```
   
   - 动态binding
     - 父类的引用可以引用任何子类的实例对象；
     - 如果父类的引用引用了子类的实例，调用方法时，如果方法构成重写，那么调用子类方法，否则调用父类方法;===>>动态绑定
     
   - 对象数组：
   
     ```java
     /*
     传不同的父类，调用不同的check()函数;使用起来很方便
     check()函数已经重写===》singleQuestion调用她重写后的check()方法
     ===》multiQuestion调用她重写后的check()方法
     */
     import java.util.Arrays;
     class class6 {
     	public static void main(String[] args) {
     		singleQuestion sq1 = new singleQuestion(1,"好人是___",new String[]{"A.WangJ","B.cugx"},"A");
     		sq1.printQuestion();
     		System.out.println(sq1.check(new String[]{"A"}));
     		singleQuestion sq2 = new singleQuestion(2,"帅哥是___",new String[]{"A.WangJ","B.cugx"},"A");
     		sq2.printQuestion();
     		System.out.println(sq1.check(new String[]{"A"}));
     		/*//测试父类引用子类对象
     		Question q1 = new singleQuestion(1,"好人是___",new String[]{"A.WangJ","B.cugx"},"A");
     		q1.printQuestion();
     		System.out.println(q1.check(new String[]{"A"}));*/
     		
     		//System.out.println(Arrays.equals(new String[] {"A","B"},new String[] {"A","B"}));
     		multiQuestion mq1 = new multiQuestion(1,"好人是___",new String[]{"A.WangJ","B.cugx"},new String[]{"A","B"});	
     		mq1.printQuestion();
     		System.out.println(mq1.check(new String[]{"A","B"}));
     		System.out.println("===============================");
     
     		//创建对象数组
     		singleQuestion[] questions = new singleQuestion[]{sq1,sq2};
     		for (int i=0;i<questions.length;i++ ) {
     			questions[i].printQuestion();
     		}
     		System.out.println("===============================");
     
     		//创建既有单选又有多选题
     		Question[] qs = new Question[]{sq1,sq2,mq1};
     		for(Question q:qs) {
     			q.printQuestion();
     			/*
     			传不同的父类，调用不同的check()函数;使用起来很方便
     			check()函数已经重写===》singleQuestion调用她重写后的check()方法
     			                   ===》multiQuestion调用她重写后的check()方法
     			*/
     			System.out.println(mq1.check(new String[]{"A","B"}));
     		}
     	}
     }
     
     class Question {
     	int id;
     	String title; 
     	String[] options;
     	Question(int id,String title,String[] options) {
     		this.id = id;
     		this.title = title;
     		this.options = options;
     	}
     	void printQuestion() {
     		System.out.println(id+"."+title);
     		for (int i=0;i<options.length ;i++ ) {
     			System.out.println(options[i]);
     		}
     	}
     	boolean check(String[] ans) {
     		return false;
     	}
     }
     
     class singleQuestion extends Question {
     	private String answer;
     	singleQuestion(int id,String title,String[] options,String answer) {
     		super(id,title,options);
     		this.answer = answer;
     	}
     	boolean check(String[] ans) {
     		if(ans.length!=1)
     			return false;
     		return ans[0].equals(answer);
     	}
     }
     
     class multiQuestion extends Question {
     	String[] answers;
     	multiQuestion(int id,String title,String[] options,String[] answers) {
     		super(id,title,options);
     		this.answers = answers;
     	}
     	boolean check(String[] ans) {
     		Arrays.sort(ans);
     		return Arrays.equals(this.answers,ans);
     	}
     }
     ```
   
     ```java
     //简单语句实现不同函数的调用
     class class7  {
     	public static void main(String[] args)  {
     		Shape s = new Shape();
     		Shape r1 = new Rectangle();
     		Shape c1 = new Circle();
     		person p = new person();
     		Shape[] shapes = new Shape[]{s,r1,c1};
     		//System.out.println("Hello World!");
     		for (int i = 0;i < shapes.length; i++ ) {
     			//简单语句实现不同函数的调用
     			p.drawShape(shapes[i]);
     		}
     		System.out.println("========================");
     
     		person p2 = new person();
     		p2.getOneShape().draw();//两重调用
     	}
     }
     
     class person {
     	void drawShape(Shape s) {//Shape类作为参数；将来能够传递Shape及其子类
     		s.draw();
     	}
     	Shape getOneShape() {//Shape作为返回值类型，可以返回它及其子类的实例
     		return new Circle();
     	}
     }
     
     class Shape {
     	void draw() {
     		System.out.println("draw.....");
     	}
     }
     class Rectangle extends Shape {
     	void draw() {
     		System.out.println("draw.....Rectangle!");
     	}
     }
     class Circle extends Shape {
     	void draw() {
     		System.out.println("draw.....Circle!");
     	}
     }
     ```
   
     

### 2.3、访问权限

1. private、protected、public、默认friendly；
2. private：成员只能在这个类内部使用，即类的{}内访问；可通过其它非private方法访问；
3. 包：是类名的一部分，主要用于命名的管理；不写则在默认的包里；
   - 显式声明包：package 包名；包对于系统来说就是文件夹；
   - 有包的类，建立包名（文件夹），javac -d. 类名.java<======>javac -d. 代表默认在当前目录下建立包；
   - 运行：java.包名.类名；
   - 导包；import 包名.类名/import 包名.*；
4. friendly(默认)：可修饰成员和类===》在同一个包中可以直接访问；
5. protected：在同一个包中，或者不同包的子类中可以访问；
6. public：在不同的包中可以访问；

> 访问权限总结：

| 作用域              | 当前类 | 同package | 子孙类 | 其它package |
| ------------------- | ------ | --------- | ------ | ----------- |
| public              | √      | √         | √      | √           |
| protected           | √      | √         | √      | ×           |
| friendly（default） | √      | √         | ×      | ×           |
| private             | √      | ×         | ×      | ×           |

7. classpath环境变量：找类的变量，默认是当前路径；

   set classpath=D:\Javaworkspace\JavaDemo>;.=====》声明多个classpath用“；”隔开

8. 一般情况下，成员变量private，函数public

9. 构造函数private；如何初始化？利用静态函数public static直接调用；

10. public函数不能调用private函数===》因为只有类才能调用函数；没有构造函数对象无法创建成功；所以不行。

    ```java
    //example:
    class class8 {
    	public static void main(String[] args) {
    		/*
    		A a = new A();  错误，构造函数私有无法调用
    		需要通过源类中声明静态函数accessA()来调用
    		*/
    		A.accessA();
    		System.out.println("Hello World!");
    	}
    }
    
    class A {
    	private A(){}
    	public static void accessA() {
    		A a = new A();
    		a.print();
    	}
    	public void print() {
    		System.out.println("Access A");
    	}
    }
    ```

    

### 2.4、Java Bean

1. JavaBean就是一个Java类；

2. 标准的JavaBean就是一个Java类属性private，有无参数的构造方法，对所有的属性都有getter/setter方法，所以private变量在其它类中访问时，需要通过set或get方法来调用类。

   ```java
   public user() {
       private String name;
       //无参数的构造方法
       public user() {
       }
       //getter & setter 方法
       public String getname() {
           return name;
       }
       public void setname(String name) {
           this.name = name;
       }
   }
   ```

   

### 2.5、Object类 java.lang.Object

1. 很多类没有导包，如String类，math类，这些都是java.lang包下的类，是jdk提供的；

   jdk提供的类中java.lang下的类，不用导包就能直接使用；

2. 一个类如果没有父类，他的父类就是Object类。

   - Java中的继承是单一继承，就是一个类只有一个直接父类；

   - Java中类的继承有传递性；

     ```java
     /*
     A继承了B，B继承了C，C无父类
     则==》C的父类为Object类，Object类中的元素继承到C，也就集成到B和A中可以认为：B、C、Object类都是A的父类
     */
     ```



### 2.6、变量的隐藏

​		非private变量，子类中直接访问的都是子类自己声明的；父类继承的必须通过继承的方法或者super访问。

```java
class a {
    main(){
        d d = new d();
        syso(d.g());
        //父类c中g为public时，打印2，构成重写
        //父类c中g为private时，打印1，父类的引用引用子类的实例，不构成重写，只调用父类方法
    }
}
class c{
    int i = 1;
    public int p() { return i; }
    //public int g() { return i; }//返回2
    private int g() { return i;}//返回1
}
class d extends c {
    int i = 2;
    //public int g() { return i; }//返回2
    public int g() { return super.i; }//返回1
}
```



## 3、抽象类与接口

### 3.1、抽象类

1. 抽象方法；有些行为确实存在，但必须到子类中才会有具体实现；此时，父类中只需要声明即可，不需要有实现。

   声明方法 ： abstract

2. 若一个类有了抽象方法，该类就是抽象类，必须用abstract关键字声明；

3. 抽象类就是用来继承的，其本身没有意义；

4. 当一类继承了抽象类，就要重写抽象类的方法，若不重写，那么该类就是抽象类，必须要用abstract关键字声明；

5. 抽象类不能实例化，只能实例化其子类；

   - 抽象类的引用，引用的一定是其子类的实例；

   - 若函数的参数是抽象类，那么调用函数是传递的是该抽象类子类的对象；

   - 若函数返回值类型为抽象类，那么返回的是该抽象类子类的实例对象。

     

### 3.2、final关键字

1. 修饰变量：变量只能赋值一次；若作为成员变量，必须有初始值（定义初始化or构造初始化）；
2. 修饰函数：该函数不能被重写；
3. 修饰类：该类不能被继承；
4. final和abstract不能一起使用，private 和abstract不能一起使用。



### 3.3、接口

1. 接口是一个标准，更能体现干什么  can-do；

   抽象类是体现的继承  is-a  ====》接口比抽象类更抽象；

2. 接口用interface关键字声明====》地位等价于类的地位,也会产生.class文件；

3. 接口中所有的方法都是public abstract的，即使没有这样声明也是那样；所以，所有方法在接口中都不能有实现；

   ```java
   void fly();/*<===>*/public abstract void fly();
   ```

4. 定义一个canFly接口，实现了这个接口的类的对象，就认为具备fly功能，实现时使用implements关键字；

5. 实现接口，必须重写接口中的抽象方法，否则自己变成抽象类；

   ```java
   public interface canFly {
       void fly{};//等价public abstract void  fly();
   }
   class bird implements canFly {
       public viod fly(){
           System.out.println("Bird fly");
       }
   }
   ```

6. 接口不能实例化，接口中不能有构造函数，全部都是抽象方法；看到接口的引用，引用的一定是实现了该接口的类的对象，都是实现了接口的对象。

7. 如何理解接口：

   ```java
   /*
   	主板制造商===内存===显卡
   主板上实现(implements)了内存和显卡接口(interface)，即认为主板拥有了内存和显卡的功能，可以提前制造主板，也能够实现不同兼容多品牌显卡和内存，因为其它品牌的显卡和内存都必须实现显卡和内存的接口.
   */
   ```

8. 一个类可以实现多个接口（拥有多项功能），要实现接口中的多个方法

   ```java
   class hero implements canSwim,canFly,canSing {
       hero hero = new hero();//能调用三种方法
       canSwim s = new her0();//只能调用canSwim()方法
       //要想实现可做强制类型转换
   }
   ```

9. Java中是单一继承，但是接口与接口之间可以进行多继承，就是一个接口可以由多个接口继承而来

10. 接口中甚至可以没有任何方法，只是一套标准;

    ```java
    //Serializable、CloneAble类只是接口，无方法
    ```

11. 接口中可以声明变量，这些变量都是public static final类型，即使没有那样声明也是上述类型；

    ```java
    //类名可以直接.出来i==》静态变量
    int i = 11;		/*<=等价=>*/	
    public static final int i = 11;
    ```

    

    #### 适配器，解决接口“肥胖”问题

12. 接口中有多个方法，在实现时，所用不多，“肥胖”问题，可以实现一个适配器类

    ```Java
    //适配器类，这个类没有必要有任何对象，所以适配器类可以直接声明成abstract类
    public abstract class ttAdapter implements TT {
        public void a() {};
        public void b() {};
        public void c() {};
        public void d() {};
        public void e() {};
    }
    //避免了直接实现接口导致所有方法都需要重写
    //抽象类：只要有abstract声明的类都是抽象类，不一定要有抽象方法，抽象类可以实现接口
    class a extends ttAdapter {
        public void a() {
            syso("a...");
        }
    }
    class b extends ttAdapter {
        public void b() {
            syso("b...");
        }
    }
    ```

    

### 3.4、内部类

#### 3.4.1、成员内部类

1. 直接在外部类的其他成员中访问，创建内部类的对象(最简单常用)；

2. 如果成员内部类访问权限为非private，直接访问就可以；

3. 类名.this : 成员内部类中访问外部类的当前对象；

   ```java
   Out.inner inner = out.new inner("wan",21);
   ```

#### 3.4.2、静态内部类《===》静态成员

1. 直接在外部成员中访问（简单常用）;

2. 若权限非private直接创建对象;

   ```java
   //不需要类名.new创建内部类对象
   Out.inner inner = new inner1();
   ```

3. 静态内部类只能访问外部类的静态成员===》静态的只能访问静态的 

   ```java
   class Out{
       private int i;
       public Out(){}
       public void outerMethod1() {
           //相当于函数访问了另一个成员，只是这个成员是个类，需要新建一个类
           inner in = new inner("wang",20);
       }
       //默认权限，可以为public等，这个类的地位等价于类的一个普通成员
       class inner {
           private String name;
           private int age;
           inner(){}
           public inner(String name,int age){
               this.name = name;
               this.age = age;
           }
           public String getName() {
               return name;
           }
           //其他set\get方法没写
           public void innerMethod1(){
               syso("inner method1");
           }
           public void innerMethod2(){
               /*在成员内部直接访问外部类的成员
   				下面两者在这里等价，Out.this可以省略
   				但是将来在其他的一些应用中可能无法省略*/
               outerMethod1();
               Out.this.outerMethod1();
           }
       }
       static class inner1 () {
           public void innertest1(){
               syso("innertest1...");
           }
       }
   }
   
   class demo () {
       public static void main(String[] args) {
           //创建out对象
           Out out = new Out();
           out.outerMethod1();
           //直接创建内部类的对象
           Out.inner inner = out.new inner("wan",21);
   
       }
   }
   ```



#### 3.4.3、局部内部类

1. 一般声明在某个函数内部，只在函数内部有效；

2. 也会生成.class文件；

   ```java
   class Person {
       public canFly getOne() {
           /*
   		  函数的返回值类型是接口，返回的是实现了接口的对象
   		*/
           class Bird implements canFly {
               public void fly() {
                   syso("bird..fly..");
               }
           }
           return new Bird();
       }
       //这个类声明在了一个函数的内部，这就是局部内部类，只在该函数内部有效
   }
   class demo () {
       public static void main(String[] args) {
           Person person = new Person();
           person.getOne().fly();
       }
   }
   ```

   

#### 3.4.4、匿名内部类====>(局部内部类的特殊情况)

1. 什么时候用：

   - 已经知道父类，获取其子类的实例对象；
   - 已经知道接口，获取实现了该接口的类的对象；

2. 怎么用？如何获取对象

   ```java
   //公式得到的是匿名类的对象，实现了该接口(继承了该抽象类)的类的实例对象
   //公式：
   new 父类 or 接口 () {
       子类的实现
   	or实现该接口的类的实现
   	//实现===》即为重写其中的方法
   }
   ```

3. 上面的公式看起来一样，实际上有区别、

   - new 父类(可以给父类的构造函数传递参数){子类实现}；

   - 接口不存在此问题，接口没有构造函数；

   - 注意：在内部类(三个都是)中访问局部变量，该变量必须声明为final。
   
   ```java
   class PersonZ {
       public canFly getOne() {
           //返回类型是接口，要获取实现了该接口的类的对象
           return new canFly() {
               public void fly(){
                   syso("bird..fly..");
               }
           };
       }
   
       public void playFly(canFly fly) {
           fly.fly();
       }
   }
   class demo () {
       public static void main(String[] args) {
           PersonZ pz = new PersonZ();
           pz.getOne().fly();
           /*  调用方法时发现，参数是一个接口
   			传递的就是实现了该接口的类的对象
   			已经知道接口，获取其实现类的对象
   			可以直接使用匿名类
   		*/
           pz.palyFly(new canFly(){
               public void fly(){
                   syso("plane..fly..");
               }
           });
       }
   }
   ```



## 4、Java异常

> 常见异常：

```java
ArrayIndexOutOfBoundException===数组越界;
ArithmeticException==============除数为0;
……
```
1. 什么是异常：程序中可能出现的意外情况

   Java把异常or错误分类：

   - 首先分成了两大类都是java.lang.Throwable的子类
   - java.lang.Error：系统级的异常，一般程序很难调试，如内存问题；
     java.lang.Exception：程序级异常，一般通过代码可以处理；

2. java.lang.Exception他是一切异常的父类

   - Java中把所有产生的程序级异常进行了分类，我们程序中出现的任何异常都能在分类中找到；

   - Exception大方向也分成两种：
     - jvm能自动捕获的异常java.lang.RuntimeException及其子类，也能自己处理；
     - jvm不能捕获的异常，必须通过程序自己处理，若不处理，程序编译不通过；
     - try后可以跟多个catch；多个catch时注意：catch的异常，子类一定要放在父类前面，因为父类的应用可以应用子类的对象；
     - finally关键字：不管有没有异常最后都必须去执行（有一些资源需要关闭）；
     
     ```java
     //语法1：
     try {
         //业务代码
     } catch() {
         //异常处理
     } catch() {
         //异常处理
     }
     //语法2：
     try {
         //业务代码
     } catch() {
         //异常处理
     } finally{
         //后处理
     }
     
     //eg:
     public static void main(String[] args) {
         try{
             int a = Integer.parseInt(args[0]);
             int b = Integer.parseInt(args[1]);
             syso(a/b);
         } catch(ArrayIndexOutOfBoundException e) {
             syso("数组下标越界！");
         } catch(ArithmeticException) {
             syso("除0异常");
         } catch(Exception e) {
             syso("其他异常");
         } finally {
             syso("最终要关闭的资源");
         }
     }
     ```

3. throw/throws关键字

   - throw 在某种情况下抛出（制造）一个异常;

   - throws 用来回避异常；

   - 注意：当子类重写父类方法时，不能throws比父类更多的异常，除非多throws的异常是RuntimeException（因为RuntimeException可以由虚拟机捕获）；

   - 原理：Java中父类可以接受任何子类的对象，如果子类比父类有更多的约束，那么父类处理会出问题。所以在Java中子父类 进行继承，子类不能比父类有更多的约束。面向对象的设计中 有一个里氏替换原则，说明的就是这个问题。

     ```java
     //throw:
     public void setAge(int age) {
         try {
             if(age<0 || age>150) {
                 throw new Exception();
             }
         } catch(Exception e) {
             syso("illegal age");
         }
         this.age = age;
     }
     
     //throws:
     /*
      *setAge()方法是给别人调用的，所以想让调用者去处理异常，
      *自己不处理，这种情况下为了通过编译，使用throws关键字，
      *回避异常，使得编译通过，并告知使用者可能产生的异常
      */
     public void setAge(int age) throws Exception,... {
         if(age<0 || age>150) {
             throw new Exception();
         }
         this.age = age;
     }
     public static void main() {
         try{
             Person p = new Person();
             p.setAge(1000);
             syso(p.getAge());
         } catch(Exception e) {
             syso("illegal age");
         }
     }
     ```

4. Exception的几个常用方法(子类都有)：

   - getMessage()获取异常信息；

   - toString()异常类型+message；

   - printStackTrace();异常类型+message+异常在哪个类的哪个方法产生的；

     ```java
     public static void main(){
         try {
             int a = Integer.parseInt(args[0]);
             syso(a);
         } catch(ArrayIndexOutOfBoundsException e) {
             syso(e.getMessage);	//异常类型
             syso(e.toString);
             syso(e);//两者等价    异常类型+message
             e.printStackTrace();//异常类型+message+异常在哪个类的哪个方法产生的
         }
     }
     
     //自定义异常===在回避异常时，可以只回避自定义的异常而捕获其他异常
     public class AgeException extends Exception {
         super(message);
     }
     class person {
         private int age;
         public void setAge(int age) throws AgeException {
             if(age < 0 || age > 150) {
                 throw new AgeException("ilegal age");
             }
             this.age = age;
         }
     }
     ```

5. 有了异常之后，一些要注意的细节

   如：函数如果有返回值，那么不管有何异常产生都要保证函数有返回值:

   ```java
   //不管顺序如何finally一定在return的前面执行，程序一遇到return就返回
   //case1:
   try {
       
   } catch() {
       
   } finally {
       
   } 
   return 返回值;
   //case2:
   try{
       ...;
       return 返回值; 
   } catch() {
       ...;
       return 返回值;
   } finally {
       
   } 
   ```

   

## 阶段性项目案例（二）  影片租赁管理系统

```java
/*
需求描述：
  1. 完成影片连锁企业租赁管理系统。可以计算每一位客户的消费金额和影片的详细信息，
  金额根据影片的类型和租赁的日期来计算；
  2. 客户把影片分成三类进行管理：A. 最新电影B. 普通电影 C. 儿童影片
  3. 费用计算规则如下：
     a. 普通电影：若租期小于2天，费用为2元，租期大于2天，费用是租期减去2，每天1.5元
     b. 最新电影：费用每天3元
     c. 儿童影片：租期小于3天，费用1.5元，若租期大于3天，费用为租期减去3，每天1.5元
  4. 每次客户租赁电影可以为客户积累积分，规则是每次累计增加1分，若是新片，
     并且租期大于1天，再增加1分
  5. 暂且不考虑系统界面问题和系统存储问题；

目的：
  1. 完成需求;
  2. 能够了解面向对象的程序设计;
  
思路：  
  电影类 {三类影片类型;}
  客户类Customer{属性：客户名；容器：存放多个租赁；租赁统计index；
		方法： addRental delRental；打印积分金额}
  租赁类Rental{租赁的电影；租赁的天数}
*/  
```

