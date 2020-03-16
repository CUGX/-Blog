# JavaStep3__JavaSECore

> 学前指导：学会使用帮助文档，Java1.8接口文档见同仓库文件jdk api 1.8_google.CHM

## 1、常用类



### 1、Java.lang.String类

1. String是final类，不能被其他类继承；

2. 查看帮助文档的构造函数，得到初始化方法；

3. 通过new新建对象一定会分配空间，直接赋值不一定分配，称为享元模式；

   ```java
   String s1 = "String";
   String s2 = new String("String1");
   ```

4. 字符串的连接:

   ```java
   String s = "a"+"b"+"c";等价于String s = "abc";
   String s = 1+2+3+“abc”;//6abc
   //字符串开头则认为其后面的都是字符串
   String s = "abc"+1+2+3;//abc123
   ```

5. 字符串的子串 		

   ```java
   //默认到最后
   string.subString(开始的位置);
   //包含头，不包含尾:
   string.subString(开始的位置，结束的位置);
   //存在则返回第一次出现的位置，否则返回-1
   string.indexOf();
   //大写小写的转换
   string.toUpperCase();
   string.toLowCase();
   //字符串比较、忽略大写小写比较是否相等
   string.equals();
   string.equalsIgnoreCase();
   //字符串的连接
   string.concat(串2);
   //第一个不相同的字符开始比较，返回ASC码的差值
   string.compareTo();
   //获取值
   string.valueOf();
   //split方法中的“.”“|”都是转义符，如果要用这个作为regex，需要加转义符“\\.”,“\\|” 
   string.split(",");//字符串的分割
   ```



### 1.2、StringBuffer、StringBuild类

1. 字符串的拼接用这两个类，效率高，用字符串+，浪费空间，产生很多垃圾；

2. StringBuffer线程安全，StringBuild线程不安全，但在多线程场景下性能比StringBuffer要高；

   ```java
   StringBuffer sb = new StringBuffer();
   sb.append("apend");
   
   StringBuilder sbd = new StringBuild();
   sbd.append("apend");
   ```



### 1.3、正则表达式（Pattern类）

1. 正则表达式：用一些有意义的字符组成的字符串；

2. 原子：正则表达式的最基本的组成单位。正则表达式可以单独使用的字符就是原子；

   - 所有可以显示的字符或非打印的字符；

   - “.”、“？”、“+”、“*”有特殊意义，若要 单独使用他们，需要使用转义字符“\”，Java中“\”也需要转义，所以“ab\\+"才代表ab+；

   - 在正则表达式可以直接使用的一些范围的原子

     | 原子 | 含义                               |
     | ---- | ---------------------------------- |
     | \\\d | 表示任意一个十进制的数字0-9；      |
     | \\\D | 表示任意一个除数字之外的字符;      |
     | \\\s | 表示任意一个空白字符：空格\n\r\t\f |
     | \\\S | 表示一个非空白字符：空格\n\r\t\f   |
     | \\\w | 表示任意一个a-zA-Z0-9的字符        |
     | \\\W | 表示非a-zA-Z0-9的字符              |

   - 自己定义个原子：

     - [0-9]
     - [5-8]
     - [a-z5-8]
     - [^0-9]
     - [^a-zA-Z0-9_]

3. 元字符：用来修饰原子，不能单独出现

   | 元字符 | 含义                                |
   | ------ | ----------------------------------- |
   | \*     | 表示原子可以出现0次或多次；         |
   | \+     | 表示原子可以出现1次或多次；         |
   | ？     | 表示原子可以出现0次或1次；          |
   | {}     | 用于定义原子出现的次数；{m} 出现m次 |
   | {m,n}  | 出现m到n次，包含m和n                |
   | {m,}   | 最少出现m次                         |
   | .      | 表示除换行之外的任意一个字符        |
   | ^      | 表示以什么开头                      |
   | $      | 表示以什么结尾                      |
   | \|     | 表示或的关系                        |

   ```java
   //eg:邮箱的输入格式
   //假设名字是数字字母下划线，@后面是数字字母接着是.字母
   String regex = "^\\w+@[a-zA-Z0-9]+(\\.[a-zA-Z])+$"
   String s = "sfsdab+kanigab+dmmab+msb";
   //String regex = "ab\\+";
   Pattern pat = Pattern.compile(regex);
   Matcher match = pat.matcher(s);
   match.matcher(); //判断一个字符串是否匹配一个正则表达式
   
   matcher.matches //在一个字符串中找出匹配正则表达式的元素
   matcher.group() //在一个字符串中找出匹配正则表达式的元素
   ```

   

### 1.4、java.lang.Math类

