# Java

### 封装与隐藏

高内聚即类的内部数据操作细节不允许外部干涉；低耦合即仅对外暴露少量方法用于使用。

隐藏对象内部的复杂性，只对外公开简单的接口。

权限修饰符 `public、protected、default(缺省)、private` 置于类的成员定义前，用来限定对象对该类成员的访问权限。**对于 class 的权限修饰只可以用 public 和 default(缺省)。**

| 修饰符       | 类内部 | 同一包 | 不同包子类 | 同一工程 |
|:--------- | --- | --- | ----- | ---- |
| private   | Yes |     |       |      |
| 缺省        | Yes | Yes |       |      |
| protected | Yes | Yes | Yes   |      |
| public    | Yes | Yes | Yes   | Yes  |

[属性赋值](https://www.bilibili.com/video/BV1Kb411W75N?p=230&spm_id_from=pageDriver)的先后顺序为默认初始化、显式初始化、构造器中赋值、对象方法或属性的赋值。

构造器的作用在于<u>创建对象</u>、<u>初始化对象的属性</u>。如果没有显示的定义类的构造器的话，则系统默认提供一个空参的构造器。但若显示的定义了类的构造器后，系统不再提供默认的空参构造器。一个类中定义的多个构造器彼此构成重载。一个类中至少会有一个构造器。

```java
// 定义构造器的格式: 权限修饰符 类名(形参列表){}
public class PersonTest {

    public static void main(String[] args) {
        //创建类的对象:new + 构造器
        Person p = new Person();    //Person()这就是构造器

        p.eat();

        Person p1 = new Person("zs");
        System.out.println(p1.name);
    }
}
class Person{
    //属性
    String name;
    int age;

    //构造器
    public Person(){
        System.out.println("Person()......");
    }

    public Person(String n){
        name = n;
    }

    public Person(String n,int a){
        name = n;
        age = a;
    }

    //方法
    public void eat(){
        System.out.println("eat eat");
    }

    public void study(){
        System.out.println("study study");
    }
}
```

JavaBean 是一种 Java 语言写成的可重用组件。是指<u>类是公共的</u>、<u>有一个无参的公共的构造器</u>、<u>有属性且有对应的 get、set 方法</u>的标准 Java 类。用户可以使用 JavaBean 将功能、处理、值、数据库访问和其他任何可以用Java代码创造的对象进行打包，且开发者可以通过内部的 JSP页面、Servlet、其他 JavaBean、applet 程序或者应用来使用这些对象。用户可以认为 JavaBean 提供了一种随时随地的复制和粘贴的功能，而不用关心任何改变。

这里注意的是，构造器的权限修饰默认和类的权限修饰一致。类有 public，则构造器也有。

```java
// Customer.java
public class Customer {

    private int id;
    private String name;

    public Customer(){

    }

    public void setId(int i){
        id = i;
    }

    public int getId(){
        return id;
    }

    public void setName(String n){
        name = n;
    }

    public String getName(){
        return name;
    }
}
```

[UML 类图](https://zhuanlan.zhihu.com/p/109655171)

<u>关键字 this </u>用来修饰、调用属性、方法和构造器。this 可以理解为当前对象，或当前正在创建的对象。那么在类的方法中，可以使用 "this.属性" 或 "this.方法" 的方式调用当前对象属性和方法。通常情况选择直接省略 "this"，在方法的形参和类的属性同名的特殊情况下，必须显式的使用 "this.变量" 的方式表明此变量是属性而非形参。同理也在类的构造器中。

在类的构造器中可以显式的使用 "this(形参列表)" 的方式，调用本类中重载的其他的构造器。构造器中不能通过 "this(形参列表)" 的方式调用自身。若一个类中声明了 n 个构造器，则最多有 n -1 个构造器使用 "this(形参列表)"。"this(形参列表)" 必须声明在类的构造器的首行。在类的一个构造器中，最多只能声明一个 "this(形参列表)"。

```java
public class PersonTest {

    public static void main(String[] args) {
        Person p1 = new Person();

        p1.setAge(1);
        System.out.println(p1.getAge());

        p1.eat();
        System.out.println();

        Person p2 = new Person("zs" ,20);
        System.out.println(p2.getAge());
    }
}
class Person{

    private String name;
    private int age;

    public Person(){
        this.eat();
        String info = "Person 初始化时，需要考虑如下的 1,2,3...(共 n 行代码)";
        System.out.println(info);
    }

    public Person(String name){
        this();
        this.name = name;
    }

    public Person(int age){
        this();
        this.age = age;
    }

    public Person(String name,int age){
        this(age);    //调用构造器的一种方式
        this.name = name;
//        this.age = age;
    }

    public void setNmea(String name){
        this.name = name;
    }

    public String getName(){
        return this.name;
    }

    public void setAge(int age){
        this.age = age;
    }

    public int getAge(){
        return this.age;
    }

    public void eat(){
        System.out.println("人吃饭");
        this.study();
    }

    public void study(){
        System.out.println("学习");
    }
}
```

<u>关键字 import </u>常用于导入。在源文件中显式的使用 import 结构导入指定包下的类、接口。声明在包的声明和类的声明之间。如果需要导入多个结构则并列写出即可。可以用 "xxx.*" 的方式，表示导入 xxx 包下的所有结构。如果导入的类或接口是 java.lang 包下的，或者是当前包下的，可以省略此 import 语句。在代码中使用不同包下的同名的类，那么就需要使用类的全类名的方式指明调用的是哪个类。若已导入 java.a 包下的类，还需使用 a 包的子包下的类，仍然需要导入。import static 组合的使用是指调用指定类或接口下的静态的属性或方法。

```java
import java.util.*;
import account2.Bank;

public class PackageImportTest {

    public static void main(String[] args) {
        String info = Arrays.toString(new int[]{1,2,3});

        Bank bank = new Bank();

        ArrayList list = new ArrayList();
        HashMap map = new HashMap();

        Scanner s = null;    

        System.out.println("hello");

        UserTest us = new UserTest();

    }
}
```

package 关键字的使用是为了更好的实现项目中类的管理，提供包的概念。使用 package 声明类或接口所属的包，声明在源文件的首行。包属于标识符，遵循标识符的命名规则。每 "." 一次就代表一层文件目录。

```java
// JDK 中主要的包介绍
1.java.lang----包含一些 Java 语言的核心类，如 String、Math、Integer、System 和 Thread，提供常用功能
2.java.net----包含执行与网络相关的操作的类和接口。
3.java.io----包含能提供多种输入/输出功能的类。
4.java.util----包含一些实用工具类，如定义系统特性、接口的集合框架类、使用与日期日历相关的函数。
5.java.text----包含了一些 java 格式化相关的类
6.java.sql----包含了 java 进行 JDBC 数据库编程的相关类/接口
7.java.awt----包含了构成抽象窗口工具集（abstractwindowtoolkits）的多个类，这些类被用来构建和管理应用程序的图形用户界面(GUI)。B/S  C/S
```

### 继承性

多个类中存在相同的属性和方法时，可以将此些内容抽取到单独的一个类中，那么无需在多个类中重复的再定义这些属性和行为，只需继承那个类即可。

继承性存在的理由是减少代码冗余、便于功能拓展、为后续多态性提供前提。

```java
class A extends B{}
// A 子类、派生类、subclass
// B 父类、超类、基类、superclass
```

特别提出的是，一旦子类继承父类后，子类就获取父类中声明的结构、属性、方法，包括父类中声明为 private 的属性和方法。由于父类中的私有声明，子类不能直接调用父类的结构也仅是因为封装性的影响，一般父类提供出 get、set 方法即可。

```java
/*
 * 为描述和处理个人信息，定义类 Person
 */
public class Person {

    String name;
    private int age;

    public Person(){

    }

    public Person(String name,int age){
        this.name = name;
        this.age = age;
    }

    public void eat(){
        System.out.println("吃饭");
        sleep();
    }

    private void sleep(){
        System.out.println("睡觉");
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

}
/*
 * 为描述和处理学生信息，定义类 Student
 */
public class Student extends Person {

//    String name;
//    int age;
    String major;

    public Student(){

    }

    public Student(String name,int age,String major){
        this.name = name;
//        this.age = age;
        setAge(age);
        this.major = major;
    }

//    public void eat(){
//        System.out.println("吃饭");
//    }
//    
//    public void sleep(){
//        System.out.println("睡觉");
//    }

    public void study(){
        System.out.println("学习");
    }

    public void show(){
        System.out.println("name:" + name + ",age = " + getAge());
    }

}
/*
 * 测试类
 */
public class ExtendsTest {

    public static void main(String[] args) {
        Person p1 = new Person();
//        p1.age = 1;
        p1.eat();
        System.out.println("********************");

        Student s1 = new Student();
        s1.eat();
//        s1.sleep();
        s1.name = "Tom";

        s1.setAge(10);
        System.out.println(s1.getAge());

    }
}
```

一个类可以被多个类继承；Java 中类的单继承性即一个类只能有一个父类。

子父类是相对的。子类直接继承的父类称为直接父类；间接继承的父类称为间接父类。

子类继承父类后，就获取了直接父类以及所有间接父类中声明的属性和方法。

如果没有显式的声明一个类的父类的话，则此类继承于 java.lang.Object 类。所有的 java 类（除 java.lang.Object 类之外）都直接或间接地继承于 java.lang.Object 类。这也就意味着，所有的 java 类具有 java.lang.Object 类声明的功能。

#### 方法的重写 override/overwrite

子类继承父类以后，可以对父类中的方法进行覆盖操作。重写以后，当创建子类对象并通过子类对象去调用子父类中<u>同名同参数方法</u>时，执行的是子类重写父类的方法，即在程序执行时，子类的方法将覆盖父类的方法。

##### 面试题：区分方法的重载与重写/覆盖

方法的重写 Overriding 和重载 Overloading 是 Java 多态性的不同表现。

重写 Overriding 是父类与子类之间多态性的一种表现，重载 Overloading 是一个类中多态性的一种表现。

如果在子类中定义某方法与其父类有相同的名称和参数，该方法被重写  Overriding。子类的对象使用这个方法时，将调用子类中的定义，对它而言父类中的定义如同被<u>屏蔽</u>了。

如果在一个类中定义了多个同名的方法，其或有不同的参数个数或有不同的参数类型，则称为方法的重载 Overloading。

```java
public class Person {

    String name;
    int age;

    public Person(){

    }

    public Person(String name,int age){
        this.name = name;
        this.age = age;
    }

    public void eat(){
        System.out.println("吃饭");
    }

    public void walk(int distance){
        System.out.println("走路，走的距离是：" + distance + "公里");
        show();
    }

    private void show(){
        System.out.println("我是一个人。");
    }

    public Object info(){
        return null;
    }

    public double info1(){
        return 1.0;
    }
}
public class Student extends Person{

    String major;

    public Student(){

    }

    public Student(String major){
        this.major = major;
    }

    public void study(){
        System.out.println("学习，专业是:" + major);
    }

    //对父类中的eat()进行了重写
    public void eat(){
        System.out.println("学生应该多吃有营养的。");
    }

    public void show(){
        System.out.println("我是一个学生。");
    }

    public String info(){
        return null;
    }

    //不是一个类型，所以报错。
//    public int info1(){
//        return 1;
//    }

    //可以直接将父类的方法的第一行粘过来，直接写方法体
//    public void walk(int distance){
//        System.out.println("重写的方法");
//    }

    //直接输入父类的方法名，Alt + /，选择即可生成
    @Override
    public void walk(int distance) {
        System.out.println("自动生成");
    }
}
public class PersonTest {
    public static void main(String[] args) {
        Student s = new Student("计算机科学与技术");
        s.eat();
        s.walk(10);    
        s.study();
    }
}
```

约定俗成子类中的叫重写的方法，父类中的叫被重写的方法。

子类重写的方法的方法名和形参列表必须和父类被重写的方法的方法名、形参列表相同；子类重写的方法使用的访问权限不能小于父类被重写的方法的访问权限；特殊情况是子类不能重写父类中声明为 private 权限的方法。

父类被重写的方法的返回值类型是 void，则子类重写的方法的返回值类型只能是 void；父类被重写的方法的返回值类型是 A 类型，则子类重写的方法的返回值类型可以是 A 类或 A 类的子类；父类被重写的方法的返回值类型如果是基本数据类型 (eg: double)，则子类重写的方法的返回值类型必须是相同的基本数据类型 (double)。

子类方法抛出的异常不能大于父类被重写的方法抛出的异常；

子类与父类中同名同参数的方法必须同时声明为非 static 的（重写），或者同时声明为 static （非重写）。因为 static 方法是属于类的，子类无法覆盖父类的方法。

##### 面试题：父类的一个方法定义成 private 访问权限，在子类中将此方法声明为 default 访问权限，那么这样还叫重写吗？(NO)

#### super 关键字

super 理解为 "父类的"，用来调用属性、方法、构造器。可以在子类的方法或构造器中，通过 `super.属性|方法` 显式的调用。但是通常情况下，习惯去省略这个 `super.`。特殊情况是当子类和父类中定义了同名的属性时，要想在子类中调用父类中声明的属性，则必须显式的使用 `super.属性` 的方式表明调用的是父类中声明的属性；或者当子类重写了父类中的方法后，想在子类的方法中调用父类中被重写的方法时，必须显式的使用 `super.方法` 的方式表明调用的是父类中被重写的方法。

同时，可以在子类的构造器中显式使用 `super(形参列表)` 的方式，调用父类声明中指定构造器。需要注意的是 `super(形参列表)` 的使用，必须声明在子类构造器的首行！所以在类的构造器中，针对于 `this(形参列表)` 或 `super(形参列表)` 只能二选一，不能同时出现。若在构造器的首行，既没有显式的声明 `this(形参列表)` 或 `super(形参列表)`，则默认的调用的是父类中的空参构造器 `super()`。

在类的多个构造器中，至少有一个类的构造器使用了 `super(形参列表)`，调用父类中的构造器。

```java
public class Person {

    String name;
    int age;
    int id = 1003;    //身份证号

    public Person(){
        System.out.println("我无处不在");
    }

    public Person(String name){
        this.name = name;
    }

    public Person(String name,int age){
        this(name);
        this.age = age;
    }

    public void eat(){
        System.out.println("人，吃饭");
    }

    public void walk(){
        System.out.println("人，走路");
    }
}
public class Student extends Person{

    String major;
    int id = 1002;    //学号

    public Student(){

    }

    public Student(String name,int age,String major){
//        this.age = age;
//        this.name = name;
        super(name,age);
        this.major = major;
    }

    public Student(String major){
        this.major = major;
    }

    public void eat(){
        System.out.println("学生多吃有营养的食物");
    }

    public void Study(){
        System.out.println("学生，学习知识。");
        this.eat();
        //如果，想调用父类中被重写的，不想调用子类中的方法，可以：
        super.eat();
        super.walk();//子父类中未重写的方法，用"this."或"super."调用都可以
    }
    public void show(){
        System.out.println("name = " + this.name + ",age = " + super.age);
        System.out.println("id = " + this.id);    
        System.out.println("id = " + super.id);
    }
}
public class SuperTest {
    public static void main(String[] args) {

        Student s = new Student();
        s.show();

        s.Study();

        Student s1 = new Student("Ton",21,"IT" );
        s1.show();

        System.out.println("***********************");
        Student s2 = new Student();
    }
}
```

#### 子类对象实例化过程

从结果上看，子类继承父类以后，就获取了父类中声明的属性或方法；创建子类的对象中，在堆空间里就会加载所有父类中声明的属性。

从过程上看，当通过子类的构造器创建子类对象时，一定会直接或间接的调用其父类构造器，直到调用了 java.lang.Object 类中空参的构造器为止。正因为加载过所有的父类结构，所以才可以看到内存中有父类中的结构，子类对象可以考虑进行调用。

虽然创建子类对象时，调用了父类的构造器，但自始至终就创建过一个对象，即为 new 的子类对象。

### 多态性 Polymorphism

多态性即一个事物的多种态性。对象的多态性就是父类的引用指向子类的对象或者说子类的对象赋值给父类的引用。多态的使用常见于虚拟方法调用。有了对象多态性以后，在编译期只能调用父类声明的方法，但在执行期实际执行的是子类重写父类的方法。简而言之，编译时看左边父类的引用（父类中不具备子类特有的方法），运行时看右边子类的对象（实际运行的是子类重写父类的方法）。若编译时类型和运行时类型不一致，就出现了对象的多态性。

多态性的使用前提是<u>类的继承关系</u>以及方法的重写。值得注意的，对象的多态性只适用于方法，不适用于属性，因为属性编译和运行都看左边父类的引用。

```java
/*
 * 面试题:多态是编译时行为还是运行时行为？如何证明？
 * 运行时行为,证明见如下
 */
import java.util.Random;

class Animal  {
    protected void eat() {
        System.out.println("animal eat food");
    }
}
class Cat  extends Animal  {
    protected void eat() {
        System.out.println("cat eat fish");
    }
}
class Dog  extends Animal  {
    public void eat() {
        System.out.println("Dog eat bone");
    }
}
class Sheep  extends Animal  {
    public void eat() {
        System.out.println("Sheep eat grass");
    }
}
public class InterviewTest {
    public static Animal getInstance(int key) {
        switch (key) {
        case 0:
            return new Cat ();
        case 1:
            return new Dog ();
        default:
            return new Sheep ();
        }
    }
    public static void main(String[] args) {
//        Random.nextInt()方法生成一个随机的 int 值,该值介于 [0,n) 的区间.
        int key = new Random().nextInt(3);
        System.out.println(key);
        Animal animal = getInstance(key);
        animal.eat();
    }
}
/*
 * 多态性应用举例
 */
public class AnimalTest {
    public static void main(String[] args) {
        AnimalTest test = new AnimalTest();
        test.func(new Dog());
        test.func(new Cat());
    }
    public void func(Animal animal){ // Animal animal = new Dog();
        animal.eat();
        animal.shout();
    }
    //如果没有多态性就会写很多如下的方法去调用
    public void func(Dog dog){
        dog.eat();
        dog.shout();
    }
    public void func(Cat cat){
        cat.eat();
        cat.shout();
    }
}
```

#### 向下转型

```java
public class Person {
    String name;
    int age;
    public void eat(){
        System.out.println("Humans need to eat food.");
    }
    public void walk(){
        System.out.println("Humans can walk upright.");
    }    
}
public class Man extends Person{
    boolean isSmoking;
    public void earnMoney(){
        System.out.println("Men need money to support their families.");
    }
    public void eat() {
        System.out.println("man need to eat food.");
    }
    public void walk() {
        System.out.println("man can walk upright.");
    }
}
public class Woman extends Person{
    boolean isBeauty;
    public void goShopping(){
        System.out.println("woman willing to spend money on shopping.");
    }
    public void eat(){
        System.out.println("woman need to eat food.");
    }
    public void walk(){
        System.out.println("woman can walk upright.");
    }
}
public class PersonTest {
    public static void main(String[] args) {
        Person p1 = new Person();
        p1.eat();
        Man man = new Man();
        man.eat();
        man.age = 25;
        man.earnMoney();

        System.out.println("************************");
//        对象的多态性:父类的引用指向子类的对象
        Person p2 = new Man();
//        多态的使用:当调用子父类同名同参数方法时,实际调用的是子类重写父类的方法---虚拟方法调用
        p2.eat();
        p2.walk();
//        不能调用子类所特有的方法、属性,是由于编译时的p2是Person类型
//        p2.earnMoney(); undefined

        p2.name = "Tom";
//        p2.isSmoking = true;
//        有了对象的多态性后,内存中实际上是加载子类特有的属性和方法.但由于变量声明为父类类型,导致编译时只能调用父类中声明的属性和方法.子类的属性和方法不能调用.

//        如何才能调用子类所特有的属性和方法？- 使用强制类型转换符,即向下转型
        Man m1 = (Man) p2;
        m1.earnMoney();
        m1.isSmoking = true;

//        使用强转时出现ClassCastException异常
//        Woman wcce = (Woman) p2;
//        wcce.goShopping();


//        instanceof关键字的使用
//        为避免在向下转型时出现 ClassCastException 异常，在进行向下转型之前先进行 instanceof 的判断,一旦返回 true,就进行向下转型.如果返回 false 就不进行向下转型.
        if (p2 instanceof Woman) {
            Woman w1 = (Woman) p2;
            w1.goShopping();
            System.out.println("**********Woman Yes**********");
        }
        if (p2 instanceof Man) {
            Man m2 = (Man) p2;
            m2.earnMoney();
            System.out.println("**********Man Yes**********");
        }
        if (p2 instanceof Person) {
            System.out.println("**********Person Yes**********");
        }
        if (p2 instanceof Object) {
            System.out.println("**********object Yes**********");
        }    
//        向下转型的常见问题
//        1:编译时通过,运行时不通过
//        Person p3 = new Woman();
//        Man m3 = (Man)p3;
//        Person p4 = new Person();
//        Man m4 = (Man)p4;

//        2:编译通过,运行时也通过
//        Object obj = new Woman();
//        Person p = (Person)obj;

//        3:编译不通过
//        Man m5 = new woman();    
//        String str = new Date();
//        Object o = new Date();
//        String str1 = (String)o;
    }
}
```

![](./assets/转型.png)

```java
// 1.若子类重写了父类方法,就意味着子类里定义的方法彻底覆盖了父类里的同名方法,系统将不可能把父类里的方法转移到子类中
// 2.对于实例变量则不存在这样的现象,即使子类里定义了与父类完全相同的实例变量,这个实例变量依然不可能覆盖父类中定义的实例变量
public class FieldMethodTest {
    public static void main(String[] args){
        Sub s= new Sub();
        System.out.println(s.count); // 20
        s.display(); // 20
        Base b = s;
        // ==:对于引用数据类型来讲比较的是两个引用数据类型变量的地址值是否一样
        System.out.println(b == s);    // true
        System.out.println(b.count); // 10
        b.display(); // 20
    }
}
class Base {
    int count= 10;
    public void display() {
        System.out.println(this.count);
    }
}
class Sub extends Base {
    int count= 20;
    public void display() {
        System.out.println(this.count);
    }
}
```

#### Object 类

Object 类是所有 Java 类的根父类。如果在类的声明中未使用 extends 关键字指明其父类，则默认父类为 java.lang.Object 类。Object 类中的功能、属性、方法就具有通用性。值得注意的是 Object 类只声明了一个空参的构造器。

```java
equals() / toString() / getClass() / hashCode() / clone() / finalize()
wait()、notify()、notifyAll()
```

#### == 操作符与 equals 方法

== 运算符

* 可以使用在基本数据类型变量和引用数据类型变量中
* 都是基本数据类型变量就是比较两个变量保存的数据是否相等，类型不一定要相同。
* 引用数据类型就是比较两个对象的地址值是否相同，即引用是否指向同一对象实体。
* == 符号使用时，必须保证符号左右两边的变量类型一致。

equals 方法

* equals 是一个方法，而非运算符，只能适用于引用数据类型。

* Object 类中定义的 `equals()` 和 == 的作用是相同的，比较两个对象的地址值是否相同，即两个引用是否指向同一个对象实体。
  
  ```java
  public boolean equals(Object obj){
    return (this == obj);
  }
  ```

* String、Date、File、包装类等都重写了 Object 类中的 `equals()` 方法。不再是比较两个引用的地址是否相同，而是比较两个对象的 “实体内容” 是否相同，也是重写的原则。

```java
package com.polymorphism.basis;

import java.sql.Date;

public class EqualsTest {
    public static void main(String[] args) {

//        基本数据类型
        int i = 10;
        int j = 10;
        double d = 10.0;
        System.out.println(i == j);    // true
        System.out.println(i == d); // true

//        boolean b =true;
//        System.out.println(i == b);

        char c = 10;
        System.out.println(i == c); //true

        char c1 = 'A';
        char c2 = 65;
        System.out.println(c1 == c2); //true

        //引用数据类型
        Person cust1 = new Person("Tom" ,21);
        Person cust2 = new Person("Tom" ,21);
        System.out.println(cust1 == cust2); // false

        String str1 = new String("BAT");
        String str2 = new String("BAT");
        System.out.println(str1 == str2); // false
        System.out.println("*************************");
        System.out.println(cust1.equals(cust2)); // false
        System.out.println(str1.equals(str2)); // true

        Date date1 = new Date(23432525324L);
        Date date2 = new Date(23432525324L);
        System.out.println(date1.equals(date2)); // true
    }
}
```

##### 重写 equals 方法的原则

* 对称性：若 `x.equals(y)` 返回 true，那么 `y.equals(x)` 也应返回 true。
* 自反性：`x.equals(x)` 必须返回 true。
* 传递性：如果 `x.equals(y)` 返回是 true，且 `y.equals(z)` 返回是 true，那么 `z.equals(x)` 也应该返回是 true。
* 一致性：如果 `x.equals(y)` 返回是 true，只要 x 和 y 内容一直不变，不管重复 `x.equals(y)` 多少次，返回都是 true。
* 任何情况下，`x.equals(null)`，永远返回是 false；`x.equals(和x不同类型的对象)` 永远返回是 false。

```java
    int it = 65;
    float fl= 65.0f;
    System.out.println("65和65.0f是否相等？" + (it == fl)); //true
    char ch1 = 'A'; 
    char ch2 = 12;
    System.out.println("65和'A'是否相等？" + (it == ch1)); // true
    System.out.println("12和ch2是否相等？" + (12 == ch2)); // true 
    String str1 = new String("hello");
    String str2 = new String("hello");
    System.out.println("str1和str2是否相等？"+ (str1 == str2)); // false
    System.out.println("str1是否equals str2？"+(str1.equals(str2))); // true
    System.out.println("hello" == new java.util.Date()); // 编译不通过
```

```java
public class OrderTest {
    public static void main(String[] args) {
        Order order1 = new Order(1001,"AA");
        Order order2 = new Order(1001,"BB");    
        System.out.println(order1.equals(order2));    // false
        Order order3 = new Order(1001,"BB");
        System.out.println(order2.equals(order3)); // true
    }
}

class Order{
    private int orderId;
    private String orderName;
    public int getOrderId() {
        return orderId;
    }
    public void setOrderId(int orderId) {
        this.orderId = orderId;
    }
    public String getOrderName() {
        return orderName;
    }
    public void setOrderName(String orderName) {
        this.orderName = orderName;
    }
    public Order(int orderId, String orderName) {
        super();
        this.orderId = orderId;
        this.orderName = orderName;
    }
    public boolean equals(Object obj){
        if(this == obj){            
            return true;
        }
        if(obj instanceof Order){
            Order order = (Order)obj;
//            正确的
//            最后判断类的所有属性是否相等,其中String类型和Object类型可以用相应的equals()来判断
            return this.orderId == order.orderId && this.orderName.equals(order.orderName);
//            错误的
//            return this.orderId == order.orderId && this.orderName == order.orderName;
        }
        return false;
    }
}
```

#### toString

当输出一个引用对象时，实际上就是调用当前对象的 `toString()`。像 String、Date、File、包装类等都重写了 Object 类中的 `toString()` 方法。使得在调用 `toString()` 时，返回"实体内容"信息。自定义类如果重写 `toString()` 方法，当调用此方法时，返回对象的"实体内容"。

```java
public String toString() {
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
}
```

```java
import java.util.Date;
public class ToStringTest {
    public static void main(String[] args) {
        Person cust1 = new Person("zs" ,21);
        System.out.println(cust1.toString()); // com.polymorphism.basis.Person@2a139a55
        System.out.println(cust1); // com.polymorphism.basis.Person@2a139a55 ---> Person[name = Tom,age = 21]
        String str = new String("MM");
        System.out.println(str); // MM
        Date date = new Date(45362348664963L);
        System.out.println(date.toString()); // Wed Jun 24 12:24:24 CST 3407

    }
}
```

#### 包装类 Wrapper

Java 提供了8种基本数据类型对应的包装类，使得基本数据类型的变量具有类的特征。对应 byte、short、int、long、float、double、boolean、char 分别为 Byte、Short、Integer、Long、Float、Double、Boolean、Character。其中 Byte、Short、Integer、Double 的父类是 Number。

##### 包装类与基本数据类型相互转换

![](./assets/包装类与基本数据类型相互转换.png)

```java
import org.junit.Test;

public class WrapperTest {

//    String类型 -> 基本数据类型,调用包装类的parseXxx()
    @Test
    public void test5(){
        String str1 = "123";
        int num2 = Integer.parseInt(str1); 
        System.out.println(num2 + 1); // 124
        String str2 = "true";
        Boolean b1 = Boolean.parseBoolean(str2);
        System.out.println(b1);    // true
    }
//    基本数据类型、包装类 -> String类型,调用String重载的valueOf(Xxx xxx)
    @Test
    public void test4(){
        int num1 = 10;
        //方式1:连接运算
        String str1 = num1 + "";
        System.out.println(str1);
        //方式2:调用 String 的 valueOf(Xxx xxx)
        float f1 = 12.3f;
        String str2 = String.valueOf(f1); // "12.3"
        Double d1 = new Double(12.4);
        String str3 = String.valueOf(d1);
        System.out.println(str2); // "12.3"
        System.out.println(str3); // "12.4"
    }

    /*
     * JDK 5.0 新特性:自动装箱与自动拆箱
     */
    @Test
    public void test3(){
//        int num1 = 10;
//        //基本数据类型 --》 包装类的对象
//        method(num1); // Object obj = num1
//        自动装箱:基本数据类型 -> 包装类
        int num2 = 10;
        Integer in1 = num2; // 自动装箱

        boolean b1 = true;
        @SuppressWarnings("unused")
        Boolean b2 = b1; // 自动装箱
        //自动拆箱：包装类 --》 基本数据类型
        System.out.println(in1.toString()); // 10
        @SuppressWarnings("unused")
        int num3 = in1;
    }

    public void method(Object obj){
        System.out.println(obj);
    }

//    包装类 -> 基本数据类型:调用包装类的 xxxValue()
    @Test
    public void test2() {
        Integer in1 = new Integer(12);
        int i1 = in1.intValue();
        System.out.println(i1 + 1); // 13
        Float f1 = new Float(12.3f);
        float f2 = f1.floatValue();
        System.out.println(f2 + 1); // 13.3
    }

//    基本数据类型 -> 包装类,调用包装类的构造器
    @SuppressWarnings("unused")
    @Test
    public void test1() {
        int num1 = 10;
//        System.out.println(num1.toString());
        Integer in1 = new Integer(num1);
        System.out.println(in1.toString()); // 10
        Integer in2 = new Integer("123");
        System.out.println(in2.toString()); // 123
//        报异常
//        Integer in3 = new Integer("123abc");
//        System.out.println(in3.toString());
        Float f1 = new Float(12.3f);
        Float f2 = new Float("12.3");
        System.out.println(f1); // 12.3
        System.out.println(f2); // 12.3
        Boolean b1 = new Boolean(true);
        Boolean b2 = new Boolean("true");
        Boolean b3 = new Boolean("true123");
        System.out.println(b3); // false
        Oorder oorder = new Oorder();
        System.out.println(oorder.isMale); // false
        System.out.println(oorder.isFemale); // null
    }
}

class Oorder{
    boolean isMale;
    Boolean isFemale;
}
```

```java
import org.junit.Test;
/*
 * 如下两个题目输出结果相同吗？各是什么：
 *         Object o1= true? new Integer(1) : new Double(2.0);
 *         System.out.println(o1);//
 * 
 *         Object o2;
 *         if(true)
 *             o2 = new Integer(1);
 *        else 
 *            o2 = new Double(2.0);
 *        System.out.println(o2);//
 *
 */
public class InterViewTest {

    @Test
    public void test(){
        Object o1= true? new Integer(1) : new Double(2.0);
        System.out.println(o1);// 1.0
    }

    @Test
    public void test2(){
        Object o2;
        if(true)
            o2 = new Integer(1);
        else 
            o2 = new Double(2.0);
        System.out.println(o2);// 1
    }

    @Test
    public void method1() {
        Integer i = new Integer(1);
        Integer j = new Integer(1);
        System.out.println(i == j); //false

        //Integer内部定义了一个IntegerCache结构，IntegerCache中定义Integer[]
        //保存了从-128-127范围的整数。如果我们使用自动装箱的方式，给Integer赋值的范围在其中时，
        //可以直接使用数组中的元素，不用再去new了。目的，提高效率。

        Integer m = 1;
        Integer n = 1;
        System.out.println(m == n);//true

        Integer x = 128;//相当于new了一个Integer对象
        Integer y = 128;//相当于new了一个Integer对象
        System.out.println(x == y);//false

    }
}
```

#### 单元测试方法

选中当前项目工程，右键依次选择 build path、configure build path ...、Libraries、add Library、JUnit4，并创建一个 Java 类进行单元测试。此时的 Java 类要求该类是公共的，且此类提供一个公共的无参构造器。在此类中声明单元测试方法权限是 public，没有返回值与形参。需注意，此单元测试方法上需要声明注解 @Test，且需调用 import org.junit.Test;。在声明好单元测试方法以后，就可以在方法体内测试代码。左键双击单元测试方法名，再右键选择 run as 指向 JUnit Test。如果执行结果无错误，则显示是一个绿色进度条，反之错误即为红色进度条。

```java
import org.junit.Test;

public class JUnit {
    int num = 10;

    //第一个单元测试方法
    @Test
    public void testEquals(){
        String s1 = "MM";
        String s2 = "MM";
        System.out.println(s1.equals(s2));

        //ClassCastException的异常
//        Object obj = new String("GG");
//        Date date = (Date)obj;

        System.out.println(num);
        show();
    }

    public void show(){
        num = 20;
        System.out.println("show()...");
    }

    //第二个单元测试方法
    @Test
    public void testToString(){
        String s2 = "MM";
        System.out.println(s2.toString());
    }
}
```

### 关键字 static

当编写一个类时，其实就是在描述其对象的属性和行为，而并没有产生实质上的对象，只有通过 new 关键字才会产生出对象，这时系统才会分配内存空间给对象，其方法才可以供外部调用。<u>有时候希望无论是否产生了对象或无论产生了多少对象的情况下，某些特定的数据在内存空间里只有一份。</u>

例如所有的人都有个国家名称，部分人类都共享这个国别，不必在每一个人的实例对象中都单独分配一个用于代表国家名称的变量。

static 可以用来修饰属性、方法、代码块、内部类，不能修饰构造器。在使用 static 修饰属性后，类中的属性就有了静态属性和非静态属性（实例变量）的区别。实例变量从字面理解就是说每个实例对象都独立的拥有一套非静态属性。静态变量（**类变量**）会使所有的实例对象都公用一个静态属性，当修改一个静态变量时，所有的实例对象的此静态变量都会被改变。

静态变量随着类的加载而加载。可以通过"类.静态变量"的方式进行调用，所以静态变量的加载要早于对象的创建。由于类只会加载一次，则静态变量在内存中也只会存在一次。存在方法区的静态域中。

类中有类变量没有实例变量，对象中既有类变量，也有实例变量。类可以调用类变量；对象既可以调用类变量，也可以调用实例变量。

```java
System.out.Math.PI;
```

使用 static 修饰的方法也称静态方法，随着类的加载而加载。可通过 "类.静态方法" 而调用。在静态方法中，只能调用静态方法或属性，因为生命周期一致。非静态方法中既可以调用非静态的方法和属性，又可以调用静态的方法和属性。值得注意的是静态方法内不能使用 this 关键字和 super 关键字。

开发中操作静态属性的方法，通常设置为 static 的；工具类中的方法，习惯上声明为 static 的。比如：Math、Arrays、Collections ...

### 单例 Singleton 设计模式

设计模式是在大量的实践中总结和理论化之后优选的代码结构、编程风格、以及解决问题的思考方式。设计模免去我们自己再思考和摸索。就像是经典的棋谱，不同的棋局，我们用不同的棋谱。”套路”

所谓类的单例设计模式，就是采取一定的方法保证在整个的软件系统中，对**某个类只能存在一个对象实例**。并且该类只提供一个取得其对象实例的方法。如果我们要让类在一个虚拟机中只能产生一个对象，我们首先必须将类的**构造器的访问权限设置为 private**，这样，就不能用 new 操作符在类的外部产生类的对象了，但在类内部仍可以产生该类的对象。因为在类的外部开始还无法得到类的对象，**只能调用该类的某个静态方法以返回类内部创建的对象，静态方法只能访问类中的静态成员变量，所以，指向类内部产生的该类对象的变量也必须定义成静态的。**

由于单例模式只生成一个实例，**减少了系统性能开销**，当一个对象的产生需要比较多的资源时，如读取配置、产生其他依赖对象时，则可以通过在应用启动时直接产生一个单例对象，然后永久驻留内存的方式来解决。

```java
// 饿汉式
// 坏处是对象加载时间过长,好处是线程安全.
package com.designMode.basis;

public class SingletonTest {
    public static void main(String[] args) {    
        Hungry Hungry1 = Hungry.getInstance();
        Hungry Hungry2 = Hungry.getInstance();
        System.out.println(Hungry1 == Hungry2);
    }
}

// 单例的饿汉式
class Hungry{
    // 1.私有化类的构造器
    private Hungry(){
    }
    // 2.内部创见类的对象,且必须声明为静态
    private static Hungry instance = new Hungry();
    // 3.提供公共的静态的方法,返回类的对象
    public static Hungry getInstance(){
        return instance;
    }
}
```

```java
// 懒汉式
// 好处在于延迟对象的创建;目前为不安全写法,后续修改
package com.designMode.basis;

public class SingletonTest2 {
    public static void main(String[] args) {
        Sluggard Sluggard1 = Sluggard.getInstance();
        Sluggard Sluggard2 = Sluggard.getInstance();
        System.out.println(Sluggard1 == Sluggard2);
    }
}

class Sluggard{
    // 1.私有化类的构造器
    private Sluggard(){
    }
    // 2.声明当前类的未初始化静态对象
    private static Sluggard instance = null;
    // 3.声明 public、static 的返回当前类对象的方法
    public static Sluggard getInstance(){
        if(instance == null){
            instance = new Sluggard();            
        }
        return instance;
    }
}
```

单例模式的应用场景常见于便于同步的网站计数器、应用程序日志应用、数据库连接池、项目中读取配置文件的类、windows Task Manager & Mac Activity Monitor。

### main 方法的语法

由于 Java 虚拟机需要调用类的 `main()` 方法，所以该方法的访问权限必须是 public，又因为 Java 虚拟机在执行 `main()` 方法时不必创建对象，所以该方法必须是 static 的。又因静态的 `main()` 方法不能直接访问该类中的非静态成员，故必须创建该类的一个实例对象后才能通过这个对象去访问类中的非静态成员。接收一个 String 类型的数组参数，能使该数组保存执行 Java 命令时传递给所运行类的参数。

### 代码块

代码块是用来初始化类、对象的，若需要修饰代码块，只能使用 static，故又分为静态、非静态代码块。

静态代码块内部可以有输出语句，<u>随着类的加载而执行一次</u>，常用来初始化类的信息。若一个类中存在多个静态代码块，则按照声明顺序先后执行。在静态代码块中只能调用静态的属性、方法，不能调用非静态的结构。

非静态代码块内部也可以有输出语句，<u>随着对象的创建而执行</u>，但每创建一个对象就会执行一次非静态代码块，故更多用来对对象属性进行初始化。若一个类中存在多个非静态代码块，依旧按声明先后顺序执行。非静态代码块可调用静态、非静态的属性和方法。

```java
package com.designMode.basis;

// 总结:由父类到子类，静态先行
class Root{
    static{
        System.out.println("Root 的静态初始化块");
    }
    {
        System.out.println("Root 的普通初始化块");
    }
    public Root(){
        System.out.println("Root 的无参数的构造器");
    }
}
class Mid extends Root{
    static{
        System.out.println("Mid 的静态初始化块");
    }
    {
        System.out.println("Mid 的普通初始化块");
    }
    public Mid(){
        System.out.println("Mid 的无参数的构造器");
    }
    public Mid(String msg){
        //通过 this 调用同一类中重载的构造器
        this();
        System.out.println("Mid 的带参数构造器，其参数值："
            + msg);
    }
}
class Leaf extends Mid{
    static{
        System.out.println("Leaf 的静态初始化块");
    }
    {
        System.out.println("Leaf 的普通初始化块");
    }    
    public Leaf(){
        //通过 super 调用父类中有一个字符串参数的构造器
        super("anonymous flag");
        System.out.println("Leaf 的构造器");
    }
}
public class LeafTest{
    public static void main(String[] args){
        new Leaf();
        new Leaf();
    }
}
```

```java
package com.designMode.basis;

class Father {
    static {
        System.out.println("1");
    }
    {
        System.out.println("2");
    }
    public Father() {
        System.out.println("3");
    }
}

public class Son extends Father {
    static {
        System.out.println("4");
    }
    {
        System.out.println("5");
    }
    public Son() {
        System.out.println("6");
    }
    public static void main(String[] args) { // 由父及子 静态先行
        System.out.println("7");
        System.out.println("***");
        new Son();
        System.out.println("***");
        new Son();
        System.out.println("***");
        new Father();
//      147*2356*2356*23
    }
}
```

### 成员变量赋值的执行顺序

默认初始化 => 显式初始化 / 代码块中赋值 => 构造器中初始化 => "对象.属性|方法"的方式。

### 关键字 final

final 可以用来修饰类、方法、变量这三种结构。若 final 用来修饰一个类，则此类不能被其他类所继承，常见有 String 类、System 类、StringBuffer 类；若修饰一个方法，则被标记的方法不能被子类重写，见 Object 类的 `getClass()`；若修饰一个变量，则此时变量就是一个常量。

在 final 修饰属性时，常考虑的位置有显式初始化、代码快初始化、构造器初始化。在 final 修饰局部变量，尤其是形参的时候，表明此形参是一个常量，当调用此方法给形参赋一个实参后，就只能在方法体使用此形参，不能重新赋值。

```java
// final 修饰类,其中属性可以改变
public class Something {
    public static void main(String[] args) {
        Other o = new Other();
        new Something().addOne(o);
    }
    public void addOne(final Other o) {
        // o = new Other();
        o.i++;
    }
}
class Other {
    public int i;
}
// --- 分割线 ---
public class Something {
    public int addOne(final int x) {
//      错误
        return ++x; // return x + 1;
    }
}
```

### 抽象类与抽象方法

随着继承层次中一个个新子类的定义，类变得越来越具体，而父类则更一般，更通用。类的设计应该保证父类和子类能够共享特征。有时将一个父类设计得非常抽象，<u>以至于没有具体的实例</u>，这样的类叫做**抽象类**。在航运公司 Vehicle 类定义两个方法分别计算运输工具的燃料效率和行驶距离，那么应在 Truck 和 RiverBarge 中对应不用的计算方法。

抽象 abstract 关键字可以用来修饰类、方法。被 abstract 修饰的类称为抽象类，此类不能实例化；抽象类中一定有构造器，便于子类实例化时调用。被修饰的方法即为抽象方法，此方法只有方法声明，没有方法体。包含抽象方法的类一定是抽象类，但抽象类中可以没有抽象方法。若子类继承抽象类，并重写了所有的抽象方法，则此类是一个"实体类"。

abstract 不能用来修饰变量、代码块、构造器，也不能用来修饰私有方法、静态方法、final 的方法、final 的类。

```java
package com.designMode.basis;

public class PersonTest {
    public static void main(String[] args) {

        method(new Student()); // 匿名对象

        Worker worker = new Worker(); 
        method1(worker); // 非匿名的类非匿名的对象

        method1(new Worker()); // 非匿名的类匿名的对象

//        创建了一个匿名子类的对象:p
        Person p = new Person(){
            @Override
            public void eat() {
                System.out.println("吃东西");
            }
            @Override
            public void breath() {
                System.out.println("呼吸空气");
            }
        };
        method1(p);

//        创建匿名子类的匿名对象
        method1(new Person(){

            @Override
            public void eat() {
                System.out.println("吃零食");
            }

            @Override
            public void breath() {
                System.out.println("云南的空气");
            }
        });
    }

    public static void method1(Person p){
        p.eat();
        p.walk();
    }

    public static void method(Student s){

    }
}

abstract class Creature{
    public abstract void breath();
}

abstract class Person extends Creature{
    String name;
    int age;

    public Person(){

    }

    public Person(String name,int age){
        this.name = name;
        this.age = age;
    }

    //不是抽象方法
//    public void eat(){
//        System.out.println("人吃饭");
//    }

    //抽象方法
    public abstract void eat();

    public void walk(){
        System.out.println("人走路");
    }
}

class Student extends Person{
    public Student(String name,int age){
        super(name,age);
    }
    public Student(){

    }
    public void eat(){
        System.out.println("学生应该多吃有营养的。");
    }
    @Override
    public void breath() {
        System.out.println("学生应该呼吸新鲜的无雾霾空气");
    }
}

class Worker extends Person{
    @Override
    public void eat() {
    }
    @Override
    public void breath() {
    }
}
```

### 多态应用 TemplateMethod 模板方法设计模式

抽象类体现的就是一种模板模式的设计，抽象类作为多个子类的通用模板，子类在抽象类的基础上进行扩展、改造，但子类总体上会保留抽象类的行为方式。

当功能内部一部分实现是确定的，一部分实现是不确定的。这时可以把不确定的部分暴露出去，让子类去实现。换句话说，**在软件开发中实现一个算法时，整体步骤很固定、通用，这些步骤已经在父类中写好了。但是某些部分易变，易变部分可以抽象出来，供不同子类实现。这就是一种模板模式。**

```java
package com.designMode.basis;

/*
 * 抽象类的应用:模板方法的设计模式
 */
public class TemplateTest {
    public static void main(String[] args) {
        SubTemlate t = new SubTemlate();
        t.sendTime();
    }
}
abstract class Template{
    //计算某段代码执行所需花费的时间
    public void sendTime(){
        long start = System.currentTimeMillis();
        code();    // 不确定易变的部分
        long end = System.currentTimeMillis();
        System.out.println("花费的时间为:" + (end - start));
    }
    public abstract void code();
}

class SubTemlate extends Template{
    @Override
    public void code() {
        for(int i = 2;i <= 1000;i++){
            boolean isFlag = true;
            for(int j = 2;j <= Math.sqrt(i);j++){
                if(i % j == 0){
                    isFlag = false;
                    break;
                }
            }
            if(isFlag){
                System.out.println(i);
            }
        }
    }
}
```

### 接口 interface

有时必须从几个类中派生出一个子类，继承它们所有的属性和方法。但是 Java 不支持多重继承。需使用接口得到多重继承的效果。另一方面，有时必须从几个类中抽取出一些共同的行为特征，而其之间又没有 is-a 的关系，仅仅是具有相同行为特征而已。就像鼠标、键盘、打印机、扫描仪、充电器、MP3 机、手机、数码相机、移动硬盘等都支持 USB 连接。

**继承是一个"是不是"的关系；而接口实现则是"能不能"的关系，本质是契约、标准、规范。**

接口 interface 是抽象方法和常量值定义的集合。其特点有用 interface 来定义；接口中的所有成员变量都默认是由 public static final 修饰的；其中的所有抽象方法都默认是由 public abstract 修饰的；接口中没有构造器；采用多继承机制。

接口和类是并列的两个结构。定义接口主要是定义接口中的成员，在 JDK7 及以前只能定义全局常量 public static final 和抽象方法 public abstract；JDK8 后，除了全局常量和抽象方法之外，还可以定义静态方法、默认方法。主要注意的是，接口中不能定义构造器，这也意味着接口不能实例化。

接口通过让类去实现 implements 的方式来使用。如果实现类覆盖了接口中的所有方法，则此实现类就可以实例化。如果实现类没有覆盖接口中所有的抽象方法，则此实现类仍为一个抽象类。

Java 类可以实现多个接口，这意味着弥补了 Java 单继承性的局限性。接口与接口之间是继承，而且可以多继承。接口的具体使用，体现多态性。主要用途就是被实现类实现。

```java
class AA extends BB implementd CC,DD,EE...
```

```java
package com.designMode.basis;

/*
 * 接口的使用
 */
public class USBTest {
    public static void main(String[] args) {

        Computer com = new Computer();
        //1. 创建了接口的非匿名实现类的非匿名对象
        Flash flash = new Flash();
        com.transferData(flash); 
        //2. 创建了接口的非匿名实现类的匿名对象
        com.transferData(new Printer());
        //3. 创建了接口的匿名实现类的非匿名对象
        USB phone = new USB(){
            @Override
            public void start() {
                System.out.println("手机开始工作");
            }
            @Override
            public void stop() {
                System.out.println("手机结束工作");
            }            
        };
        com.transferData(phone);
        //4. 创建了接口的匿名实现类的匿名对象
        com.transferData(new USB(){
            @Override
            public void start() {
                System.out.println("mp3 开始工作");
            }
            @Override
            public void stop() {
                System.out.println("mp3 结束工作");
            }
        });
    }
}

class Computer{
    public void transferData(USB usb){//USB usb = new Flash();
        usb.start();
        System.out.println("具体传输数据的细节");
        usb.stop();
    }

}

interface USB{
    //常量:定义了长、宽
    void start();
    void stop();
}

class Flash implements USB{
    @Override
    public void start() {
        System.out.println("U 盘开始工作");
    }
    @Override
    public void stop() {
        System.out.println("U 盘结束工作");
    }
}
class Printer implements USB{
    @Override
    public void start() {
        System.out.println("打印机开启工作");
    }
    @Override
    public void stop() {
        System.out.println("打印机结束工作");
    }
}
```

### 常用类

#### 字符串相关类

Java 中的所有字符串字面值都作为此类的实例实现，String 是一个不可继承的 final 类，代表不不可变的字符序列。字符串为常量，用双引号包裹，创建后不能被更改。String 对象的字符内容是存储在一个字符数组 value[] 中的。

```java
public final class String
  implements java.io.Serializable, Comparable<String>, CharSequence {}...
  private final char value[];
  private int hash; // Default to 0
```

通过字面量的方式（区别于 new）给一个字符串复制，此时的字符串值声明在方法区里字符串常量池中。常量池不会存储两个相同内容的字符串，即有则复用。

关于 String 的两种实例化方式字面量和 new 与构造器，字符串常量存储在字符串常量池目的在于共享，而字符串非常量对象存储在堆中。

```java
public class StringTest {
    public static void test1(){
//    方法区里 0x1212 是 abc
        String s1 = "abc"; // 栈中s1:0x1212
        String s2 = "abc"; // 栈中s2:0x1212
        System.out.println(s1 == s2); // 比较两者地址值 => true
//    String:代表不可变的字符序列,简称:不可变性
//        1.当对字符串重新赋值时,需要重写指定内存区域赋值,不能使用原有的value进行赋值
//        2.当对现有的字符串进行连接操作时,也需要重新指定内存区域赋值,不能使用原有的value进行赋值
//        3.当调用String的replace()方法修改指定字符或字符串时,也需要重新指定内存区域赋值,不能使用原有的value进行赋值
        s1 = "aaa";
        System.out.println("s1: " + s1 + ";" + " s2: " + s2); // s1: aaa; s2: abc
        String s3 = "abc";
        s3 += "def";
        System.out.println("s2: " + s2 + ";" + " s3: " + s3); // s2: abc; s3: abcdef
        String s4 = "abc";
        String s5 = s4.replace('a','A');
        System.out.println("s4: " + s4 + ";" + " s5: " + s5); // s4: abc; s5: Abc

        String s6 = new String("javaEE");
        String s7 = new String("javaEE");
        System.out.println(s6 == s7); // false

//      p1和p2在堆中地址不同,但其中的name属性值指向的是方法区同一位置,只因都是实例化赋值
        Person p1 = new Person("Tom",12);
        Person p2 = new Person("Tom",12);

        System.out.println(p1.name.equals(p2.name)); // true
        System.out.println(p1.name == p2.name); // true
        System.out.println(p1 == p2); // false
    }

    public static void test2(){
        String s1 = "javaEE";
        String s2 = "hadoop";
        String s3 = "javaEEhadoop";
        String s4 = "javaEE" + "hadoop";
        String s5 = s1 + "hadoop";
        String s6 = "javaEE" + s2;
        String s7 = s1 + s2;

        System.out.println(s3 == s4); // true
        System.out.println(s3 == s5); // false
        System.out.println(s3 == s6); // false
        System.out.println(s5 == s6); // false
        System.out.println(s3 == s7); // false
        System.out.println(s5 == s6); // false
        System.out.println(s5 == s7); // false
        System.out.println(s6 == s7); // false

        String s8 = s5.intern(); // 返回值得到的s8使用的常量值中已经存在的“javaEEhadoop”
        System.out.println(s3 == s8); // true
    }
    public static void main(String[] args) {
        test1();
        test2();
    }
}
class Person {
    String name;
    int age;
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    public Person() {
    }
}
```

`String s = new String("xxx")` 在内存中创建了两个对象，一个是对空间中 new 结构，另一个是 char[] 对应的常量池的数据。

若右边赋值有其他变量名参与，不是直接字面量参与，此时都不是在常量池，需要在堆空间开辟，可以看作都是 new 出新的空间。

```java
public class StringTest1 {
    String str = new String("good");
    char[] ch = { 't', 'e', 's', 't' };

    public void change(String str, char ch[]) {
        str = "test ok";
        ch[0] = 'b';
    }
    public static void main(String[] args) {
        StringTest1 ex = new StringTest1();
        ex.change(ex.str, ex.ch);
        System.out.println(ex.str); // good
        System.out.println(ex.ch); // best
    }
}
```

### 集合

集合、数组都是对多个数据进行存储操作的结构，简称 Java 容器。此时的存储主要是指内存层面的存储，不涉及到持久化的存储。

数组在存储数据时，<u>长度</u>以及<u>数据类型</u>在初始化完成就会确定，对提供的增删改插操作方法非常有限，同时由于底层存在操作后移动元素的问题，效率不高。

Java 集合可分为 `Collection` 和 `Map` 两种体系。

* `Collection` 接口：单列数据，定义了存取一组对象的方法的集合。
  * `List`：元素有序、可重复的集合。ArrayList、LinkedList、Vector。
  * `Set`：元素无序、不可重复的集合。HashSet、LinkedHashSet、TreeSet。
* `Map` 接口：双列数据，保存具有映射关系 "key-value 对" 的集合。HashMap、LinkedHashMap、TreeMap、Hashtable、Properties。

#### Collection 接口

Collection 接口是 List、Set 和 Queue 接口的父接口，其内定义的方法既可用于操作 Set 集合，也可用于操作 List、Queue 集合。JDK 不提供此接口的任何直接实现，而是提供更具体的子接口 Set 和 List 实现。在 Java5 之前，Java 集合会丢失容器中所有对象的数据类型，把所有对象都当成 Object 类型处理；从 JDK 5.0 增加**泛型**以后，集合才可以记住容器中对象的数据类型。

```java
import org.junit.Test;

import java.util.*;

/**
 * Collection接口中的方法的使用
 */
public class CollectionTest {

    @Test
    public void test1(){
        Collection coll = new ArrayList();

        // add(Object e) => 将元素e添加到集合coll中
        coll.add("AA");
        coll.add("BB");
        coll.add(123);  // 自动装箱
        coll.add(new Date());

        // size() => 获取添加的元素的个数
        System.out.println(coll.size()); // 4

        // addAll(Collection coll1) => 将coll1集合中的元素添加到当前的集合中
        Collection coll1 = new ArrayList();
        coll1.add(456);
        coll1.add("CC");
        coll.addAll(coll1);

        System.out.println(coll.size()); // 6
        System.out.println(coll); // [AA, BB, 123, Sat Apr 02 18:45:59 CST 2019, 456, CC]

        //clear() => 清空集合元素
        coll.clear();

        //isEmpty() => 判断当前集合是否为空
        System.out.println(coll.isEmpty()); // true

        coll.add(new Person("zs",21));
        coll.add(new String("Tom"));
        coll.add(false);

        // contains(Object obj) => 判断当前集合中是否包含obj
        boolean contains = coll.contains(123);
        System.out.println(contains); // false
        System.out.println(coll.contains(new String("Tam"))); // false
        // 在判断时会调用obj对象所在类的equals()
        System.out.println(coll.contains(new Person("zs",21))); // 未重写false => 重写后true

        // containsAll(Collection coll1) => 判断形参coll2中的所有元素是否都存在于当前集合中
        Collection coll2 = Arrays.asList("CC",456);
        System.out.println(coll1.containsAll(coll2)); // true

        // hashCode() => 返回当前对象的哈希值
        System.out.println(coll.hashCode()); // 119682751

        // 集合 --->数组 => toArray()
        Object[] arr = coll.toArray();
        for(int i = 0;i < arr.length;i++){
            System.out.println(arr[i]);
        }

        // 数组 --->集合 => 调用Arrays类的静态方法asList()
        List<String> list = Arrays.asList(new String[]{"AA", "BB", "CC"});
        System.out.println(list); // [AA, BB, CC]

        List arr1 = Arrays.asList(123, 456);
        System.out.println(arr1); // [123, 456]

        // 这里视作放入一个数组
        List arr2 = Arrays.asList(new int[]{123, 456});
        System.out.println(arr2.size()); // 1

        List arr3 = Arrays.asList(new Integer[]{123, 456});
        System.out.println(arr3.size()); // 2
    }
}
```

```java
import java.util.Objects;

public class Person {
    private String name;
    private int age;

    public Person() {
        super();
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
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

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return age == person.age && Objects.equals(name, person.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}
```

#### Iterator 迭代器接口

设计模式的一种 Iterator 对象称为迭代器，<u>主要用于遍历 Collection 集合中的元素</u>。GOF 给迭代器模式的定义为，<u>提供一种方法访问一个容器 container 对象中各个元素，而又不需暴露该对象的内部细节</u>。**迭代器模式，就是为容器而生**。

Collection 接口继承了 java.lang.Iterable 接口，该接口有一个 `iterator()` 方法，那么所有实现了 Collection 接口的集合类都有一个 `iterator()` 方法，用以返回一个实现 Iterator 接口的对象。

**Iterator 仅用于遍历集合**，其本身并不提供承装对象的能力。如果需要创建 Iterator 对象，则必须有一个被迭代的集合。**集合对象每次调用 `iterator()` 方法都得到一个全新的迭代器对象**，<u>默认游标都在集合的第一个元素之前</u>。

![](./assets/Iterator.png)

```java
import org.junit.Test;

import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;

/**
 * 集合元素的遍历操作 => 使用迭代器Iterator接口
 */
public class IteratorTest {

    @Test
    public void test(){
        Collection coll = new ArrayList();
        coll.add(123);
        coll.add(456);
        coll.add(new Person("zs",21));
        coll.add(new String("Tom"));
        coll.add(false);

        Iterator iterator = coll.iterator();

        while(iterator.hasNext()){
            Object obj = iterator.next();
            if("Tom".equals(obj)){
                // 内部定义了remove(),可以在遍历的时候,删除集合中的元素.此方法不同于集合直接调用remove()
                iterator.remove();
            }
        }

        //遍历集合
        iterator = coll.iterator();
        // hasNext() => 判断是否还有下一个元素
        while(iterator.hasNext()){
            // next() => 指针下移;将下移后集合位置上的元素返回
            System.out.println(iterator.next());
        }

//        错误方式一：会跳过迭代元素
//        while(iterator.next() != null){
//            System.out.println(iterator.next());
//        }

//        错误方式二：集合对象每次调用iterator()方法都得到一个全新的迭代器对象，默认游标都在集合的第一个元素之前。
//        while(coll.iterator().hasNext()){
//            System.out.println(coll.iterator().next());
//        }
    }
}
```

* Iterator可以删除集合的元素，但是是遍历过程中通过迭代器对象的remove方法，不是集合对象的remove方法。
* **如果还未调用next()或在上一次调用next方法之后已经调用了remove方法，再调用remove都会报IllegalStateException**。

#### foreach 循环遍历集合或数组

Java 5.0 提供 foreach 循环迭代访问 Collection 和数组。遍历操作不需获取 Collection 或数组的长度，无需使用索引访问元素。遍历集合的底层调用 Iterator 完成操作。

```java
for(Object obj: objs){ // (要遍历的元素类型 遍历后自定义元素的名称: 要便利的结构名称)
    system.out.println(obj.getName())
}
```

#### List 接口

鉴于 Java 中数组用来存储数据的局限性，通常使用 Collection 子接口 List 替代数组。<u>List 集合类中元素有序且可重复，集合中的每个元素都有其对应的顺序索引</u>。List 容器中元素都对应一个整数型的序号记载其在容器中的位置，可以根据序号存取容器中的元素。JDK API 中 List 接口的实现类常用的有 ArrayList、LinkedList 和 Vector。

ArrayList 作为 List 接口的主要实现类，虽线程不安全的，但是效率高；底层使用 `Object[] elementData` 存储。使用 LinkedList 对于频繁的插入、删除操作，效率比 ArrayList 高，其底层使用双向链表存储。Vector 作为 List 接口的古老实现类；线程安全的，效率低；底层使用 `Object[] elementData` 存储。

* ArrayList 的源码分析

ArrayList 是List 接口的典型实现类、主要实现类。本质上 ArrayList 是对象引用的一个”变长”数组。

```java
// jdk7
ArrayList list = new ArrayList(); // 底层创建了长度是10的Object[]数组elementData
list.add(123); // elementData[0] = new Integer(123);
...
list.add(11); // 如果此次的添加导致底层elementData数组容量不够则扩容。
// 默认情况下，扩容为原来的容量的1.5倍，同时需要将原有数组中的数据复制到新的数组中。
// 结论：建议开发中使用带参的构造器：ArrayList list = new ArrayList(int capacity)

// jdk8
ArrayList list = new ArrayList(); // 底层Object[] elementData初始化为{}.并没有创建长度为10的数组
list.add(123); // 第一次调用add()时，底层才创建了长度10的数组，并将数据123添加到elementData[0]
...
// 后续的添加和扩容操作与jdk 7 无异。
// jdk7中的ArrayList的对象的创建类似于单例的饿汉式，而jdk8中的ArrayList的对象的创建类似于单例的懒汉式，延迟了数组的创建，节省内存。
```

* LinkedList 的源码分析

对于频繁的插入或删除元素的操作，建议使用 LinkedList 类，效率较高。LinkedList 为双向链表，内部没有声明数组，而是定义了 Node 类型的 first 和 last，用于记录首末元素。同时，定义内部类 Node 作为 LinkedList 中保存数据的基本结构。

```java
LinkedList list = new LinkedList(); // 内部声明了Node类型的first和last属性，默认值为null
list.add(123); // 将123封装到Node中，创建了Node对象。
// 其中，Node定义为：体现了LinkedList的双向链表的说法
private static class Node<E> {
    E item;
    Node<E> next;
    Node<E> prev;
    Node(Node<E> prev, E element, Node<E> next) {
        this.item = element;
        this.next = next;     //next变量记录下一个元素的位置
        this.prev = prev;     //prev变量记录前一个元素的位置
    }
}
```

* Vector 的源码分析

Vector 是一个 JDK1.0 的古老集合。大多数操作与 ArrayList 相同，区别之处在于 Vector 是线程安全的。在各种 list 中，最好把 ArrayList 作为缺省选择。当插入、删除频繁时，使用 LinkedList；Vector 总是比 ArrayList 慢，所以尽量避免使用。

Vector 源码层面在 jdk7 和 jdk8 中通过 `Vector()` 构造器创建对象时，底层都创建了长度为10的数组。在扩容方面，默认扩容为原来的数组长度的2倍。

```java
public Vector(int initialCapacity) { this(initialCapacity, 0); }
// Constructs an empty vector so that its internal data array has size 10 and its standard capacity increment is zero
public Vector() { this(10); }
public Vector(int initialCapacity, int capacityIncrement) {
    super();
    if (initialCapacity < 0)
        throw new IllegalArgumentException("Illegal Capacity: " + initialCapacity);
        this.elementData = new Object[initialCapacity];
        this.capacityIncrement = capacityIncrement;
}
```

List 除从 Collection 集合继承的方法外，还添加了一些根据索引来操作集合元素的方法。

```java
import org.junit.Test;

import java.util.*;

/**
 * List 接口的常用方法
 */
public class ListTest {
    /**
     *
     * void add(int index, Object ele):在index位置插入ele元素
     * boolean addAll(int index, Collection eles):从index位置开始将eles中的所有元素添加进来
     * Object get(int index):获取指定index位置的元素
     * int indexOf(Object obj):返回obj在集合中首次出现的位置
     * int lastIndexOf(Object obj):返回obj在当前集合中末次出现的位置
     * Object remove(int index):移除指定index位置的元素，并返回此元素
     * Object set(int index, Object ele):设置指定index位置的元素为ele
     * List subList(int fromIndex, int toIndex):返回从fromIndex到toIndex位置的子集合
     *
     * 总结：常用方法
     * 增：add(Object obj)
     * 删：remove(int index) / remove(Object obj)
     * 改：set(int index, Object ele)
     * 查：get(int index)
     * 插：add(int index, Object ele)
     * 长度：size()
     * 遍历：① Iterator迭代器方式
     *      ② 增强for循环
     *      ③ 普通的循环
     */

    @Test
    public void test3(){
        ArrayList list = new ArrayList();
        list.add(123);
        list.add(456);
        list.add("AA");

        //方式一：Iterator迭代器方式
        Iterator iterator = list.iterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        }

        System.out.println("***************");

        //方式二：增强for循环
        for(Object obj : list){
            System.out.println(obj);
        }

        System.out.println("***************");

        //方式三：普通for循环
        for(int i = 0;i < list.size();i++){
            System.out.println(list.get(i));
        }
    }

    @Test
    public void tets2(){
        ArrayList list = new ArrayList();
        list.add(123);
        list.add(456);
        list.add("AA");
        list.add(new Person("Tom",12));
        list.add(456);
        // int indexOf(Object obj) => 返回obj在集合中首次出现的位置。如果不存在，返回-1.
        int index = list.indexOf(4567);
        System.out.println(index);

        // int lastIndexOf(Object obj) => 返回obj在当前集合中末次出现的位置。如果不存在，返回-1.
        System.out.println(list.lastIndexOf(456));

        // Object remove(int index) => 移除指定index位置的元素，并返回此元素
        Object obj = list.remove(0);
        System.out.println(obj);
        System.out.println(list);

        // Object set(int index, Object ele) => 设置指定index位置的元素为ele
        list.set(1,"CC");
        System.out.println(list);

        // List subList(int fromIndex, int toIndex) => 返回从fromIndex到toIndex位置的左闭右开区间的子集合
        List subList = list.subList(2, 4);
        System.out.println(subList);
        System.out.println(list);
    }

    @Test
    public void test(){
        ArrayList list = new ArrayList();
        list.add(123);
        list.add(456);
        list.add("AA");
        list.add(new Person("Tom",12));
        list.add(456);

        System.out.println(list);

        // void add(int index, Object ele) => 在index位置插入ele元素
        list.add(1,"BB");
        System.out.println(list); // [123, BB, 456, AA, com.xxxzs.collection.Person@27e0db, 456]

        // boolean addAll(int index, Collection eles) => 从index位置开始将eles中的所有元素添加进来
        List list1 = Arrays.asList(1, 2, 3);
        list.addAll(list1);
        System.out.println(list); // [123, BB, 456, AA, com.xxxzs.collection.Person@27e0db, 456, 1, 2, 3]
        list.add(list1);
        System.out.println(list); // [123, BB, 456, AA, com.xxxzs.collection.Person@27e0db, 456, 1, 2, 3, [1, 2, 3]]
        System.out.println(list.size()); // 10

        // Object get(int index) => 获取指定index位置的元素
        System.out.println(list.get(2)); // 456
    }
}
```

谈谈 ArrayList/LinkedList/Vector 的异同？ArrayList 底层是什么？扩容机制？Vector 和 ArrayList 的最大区别是?

相对线程安全的 Vector，ArrayList 是实现了基于动态数组的数据结构，LinkedList 基于链表的数据结构，虽线程不安全，但执行效率更高。对于随机访问 get 和 set，ArrayList 优于 LinkedList，因后者要移动指针。对于新增和删除操作 add 和 remove，LinkedList 较占优势，因 ArrayList 要移动数据。Vector 和 ArrayList 几乎是完全相同，唯一区别在于 Vector 是同步类 synchronized 属于强同步类，开销比 ArrayList 要大，故访问要慢。正常情况下多数的程序员使用 ArrayList 而不是 Vector，因为同步完全可以自行控制。Vector 每次扩容请求其大小的2倍空间，而 ArrayList 是1.5倍。Vector 还有一个子类 Stack。

```java
import org.junit.Test;

import java.util.ArrayList;
import java.util.List;

public class ListEver {
    /**
     * 区分List中remove(int index)和remove(Object obj)
     * 总结：index为集合的下标，而object表示的是集合中的数据
     */

    @Test
    public void testListRemove() {
        List list = new ArrayList();
        list.add(1);
        list.add(2);
        list.add(3);
        updateList(list);
        System.out.println(list);//
    }

    private void updateList(List list) {
//        list.remove(2);
        list.remove(new Integer(2));
    }
}
```

#### Set 接口

Set 接口是 Collection 的子接口，且没有提供额外的方法。Set 集合不允许包含相同的元素，如果试把两个相同的元素加入同一个 Set 集合中，则添加操作失败。**Set 判断两个对象是否相同不是使用 `==` 运算符，而是根据 `equals()` 方法**。

Set 接口存储无序的、不可重复的数据。HashSet 作为 Set 接口的主要实现类，线程不安全却可以存储 null 值。LinkedHashSet 作为 HashSet 的子类，在遍历其内部数据时可以按照添加的顺序进行遍历，对于频繁的遍历操作，LinkedHashSet 效率高于 HashSet。TreeSet 可以按照添加对象的指定属性进行排序。

Set 的无序性不等于随机性。<u>存储的数据在底层数组中并非按照数组索引顺序添加，而是根据数据哈希值决定。</u>不可重复性保证添加的元素按照 `equals()` 判断时不能返回 true，即相同的元素只能添加一个。

```java
import org.junit.Test;

import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

public class SetTest {
    @Test
    public void test(){
        Set set = new HashSet();
        set.add(123);
        set.add(456);
        set.add("zzz");
        set.add("full");
        set.add(new Person("zs",21));
        set.add(new Person("zs",21));
        set.add(129);

        Iterator iterator = set.iterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        }
    }
}
```

* HashSet 中元素的添加过程

HashSet 是 Set 接口的典型实现，大多数时候使用 Set 集合时都使用这个实现类。HashSet 按 Hash 算法来存储集合中的元素，因此具有很好的存取、查找、删除性能。

HashSet 不能保证元素的排列顺序且不是线程安全的，集合元素可以是 null；底层也是数组，初始容量为16，当如果使用率超过0.75，（16*0.75=12）就会扩大容量为原来的2倍。（16扩容为32，依次为64,128…等）。HashSet 集合判断两个元素相等的标准是两个对象通过 `hashCode()` 方法比较相等，并且两个对象的 `equals()` 方法返回值也相等。

对存放在 Set 容器中的对象，对应类一定要重写 `equals()` 和 `hashCode(Object obj)` 方法以实现对象相等规则。即“**相等的对象必须具有相等的散列码**”。

HashSet 中添加元素 a，首先调用其所在类的 `hashCode()` 方法计算元素 a 的哈希值，此哈希值接着通过算法计算出在 HashSet 底层数组中的存放位置即索引位置，并判断数组此位置上是否已有元素。若此位置没有其他元素，则元素 a 添加成功；如果此位置上有其他元素 b 或以链表形式存在的多个元素，则比较元素 a 与元素 b 的 hash 值，若 hash 值不相同，则元素 a 添加成功；如果 hash 值相同，则需要调用元素 a 所在类的 `equals()` 方法，返回 true 则元素 a 添加失败，返回 false 则元素 a 添加成功。对于添加成功的情况，元素 a 与已经存在指定索引位置上数据以链表的方式存储。在 JDK7 中元素 a 放到数组中，指向原来的元素；而在 JDK8 中原来的元素在数组中指向元素 a。总结为<u>七上八下，底层为数组与链表</u>。

* hashCode 和 equals 的重写

在程序运行时，同一个对象多次调用 `hashCode()` 方法应该返回相同的值。当两个对象的 `equals()` 方法比较返回 true 时，这两个对象的 `hashCode()` 方法的返回值也应相等。对象中用作 `equals()`  方法比较的 Field，都应该用来计算 `hashCode` 值。

往 Set、Map 接口的实现类对象中存放 Java 中的包装类对象 String、Integer... 可以直接存取，而若存放自定义类对象，需要重写 `hashCode()` 和 `equals()` 方法，才能正确的存放和获取对象。在不重写上述两种方法时，在自定义对象传参一致的情况下，在两次 new 出的实例会因为 hashCode 不同而得到堆内存中两个不同的对象。两个截然不同的实例可能在逻辑上是相等，但根据 `Object.hashCode()` 方法，它们仅仅是两个对象。因此违反了“**相等的对象必须具有相等的散列码**”的原则。

若只重写`hashCode()`，而不重写`equals()`，执行过程虽然可以得到相等的 hashCode，但是底层存储的结构可能是数组与链表的组合，那么相同的索引位置可能会在一条链表上，即使待比较两者的索引相同，也只能说明处于同一条链上，但并不能证明两者就是相同的。同理只重写 `equals()` 方法没有重写 `hashCode()`，待比较两者的 hashCode 不同直接导致在底层结构中的索引位置不同，自然就不能正确的存放与获取。

重写的 `hashCode()` 只关心指定属性是否相同，只要值相同则调用 `Objects.hashCode()`得到的哈希值就是相同的，其在底层存储结构中的索引就相同。`equals()` 首先判断两个对象的引用是否相同，相同则表示两者就是同样的对象，直接返回 true。如果两者的引用不同则继续判断传入的对象是否为 null 或者两者的类型是否相同，不同返回 false ，即根本就不是同一类型对象；否则将传入的对象强转为待比较的对象类型，调用 `Obejcts.equals()` 进行比较，如果两者的类型相同，传入的对象不为 null，且比较的属性也相同，那么证明就是相同的。

<u>为什么用 Eclipse/IDEA 复写 hashCode 方法时有31这个数字？</u>这是因为选择系数的时候要选择尽量大的系数。因为如果计算出来的 hash 地址越大，所谓的“冲突”就越少，查找起来效率也会提高（减少冲突）。并且31只占用5bits，相乘造成数据溢出的概率较小。现在很多虚拟机里面都有做相关优化（提高算法效率），31可以由 `i*31== (i<<5)-1` 来表示。31是一个素数，素数作用就是若用一个数字来乘以这个素数，那么最终出来的结果只能被素数本身和被乘数还有1来整除。

* LinkedHashSet 的使用

不允许集合元素重复的 LinkedHashSet 是 HashSet 的子类，LinkedHashSet 是根据元素的 hashCode 值来决定元素的存储位置，但同时使用双向链表维护元素的次序，这使得元素看起来是以插入顺序保存。LinkedHashSet 插入性能略低于 HashSet，但在迭代访问 Set 里的全部元素时有很好的性能，这是因为 LinkedHashSet 在添加数据的同时，每个数据还维护了两个引用，记录此数据前一个数据和后一个数据。

* TreeSet 的使用

TreeSet 是 SortedSet 接口的实现类，可以确保集合元素处于排序状态，底层使用**红黑树**结构存储数据。新增方法 `Comparator comparator()`、`Object first()|last()`、`Object lower|higher(Object e)`、`SortedSet subSet(fromElement, toElement)`、`SortedSet headSet|tailSet(toElement)`。

TreeSet 具有自然排序和定制排序的两种排序方法，默认情况 TreeSet 采用自然排序。相比于 List 具有更快的查询速度。

在自然排序中，TreeSet 会调用集合元素的 compareTo 方法来比较元素大小关系并将集合元素按升序排列。若试图把一个对象添加到 TreeSet 时，则该对象的类必须实现 Comparable 接口。实现 Comparable 的类必须实现 `compareTo(Object obj)` 方法，即两个对象应通过此方法的返回值比较大小。

向 TreeSet 中添加元素时，只有第一个元素无须比较 `compareTo()` 方法，后面添加的所有元素都会调用 `compareTo()` 方法进行比较。**因为只有相同类的两个实例才会比较大小，所以向 TreeSet 中添加的应该是同一个类的对象**。对于 TreeSet 集合而言，判断两个对象是否相等的唯一标准是通过 `compareto(Object obj)` 方法比较返回值。当需要把一个对象放入 TreeSet 中重写该对象对应的 equals 方法应保证结果 true 与 compareTo 比较结果0一致。

自然排序比较两个对象是否相同的标准为 `compareTo()` 返回0，不再是 `equals()`；定制排序比较两个对象是否相同的标准为 `compare()` 返回0，不再是 `equals()`。

若元素所属的类没有实现 `Comparable` 接口，又不希望按照升序的方式排列元素或希望按照其它属性大小进行排序，则考虑使用定制排序。定制排序要通过 `Comparator` 接口来实现。需要重写 `compare(T o1,T o2)` 方法。

利用 `int compare(T o1,T o2)` 方法，比较 o1 和 o2 的大小。如果方法返回正整数，则表示 o1 大于 o2；如果返回0，表示相等；返回负整数，表示 o1 小于 o2。

要实现**定制排序**，需要将实现 `Comparator` 接口的实例作为形参传递给  TreeSet 的构造器。此时仍然只能向 TreeSet 中添加类型相同的对象。否则发生 `ClassCastException` 异常。使用定制排序判断两个元素相等的标准是通过 Comparator 比较两个元素返回了0。

```java
import org.junit.Test;

import java.util.Comparator;
import java.util.Iterator;
import java.util.TreeSet;

/**
 * 向TreeSet中添加的数据，要求是相同类的对象。
 */
public class TreeSetTest {
    @Test
    public void test() {
        // 默认排序
        TreeSet set = new TreeSet();

        //失败：不能添加不同类的对象
//        set.add(123);
//        set.add(456);
//        set.add("AA");
//        set.add(new User("Tom",12));

        //举例一：
//        set.add(34);
//        set.add(-34);
//        set.add(43);
//        set.add(11);
//        set.add(8);

        //举例二：
        set.add(new User("Tom",12));
        set.add(new User("Jerry",32));
        set.add(new User("Jim",2));
        set.add(new User("Mike",65));
        set.add(new User("Jack",33));
        set.add(new User("Jack",56));

        Iterator iterator = set.iterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        }
    }

    @Test
    public void tets2(){
        // 定制排序
        Comparator com = new Comparator() {
            //按照年龄从小到大排列
            @Override
            public int compare(Object o1, Object o2) {
                if(o1 instanceof User && o2 instanceof User){
                    User u1 = (User)o1;
                    User u2 = (User)o2;
                    return Integer.compare(u1.getAge(),u2.getAge());
                }else{
                    throw new RuntimeException("输入的数据类型不匹配");
                }
            }
        };

        TreeSet set = new TreeSet(com);
        set.add(new User("Tom",12));
        set.add(new User("Jerry",32));
        set.add(new User("Jim",2));
        set.add(new User("Mike",65));
        set.add(new User("Mary",33));
        set.add(new User("Jack",33));
        set.add(new User("Jack",56));


        Iterator iterator = set.iterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        }
    }
}
```

```java
public class User implements Comparable{
    private String name;
    private int age;

    public User() {
    }

    public User(String name, int age) {
        this.name = name;
        this.age = age;
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

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    @Override
    public boolean equals(Object o) {
        System.out.println("User equals()....");
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        User user = (User) o;

        if (age != user.age) return false;
        return name != null ? name.equals(user.name) : user.name == null;
    }

    @Override
    public int hashCode() { //return name.hashCode() + age;
        int result = name != null ? name.hashCode() : 0;
        result = 31 * result + age;
        return result;
    }

    //按照姓名从大到小排列,年龄从小到大排列
    @Override
    public int compareTo(Object o) {
        if (o instanceof User) {
            User user = (User) o;
//            return this.name.compareTo(user.name);  //按照姓名从小到大排列
//            return -this.name.compareTo(user.name);  //按照姓名从大到小排列
            int compare = -this.name.compareTo(user.name);  //按照姓名从大到小排列
            if(compare != 0){   //年龄从小到大排列
                return compare;
            }else{
                return Integer.compare(this.age,user.age);
            }
        } else {
            throw new RuntimeException("输入的类型不匹配");
        }
    }
}
```

* Set 面试题

```java
// 在List内去除重复数字值，要求尽量简单
import org.junit.Test;

import java.util.ArrayList;
import java.util.Collection;
import java.util.HashSet;
import java.util.List;

public class InterviewTest {
    public static List duplicateList(List list) {
        HashSet set = new HashSet();
        set.addAll(list);
        return new ArrayList(set);
    }
    @Test
    public void test2(){
        List list = new ArrayList();
        list.add(new Integer(1));
        list.add(new Integer(2));
        list.add(new Integer(2));
        list.add(new Integer(4));
        list.add(new Integer(4));
        List list2 = duplicateList(list);
        for (Object integer : list2) {
            System.out.println(integer);
        }
    }
}
```

#### Map 接口

Map 存储双列数据，即 key-value 对，作为 Map 的主要实现类，虽线程是不安全的，但效率高，可以存储 null 的 key 和 value。LinkedHashMap 保证在遍历 map 元素时，可以按照添加的顺序实现遍历，原因是在原有的 HashMap 底层结构基础上添加了一对指针，指向前一个和后一个元素，故在频繁的遍历操作时此类执行效率高于 HashMap。TreeMap 保证按照添加的 key-value 对进行排序，实现排序遍历。此时考虑 key 的自然排序或定制排序，其底层使用红黑树。Hashtable 作为古老的实现类，尽管线程安全，但效率低；不能存储 null 的 key 和 value。Properties 常用来处理配置文件，其存储元素 key 和 value 都是 String 类型。HashMap的底层在 JDK7 及其之前为数组与链表，JDK8后为数组、链表与红黑树。

* Map 中存储的 key-value 特点

Map 与 Collection 并列存在。用于保存具有映射关系的数据 key-value。Map 中的 key 和 value 都可以是任何引用类型的数据。Map 中的 key  用 Set 来存放，不允许重复，即同一个 Map 对象所对应的类，须重写 `hashCode()` 和 `equals()` 方法。常用 String 类作为 Map 的“键”。key 和 value 之间存在单向一对一关系，即通过指定的 key 总能找到唯一的、确定的 value。Map 中的 value 是无序的、可重复的，使用 Collection 存储所有的 value，但其所在类需重写 equals。一个键值对构成一个无序、不可重复的 Entry 对象，使用 Set 存储。

* HashMap

HashMap 是 Map 接口使用频率最高的实现类，键和值允许使用 null，与 HashSet 一样不保证映射的顺序。所有的 key 构成的集合是无序、不可重复的 Set。所以 key 所在的类要重写 `equals()` 和 `hashCode()`。所有的 value 构成的集合是无序可重复的 Collection，故 value 所在的类要重写 `equals()`。一个 key-value 构成一个 entry，所有 entry 构成的集合是无序不可重复的 Set。HashMap 判断两个 key 相等的标准是两个 key 通过 `equals()` 方法返回 true，且 hashCode 值也相等。HashMap 判断两个 value 相等的标准是两个 value 通过 `equals()` 方法返回 true。

在 JDK7 及以前版本，HashMap 底层是数组和链表结构；JDK8 后是数组、链表与红黑树。

JDK7 实例化一个 HashMap 时，系统会创建一个长度为 Capacity 的 Entry 数组，这个长度在哈希表中称为容量 Capacity，在此数组中放置元素的位置称之为"桶" bucket，每个"桶"都有各自的索引，故可根据索引快速查找 bucket 中的元素。每个 bucket 中存储一个 Entry 对象，且此元素可以带一个引用变量用于指向下一元素，由此就有概率生成一个 Entry 链，且新添加的元素作为链表的 head。

向 HashMap 里添加 `entry(key, value)` 首先应需根据 key 所在类的 hashCode 计算出 entry 中 key 的哈希值，依据哈希值得到在底层 Entry 数组里的存储位。若该位无元素，则 entry 添加成功；若该位置存在元素或链表，则需要通过循环依次比较新加入 entry 中 key 的哈希值和已存在的 entry 的 hash 值，彼此之间 hash 值不同则添加成功；如果 hash 值相同就继续比较二者是否 equals，若返回值为 true 则使用新 entry 里的 value 替换 equals 为 true 的原有 entry 的 value。若遍历一遍后所有的 equals 都返回 false，则新 entry 仍添加成功，新 entry 指向原有的 entry 元素。

当 HashMap 中的元素越来越多的时候，hash 冲突的几率也就越来越高，因数组的长度是固定的。为提高查询效率，要对 HashMap 的数组进行扩容，而在数组扩容之后原数组中的数据必须重新计算其在新数组中的位置，这就是最消耗性能的 resize。

![](./assets/HashMap.png)

当 HashMap 中的元素个数超过 `数组大小*loadFactor` 时，就会进行数组扩容，loadFactor 的默认值 DEFAULT_LOAD_FACTOR 为0.75。默认情况，数组 DEFAULT_INITIAL_CAPACITY 为16，那么当 HashMap 中元素个数超过临界值 threshold 16\*0.75=12的时候，就把数组的大小扩展一倍，为2*16=32，然后重新计算每个元素在数组中的位置，这是一个非常消耗性能的操作，所以如果已经预知 HashMap 中元素的个数，那么预设元素的个数能够有效的提高 HashMap 的性能。

JDK8 实例化 HashMap 时，会初始化 initialCapacity 和 loadFactor，在 put 第一对映射关系时，会创建一个长度为 initialCapacity 的 Node 数组，此数组存放元素的位置也称"桶"，每个 bucket 中存储的元素就是 Node 对象，Node 对象可以带一个引用变量 next   指向下一个元素，故一个桶中可能生成一个 Node 链或 TreeNode 对象，而每个 TreeNode 对象可以有两个叶节点 left 和 right，而新添加的元素作为链表的 last 或 TreeNode 树的叶节点。

在 JDK7 的扩容基础上，JDK8 当 HashMap 的其中一个链的对象个数如果达到8个，capacity 没有到达64，那么 HashMap 会先扩容解决，如果已经到达64则此链变成红黑树，节点类型由 Node 成为 TreeNode 类型。当然，如果当映射关系被移除后，下次 resize 方法判断树的节点个数低于6个则会把红黑树再转为链表。

JDK8 相较于 JDK7 在底层实现方面的不同体现在 new HashMap 底层没有创建一个长度为16的数组，且 JDK8 底层数组是 Node 而非 Entry；在首次调用 put 方法时底层才会创建长度为16的数组；底层结构在 JDK8 时在数组与链表的基础上增加红黑树，且旧元素指向新元素；当数组的某一索引位置元素以链表形式存在个数多余8且当前数组长度大于64则此时索引位置上的数据改为红黑树存储。

```java
/*    
 * HashMap源码中的重要常量:
 * DEFAULT_INITIAL_CAPACITY: HashMap的默认容量 => 16
 * DEFAULT_LOAD_FACTOR: HashMap的默认加载因子：0.75
 * threshold: 扩容的临界值=容量*填充因子: 16 * 0.75 => 12
 * TREEIFY_THRESHOLD: Bucket中链表长度大于该默认值转化为红黑树 => 8
 * MIN_TREEIFY_CAPACITY: 桶中的Node被树化时最小的hash表容量 => 64
*/
```

* LinkedHashMap

LinkedHashMap 是 HashMap 的子类，在 HashMap 的存储结构基础上，使用了一对双向链表来记录添加元素的顺序。与 LinkedHashSet 类似，LinkedHashMap 可以维护 Map 的迭代顺序与 Key-Value 的插入顺序一致。

```java
// HashMap 中的 Node
static class Node<K,V> implements Map.Entry<K,V> {
    final int hash;
    final K key;
    V value;
    Node<K,V> next;
}
// LinkedHashMap 中的 Entry
static class Entry<K,V> extends HashMap.Node<K,V> {
    Entry<K,V> before, after;//能够记录添加的元素的先后顺序
    Entry(int hash, K key, V value, Node<K,V> next) {
        super(hash, key, value, next);
    }
} 
```

```java
import org.junit.Test;

import java.util.*;

public class MapTest {
    /**
     *  元素查询操作
     */
    @Test
    public void test1(){
        Map map = new HashMap();
        map.put("AA",123);
        map.put(45,123);
        map.put("BB",56);
        // Object get(Object key) => 获取指定key对应的value
        System.out.println(map.get(45));
        // containsKey(Object key) => 是否包含指定的key
        boolean isExist = map.containsKey("BB");
        System.out.println(isExist);
        // boolean containsValue(Object value) => 是否包含指定的value
        isExist = map.containsValue(123);
        System.out.println(isExist);
        // int size() => 返回map中key-value对的个数
        System.out.println(map.size());
        // boolean equals(Object obj) => 判断当前map和参数对象obj是否相等
        System.out.println(map.equals(map));
        map.clear();
        // boolean isEmpty() => 判断当前map是否为空
        System.out.println(map.isEmpty());
    }
    /**
     * 添加、删除、修改操作
     */
    @Test
    public void test2(){
        Map map = new HashMap();
        // Object put(Object key,Object value) => 将指定key-value添加到(或修改)当前map对象中
        map.put("AA",123);
        map.put(45,123);
        map.put("BB",56);
        // void putAll(Map m) => 将m中的所有key-value对存放到当前map中
        map.put("AA",87);

        System.out.println(map);
        // void putAll(Map m) => 将m中的所有key-value对存放到当前map中
        Map map1 = new HashMap();
        map1.put("CC",123);
        map1.put("DD",456);
        map.putAll(map1);

        System.out.println(map);

        // Object remove(Object key) => 移除指定key的key-value对并返回value
        Object value = map.remove("CC");
        System.out.println(value);
        System.out.println(map);

        // void clear() => 清空当前map中的所有数据
        map.clear();//与map = null操作不同
        System.out.println(map.size());
        System.out.println(map);
    }
    /**
     *  元视图操作的方法
     */
    @Test
    public void test3(){
        Map map = new HashMap();
        map.put("AA",123);
        map.put(45,1234);
        map.put("BB",56);

        // Set keySet() => 返回所有key构成的Set集合
        Set set = map.keySet();
        Iterator iterator = set.iterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        }
        System.out.println("*****************");

        // Collection values() => 返回所有value构成的Collection集合
        Collection values = map.values();
        for(Object obj : values){
            System.out.println(obj);
        }
        System.out.println("***************");

        // Set entrySet() => 返回所有key-value对构成的Set集合
        //方式一：
        Set entrySet = map.entrySet();
        Iterator iterator1 = entrySet.iterator();
        while (iterator1.hasNext()){
            Object obj = iterator1.next();
            //entrySet集合中的元素都是entry
            Map.Entry entry = (Map.Entry) obj;
            System.out.println(entry.getKey() + "---->" + entry.getValue());

        }
        System.out.println("/");

        //方式二：
        Set keySet = map.keySet();
        Iterator iterator2 = keySet.iterator();
        while(iterator2.hasNext()){
            Object key = iterator2.next();
            Object value = map.get(key);
            System.out.println(key + "=====" + value);
        }
    }
}
```

HashMap 与 Hashtable 都实现了 Map 接口。由于前者的非线程安全性，效率上可能高于 后者。Hashtable 的方法是 Synchronize 的，在多个线程访问 Hashtable 时，不需要为其的方法实现同步；而 HashMap 不是，必须为之提供外同步。HashMap 允许将 null 作为一个 Entry 的 key 或者 value，但 Hashtable 不允许。HashMap 把 Hashtable 的 contains 方法改成 containsvalue 和 containsKey，只因 contains 方法容易引起误解。Hashtable 继承自 Dictionary 类，而 HashMap 是 Java1.2 引进的 Map interface 的一个实现。Hashtable 和 HashMap 采用的 hash/rehash 算法大体一样，故性能没有很大的差异。

* TreeMap

底层使用**红黑树**结构存储数据的 TreeMap 存储 Key-Value 对时，需根据 Key-Value 对进行排序，保证所有的 Key-Value 对处于有序状态。TreeMap 的 Key 排序有自然排序和定制排序。前者所有 key 必须实现 Comparable 接口，且所有的 Key 应该是同一个类的对象，否则抛出 ClassCastException。后者在创建 TreeMap 时传入一个 Comparator 对象，该对象负责对 TreeMap 中所有 Key 进行排序，此时不需要实现 Comparable 接口。TreeMap 判断两个 Key 相等的标准是通过 `compareTo()` 或 `compare()` 方法返回0。

```java
public class User implements Comparable{
    private String name;
    private int age;

    public User() {
    }

    public User(String name, int age) {
        this.name = name;
        this.age = age;
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

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    @Override
    public boolean equals(Object o) {
        System.out.println("User equals()....");
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        User user = (User) o;

        if (age != user.age) return false;
        return name != null ? name.equals(user.name) : user.name == null;
    }

    @Override
    public int hashCode() { //return name.hashCode() + age;
        int result = name != null ? name.hashCode() : 0;
        result = 31 * result + age;
        return result;
    }

    //按照姓名从大到小排列,年龄从小到大排列
    @Override
    public int compareTo(Object o) {
        if(o instanceof User){
            User user = (User)o;
//            return -this.name.compareTo(user.name);
            int compare = -this.name.compareTo(user.name);
            if(compare != 0){
                return compare;
            }else{
                return Integer.compare(this.age,user.age);
            }
        }else{
            throw new RuntimeException("输入的类型不匹配");
        }

    }
}
```

```java
import org.junit.Test;

import java.util.*;

public class TreeMapTest {
    /**
     * 向TreeMap中添加key-value，要求key必须是由同一个类创建的对象
     * 因为要按照key进行排序：自然排序 、定制排序
     */
    // 自然排序
    @Test
    public void test(){
        TreeMap map = new TreeMap();
        User u1 = new User("Tom",23);
        User u2 = new User("Jerry",32);
        User u3 = new User("Jack",20);
        User u4 = new User("Rose",18);

        map.put(u1,98);
        map.put(u2,89);
        map.put(u3,76);
        map.put(u4,100);

        Set entrySet = map.entrySet();
        Iterator iterator1 = entrySet.iterator();
        while (iterator1.hasNext()){
            Object obj = iterator1.next();
            Map.Entry entry = (Map.Entry) obj;
            System.out.println(entry.getKey() + "---->" + entry.getValue());

        }
    }

    // 定制排序
    @Test
    public void test2(){
        TreeMap map = new TreeMap(new Comparator() {
            @Override
            public int compare(Object o1, Object o2) {
                if(o1 instanceof User && o2 instanceof User){
                    User u1 = (User)o1;
                    User u2 = (User)o2;
                    return Integer.compare(u1.getAge(),u2.getAge());
                }
                throw new RuntimeException("输入的类型不匹配！");
            }
        });
        User u1 = new User("Tom",23);
        User u2 = new User("Jerry",32);
        User u3 = new User("Jack",20);
        User u4 = new User("Rose",18);

        map.put(u1,98);
        map.put(u2,89);
        map.put(u3,76);
        map.put(u4,100);

        Set entrySet = map.entrySet();
        Iterator iterator1 = entrySet.iterator();
        while (iterator1.hasNext()){
            Object obj = iterator1.next();
            Map.Entry entry = (Map.Entry) obj;
            System.out.println(entry.getKey() + "---->" + entry.getValue());

        }
    }
}
```

* Hashtable

Hashtable 是个 JDK1.0 就提供的古老 Map 实现类，不同于 HashMap，Hashtable 是线程安全的。Hashtable 和  HashMap实现原理相同，底层都是用哈希表结构，常可以互用。与 HashMap 不同的是，Hashtable 不允许使用 null 作为 key 和 value；与 HashMap 相同的是，Hashtable 也不能保证其中 Key-Value 对顺序。在判断两个 Key 和 Value 相等的标准与 HashMap 一致。

* Properties

Properties 类是 Hashtable 的子类，常用于处理属性文件。由于属性文件里的 key、value 都是字符串类型，故 Properties 里的 key 和 value 都是字符串类型。存取数据时，建议使用`setProperty(String key,Stringvalue)`和`getProperty(String key)`方法。

在 IDEA 建立 jdbc.properties 文件的步骤是 new 一个 Resource Bundle，并命名 jdbc 点击 OK。在 jdbc.properties 文件以等号连接 key 与 value。

#### Collections 工具类

和操作数组的工具类 Arrays 类似，Collections 是一个操作 Set、List 和 Map 等集合的工具类，提供了一系列<u>静态的方法</u>对集合元素进行排序、查询和修改等操作，还提供了对集合对象设置不可变、对集合对象实现同步控制等方法。

```java
import org.junit.Test;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.List;

/**
 * Collections:操作Collection、Map的工具类
 *
 * 面试题：Collection 和 Collections的区别？
 *       Collection是集合类的上级接口，继承于他的接口主要有Set 和List.
 *       Collections是针对集合类的一个帮助类，他提供一系列静态方法实现对各种集合的搜索、排序、线程安全化等操作.
 */
public class CollectionTest {
    /**
     * reverse(List)：反转 List 中元素的顺序
     * shuffle(List)：对 List 集合元素进行随机排序
     * sort(List)：根据元素的自然顺序对指定 List 集合元素按升序排序
     * sort(List，Comparator)：根据指定的 Comparator 产生的顺序对 List 集合元素进行排序
     * swap(List，int， int)：将指定 list 集合中的 i 处元素和 j 处元素进行交换
     *
     * Object max(Collection)：根据元素的自然顺序，返回给定集合中的最大元素
     * Object max(Collection，Comparator)：根据 Comparator 指定的顺序，返回给定集合中的最大元素
     * Object min(Collection)
     * Object min(Collection，Comparator)
     * int frequency(Collection，Object)：返回指定集合中指定元素的出现次数
     * void copy(List dest,List src)：将src中的内容复制到dest中
     * boolean replaceAll(List list，Object oldVal，Object newVal)：使用新值替换 List 对象的所有旧值
     *
     */

    @Test
    public void test(){
        List list = new ArrayList();
        list.add(123);
        list.add(43);
        list.add(765);
        list.add(765);
        list.add(765);
        list.add(-97);
        list.add(0);

        System.out.println(list);

//        Collections.reverse(list);
//        Collections.shuffle(list);
//        Collections.sort(list);
//        Collections.swap(list,1,2);
        int frequency = Collections.frequency(list, 123);

        System.out.println(list);
        System.out.println(frequency);
    }

    @Test
    public void test2(){
        List list = new ArrayList();
        list.add(123);
        list.add(43);
        list.add(765);
        list.add(-97);
        list.add(0);

        //报异常：IndexOutOfBoundsException("Source does not fit in dest")
//        List dest = new ArrayList();
//        Collections.copy(dest,list);
        //正确的：
        List dest = Arrays.asList(new Object[list.size()]);
        System.out.println(dest.size());//list.size();
        Collections.copy(dest,list);

        System.out.println(dest);

        /**
         * Collections 类中提供了多个 synchronizedXxx() 方法，
         * 该方法可使将指定集合包装成线程同步的集合，从而可以解决
         * 多线程并发访问集合时的线程安全问题
         */
        //返回的list1即为线程安全的List
        List list1 = Collections.synchronizedList(list);
    }
}
```

#### Enumeration

Enumeration 接口是 Iterator 迭代器的“古老版本”。

![](./assets/Enumeration.png)

```java
Enumeration stringEnum = new StringTokenizer("a-b*c-d-e-g", "-");
    while(stringEnum.hasMoreElements()){
        Object obj = stringEnum.nextElement();System.out.println(obj); 
    }
```

### 泛型

集合容器类在设计阶段/声明阶段不能确定这个容器到底实际存的是什么类型的对象，所以**在JDK1.5之前只能把元素类型设计为Object，JDK1.5之后使用泛型来解决**。因为这个时候除了元素的类型不确定，其他的部分是确定的，例如关于这个元素确定的保存与管理等，因此此时**把元素的类型设计成一个参数，这个类型参数叫做泛型**。Collection，List，ArrayList 这个就是类型参数，即泛型。若集合没有泛型则会出现任何类型都可以添加在集合中的情况导致类型不安全，且读取出来的对象需要强转，不但繁琐还可能出现 ClassCastException。

#### 集合中使用泛型

泛型类型必须是类，不能是基本数据类型。需要用到基本数据类型的位置，拿包装类替换。

```java
import org.junit.Test;

import java.util.*;

/**
 * 在集合中使用泛型总结
 *  ①集合接口或集合类在jdk5.0时都修改为带泛型的结构。
 *  ②在实例化集合类时，可以指明具体的泛型类型
 *  ③指明完以后，在集合类或接口中凡是定义类或接口时，内部结构（比如：方法、构造器、属性等）使用到类的泛型的位置，都指定为实例化的泛型类型。
 *    比如：add(E e)  --->实例化以后：add(Integer e)
 *  ④注意点：泛型的类型必须是类，不能是基本数据类型。需要用到基本数据类型的位置，拿包装类替换
 *  ⑤如果实例化时，没有指明泛型的类型。默认类型为java.lang.Object类型。
 */
public class GenericTest {

    //在集合中使用泛型的情况：以HashMap为例
    @Test
    public void test1(){
//        Map<String,Integer> map = new HashMap<String,Integer>();
        // jdk7新特性：类型推断
        Map<String,Integer> map = new HashMap<>();

        map.put("Tom",87);
        map.put("Tone",81);
        map.put("Jack",64);

//        map.put(123,"ABC");

        // 泛型的嵌套
        Set<Map.Entry<String,Integer>> entry = map.entrySet();
        Iterator<Map.Entry<String, Integer>> iterator = entry.iterator();

        while(iterator.hasNext()){
            Map.Entry<String, Integer> e = iterator.next();
            String key = e.getKey();
            Integer value = e.getValue();
            System.out.println(key + "----" + value);
        }
    }

    // 在集合中使用泛型的情况：以ArrayList为例
    @Test
    public void test2(){
        ArrayList<Integer> list = new ArrayList<Integer>();

        list.add(78);
        list.add(49);
        list.add(72);
        list.add(81);
        list.add(89);
        //编译时，就会进行类型检查，保证数据的安全
//        list.add("Tom");

        //方式一：
//        for(Integer score :list){
//            //避免了强转的操作
//            int stuScore = score;
//
//            System.out.println(stuScore);
//        }

        //方式二：
        Iterator<Integer> iterator = list.iterator();
        while(iterator.hasNext()){
            int stuScore = iterator.next();
            System.out.println(stuScore);
        }
    }
}
```

#### 自定义泛型结构

自定义泛型结构应该从泛型类、泛型接口、泛型方法三方面进行考虑。泛型类可能有多个参数，此时应将多个参数一起放在尖括号内 <E1,E2,E3>。泛型类的构造器应该是 `public GenericClass(){}`。 而并非 `public GenericClass<E>(){}`。实例化后，操作原来泛型位置的结构必须与指定的泛型类型一致。泛型不同的引用不能相互赋值。泛型如果不指定，将被擦除，泛型对应的类型均按照 Object 处理，但不等价于 Object。如果泛型结构是一个接口或抽象类，则不可创建泛型类的对象。JDK7.0 后由于存在类型推断，范型可以省略 new 的范型尖括号内容 `ArrayList<Fruit> flist = new ArrayList<>();`。在类/接口上声明的泛型，在本类或本接口中即代表某种类型，可以作为非静态属性的类型、非静态方法的参数类型、非静态方法的返回值类型。但在静态方法中不能使用类的泛型。 异常类不能是泛型的。不能使用 `new E[]`。但是可以 `E[] elements = (E[])new Object[capacity];`，参考 ArrayList 源码中声明 `Object[] elementData`，而非泛型参数类型数组。

```java
import java.util.ArrayList;
import java.util.List;

/**
 * 自定义泛型类
 */
public class OrderTest<T> {

    String orderName;
    int orderId;

    // 类的内部结构就可以使用类的泛型
    T orderT;

    public OrderTest(){

    };

    public OrderTest(String orderName,int orderId,T orderT){
        this.orderName = orderName;
        this.orderId = orderId;
        this.orderT = orderT;
    }

    // 如下的三个方法都不是泛型方法
    public T getOrderT(){
        return orderT;
    }

    public void setOrderT(T orderT){
        this.orderT = orderT;
    }

    @Override
    public String toString() {
        return "Order{" +
                "orderName='" + orderName + '\'' +
                ", orderId=" + orderId +
                ", orderT=" + orderT +
                '}';
    }

    // 泛型方法是在方法中出现了泛型的结构,泛型参数与类的泛型参数没有任何关系
    // 换句话说,泛型方法所属的类是不是泛型类都没有关系
    // 泛型方法可以声明为静态的.原因是泛型参数是在调用方法时确定的,并非在实例化类时确定
    public static <E> List<E> copyFromArrayToList(E[] arr){
        ArrayList<E> list = new ArrayList<>();
        for(E e : arr){
            list.add(e);
        }
        return list;
    }
}
```

```java
public class SubOrder extends OrderTest<Integer>{ // SubOrder 不是泛型类
}
```

```java
public class SubOrder1<T> extends OrderTest<T> { // SubOrder1<T> 仍然是泛型类
}
```

```java
import org.junit.Test;

/**
 * 自定义泛型结构：泛型类、泛型接口、泛型方法
 */
public class GenericTest1 {

    @Test
    public void test1(){
        // 如果定义了泛型类,实例化没有指明类的泛型,则认为此泛型类型为Object类型,故若定义了类是带泛型的,建议在实例化时要指明类的泛型
        OrderTest order = new OrderTest();
        order.setOrderT(123);
        System.out.println(order);
        order.setOrderT("ABC");
        System.out.println(order);

        // 建议实例化时指明类的泛型
        OrderTest<String> order1 = new OrderTest<String>("orderAA",1001,"order:AA");
        order1.setOrderT("AA:hello");
        System.out.println(order1);
    }

    @Test
    public void test2(){
        SubOrder sub1 = new SubOrder();
        sub1.setOrderT(1122);
        System.out.println(sub1);

        // 由于子类在继承带泛型的父类时指明了泛型类型,则实例化子类对象时不再需要指明泛型
        SubOrder1<String> sub2 = new SubOrder1<>();
        sub2.setOrderT("order2...");
        System.out.println(sub2);
    }
}
```

若父类有泛型，则子类可以选择保留泛型也可以选择指定泛型类型。

```java
class Father<T1, T2> {
}
// 子类不保留父类的泛型
// 1)没有类型 擦除
class Son1 extends Father { // 等价于class Son extends Father<Object,Object>{
}
// 2)具体类型
class Son2 extends Father<Integer, String> {
}
// 子类保留父类的泛型
// 1)全部保留
class Son3<T1, T2> extends Father<T1, T2> {
}
// 2)部分保留
class Son4<T2> extends Father<Integer, T2> {
}
```

方法也可以被泛型化，不管此时定义在其中的类是不是泛型类。在泛型方法中可以定义泛型参数，此时参数的类型就是传入数据的类型。

```java
 // [访问权限] 返回类型 方法名([泛型标识 参数名称]) 抛出的异常
 public static <E>  List<E> copyFromArrayToList(E[] arr)　throws  Exception{ }
```

泛型在继承方面的体现于虽然类 A 是类 B 的父类，但是 `G<A>` 和 `G<B>` 二者不具备子父类关系，二者是并列关系。值得注意的有类 A 是类 B 的父类，那么 `A<G>` 是 `B<G>` 的父类。

```java
import org.junit.Test;

import java.util.*;

public class GenericTest {
    @Test
    public void test3(){

        Object obj = null;
        String str = null;
        obj = str;

        Object[] arr1 = null;
        String[] arr2 = null;
        arr1 = arr2;
//      编译不通过
//      Date date = new Date();
//      str = date;
        List<Object> list1 = null;
        List<String> list2 = new ArrayList<String>();
//      此时的list1和list2的类型不具有子父类关系,编译不通过
//      list1 = list2;
        /**
         * 反证法假设list1 = list2;list1.add(123);导致混入非String的数据。出错。
         */
        show(list1);
        show2(list2);
    }

    public void show2(List<String> list){

    }

    public void show(List<Object> list){

    }

    @Test
    public void test4(){
        AbstractList<String> list1 = null;
        List<String> list2 = null;
        ArrayList<String> list3 = null;

        list1 = list3;
        list2 = list3;

        List<String> list4 = new ArrayList<>();
    }
}
```

#### 通配符的使用

通配符 ? 常使用于 List<?>、Map<?,?> 等之流，可看作所有 List、Map 等泛型的父类，在读取通配符泛型容器中的元素是安全的，因不论容器中的真实类型，包含的都是 Object。但是在写入元素时由于不知道元素类型故不能向其中添加对象，当然出了 null 例外，因其是所有类型的成员。

类 A 是类 B 的父类，`G<A>` 和 `G<B>` 是没有关系的，但是两者有共同的父类 `G<?>`。

```java
Collection<?> c = new ArrayList();
c.add(new Object()); // 编译时错误
```

使用通配符泛型导致<u>编译错误</u>的情况通常有三种，分别是泛型方法声明的返回值前 `<>`、泛型类的声明、创建对象。

```java
public static <?> void test(ArrayList<?> list){}
class GenericTypeClass<?>{}
ArrayList<?> list2 = new ArrayList<?>();
```

```java
import org.junit.Test;

import java.util.*;

public class GenericTest {

    @Test
    public void test5(){
        List<?> list = null;
        List<String> list3 = new ArrayList<>();
        list3.add("AA");
        list3.add("BB");
        list3.add("CC");
        list = list3;
        list.add(null);
        // 获取(读取):允许读取数据,读取的数据类型为Object
        Object o = list.get(0);
        System.out.println(o);
        System.out.println(list);
    }
}
```

`<?>` 允许所有泛型的引用调用。在需通配符指定上限的需求下可使用 extends，指定的类型必须继承某个类或接口，即可完成 <= 效果；若需要通配符指定下限则可使用 super，指定的类型不能小于操作的类，即可完成 >= 效果。

```java
<? extends Number> (无穷小, Number] => 只允许泛型为Number及Number子类的引用调用
<? super Number> [Number, 无穷大) => 只允许泛型为Number及Number父类的引用调用
<? extends Comparable> => 只允许泛型为实现Comparable接口的实现类的引用调用
```

```java
public class Person {}
```

```java
public class Dev extends Person{}
```

```java
import org.junit.Test;

import java.util.*;

public class GenericTest {
    @Test
    public void test6(){
        List<? extends Person> list1 = null;
        List<? super Person> list2 = null;

        List<Dev> list3 = new ArrayList<Dev>();
        List<Person> list4 = new ArrayList<Person>();
        List<Object> list5 = new ArrayList<Object>();

//      只允许 Person 与其子类调用
        list1 = list3;
        list1 = list4;
//        list1 = list5;

//      只允许 Person 与其父类调用
//        list2 = list3;
        list2 = list4;
        list2 = list5;

//      读取数据
        list1 = list3;
        Person p = list1.get(0);
//      编译不通过
//      Dev s = list1.get(0);

        list2 = list4;
        Object obj = list2.get(0);
//      编译不通过
//      Person obj = list2.get(0);

//      写入数据
//      编译不通过
//      list1.add(new Dev());

//      编译通过
        list2.add(new Person());
        list2.add(new Dev());
    }
}
```

#### 泛型嵌套

```java
public static void main(String[] args) {
    HashMap<String, ArrayList<Citizen>> map= new HashMap<String, ArrayList<Citizen>>();
    ArrayList<Citizen> list= new ArrayList<Citizen>();
    list.add(new Citizen("D"));
    list.add(new Citizen("M"));
    list.add(new Citizen("S"));
    map.put("D", list);
    Set<Entry<String, ArrayList<Citizen>>> entrySet= map.entrySet();
    Iterator<Entry<String, ArrayList<Citizen>>> iterator= entrySet.iterator();
    while(iterator.hasNext()) {
        Entry<String, ArrayList<Citizen>> entry= iterator.next();
        String key= entry.getKey();
        ArrayList<Citizen> value= entry.getValue();
        System.out.println("户主："+ key);
        System.out.println("家庭成员："+ value);
    }
}
```

### IO流

#### File 类使用

* 实例化

java.io.File 类的一个对象，<u>代表一个文件或文件目录</u>，与平台无关。File 能新建、删除、重命名文件和目录，但 File 不能访问文件内容本身。如果需要访问文件内容本身，则需要使用输入/输出流。想要在 Java 程序中表示一个真实存在的文件或目录，那么必须有一个 File 对象，但是 Java 程序中的一个 File 对象，可能没有一个真实存在的文件或目录。File 对象可以作为参数传递给流的构造器。

```java
import org.junit.Test;
import java.io.File;

public class FileTest {
    /**
     * ```java
     * // 创建file类的实例
     * File(String filePath) => 以filePath为路径创建File对象.可以是绝对路径或者相对路径
     * File(String parentPath, String childPath) => 以parentPath为父路径,childPath为子路径创建File对象
     * File(File parentFile, String childPath) => 根据一个父File对象和子文件路径创建File对象
     * ```
     */
    @Test
    public void test1(){
        // 构造器1
        File file1 = new File("hello.txt"); // 相对于当前module
        File file2 = new File("/Users/.../IdeaProjects/zszy/src/com/.../IO/helloIO.txt");

        System.out.println(file1); // hello.txt
        System.out.println(file2); // /Users/xieziyi/IdeaProjects/zszy/src/com/.../IO/helloIO.txt

        // 构造器2
        File file3 = new File("/Users/.../IdeaProjects/zszy/src/com/.../IO/","JavaIO");
        System.out.println(file3); // /Users/xieziyi/IdeaProjects/zszy/src/com/.../IO/JavaIO

        // 构造器3
        File file4 = new File(file3,"hi.txt");
        System.out.println(file4); // /Users/.../IdeaProjects/zszy/src/com/.../IO/JavaIO/hi.txt
    }
}
```

由于 Java 程序支持跨平台运行，因此路径分隔符存在区别，windows 为 \\\，Unix 是 /。为解决此隐患，File 类提供一个常量 separator 可根据操作系统动态提供分隔符。

```java
// public  static final String separator
// 等价于 File file = ("d:\\Backend\\info.txt");
File file = new File("d:" + File.separator + "Backend" + File.separator + "info.txt");
```

* File 类的常用方法

```java
import org.junit.Test;
import java.io.File;
import java.util.Date;

public class FileTest {
    /**
     * public String getAbsolutePath() => 获取绝对路径。
     * public String getPath() => 获取路径。
     * public String getName() => 获取名称。
     * public String getParent() => 获取上层文件目录路径。若无，返回null
     * public long length() => 获取文件长度（字节数）。不能获取目录的长度。
     * public long lastModified() => 获取最后一次的修改时间，毫秒值。
     *
     * 适用于文件目录方法
     * public String[] list() => 获取指定目录下的所有文件或者文件目录的名称数组。
     * public File[] listFiles() => 获取指定目录下的所有文件或者文件目录的File数组。
     */
    @Test
    public void test2(){
        File file = new File("Hello.txt");
        File file2 = new File("/Users/.../IdeaProjects/zszy/src/com/.../IO/JavaIO/hi.txt");

        System.out.println(file.getAbsolutePath()); // /Users/.../IdeaProjects/zszy/Hello.txt
        System.out.println(file.getPath()); // Hello.txt
        System.out.println(file.getName()); // Hello.txt
        System.out.println(file.getParent()); // null
        System.out.println(file.length()); // 0
        System.out.println(new Date(file.lastModified())); // Thu Jan 01 08:00:00 CST 1970

        System.out.println();

        System.out.println(file2.getAbsolutePath()); // /Users/.../IdeaProjects/zszy/src/com/.../IO/JavaIO/hi.txt
        System.out.println(file2.getPath()); // /Users/.../IdeaProjects/zszy/src/com/.../IO/JavaIO/hi.txt
        System.out.println(file2.getName()); // hi.txt
        System.out.println(file2.getParent()); // /Users/.../IdeaProjects/zszy/src/com/.../IO/JavaIO
        System.out.println(file2.length()); // 0
        System.out.println(file2.lastModified()); // 0
    }

    @Test
    public void test3(){
        // 文件需存在！！！
        File file = new File("/Users/.../IdeaProjects/zszy/src/com/.../IO");

        // list() => Returns an array of strings naming the files and directories in the directory denoted by this abstract pathname.
        String[] list = file.list();
        for(String s : list){
            System.out.println(s); // 遍历文件夹下所有文件(相对路径)
        }

//        System.out.println();

        // listFiles() => Returns an array of abstract pathnames denoting the files in the directory denoted by this abstract pathname.
        File[] files = file.listFiles();
        for(File f : files){
            System.out.println(f); // 遍历文件夹下所有文件(绝对路径)
        }
    }

    /**
     * File类的重命名
     * public boolean renameTo(File dest) => 把文件重命名为指定的文件路径
     * file1.renameTo(file2) =>  要想保证返回true,需要file1在硬盘中是存在的,且file2不能在硬盘中存在
     */
    @Test
    public void test4(){
        File file1 = new File("/Users/.../IdeaProjects/zszy/src/com/.../IO/helloIO.txt");
        File file2 = new File("/Users/.../IdeaProjects/zszy/src/com/.../IO/hiIO.txt");

        boolean renameTo = file1.renameTo(file2);
        System.out.println(renameTo);
    }
    /**
     * File类中涉及到关于文件或文件目录的创建、删除、重命名、修改时间、文件大小等方法,并未涉及到写入或读取文件内容的操作.如果需要读取或写入文件内容,必须使用IO流来完成
     * 后续File类的对象常会作为参数传递到流的构造器中,指明读取或写入的"终点".
     * public boolean isDirectory() => 判断是否是文件目录
     * public boolean isFile() => 判断是否是文件
     * public boolean exists() => 判断是否存在
     * public boolean canRead() => 判断是否可读
     * public boolean canWrite() => 判断是否可写
     * public boolean isHidden() => 判断是否隐藏
     */
    @Test
    public void test5(){

        File file1 = new File("./src/com/.../IO/helloIO.txt");

        System.out.println(file1.isDirectory());
        System.out.println(file1.isFile());
        System.out.println(file1.exists());
        System.out.println(file1.canRead());
        System.out.println(file1.canWrite());
        System.out.println(file1.isHidden());
    }

    /**
     * 创建硬盘中对应的文件或文件目录
     * public boolean createNewFile() => 创建文件,若文件存在,则不创建,返回false
     * public boolean mkdir() => 创建文件目录.如果此文件目录存在,就不创建了.如果此文件目录的上层目录不存在,也不创建
     * public boolean mkdirs() => 创建文件目录.如果此文件目录存在,就不创建了.如果上层文件目录不存在,一并创建
     *
     * 删除磁盘中的文件或文件目录
     * public boolean delete() => 删除文件或者文件夹
     * 删除注意事项  =>  Java中的删除不走回收站
     */
    @Test
    public void test6() throws IOException {
        File file1 = new File("hi.txt");
        if(!file1.exists()){
            // 文件的创建
            file1.createNewFile();
            System.out.println("创建成功");
        }else{ // 文件存在
            file1.delete();
            System.out.println("删除成功");
        }
    }

    @Test
    public void test7(){
        // 文件目录的创建
        File file1 = new File("./src/com/.../IO/IOmkdir");

        boolean mkdir = file1.mkdir();
        if(mkdir){
            System.out.println("创建成功1");
        }

        File file2 = new File("./src/com/.../IO/IOmkdirP/IOmkdirs");

        boolean mkdir1 = file2.mkdirs();
        if(mkdir1){
            System.out.println("创建成功2");
        }
        // 要想删除成功,文件目录下不能有子目录或文件
        File file3 = new File("./src/com/.../IO/IOmkdir");
        System.out.println(file3.delete());
        File file4 = new File("./src/com/.../IO/IOmkdirP/IOmkdirs");
        System.out.println(file4.delete());
        File file5 = new File("./src/com/.../IO/IOmkdirP");
        System.out.println(file5.delete());
    }
}
```

#### IO 流原理及流的分类

* IO 流原理

I/O 用于处理设备之间的数据传输。如读/写文件，网络通讯等。Java 中对于数据的输入/输出操作以“流(stream)”的方式进行。java.io包下提供了各种“流”类和接口，用以获取不同种类的数据，并通过标准的方法输入或输出数据。输入 input 读取外部数据（磁盘、光盘等存储设备的数据）到程序（内存）中。输出 output 将程序（内存）数据输出到磁盘、光盘等存储设备中。

* 流的分类

按操作**数据单位**不同分为8 bit字节流，16 bit字符流；按数据流的**流向**不同分为输入流，输出流；按流的**角色**的不同分为节点流，处理流。

| 抽象基类 | 字节流          | 字符流    |
| ---- | ------------ | ------ |
| 输入流  | InputStream  | Reader |
| 输出流  | OutputStream | Writer |

Java 的 IO 流共涉及40多个类，实际上非常规则，都是从4个抽象基类派生的。由这四个类派生出来的子类名称都是以其父类名作为子类名后缀。

![IO流体系](./assets/IO流体系.png)

#### 节点流|文件流

* FileReader 读入数据

```java
// 建立一个流对象,将已存在的一个文件加载进流
FileReader fr= new FileReader(new File("Test.txt"));
// 创建一个临时存放数据的数组
char[] ch= new char[1024];
// 调用流对象的读取方法将流中的数据读入到数组中
// read()返回读入的一个字符.如果达到文件末尾,返回-1.
// read(char[] cbuf)返回每次读入cbuf数组中的字符的个数.如果达到文件末尾,返回-1.
fr.read(ch);
// 关闭资源
fr.close();
```

垃圾回收机制只回收 JVM 对内存的对象空间。对于其他的数据库、输入输出流、Socket物理连接无力，故需要手动关闭。

在抛出异常时，建议使用 try-catch，因在使用流时，若因调用 read 导致阻塞出现，可能出现 IOExcerption 异常，在创建异常对象后因是运行时异常就会抛出，导致后面程序不执行，那么就会使流未关闭，存在资源浪费的泄漏问题。

```java
import org.junit.Test;

import java.io.*;

/**
 * 一、流的分类
 * 1.操作数据单位:字节流、字符流
 * 2.数据的流向:输入流、输出流
 * 3.流的角色:节点流、处理流
 *
 * 二、流的体系结构
 * 抽象基类         节点流(或文件流)                                  缓冲流(处理流的一种)
 * InputStream     FileInputStream   (read(byte[] buffer))        BufferedInputStream (read(byte[] buffer))
 * OutputStream    FileOutputStream  (write(byte[] buffer,0,len)  BufferedOutputStream (write(byte[] buffer,0,len) / flush()
 * Reader          FileReader (read(char[] cbuf))                 BufferedReader (read(char[] cbuf) / readLine())
 * Writer          FileWriter (write(char[] cbuf,0,len)           BufferedWriter (write(char[] cbuf,0,len) / flush()
 */
public class FileReaderWriterTest {

    /**
     * 1. read()返回读入的一个字符.如果达到文件末尾,返回-1
     * 2. 异常的处理:为了保证流资源一定可以执行关闭操作.需要使用try-catch-finally处理
     * 3. 读入的文件一定要存在,否则就会报FileNotFoundException
     */
    @Test
    public void test1(){
        FileReader fr = null;
        try {
            // 实例化File对象,指明要操作的文件
            File file = new File("./src/com/.../IO/helloIO.txt");
            // 提供具体的流
            fr = new FileReader(file);

            // 3.数据的读入过程
            // read()返回读入的一个字符,如果达到文件末尾,返回-1.
            // 方式一:
        int data = fr.read();
        while(data != -1){
            System.out.print((char) data);
            data = fr.read();
        }

            // 方式二:语法上针对于方式一的修改
//            int data;
//            while((data = fr.read()) != -1){
//                System.out.print((char) data);
//            }
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            // 流的关闭操作
//            try {
//                if(fr != null)
            // 此时若 fr 对象未实例化就 close 会出现空指针问题
//                    fr.close();
//            } catch (IOException e) {
//                e.printStackTrace();
//            }

            // 或
            if(fr != null){
                try {
                    fr.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

        }
    }

    // 对read()操作升级 => 使用read的重载方法
    @Test
    public void test2(){
        FileReader fr = null;
        try {
            // 1.File类的实例化
            File file = new File("./src/com/.../IO/helloIO.txt");

            // 2.FileReader流的实例化
            fr = new FileReader(file);

            // 3.读入的操作
            // read(char[] cbuf) => 返回每次读入cbuf数组中的字符的个数.如果达到文件末尾,返回-1
            char[] cbuf = new char[5];
            // char类型也对应一个int类型 => a = 97
            int len;

            while((len = fr.read(cbuf)) != -1){
                // 方式一
                // 错误的写法 => 若len不足cbuf.length会导致上次部分索引处数据未覆盖而读出
//                for(int i = 0;i < cbuf.length;i++){
//                    System.out.print(cbuf[i]);
//                }
                // 正确的写法
//                for(int i = 0;i < len;i++){
//                    System.out.print(cbuf[i]);
//                }

                // 方式二
                // 错误的写法,对应着方式一的错误的写法
//                String str = new String(cbuf);
//                System.out.print(str);
                // 正确的写法
                String str = new String(cbuf,0,len);
                System.out.print(str);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            if(fr != null){
                // 4.资源的关闭
                try {
                    fr.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
        }
    }
}
```

* FileWriter 写出数据

```java
// 创建流对象,建立数据存放文件
FileWriterfw= new FileWriter(new File("Test.txt"));
// 调用流对象的写入方法，将数据写入流
fw.write("write something");
// 关闭流资源,并将流中的数据清空到文件中
fw.close();
```

```java
import org.junit.Test;

import java.io.*;

public class FileReaderWriterTest {
    /**
     * 从内存中写出数据到硬盘的文件里
     * 说明:
     * 1.输出操作,对应的File可以不存在,并不会报异常
     * 2.
     *   File对应的硬盘中的文件如果不存在,在输出的过程中,会自动创建此文件.
     *   File对应的硬盘中的文件如果存在
     *       如果流使用的构造器 => FileWriter(file,false) / FileWriter(file):对原有文件的覆盖
     *       如果流使用的构造器是 => FileWriter(file,true):不会对原有文件覆盖,而是在原有文件基础上追加内容
     */
    @Test
    public void test3(){
        FileWriter fw = null;
        try {
            // 提供File类的对象,指明写出到的文件
            File file = new File("./src/com/.../IO/helloIO.txt");

            // 提供FileWriter的对象,用于数据的写出
            fw = new FileWriter(file,true);

            // 写出的操作
            fw.write("I have a dream!\n");
            fw.write("you need to have a dream!");
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            // 流资源的关闭
            if(fw != null){
                try {
                    fw.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

* FileReader 和 FileWriter 实现文本文件的复制

```java
import org.junit.Test;

import java.io.*;

public class FileReaderWriterTest {
    // 使用FileInputStream和FileOutputStream读写非文本文件
    @Test
    public void test4() {
        FileReader fr = null;
        FileWriter fw = null;
        try {
            // 1.创建File类的对象，指明读入和写出的文件
            File srcFile = new File("./src/com/.../IO/helloIO.txt");
            File srcFile2 = new File("./src/com/.../IO/helloIO.txt");

            // 不能使用字符流来处理图片等字节数据
//            File srcFile = new File("假装有图片.jpg");
//            File srcFile2 = new File("假装有图片1.jpg");

            // 2.创建输入流和输出流的对象
            fr = new FileReader(srcFile);
            fw = new FileWriter(srcFile2);

            //3.数据的读入和写出操作
            char[] cbuf = new char[5];
            int len; // 记录每次读入到cbuf数组中的字符的个数
            while((len = fr.read(cbuf)) != -1){
                // 每次写出len个字符
                fw.write(cbuf,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            // 4.关闭流资源
            // 方式一
//            try {
//                if(fw != null)
//                    fw.close();
//            } catch (IOException e) {
//                e.printStackTrace();
//            }finally{
//                try {
//                    if(fr != null)
//                        fr.close();
//                } catch (IOException e) {
//                    e.printStackTrace();
//                }
//            }
            // 方式二
            try {
                if(fw != null){
                    fw.close();}
            } catch (IOException e) {
                e.printStackTrace();
            }

            try {
                if(fr != null){
                    fr.close();}
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

* 使用FileInputStream不能读取文本文件

```java
import org.junit.Test;

import java.io.*;

public class FileReaderWriterTest {
    /**
     * 测试FileInputStream和FileOutputStream的使用
     *
     * 结论
     *    1. 对于文本文件(.txt,.java,.c,.cpp)，使用字符流处理
     *    2. 对于非文本文件(.jpg,.mp3,.mp4,.avi,.doc,.ppt,...)，使用字节流处理
     */
    @Test
    public void testFileInputStream(){
        FileInputStream fis = null;
        try {
            // 1.造文件
            File file = new File("./src/com/.../IO/helloIO.txt");

            // 2.造流
            fis = new FileInputStream(file);

            // 3.读数据
            byte[] buffer = new byte[5];
            int len; // 记录每次读取的字节的个数
            while((len = fis.read(buffer)) != -1){
                String str = new String(buffer,0,len);
                // 中文三个字节,英文一个字节 => 有中文则有概率乱码
                System.out.print(str);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            if(fis != null) {
                //4.关闭资源
                try {
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

* FileInputStream 和 FileOutputStream 读写非文本文件

```java
import org.junit.Test;

import java.io.*;

public class FileReaderWriterTest {
    /**
     * 实现对图片的复制操作
     */
    @Test
    public void testFileInputOutputStream()  {
        FileInputStream fis = null;
        FileOutputStream fos = null;
        try {
            // 1.造文件
            File srcFile = new File("./src/com/zairesinatra/IO/假装有图片.jpeg");
            File destFile = new File("./src/com/zairesinatra/IO/假装有图片2.jpeg");

            // 2.造流
            fis = new FileInputStream(srcFile);
            fos = new FileOutputStream(destFile);

            // 3.复制的过程
            byte[] buffer = new byte[5];
            int len;
            //4.读数据
            while((len = fis.read(buffer)) != -1){
                fos.write(buffer,0,len);
            }
            System.out.println("复制成功");
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(fos != null){
                //5.关闭资源
                try {
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(fis != null){
                try {
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
        }
    }
}
```

#### 缓冲流

为提高数据读写的速度，Java API 提供了带缓冲功能的流类，在使用这些流类时，会创建一个内部缓冲区数组，缺省使用8192个字节(8Kb)的缓冲区。

```java
public class BufferedInputStream extends FilterInputStream {
    private static int DEFAULT_BUFFER_SIZE = 8192;
}
```

冲流要“套接”在相应的节点流之上，根据数据操作单位可以把缓冲流分为 BufferedInputStream 和 BufferedOutputStream 组和 BufferedReader 和 BufferedWriter 组。当读取数据时，数据按块读入缓冲区，其后的读操作则直接访问缓冲区。当使用 BufferedInputStream 读取字节文件时，BufferedInputStream 会一次性从文件中读取8192个(8Kb)，存在缓冲区中，直到缓冲区装满了，才重新从文件中读取下一个8192个字节数组。向流中写入字节时，不会直接写到文件，先写到缓冲区中直到缓冲区写满，BufferedOutputStream 才会把缓冲区中的数据一次性写到文件里。使用方法 `flush()` 可以强制将缓冲区的内容全部写入输出流。关闭流的顺序和打开流的顺序相反。只要关闭最外层流即可，关闭最外层流也会相应关闭内层节点流。`flush()` 方法的使用是手动将 buffer 中内容写入文件。如果是带缓冲区的流对象的 `close()` 方法，不但会关闭流，还会在关闭流之前刷新缓冲区，关闭后不能再写出。

![IO流体系](./assets/缓冲区.png)

* 缓冲流(字节型)实现非文本文件的复制

```java

```