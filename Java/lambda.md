# lambda表达式没有命名，用来像**传递数据一样传递操作**。

紧密关联对象与方法。

+ ==基本语法: **(parameters) -> expression**==

  ==或 **(parameters) ->{ statements; }**==

```java
// 1. 不需要参数,返回值为 5  
() -> 5  
  
// 2. 接收一个参数(数字类型),返回其2倍的值  
x -> 2 * x  
  
// 3. 接受2个参数(数字),并返回他们的差值  
(x, y) -> x – y  
  
// 4. 接收2个int型整数,返回他们的和  
(int x, int y) -> x + y  
  
// 5. 接受一个 string 对象,并在控制台打印,不返回任何值(看起来像是返回void)  
(String s) -> System.out.print(s) 
```

+ obj.forEach((obj)->expression);

```java
String[] atp = {"Rafael Nadal", "Novak Djokovic",  
       "Stanislas Wawrinka",  
       "David Ferrer","Roger Federer",  
       "Andy Murray","Tomas Berdych",  
       "Juan Martin Del Potro"};  
List<String> players =  Arrays.asList(atp);  
  
// 以前的循环方式  
for (String player : players) {  
     System.out.print(player + "; ");  
}  
  
// 使用 lambda 表达式以及函数操作(functional operation)  
players.forEach((player) -> System.out.print(player + "; "));  
   
// 在 Java 8 中使用双冒号操作符(double colon operator)  此现象较静态引用。
players.forEach(System.out::println);  
```

+ new Obj(()->expression);

  ==Obj o = ()->expression;==

```java
// 1.1使用匿名内部类  
new Thread(new Runnable() {  
    @Override  
    public void run() {  
        System.out.println("Hello world !");  
    }  
}).start();  
  
// 1.2使用 lambda expression  
new Thread(() -> System.out.println("Hello world !")).start();  
  
// 2.1使用匿名内部类  
Runnable race1 = new Runnable() {  
    @Override  
    public void run() {  
        System.out.println("Hello world !");  
    }  
};  
  
// 2.2使用 lambda expression  
Runnable race2 = () -> System.out.println("Hello world !");  
    
race1.run();  
race2.run();
```

#  函数接口指的是只有一个**抽象方法**的接口，被当做是lambda表达式的类型

1.类名::静态方法名

```java
Student student1 = new Student("zhangsan",60);
Student student2 = new Student("lisi",70);
Student student3 = new Student("wangwu",80);
Student student4 = new Student("zhaoliu",90);
List<Student> students = Arrays.asList(student1,student2,student3,student4);
 
students.sort((o1, o2) -> o1.getScore() - o2.getScore());
students.forEach(student -> System.out.println(student.getScore()));
```

2.对象::实例方法名

```java
public class StudentComparator {
    public int compareStudentByScore(Student student1,Student student2){
        return student2.getScore() - student1.getScore();
    }
}

StudentComparator studentComparator = new StudentComparator();
students.sort(studentComparator::compareStudentByScore);
students.forEach(student -> System.out.println(student.getScore()));

```





