面向过程：自顶而下，资低速快；问题分解成一个一个步骤，每个步骤用**函数**实现，依次调用。

面向对象：将事务高度抽象化，资高速慢；把属性、行为等封装成**对象**，基于这些对象及对象的能力进行业务逻辑的实现。





**封装(Encapsulation)** ：把客观事物封装成抽象的类（封装了数据以及操作这些数据的代码的逻辑实体），数据和方法只让可信类或对象操作，不可信信息隐藏。权限修饰为对象对内部数据提供了不同级别的保护。



**继承(Inheritance)**：使用现有类的所有功能，并在无需重新编写原来的类的情况下（复用）对这些功能进行扩展。一般到特殊，实现继承与接口继承。



**多态(Polymorphism)**：**运行**期间。一个类实例的相同方法在不同情形有不同表现形式。

将子类传入父类参数中，运行时调用父类方法时通过传入的子类决定具体的内部结构或行为。 

（有类继承或者接口实现、子类要重写父类的方法、==父类的引用指向子类的对象==）





**单一职责**：低耦合、高内聚。一个类，最好只做一件事，只有一个引起它的变化。

职责过多，可能引起它变化的原因就越多，这将导致职责依赖。



**开放封闭**：对抽象编程。对扩展开放，对修改封闭的。



**Liskov 替换**：子类必须能够替换其基类。保证系统在运行期内识别子类。



**依赖倒置**：依赖于抽象。高层模块不依赖于底层模块，二者都同依赖于抽象；抽象不依赖于具体，具体依赖于抽象。



**接口隔离**：多个小的专门的接口，而不要使用一个大的总接口。

委托分离（新建类型），多重继承。



**重载**：同名不同参数，同一个类内。编译期间，参数变量的类型。

**重写**：同名同参数，父子类间。运行期间，引用变量所指向的实际对象的类型。弱访问|异常处理





继承因为要复用，实现因为需要定义一个标准（传递性）

接口中只能定义全局常量（static final），无实现法，default法



复用：继承（is-a），联结类与类的层次模型，细节可见（白盒），继承编译；

​            组合（has-a），整体与部分、拥有的关系，细节不可见（黑盒），实现运行；

​			组合优于继承。



默认构造函数一般会把成员变量的值初始化为默认值，基本->0, 包装->null





类变量、成员变量和局部变量 在 JVM的方法区、堆内存和栈内存中

public  > protected > default > private 





平台无关性：一次编译，到处执行(用 Java 创建的可执行二进制程序，能够不加改变的运行于多个平台)

编译 -> 高级语言转换成低级语言

![image-20220920154602254](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220920154602254.png)



*.java --javac-->  *.class --JVM--> bin





JVM： (与平台有关,缓冲角色)根据对应的硬件和操作系统生成对应的二进制指令

Class文件： 统一的程序存储格式 字节码（中间代码）-> 统一的Class文件

Java 语言规范：保证基本数据类型在所有平台的一致性



形参：定义函数名和函数体，**接收**调用该函数时传入的参数

实参：主调有参函数括号里的参数（被**传递**）

主调函数和被调函数之间有数据传递关系。



值传递：复制传副本，不影响原始对象

引用传递：直接传地址，影响

Java按值传递	（把对象的引用当做值传递给方法。）

==对参数值的任何更改都只存在于方法的范围内。==







byte 1, short 2, int 4, long 8

float 4, double 8   近似值

char 1

boolean

==String 不是基本数据类型，是引用类型。==

溢出 (数值运算的结果超出了机器码可以表示的范围) 的时候并不会抛异常，也没有任何提示。

BigDecimal 或者 Long（单位为分）来表示金额。

Unicode 为每种语言中的每个字符设定了统一并且唯一的二进制编码（2个字节）





## Java关键字

被 transient 修饰的变量，在序列化 (持久化对象, serialization) 的时候其值会被忽略，在被反序列化后， transient 变量的值被设为初始值 (0 或 null)



instanceof 左边的对象是否是它右边的类的实例



volatile 只能修饰变量（可能被多线程同时访问），每次数据变化后都会将值强制刷入主存，使得该值在缓存中一致，保证多线程操作时该变量的**可见**性。禁止指令重排，保证指令**有序**。



synchronized 同步 方法|代码块，在同一时间，只能被单个线程访问。

原子，有序，可见 性

![image-20220921160021025](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220921160021025.png)





final 该部分无法修改

变量常量化，方法无法被覆盖，类无法被继承。



static 修饰变量，方法，代码块

静态变量 为所有的对象实例共享，不具线程安全，常用final限制。

静态方法 是属于类而不是实例。

静态块 常用于初始化类的静态变量，只在类装载入内存时，由ClassLoader执行一次。



String类不可变，一旦一个 string 对象在内存(堆)中被创建出来，他就无法被修改。

replaceAll 以及 replaceFirst 是和正则表达式有关，replace无关。

'+' 拼接字符串 是语法糖 而不是运算符重载

StringBuffer线程安全，StringBuilder连接首选（非并发）

String.valueOf() 调 Integer.toString()

switch 只支持整型，其他数据类型都是转换成后再使用



**Class 常量池**

是 Class 文件中的资源仓库（其中保存了各种常量，运行）

字节码 解除了 JVM 和 Java 语言之间的耦合。

16 进制打开 class 文件

![image-20220922184331157](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220922184331157.png)



常量池中两大类常量：字面量（literal）和 符号引用（symbolic references）。

字面量：由字母、数字等构成的字符串或者数值。

符号引用：\* 类和接口的全限定名 * 字段的名称和描述符 * 方法的名称和描述符

在 JVM 真的运行时，需要把常量池中的常量加载到内存中。

 

**运行时常量池**