```java
floor(10.3);//返回去余的值10 
ceil(10.3);//返回进一的值11
round(10.3);//四舍五入 10 
    
//随机数的生成
Math.random()//返回0-1之间的随机值
int a = (int)( Math.random()*100 ) //产生0-100之间的随机数
```



### 1.5、java.util.Date

1. 年份从1900年开始，+1900；

2. 月份是从0开始计的，得到当前月份+1 都已经过时，不常用，用下面的Calendar类；

3. getTime()返回从1970年1月1日00以来的毫秒数；

4. Date常用来做日期格式转换 或 将字符串转换成Date格式；

   ```java
   //转换中MM表示月份，mm表示分钟
   //HH表示24小时制 hh表示12小时制
   public static String formatDate(Date date) {
       SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
       return sdf.format(date);
   }
   public static Date stringToDate(String s) {
       //传递来的字符串必须是跟设定的格式相同，否则产生异常
       //在这里可以使用正则表达式来限定日期对象是否合法
       SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
       try {
           return sdf.parse(s);
       }catch(ParseException e) {
           e.printStackTrace();
           return null;
       }
   
   }
   ```

   

### 1.6、java.util.Calendar

1. 从1970年开始计时，Calendar是抽象类，不能直接初始化；

   ```java
   //Calendar是抽象类，获取的是子类的对象
   Calendar cal = Calendar.getInstance();
   c.get(Calendar.Year);
   c.get(Calendar.Month)+1;//月份从零开始
   c.get(Calendar.Day);
   c.get(Calendar.Year);
   
   c.setTime();//设置日期的方法
   ```

2. 适合做日期的运算

   ```java
   //eg：计算转正日期，开始日期的三个月后转正
   public static void From(){
       String s = "2018-9-9";
       //调用上面的方法stringToDate
       Date date = stringToDate(s);
       Calendar cal = Calendar.getInstance();
       c.setTime(date);
       //计算转正日期
       c.add(Calendar.Month,3);
   }
   ```



### 1.7、System类

```java
System.out
System.currentTimeMillis()//毫秒数 

//得到当前时间和日期的方法二
Calendar cal = Calendar.getInstance();
c.setTimeInMillis(System.currentTimeMillis());
syso(c.get()Calendar.Year);

//退出程序：	 0:表示程序正常终止
//			非0：表示程序异	常终止
System.exit(int status);
```



## 2、反射（重难点）

### 2.1、java.lang.Class

​		在面向对象的原理中，万事万物皆为对象；那么类也是对象，那么这个对象该如何表示？这个对象是哪个类的呢？

```java
class A {}
//A类对象的表示：
A a1 = new A();
//那么A类是那个类的对象呢？A类是java.lang.Class的对象
//这个对象称为类的类类型（ClassType），有如下三种表示方法：

//1、类名.class	
Class c1 = A.class; //任何一个类都有一个隐含的静态成员变量class
//2、如果对象存在，可以直接  对象.getClass();
Class c2 = a1.getClass();
//3、core为包名，A为类名
/*
	这种方式也代表了动态加载类；
	动态加载类指 在运行时加载类，而非编译时
	//用new为静态加载的方式；
*/
c3 = Class.forName("core.A");
//4、通过类类型，直接获取类的对象通过c1，c2，c3直接获取A类的对象
//父类的引用，引用的是获取的子类的实例
A aa = (A) c1.newInstance(); //调用无参的构造函数，返回时需要做强制类型转换
```



### 2.2、动态加载类

1. 以后一般将功能性的类往往都是动态加载的而非静态加载的；

2. 动态加载类使用一个接口，其他功能类实现此接口即可，再使用类类型获取具体的类，调用具体的方法。

   

### 2.3、Class的常用方法

> 通过Class类就可以获取类的所有信息，包括方法，成员变量，构造函数等等，要获取类的信息，先获取类类型，介绍如下：

```java
//1. 获取类的方法===>方法也是对象 java.lang.reflecct.Method类的对象
getMethods;	//获取了所有的public方法，父类继承也是
getDeclaredMethods();	//获取自己声明的方法，不管访问权限，继承的方法得不到
Method[] ms = c.getMethods();	

//getReturnType 获取的是方法的返回值类型的类类型
Class returnType = ms[i].getReturnType();
syso(returnType.getName());
syso(ms[i].getName())//获取方法的名字
//getParameterTypes 获取方法的参数列表的类类型
Class[] paramsType = ms[i].getParameterTypes();
syso(paramsType[i]);

//2. 获取成员变量的信息
//成员变量也是对象，是java.lang.reflect.Field
getFields()//获取所有public的成员变量
Field[] fs = c.getDeclaredFields();
//获得成员变量的类类型
Class fieldType = field.getType();

//3. 获取构造函数
Constructor[] cs = c.getDeclaredConstructors();
//获取方法名
syso(cs[i].getName());
//获取参数列表的类类型
Class[] paramsType = cs[i].getParameterTypes();
syso(paramsType.getName());

//4. 获取包的信息，接口；判断是否为String类，是否为接口等等判断……

//获取类名、返回值类型、参数列表	类类型
```