每一个类或接口的常量池（Constant_Pool）的运行时表示形式。（JVM加载后就创建出）

类似传统语言中符号表（ SymbolTable）的角色

分配在 Java 虚拟机的方法区之中



**字符串常量池**

为了减少相同的字符串的重复创建，为了达到节省内存的目的。会单独开辟一块内存，用于保存字符串常量。



在每次赋值的时候使用 String 的 **intern** 方法，如果常量池中有相同值，就会重复使用该对象，返回对象引用。



字符串有长度限制，在编译期，要求字符串常量池中的常量不能超过 2^16-1;

运行期，长度不能超过 Int 的范围。







new 一个对象是存储在堆里的，我们通过栈中的引用来使用这些对象；所以，对象本身来说是比较消耗资源的。

基本数据类型直接存储在栈中。

除了 Integer 和 Character 类以后，其它六个类的类名和基本数据类型一致，只是类名的第一个字母大写即可。

包装类型 使得 基本数据类型 具有了对象的性质，并且为其添加了属性和方法，丰富了基本类型的操作。

```java
Integer integer = Integer.valueOf(1); 	// boxing
int i = integer.intValue();		// unboxing
```



包装对象的数值比较，不能简单的使用 ==，虽然 -128 到 127 之间的数字可以（Integer缓存机制），但是这个范围之外还是需要使用 equals 比较。

Integer 的缓存：整型对象通过使用相同的对象引用实现了缓存和重用。 



POJO类中的布尔类型变量不加is，否则部分框架解析会引起序列化错误。

建议 Boolean success（null异常捕获）







Exception（程序引起） 和 Error(系统级，虚拟机抛出)， ⼆者都是 Java 异常处理的重要⼦类， 各⾃都包含⼤量⼦类。 

均继承自 Throwable 类



对该异常进⾏处理（ 捕获 / 向上抛出，调用者处理）



try⽤来指定⼀块预防所有异常的程序； 

catch⼦句紧跟在 try 块后⾯， ⽤来指定异常类型； 

finally 为确保⼀段代码不管发⽣什么异常状况都要被执⾏； 

throw 语句⽤来明确地抛出⼀个异常； 

throws⽤来声明⼀个⽅法可能抛出的各种异常； 



将**异常链**抛出到下一个更高级别的异常处理程序

```java
try {
	...
} catch(IOException e) {
    throw new SampleException("Other IOException + ", e);
}
```



try-with-resources 自动关闭流操作

```java
try(BufferReader br = new BufferReader(new FileReader("e:\\A.txt"))) {
    String line;
    while((line = br.readLine()) != null) {
        SYstem.out.println(line);
    }
} catch(IOException e) {
    ... // handle exception
}
```



（try中的）return 前执行的 finally 块内，

对数据的修改效果对于 引用类型（有影响）和 值类型（不影响）会不同。





## 集合框架

Collections 是一个包含有各种有关集合操作的静态多态方法的包装类（工具），服务于Collection框架。



List，Set 都是继承自 Collection 接口,都是存储一组 相同类型 的元素的。

List 特点：元素有放入顺序，元素可重复 。

Set 特点：元素无放入顺序，元素不可重复。 



ArrayList 是一个可改变大小的数组（默认初始值10，分配一个较大的初始值）

LinkedList 是一个双链表

Vector线程安全



扩容：Vector 每次请求其大小的双倍空间，而 ArrayList 每次对 size 增长 50%



TreeSet 二叉树，自动排序，不准 null 

底层TreeMap （平衡二叉查找树）的 keySet()。元素按 key 排序，compareTo()来判断重复元素的。





HashSet 哈希表，无序，可null

底层 HashMap（链表的数组） 存储数据。添加元素时，计算hashcode值，通过 扰动计算 和 按位与 的方式计算出这个元素的存储位置。 空 ？添加：（equals（）？不添加 ：另空位）； 

扩容：(默认16) 选择大于该数字的第一个 2 的幂作为容量（9 -> 16）

capacity 就是 Map 的容量，而 size 当前元素个数。





int hash(Object k) 将任意长度的消息**压缩**到某一固定长度的消息摘要

int indexFor(int h, int length) 将 hash 生成的整型转换成链表数组中的下标

根据同一散列函数计算出的散列值如果不同，那么输入值肯定也不同。

两个不同的输入值，根据同一散列函数计算出的散列值相同的现象叫做**碰撞**





*X % 2^n = X & (2^n – 1)*

==位运算直接对内存数据进行操作==，不需要转成十进制，因此处理速度非常快。





Stream类 似用 SQL语句 从数据库查询数据的直观方式来提供一种对Java 集合运算和表达的高阶抽象。 (将要处理的元素集合看作一种流，流在管道中传输，并且可以在管道的**节点**上进行处理，比如筛选，排序，聚合)



创建

Collection.stream()

```java
List<String> strings = Arrays.asList("Hollis", "HollisChuang", "hollis", "H ello", "HelloWorld", "Hollis");	// 集合
Stream<String> stream = strings.stream();
```



Stream.of(...)

```java
Stream<String> stream = Stream.of("Hollis", "HollisChuang", "hollis", "Hell o", "HelloWorld", "Hollis");
```



中间操作

![image-20220923151231616](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220923151231616.png)





最终操作

![image-20220923151343853](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220923151343853.png)



Collection迭代

```java
List<String> list = ImmutableList.of("Hollis", "hollischuang");

// 普通 for 循环遍历
for (int i = 0; i < list.size(); i++) System.out.println(list.get(i)); 

//增强 for 循环遍历 
for (String s : list) System.out.println(s);

//Iterator 遍历 
Iterator it = list.iterator(); 
while (it.hasNext()) System.out.println(it.next());

//Stream 遍历 
list.forEach(System.out::println); 
list.stream().forEach(System.out::println);
```