### 2.4、方法的反射

```java
//java.lang.reflect.Method类的使用

//1. 原来使用对象调用方法，现在使用Method方法的对象直接调用方法
MethodTest mt = new MethodTest();
Class c = mt.getClass();
Method method = c.getMethod("f",new Class[]{int.class,int.class});//f:方法名，后面是参数列表的类类型
mt.f(10,10);
//两者等价
//先获取一个类类型和方法名称，再考虑 方法的反射操作method.invoke(object,参数)
method.invoke(mt,new Object[]{10,10});

//2. 为什么要使用方法的反射，有时我们需要根据函数名调用函数
//eg: 当输入的字符就是要调用的函数的名称时，可以省略很多的if...else...语句
```



### 2.5、成员变量的反射

​		获取成员变量，成员变量的反射操作，set/get()。

### 2.6、构造函数的反射

```java
/*
如何获取一个构造函数
如何通过构造函数的反射创建对象
*/
//通过构造函数的反射创建对象：
student stu = (student)constractor.newInstance(new Object[]{});
```



### 2.7、数组的反射

```java
/*
Java中的数组都是对象
数组的类类型只跟数组的类型和维数相关
*/
Method method = c.getMethod("f",new Class[]{int[].class,int[].class});

//java.lang.refactor.Array，判断是否为数组
Class c = obj.getClass();
c.isArray();

/*
总结：反射使用步骤：
1. 获取类类型；Class c = 
2. 获取方法: Method method = 
3. 反射的操作
*/
```

> Demo:

```java
package reflectDemo;
//步骤：
//1. 获取类类型；Class c = 
//2. 获取方法: Method method = 
//3. 反射的操作
import java.lang.reflect.Method;

public class ReflectDemo1 {

	public static void main(String[] args) {
		ReflectTest rt = new ReflectTest();
		Class rtc = rt.getClass();
		Class rtc1 = ReflectTest.class;
		try {
			Class rtc2 = Class.forName("reflectDemo.ReflectTest");
			try {
				ReflectTest rt2 = (ReflectTest) rtc2.newInstance();
				Method m2 = rtc2.getDeclaredMethod("test1", new Class[]{int.class,int.class});
				m2.invoke(rt2, new Object[]{10,5});
			} catch (Exception e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			} 
		} catch (ClassNotFoundException e1) {
			e1.printStackTrace();
		}
		
		try {
			Method ms = rtc.getDeclaredMethod("test1", new Class[]{int.class,int.class});
			ms.invoke(rt, new Object[]{10,15});
			
			Method ms1 = rtc1.getDeclaredMethod("test2", new Class[]{int.class,int.class});
			ms1.invoke(rt, new Object[]{10,13});
		} catch (Exception e) {
			e.printStackTrace();
		} 
	}

}


class ReflectTest {
	private int data1;
	public int data2;
	public ReflectTest(){}
	
	public ReflectTest(int data1, int data2) {
		super();
		this.data1 = data1;
		this.data2 = data2;
	}

	public int test1(int a,int b){
		System.out.println(a+b);
		return a+b;
	}
	public int test2(int a,int b){
		System.out.println(a>b?a:b);
		return a>b?a:b;
	}
	
}
```



## 3、集合框架

java.util.* 
	---->java.util.Collection接口
		---->java.util.List接口
			---->java.util.ArrayList(Vector )
			---->java.util.LinkedList
		---->java.util.Set ；Set集合中不能放重复对象，是否重复由equals方法决定,equals方法需要自己重写。



### 3.1、java.util.ArrayList

#### 3.1.1、基本使用

> 底层操作的就是数组 对数组进行添加元素、移除元素、扩容操作等）；

```java
list.add();//添加元素
list.size();//查看有效元素的个数，
list.get(0);//获取第1个元素

//可以存放任何类型的元素，任何对象，若存取的是对象，取出来的时候可以对其进行强制类型转换
ArrayList list = new ArrayList(); 
syso(list);//打印所有元素的toString()

//ArrayList提供的迭代器java.util.Iterator
//以下实现 迭代器遍历
Iterator itor = list.iterator;
while(itor.hasNext()){
Product pro = (Product) itor.next();
syso(pro);
}
//法二：foreach遍历
for(Object object : list) {
Product pro = (Product) itor.next();
syso(pro);
}

list.contains(product1);//判断是否包含某一元素
/*
contains的与实际应用有区别：
	contains()中使用equals方法进行判断，是比较两个对象的地址，与实际应用中的比较内容不同，所以一般需要根据实际情况重写 contains方法；利用工具自动生成equals()方法。
*/
    
//移除某个元素
list.remove(0);list.remove(pro1);
//移除所有元素
list.clear();  
list.removeall();  
...
```

#### 3.1.2、ArrayList源码分析

```java
//ArrayList底层是数组	
//    重要成员：
    Object[] elementData;//底层数组
	int size; //有效元素的个数
//分析可得到数组长度默认为10，扩容、移位的操作
//移除元素要做前移操作

//注意：ArrayList这种有数组作为底层的操作，最好估算元素的多少来指定底层数组的大小，避免扩容操作，因为扩容操作会产生垃圾，需要垃圾回收，占用内存，性能较差
ArrayList list = new ArrayList(大小);      
```



#### 3.1.3、java.util.Vector

1. 常与ArrayList一起比较/使用，底层操作的也是数组；
2. 大部分功能和ArrayList一样，遍历方式多了一种Enumeration遍历；
3. ArrayList线程不安全，Vector是线程安全的；
4. jdk7之前的版本两者的扩容大小稍有差异。



### 3.2、集合中的泛型转换

1. 集合容器可以容纳任何对象，实际应用中，一个容器中往往存取一个类型的对象；

2. 泛型操作，指明products集合中只存取Product类型的对象，而且在实际 使用中不需要进行强制类型转换；

   ```java
   ArrayList<Product> products = new ArrayList<Product>();
   //迭代器同理
   ```

3. 泛型操作只在编译阶段有效，防止错误输入，绕过编译就能绕过泛型；

   反射都是绕过编译操作的。	
   		

### 3.3、LinkedList（链表）

1. 链表的操作：方法与ArrayList的操作类似,方法和迭代器均相同 ；

   ```java
   llist.contains();//需要重写equals方法
   LinkedList<String> llist = new LinkedList<String>();
   llist.add("xxx");
   llist.add("hello");
   llist.add("world");
   llist.size();
   .......
   ```

2. 提供了更加丰富的头尾操作,适用于头尾操作较多的应用场景

   ```java
   //案例：贪吃蛇（控制台版本） ，去尾加头 
   Node Worm WormPanel WormTest
   addFirst() addLast() getFirst() getLast()
   removeFirst() removeLast
   syso(list);//打印list中的全部内容[ele1,ele2,...]
   ```

3. 数据结构（链表结构--->双向链表）

   ```java
   //分析代码可得到：其数据结构为链表结构，并且为双向链表，next、prev
   //内部包含一个静态内部类Node
   Object object
   Node next
   Node prev 
   ```

   

### 3.4、java.util.HashSet集合

1. 基本方法和上面的集合相同

   - 容纳的对象必须根据自己的唯一标识来重写equals和hashCode方法；

   - 对象一旦放进HashSet容器中，那么对象的唯一标识属性就不能修改，否则导致对象无法删除加入重复元素会自动过滤加不进去；

     ```java
     //equals和hashCode方法 必须要重写，用唯一标识符去重写 ，识别同一对象
     //hashCode方法是一种散列算法，和 HashSet中数据的存放有着直接关系
     HashSet<Student> student = new HashSet<Student>();
     student.add(stu1);
     student.add(stu1);//加不进去
     ```

2. HashSet的数据结构

   HashSet的底层使用的是HashMap，后面详述。



### 3.5、java.util.TreeSet

1. TreeSet中存取的对象必须能够排序，排序方式由自己来定义;

2. 两种排序的比较器：

   - java.lang.Compareable，TreeSet 使用无参的构造函数，那么容纳的对象必须实现Compareable接口 

   - java.lang.Comparator；

     

> java.util.Map
> 	---->java.util.HashMap
> 	---->java.util.TreeMap
> 	---->java.util.HashTable

### 3.6、java.util.HashMap

1. 基本操作：

   ```java
   User u1 = new User("001","zhangsan",20);
   User u2 = new User("002","lisi",21);
   User u3 = new User("003","wangwu",23);
   HashMap hm = new HashMap();
   hm.put(u1.getId(),u1);//两个参数key 和 value
   hm.put(u2.getId(),u2);hm.put(u3.getId(),u3);
   hm.size();
   User u2 = (User)hm.get("001");//get(key),得到value
   hm.containsKey("002");//判断是否包含某一key
   
   //遍历方法
   //方法一：
   Set set = hm.keySet();
   for(Object object : set ){
       String key = (String) object;
       User u = (User)hm.get(key);
       syso(key+""+u);
   }
   //方法二：
   //放入HashMap集合的key，value都会被包装成Mao.Entry这个内部类的属性
   //有一个键值对就存在一个Map.Entry的实例对象
   //通过entrySet()方法就可以把这些实例对象放入Set集合中，遍历Set获取每个对象
   Set set1 = hm.entrySet();
   for(Object object : set1 ){
       Map.Entry me = (Map.Entry)object;
       syso(me.getKey()+"-"+me.getValue());
   }
   ```

   

2. 通过泛型实现基本操作，避免强制类型转换，推荐使用：

   ```java
   //这两个参数分别为key和value的类的对象
   HashMap<String,User> user = new HashMap<String,User>(); 
   //方法一：
   Set<String> set = hm.keySet();
   for(String key : set ){
       syso(key + "-" + users.get(key));
   }
   //方法二：
   Set<Map.Entry <String,User> > set1 = hm.entrySet();
   for(Map.Entry <String,User> me : set1 ){
       syso(me.getKey() + "-" + me.getValue());
   }
   ```

   

3. map.put(key,value)；

   ```java
   /*
   若key相同，则后面的会覆盖后面的数据，
   这里需要重写 equals和hashCode方法，很适合计数，
   TreeSet常用来排序，两者常常联合起来使用，面试常有
   */
   //案例： 统计字符串中某个字母的出现次数，并按照字母顺序排序输出
   
   ```

   

4. 数据结构：（数组+链表结构），源码分析：

   - HashMap包含一个Entry(key,value,next,hash)的内部类，key/value放入HashMap的时候都会被包装成Entry的对象;

   - HashMap的成员就有Entry的数组，该数组的大小默认是16，且永远是2的次方数，若不是2的次方数，会自动转换成最接近的>=2的次方数；

     put(key,value)式就是转换成Entry对象并放入数组中；

   - put方法的实现

     - 根据key的hashCode进行hash运算（不关心hash算法）得到hash值

     - 根据hash值确定在数组中的位置：

       ```java
       /*
       hash&(table.length-1)
       等价于hash%table.length,length是2的次方数，则该公式java成立
       */
       ```

   - 如果计算出来的位置没有元素，直接包装Entry的实例并给数组元素赋值

   - 如果计算出来的位置已经有元素存在，则判断key是否相同，若相同，则覆盖并且要遍历整个链表；若都不覆盖，则放入链表头部

   - hashvalue不同放数组，相同放链表；

   - get方法的实现；

5. 注意：

   - 若计算出来的位置相同，这就是冲突率，我们要减少冲突率，因为一旦放入链表中，以后总要遍历整个链表，效率差，要尽量吧元素直接放入数组而非链表，所以要根据实际要求重写hashCode和equals方法；

   - 底层是数组，要尽量减少扩容，所以HashMap放入元素的时候也应该要估算数组的大小，避免扩容操作

   - HashMap中有加载因子，默认是0.75，

     比如大小16，那么数组有12个元素时会自动扩容一倍

   - 通过key放入HashMap就不应该修改hashCode和equals方法生成的相关的属性的值，否则就找不到了；

   - 在hashSet中类似，hashSet的底层是HashMap，add方法中只用HashMap的key，value。

### 3.7、java.util.TreeMap

```java
//TreeMap(key,value);
//key必须要能根据 某种规则排序通过两种比较器;
//TreeSet中使用的就是TreeMap
TreeMap<User,Integer> user = newTreemap<User,Integer>();
User u1 = new User("001","zhangsan",20);
User u2 = new User("002","lisi",21);
User u3 = new User("003","wnagsi",22);
user.put(u1,1);
user.put(u2,2);
user.put(u3,3);

for(User users : user.getKey()){
    syso(users+"-"+users.get(users));
}

TreeMap<User,Integer> user1 = newTreemap<User,Integer>(new Comparator(){
    public int compare(User o1,User o2){
        return o2.getName().compareTo(o1.getName());
    }
}
                                                      );
user1.put(u1,1);
user1.put(u2,2);
user1.put(u3,3);
syso(user1);
```



### 3.8、java.util.Hashtable 

1. 和HashMap相比类的层次结构不一样，但也实现了Map接口，基本操作类似;

   ```java
   //HashMap中key/value都可以是null，
   //而在Hashtable中不能是null，会抛出异常NullPointerException
   Hashtable<String,String> hs = new Hashtable<String,String>();
   hs.put("aa","hello");
   hs.put("bb","world");
   syso(hs);
   ```

2. Hashtable是线程安全的（线程并发时效率低，但是数据一致性高）；

   HashMap是线程不安全的（线程并发时效率高，但是数据可能会存在安全问题）

3. Hashtable中有个子类java,util.Properties类，这个类将来用的比较多的用来加载资源文件，将IO操作时会详细讲解。



### 3.9、其他 

1. 还有一些集合类可以查阅手册 ，常用的已经介绍;

2. java.util.Collections类:

   该类封装了一些对集合的操作都是静态方法，面试时可能会问和Collection类有什么关系，没什么关系，介绍两个类的用途即可;

   ```java
   List<String> list = new List<String>();
   list.add("bb");
   list.add("aa");
   list.add("cc");
   syso(list);
   
   Collections.reverse(list);//倒置
   syso(list);
   Collections.sort(list);//排序 
   ......
   ```

   

## 4、IO专题

### 4.1、编码问题

1. 一个字符串放入文本文件-->可以认为是字符串的序列化

2. 从文本文件中读取字符串-->可以认为是字符串的反序列化

3. 文本文件中内容都是以字节的方式呈现的；其实在字符串放入文本文件时，需要把字符串转换成字节，转换成字节的时候可以选择多种编码；

4. 以什么样的编码方式转换成字节的，将来再转换成字符串也需要用该编码；

   ```java
   /*常用的编码：gbk、UTF-8、UTF-16BE
   java中一个字符占两个字节，中英文都是；
       Java默认使用UTF-16BE编码，一个中英字符占两个字节;
       gbk编码1个英文字母占1B，中文占2个字节;
       UTF-8编码1个英文字母占1B，中文占3个字节;
   */
   String s = "HelloWorld";
   byte[] b1 = s.getBytes();//转换成字节数组
   for(byte c : b1) {
       syso(Integer.toHexString(c&0xFF)+" ");//16进制显示
   }
   
   byte[] b1 = s.getBytes("gbk"); //以什么编码方式转换成字节数组
   
   String s = new String(b1);//将字符数组变成字符串
   String s = new String(b1,"gbk"); //以gbk编码将字符数组变成字符串
   ```

   

### 4.2、java.io.File

```java
//java.io.File 表示文件或目录
File f = new File("D:\\Javaworkspace");
f.exists();//判断目录是否存在
File f1 = new File("D:\\Javaworkspace\\aa");
//=====》等价于：
File f1 = new File(f,"aa");//f相当于f1的父目录
File f1 = new File("D:\\Javaworkspace","aa");

if(!f1.exists()){
    f1.mkdir();	//不存在则创建目录
    f1.mkdirs();//创建多级目录
}else{
    f1.delete();//删除目录
}
f1.isDirectory();//判断是否为目录，不存在也返回false
f1.isFile();//判断是否为文件 ，不存在也返回false

//文件操作
File f2 = new File("D:\\Javaworkspace\\a.java");
f2.exists();
f2.isFile();
File f3 = new File(f,"a.java");
if(!f3.exists()){
    f3.createNewFile();//创建文件，要捕捉异常 
}else {
    f3.delete();/删除文件
}
file.getAbsolutePath();//得到绝对路径
file.getName();//返回最后一级
file.getParent();//返回非最后一级的目录，父目录的字符串

file.getParentFile();//得到父目录的File对象
```



### 4.3、文件的过滤操作

```java
list() & lsitFiles();
list(FilenameFilter);
listFile(FileFilter or FilenameFilter);//注意递归问题
/*
文件删除：目录必须为空目录才能 被删除，所以 删除一个目录必须先删除其子目录的内容
	list(): 列出该目录下所有的目录名和文件名，不会列出父目录和子目录;
	lsitFiles() 列出该目录下所有的目录名和文件名，不会列出整个目录的名称，也不会遍历子目录，直接列出目录下的，产生文件对象
*/
String[] filename = file.list();
String[] files = file.listFiles();
//打印所有文件和目录 ，包括子目录
public static void main(File file) {
    String[] files = file.listFiles();
    for(File f : files) {
        if(f,isFile){
            syso(f.getAbsolutePath);
        }else{
            listFile1(f);//递归调用
        }
    }
}

File[] files = file.listFiles(new FileFilter(){
    public boolean accept(File pathname) {
        //pathname就是每个file下的文件或目录
        return pathname.getAbsolutePath().endwith(".java"); //过滤以.java结尾的文件
    }
});
public static void listFile2(File file) {

}
```



> 文件的读写操作：
> 字节流----->读写以字节为单位
> 字符流----->读写以字符为单位，只适合文本类的文件，不适合二进制文件
> 	输入流--->读操作
> 	输出流--->写操作

### 4.4、以字节为单位的输入输出流

输入流--->读操作 InputStream
		---->FileInputStream 把文件作为字节流进行读操作
		---->BufferedInputStream 缓冲输入（缓冲区，先读入缓冲区）
		---->PipedInputStream 管道流
		---->DataInputStream  readInt readBoolean ...
		---->System.in  从键盘度数据 
	输出流--->写操作 OutputStream
		---->FileOutputStream 把文件作为字节流进行写操作
		---->BufferedOutputStream
		---->PipedOutputStream 管道流
		---->DataOutputStream writeInt writeBoolean ...
		---->System.out  属于PrintStream

```java
/*将输入管道和输出管道关联起来，需要用一句体现关联关系
out 写出来的数据正好被in读到
  输入输出过程中，可以读取文本、图片、mp3、MP4等格式文件，输出都能够达到对应格式的文件
*/
PipedInputStream(PipedOutputStream());
PipedOutputStream((PipedInputStream));
    
//文件已经存在时,直接往文件中追加内容，则将IO流的第二个参数置为true即可
FileInputStream fis new FileInputStream("D:\\clanguage\\app.c"，true);

//所有IO操作都有IOException产生
public static void test1() {
    FileInputStream fis new FileInputStream("D:\\clanguage\\app.c");
    //若app.txt不存在就直接创建，存在先删除再创建 	
    FileOutputStream fos new FileOutputStream("D:\\clanguage\\app.txt");
    int c;
    while((c = fis.read())!=-1) {
        syso((char)c);
        fos.writer(c);
    }
    fis.close();
    fos.close();//关闭文件
    /*fis.read(); //一次读取一个字节，直到文件末尾，返回-1
    read()方法的问题：
        1. 对于单字节编码字符没有问题，对于一个字符占用多个字节的文字打印直接打印会乱码
        2. 对于较大的文件读取效率比较低 
    */
}

public static void test2(){
    FileInputStream fis new FileInputStream("D:\\clanguage\\app.c");
    FileOutputStream fos new FileOutputStream("D:\\clanguage\\app.txt");
    byte[] b = new byte[1024*5];//批量读取，效率较高
    //往b字节数组中放，从第0个位置开始放，最多放b.length个
    int bytes = fis.read(b,0,b.length);
    String s = new String(b,0,bytes);
    syso(s);
    //方法二：好处是，文件较大时操作不需要定义一个特别大的数组，会分次批量读取
    int bytes;
    while((bytes=fis.read()) != -1 ){
        String s = new String(b,0,bytes);
        fos.write(b,0,bytes);
    }	
    fis.close();//关闭文件
}

public static void test3(){
    FileInputStream fis new FileInputStream("D:\\clanguage\\app.c");
    FileOutputStream fos new FileOutputStream("D:\\clanguage\\app.txt");
    //缺点：若文件太大，开辟的内存看见就太大，不合适，建议使用test2中的方法读取文件
    int length = in.available();//返回可读的字节数
    byte[] b = new byte[length];
    fis.read(b);
    fos.write(b);

    fis.close();//关闭文件
    fos.close();
}

public static void test4(){
    FileInputStream fis new FileInputStream("D:\\clanguage\\app.c");
    FileOutputStream fos new FileOutputStream("D:\\clanguage\\app.txt");

    //aa.txt文件是UTF-编码，bb.txt为GBK编码，在转换成字符串时需要使用目标编码格式
    byte[] b = new bytes[1024];
    int bytes = fis.read(b,0,b.length);
    String s = new String(b,0,b.length,"utf-8");
    out.write(b,0,bytes);

    fis.close();//关闭文件
}
```



### 4.5、字符流

> 字符流(适合文本的操作)-->都是由字节流构造而来（适配器模式）,字符流都带有缓冲区，需要flsuh

输入流--->Reader
		--->InputStreamReader
		--->FileReader
		--->BufferedReader 一次读一行readLine()
			PrintWriter一起使用，可以直接使用println()方法，不用使用newLine()方法
	输出流--->Writer
		--->OutputStreamWriter
		--->FileWriter
		--->BufferedWriter write(String, int, int)                                                                                                         newLine()可以写字符串，但是需要知道起始、结束位置.

```java
public static void test() {
    InputStreamReader in = new InputStreamReader(
        new FileInputStream("D:\\clanguage\\app.c"),"GBK");
    OutputStreamReader out = new OutputStreamReader(
        new FileOutputStream("D:\\clanguage\\app.c"),"GBK");
    //FileReader、
    FileReader fr = new FileReader("D:\\clanguage\\app.c"); //等价上面的声明方法
    FileWriter fw = new FileWriter("D:\\clanguage\\app.c");

    //方法一：
    int c;
    while((c=in.read(c,0,c.length)!=-1)){
        syso((char)c);
        out.write(c);
        out.flush();
    }
    //方法二：
    int chars;
    char[] c = new char[1024];
    while((chars = in.read(c,0,c.length))!=-1){
        String s = new String(c);
        syso(s);
        out.write(c,0,chars);
        out.flush();
    }
    in.close();//关闭文件
}

public static void test2(){
    BufferedReader br = new BufferedReader(( 
        new InputStreamReader( 
            new FileInputStream("D:\\clanguage\\app.c"))));
    //上下等价
    BufferedReader br = new BufferedReader( 
        new FileReader("D:\\clanguage\\app.c"));
    BufferedWriter bw = new BufferedWriter(
        new FileWriter("D:\\clanguage\\app.c"));
    //普通的读方法都有，其主要有readLine()方法 
    //不识别换行，需要自己换行
    String str = null;
    while((str.br.readLine())!= null ){
        syso(str);
        bw.write(str,0,str.length());
        bw.newLine();
        bw.flush();
    }
    br.close();
    bw.close();
}

//RandomAccessFile : 随机访问文件流
public static void test() {
    RandomAccessFile raf = new RandomAccessFile("D:\\clanguage\\app.c", "rw");
    syso(raf.getPinter());
    raf.writeInt(1000);

    raf.seek(0);//定义位置
    raf.readInt();
}
```



### 4.6、java.util.Properties类

java.util.Properties类，不是IO类、是HashSet的子类

- 实际使用需要和IO集合在一起使用

- 资源文件：以properties作为文件的扩展名

- 文件的内容是以key = value的方式存放

- 资源文件将来在设计中可以用来解耦+反射结合使用

  后缀名：.properties		eg: a.properties

```java
public static void test() {
    Properties prop = new Properties();
    prop.load(new FileInputStream("D:\\clanguage\\a.properties"));

    for(Object object : prop.keySet()){
        String key = (String) object;
        syso(key + " = " + prop.get(key));
    }
}
```



### 4.7、序列化

> 序列化--->对象的序列化(对象进行存储or在网络中进行传递，该对象必须进行序列化)
>
> ​    java.io.ObjectOutputStream
> ​	java.io.ObjectInputStream
> ​	java.io.Serializable//该接口只是个规范，没有任何方法

1. transient关键字

   - 如果一个对象的属性用transient修饰，那么该属性不会做jvm默认的序列化工作;	

   - 已经用transient关键字修饰的变量可以用如下方式进行自己的序列化和反序列化

     ```java
     private void readObject(java.io.ObjectInputStream stream) throws IOException,ClassNotFoundException{}
     
     private void writeObject(java.io.ObjectOutputStream stream) throws IOException,ClassNotFoundException{}
     ```

     

2. 集合类如何序列化，比如ArrayList

   1. 底层的对象数组是使用transient修饰的，
   2. ArrayList中有数组，且数组不一定放元素，所以没有必要把整个数组序列化，只需要序列化有效元素;
   3. 用以上两个方法签名来进行序列化和反序列化工作；

3. java实现接口和继承都具有传递性:

   - 如果父类实现了Serializable接口，那么其子类都可以序列化；

   - 注意： 如果父类是可序列化的，那么反序列化子类的时候不会调用父类的构造函数；

     如果父类是不可序列化的，而子类是可序列化的那么在子类进行反序列化时，会调用父类的构造函数；

4. Java中的克隆功能 Object类中已经提供了相应功能;

   - 一个类的对象要进行克隆可以实现Cloneable接口；
   - 浅克隆or浅拷贝 原对象和拷贝对象不等，但是两者的地址相等
   - 深克隆or深拷贝 需要通过序列化和反序列化的方式 ：原对象和拷贝对象不等，但是两者的地址也不相等；
   - 这是一种设计模式--->原型模式。
   - native关键字 表示在Java中进行声明，用C/C++实现的方法，这种方法是protected的，只能在本包中使用。

5. IO和集合一起操作

   ```
   /*
   案例 ：
   	如下文本文件：
   	1,zhangsan,20 
   	2,lisi,21
   	3,wangwu,22
   	4,zhangsan,24
   	5,lisi,35
   	6,zhangsan,25
   	7,wnagwu,26
   	
   要求：
   	统计姓名出现的次数，按照次数降序输出，次数相同按照姓名字母升序排序。
   思路：
   	把名字放入Map集合中统计次数，再使用TreeSet进行排序
   */
   ```

   

