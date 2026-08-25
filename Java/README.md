# 📚 Java 语言基础
## 前言
- Java语言基础是整个Java Web技术体系的根基，也是学习Spring、SpringBoot、MyBatis、Redis、MQ等框架的前置核心条件。像IOC、AOP、反射、动态代理、线程池这些框架底层机制，都建立在Java基础之上。
- 学习建议：不要单纯背诵面试八股文，重点理解底层原理，能够手写代码、讲清楚“为什么”，才算是真正掌握。扎实的Java基础，会让后续框架学习事半功倍。
### 以下是从本文中整理出的八股复习对照清单：
> 🎯【⭐⭐⭐⭐⭐ 必须掌握】
1. `==` 和 `equals()` 的区别？
2. 为什么重写 `equals()` 必须重写 `hashCode()`？
3. Java 是值传递还是引用传递？
4. String 为什么不可变？
5. StringBuilder 和 StringBuffer 的区别？
6. Integer 缓存机制？
7. ArrayList 和 LinkedList 的区别？
8. ArrayList 扩容机制？
9. HashMap 底层数据结构？
10. HashMap `put()` 流程？
11. HashMap 为什么使用红黑树？
12. HashMap 为什么数组长度通常是 2 的幂？
13. HashMap 为什么需要 Hash 扰动？
14. HashMap 扩容机制？
15. HashSet 为什么不能存储重复元素？
16. HashMap 和 ConcurrentHashMap 的区别？
17. ConcurrentHashMap 如何保证线程安全？
18. CopyOnWriteArrayList原理和适用场景
19. JVM 内存区域有哪些？
20. 堆和栈有什么区别？栈帧包含什么？
21. 类加载过程？
22. 双亲委派模型是什么？什么时候打破？
23. GC Roots 是什么？四种引用？
24. 垃圾回收算法有哪些？
25. 新生代和老年代是什么？G1、ZGC特点
26. synchronized 原理？可重入？
27. synchronized 和 ReentrantLock 区别？读写锁？
28. volatile 能保证什么？为什么不能保证原子性？
29. CAS 是什么？ABA 问题是什么？原子类？
30. AQS 是什么？
31. ThreadLocal原理，为什么要remove()
32. 线程池七大参数和工作流程？
33. 线程池拒绝策略有哪些？
34. execute和submit区别，shutdown/shutdownNow
35. 为什么不建议直接使用 Executors 创建线程池？
36. CountDownLatch、CyclicBarrier、Semaphore区别

> 🎯【⭐⭐⭐⭐ 建议掌握】
37. 泛型擦除是什么？
38. `<? extends T>` 和 `<? super T>` 区别？
39. PECS 原则？
40. BIO 和 NIO区别，Reactor、零拷贝？
41. `sleep()`、`wait()`、`join()` 区别？
42. 反射是什么？
43. 注解和元注解是什么？
44. Lambda 和函数式接口？
45. Stream中间操作、终止操作；map/flatMap
46. CompletableFuture常用API，join与get
47. Optional 是什么？
48. try‑with‑resources 是什么？
49. JDK动态代理与CGLIB区别
50. 单例模式，DCL为什么要volatile
51. Java21虚拟线程特点

> 📌 Java后端高频面试陷阱汇总
1. Java只有值传递，不存在引用传递；引用类型传递的是引用副本。
2. volatile不能保证i++原子性，因为是读‑改‑写多步操作。
3. LinkedList索引增删不一定比ArrayList快，索引定位消耗O(n)。
4. ConcurrentHashMap多个独立操作组合不天然原子，要用putIfAbsent、compute。
5. finally不是100%执行，System.exit会终止JVM，跳过finally。
6. 不能绝对说switch性能一定优于if‑else，JIT会做优化。
7. 不能说基本类型全部在栈上，JIT逃逸分析会改变分配位置。
8. parallelStream并行流不一定变快，小数据量调度开销更大。

**备战面试建议对照这个常见八股问题清单来复习，复习效率事半功倍**
**接下来是正文部分：**

## 一、Java 基础语法
### 1. 标识符的定义规则
标识符就是我们给类、变量、方法、常量起的名字，它的组成、命名都有固定约束：
1. 允许使用的字符集合：大小写英文字母(A‑Z、a‑z)、数字(0‑9)、下划线`_`、美元符号`$`；
2. 不能以数字作为开头；
3. Java严格区分大小写；
4. 禁止使用Java关键字、保留字作为标识符；
5. 工程开发遵循驼峰命名规范：类名使用大驼峰（首字母大写）；变量、普通方法使用小驼峰（首字母小写）；常量全部大写，多个单词使用下划线分隔。
### 2. 基本数据类型四类八种
Java当中一共提供8种基本数据类型，保存的是真实的值，存储在栈内存：
- 整数类型：`byte`（1字节）、`short`（2字节）、`int`（4字节）、`long`（8字节）
- 浮点类型：`float`（4字节，单精度浮点数）、`double`（8字节，双精度浮点数）
- 字符类型：`char`（2字节，存储单个字符）
- 布尔类型：`boolean`，取值只有`true`、`false`。
> 注意：基本类型没有对象的概念，不存在引用地址。
### 3. 复合（引用）数据类型
复合数据类型也叫引用数据类型，包含类Class、接口Interface、数组Array、枚举Enum。
引用类型变量栈上保存的不是实际数据，而是堆内存当中对象的内存地址；通过这个地址，才能定位到堆里面真实的对象实体。
### 4. 程序流控制语句
1. **顺序结构**：代码从上到下逐行依次执行，是默认执行逻辑；
2. **分支结构**：`if‑else`多分支判断、`switch`匹配分支；
3. **循环结构**：`for`循环、`while`循环、`do‑while`循环；
4. **异常处理结构**：`try‑catch‑finally`，后面异常章节详细展开。
### 5. switch语句和if‑else语句的区别，能不能互换使用
> 🎯【面试题】switch和if‑else的区别，二者是否可以互相替换？
> 参考答案：
> 两者在逻辑上可以互相实现，所有switch能完成的逻辑都可以改写为if‑else。但是两者适用场景有区别。
> if‑else适合区间判断、多条件复合判断；switch适合固定常量值匹配，比如枚举、整数、字符串。
> switch代码可读性更高，底层是跳转表实现，固定值匹配场景执行效率会优于if‑else。但是switch有局限，不能直接做大于小于这类区间判断。
> 补充：性能不能绝对化，编译器、JIT会做优化，实际性能要结合运行环境。
### 6. 增强型for循环
增强for循环也叫for‑each循环，专门用来遍历数组、实现Iterable接口的集合（List、Set等），语法简洁，底层基于迭代器实现，隐藏迭代器的细节。
> 注意：增强for循环只适合读取遍历，遍历过程中不能做集合元素的删除操作，否则会抛出并发修改异常。
```java
public class ScoreAnalyzer {
    public static void main(String[] args) {
        int[] scores = {90, 85, 95, 78, 88};
        int sum = 0;
        int max = scores[0];
        // 增强型 for 循环：累加求和并找出最高分
        for (int score : scores) {
            sum += score;
            if (score > max) max = score;
        }
        double avg = (double) sum / scores.length;
        System.out.println("平均分: " + avg);   // 平均分: 87.2
        System.out.println("最高分: " + max);   // 最高分: 95
    }
}
```
### 7. String、StringBuilder 与 StringBuffer
> 🎯【面试题】String、StringBuilder、StringBuffer三者区别？
> 参考答案：
> 1. String是字符串常量，底层char数组（Java9改为byte数组）被final修饰，对象本身不可变。做字符串拼接操作的时候，不会修改原对象，会不断生成新的String对象，大量拼接场景效率很低；
> 2. StringBuilder是可变字符串，底层数组可以扩容修改，不会频繁创建新对象，性能好；但是方法没有加锁，**非线程安全，只适合单线程场景**；
> 3. StringBuffer同样可变，几乎所有核心方法都加上`synchronized`同步锁，是线程安全的；多线程字符串拼接可以使用，但是锁会带来性能损耗，速度比StringBuilder慢。
> 开发建议：绝大多数业务单线程优先使用StringBuilder；只有多线程并发拼接字符串才选用StringBuffer。
```java
public class TestString {
    public static void main(String[] args) {
        String s = "Hello";
        s = s + " World"; // 产生新对象，原"Hello"被丢弃
        StringBuilder sb = new StringBuilder("Hello");
        sb.append(" World"); // 在原有对象上修改，效率高
        StringBuffer sbf = new StringBuffer("Hello");
        sbf.append(" World"); // 线程安全版
        System.out.println(s);
        System.out.println(sb.toString());
        System.out.println(sbf.toString());
    }
}
```
### 8. `==`、`equals()` 与 `hashCode()`
> 🎯【面试题】讲一下 ==、equals、hashCode 的区别和联系？
> 参考答案：
> 1. `==`是运算符：如果比较的是基本数据类型，直接对比两边存储的值是否相等；如果比较引用类型，比较栈中保存的对象地址，也就是判断两个引用是不是指向堆里面同一个对象。
> 2. `equals()`是Object当中定义的实例方法，Object原生的equals方法内部就是直接使用`==`做判断。但是很多业务类、JDK类（例如String）会重写equals方法，不再比较地址，而是比较对象内部的业务内容是否相等。
> 3. `hashCode()`同样来自Object，返回对象的哈希int值，主要服务于HashMap、HashSet这类哈希集合。
> 契约规则：**如果两个对象调用equals返回true，那么两个对象的hashCode返回值必须相等；但是hashCode相等，equals不一定返回true（哈希碰撞）。**
> 基于这个契约，开发中重写equals方法的时候，必须同步重写hashCode方法，否则放到哈希集合当中会出现逻辑错误。
### 9. Java 是值传递还是引用传递？
> 🎯【面试题】Java到底是值传递还是引用传递？
> 参考答案：Java当中只有值传递，不存在引用传递。
> 当传入基本类型的时候，方法拿到的是原始变量值的拷贝，方法内部修改参数，不会影响外面原始变量；
> 传入引用类型的时候，传递的是引用地址的副本，不是对象本身。方法拿到副本地址之后，可以通过这个地址修改堆里面对象的属性；但是如果在方法内部直接给参数重新new一个对象，只是修改副本引用，外部原始引用不会发生任何变化。
### 10. 基本类型与包装类
8种基本类型都有对应的包装类，包装类是引用类型对象。
- 基本类型：`byte`、`short`、`int`、`long`、`float`、`double`、`char`、`boolean`
- 对应包装类：`Byte`、`Short`、`Integer`、`Long`、`Float`、`Double`、`Character`、`Boolean`

自动装箱：编译器自动把基本类型转为包装对象；
自动拆箱：编译器自动把包装对象取出里面的基本数值。
> 注意：包装类对象如果是null，执行自动拆箱操作，会直接抛出`NullPointerException`空指针异常。
### 11. `Integer` 缓存
> 🎯【面试题】Integer缓存机制，为什么包装类比较不能直接用==？
> 参考答案：Integer内部存在静态缓存数组，默认缓存‑128 ~ 127这个区间的Integer对象。当我们使用自动装箱方式获取这个范围的数字，会直接复用缓存里面已有的对象，所以`==`比较地址会得到true。
> 超过‑128~127区间，就会new全新Integer对象，这时候`==`对比地址就会返回false。
> 所以开发中，包装类数值判断相等，不要使用`==`，统一调用equals方法。
```java
Integer a = 127;
Integer b = 127;
System.out.println(a == b); // true
Integer c = 128;
Integer d = 128;
System.out.println(c == d); // 通常为 false
```
### 12. `final`、`finally`、`finalize`
> 🎯【面试题】final、finally、finalize三者的区别？
> 参考答案：
> 1. final是修饰关键字，可以修饰类、方法、变量。final修饰类代表类不能被继承；修饰方法代表该方法不能够被子类重写；修饰变量代表变量只能赋值一次。如果final修饰引用类型，引用地址不能修改，但是对象里面的属性值是可以修改的。
> 2. finally是异常处理的代码块，紧跟try‑catch，无论是否发生异常（除JVM直接退出），代码块都会执行，主要用来做资源关闭释放。
> 3. finalize()是Object里面的一个protected方法，对象被垃圾回收之前会尝试调用。这是JDK遗留历史机制，不建议业务使用，无法保证执行时机，不能用来做资源释放。

## 二、面向对象基础
### 1. 封装、继承、多态定义
面向对象三大特性：封装、继承、多态。
1. **封装**：把对象内部属性、实现细节隐藏起来，对外只暴露公开访问接口。一般使用private修饰成员变量，对外提供getter、setter访问。好处：保护内部数据安全，隔离实现细节，提高代码复用。
2. **继承**：子类去复用父类的属性与方法，体现is‑a的关系。继承是多态的前提，提高代码复用，Java是单继承。
3. **多态**：同一个方法，作用在不同子类对象上，表现出不同执行逻辑。核心写法就是父类引用指向子类对象。
### 2. final关键字
见上文final部分。
### 3. 方法重载是什么？规则？
> 🎯【面试题】什么是方法重载？重载有哪些规则？
> 参考答案：方法重载发生在**同一个类里面**，可以定义多个名字完全一样的方法，依靠参数列表区分。
> 规则总结：两同一不同。方法名相同；所在类相同；参数列表不同（参数数量、参数类型、参数顺序不一样）。
> ⚠️重点：**方法返回值、访问修饰符，不参与重载判断。仅仅返回值不一样，不能构成重载。**
```java
public class Calculator {
    // 重载1：两个整数相加
    public int add(int a, int b) {
        return a + b;
    }
    // 重载2：三个整数相加（参数个数不同）
    public int add(int a, int b, int c) {
        return a + b + c;
    }
    // 重载3：double类型相加（参数类型不同）
    public double add(double a, double b) {
        return a + b;
    }
}
```
### 4. 方法重写是什么？规则？
> 🎯【面试题】什么是方法重写？重写有什么语法约束？
> 参考答案：重写发生在父子继承关系中。子类定义和父类方法签名完全一致的方法；当子类对象调用该方法，执行子类重写后的业务逻辑。
> 约束：
> 1. 方法名、参数列表必须完全一致；
> 2. 返回值要求兼容，Java支持协变返回；
> 3. 子类重写方法访问权限不能比父类更小；
> 4. 重写方法不能抛出比父类更宽泛的受检异常；
> 补充：final修饰的方法不能重写；private私有方法、static静态方法不存在真正的重写。开发建议使用`@Override`注解，编译器自动校验重写合法性。
```java
class Animal {
    public void sound() {
        System.out.println("动物发出叫声");
    }
}
class Dog extends Animal {
    @Override // 注解，用于检查是否成功重写
    public void sound() {
        System.out.println("汪汪汪！");
    }
}
```
### 5. 构造方法
构造方法是类当中特殊的方法：方法名和类名完全一致，**没有返回值，连void都不能写**。
new创建对象的时候自动调用，作用是给对象成员变量做初始化。
如果类没有手写任何构造，编译器自动生成无参构造；一旦手写任意一个有参构造，默认无参构造就消失。业务开发建议手动写出无参构造，很多框架反射创建对象需要无参构造。
```java
public class Person {
    String name;
    int age;
    // 无参构造方法
    public Person() {
        this.name = "未知";
    }
    // 有参构造方法
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```
### 6. 访问控制四种修饰符，作用范围
Java四种访问权限，控制类、成员变量、方法对外可见范围：
| 修饰符 | 当前类 (Class) | 同一包内 (Package) | 子类 (Subclass) | 其他包 (World) |
| --- | --- | --- | --- | --- |
| **public** | √ | √ | √ | √ |
| **protected** | √ | √ | √ | × |
| **default(不写修饰符)** | √ | √ | × | × |
| **private** | √ | × | × | × |
### 7. 类多重继承如何实现
> 🎯【面试题】Java类为什么不能多继承，如何实现类似多继承的效果？
> 参考答案：Java类只支持单继承，一个类只能有一个直接父类，避免多继承带来的菱形继承冲突问题。
> 如果想要实现多继承的能力，可以通过**实现多个接口**完成。一个类可以同时implements多个接口，获取多个接口定义的能力。
```java
interface Flyable {
    void fly();
}
interface Swimmable {
    void swim();
}
// 类实现多个接口，实现类似多重继承的功能
class Duck implements Flyable, Swimmable {
    @Override
    public void fly() { System.out.println("鸭子在飞"); }
    @Override
    public void swim() { System.out.println("鸭子在游泳"); }
}
```

## 三、面向对象高级特性与多态
### 1. 编译时多态？运行时多态？
> 🎯【面试题】编译期多态和运行期多态分别是什么？
> 参考答案：
> 编译期多态也叫静态多态，对应的就是**方法重载**。编译阶段编译器根据传入实参，直接确定调用哪一个重载方法。
> 运行时多态，对应**方法重写**。需要满足三个条件：继承关系、子类重写父类方法、父类引用指向子类对象。编译期引用看父类，运行时JVM识别真实对象类型，动态绑定执行子类重写后的逻辑。
```java
public class TestPolymorphism {
    public static void main(String[] args) {
        // 运行时多态：父类引用指向子类对象
        Animal myDog = new Dog();
        myDog.sound(); // 运行期动态绑定，输出 "汪汪汪！"
    }
}
```
### 2. instanceof 关键字
instanceof用来做类型判断，判断引用指向的对象，是不是某个类、子类、实现类的实例，返回布尔值。
向下强制转型之前，一般先用instanceof做判断，避免抛出类型转换异常。
```java
Animal animal = new Dog();
if (animal instanceof Dog) {
    Dog dog = (Dog) animal; // 向下转型安全
    System.out.println("确实是狗");
}
```
### 3. 类变量（静态变量）与类方法（静态方法）
被static修饰的变量、方法，属于类本身，不属于实例对象。
特点：
1. 类加载阶段就完成初始化，优先于对象创建；
2. 所有该类的对象共享同一份静态成员；
3. 推荐使用`类名.静态变量`、`类名.静态方法()`直接访问，不建议通过对象访问；
4. **静态方法不能直接访问普通成员变量、普通实例方法**；因为实例成员依赖对象，静态执行的时候不一定存在对象。
```java
// 配置类：存放全局静态配置
class AppConfig {
    public static String appName = "学生管理系统";
    public static int maxUsers = 100;
    public static void printConfig() {
        System.out.println("应用: " + appName);
        System.out.println("最大在线人数: " + maxUsers);
    }
}
// 业务类：在不同类中直接通过类名调用静态成员
public class UserService {
    public static void main(String[] args) {
        // 不创建对象，直接通过类名访问静态变量
        System.out.println("当前系统: " + AppConfig.appName);
        // 直接通过类名调用静态方法
        AppConfig.printConfig();
        // 修改静态变量（所有类看到的值都会变）
        AppConfig.maxUsers = 200;
        System.out.println("扩容后最大在线人数: " + AppConfig.maxUsers);
    }
}
/* 输出：
当前系统: 学生管理系统
应用: 学生管理系统
最大在线人数: 100
扩容后最大在线人数: 200
*/
```
### 4. 接口的实现和使用
interface定义接口，接口代表一种能力契约。
1. 接口里面定义的变量默认`public static final`，属于常量，必须赋值；
2. Java8之前接口全部是`public abstract`抽象方法；Java8新增default默认方法、static静态方法，可以写方法体；
3. 普通类实现接口使用implements关键字，必须实现接口里面全部抽象方法；
```java
interface USB {
    void connect();
}
class Mouse implements USB {
    @Override
    public void connect() {
        System.out.println("鼠标已连接，可以移动光标。");
    }
}
```
### 5. 抽象类 (Abstract Class)
> 🎯【面试题】抽象类和接口的区别？
> 参考答案：
> abstract修饰得到抽象类，不能直接new实例，只能被子类继承。
> 抽象类既可以包含抽象方法，也可以包含普通实现方法；可以定义成员变量，拥有构造方法，构造方法用于子类super调用。子类继承抽象类，必须实现全部抽象方法，除非子类本身也是抽象类。
> 语义区别：抽象类表达is‑a，是模板；接口表达can‑do，代表具备某种能力。
> 继承限制：一个类只能继承一个抽象类；但是可以同时实现多个接口。
```java
abstract class Animal {
    protected String name;
    public Animal(String name) {
        this.name = name;
    }
    // 抽象方法：子类必须实现
    public abstract void makeSound();
    // 非抽象方法：子类可直接使用
    public void sleep() {
        System.out.println(name + " 正在睡觉");
    }
}
class Dog extends Animal {
    public Dog(String name) {
        super(name);
    }
    @Override
    public void makeSound() {
        System.out.println(name + ": 汪汪汪！");
    }
}
public class TestAbstract {
    public static void main(String[] args) {
        Animal dog = new Dog("旺财"); // 向上转型
        dog.makeSound(); // 输出：旺财: 汪汪汪！
        dog.sleep();     // 输出：旺财 正在睡觉
    }
}
```
### 6. 包的定义和使用要点
package用来管理类，解决不同包下类重名冲突。
package语句必须写在源文件第一行有效代码。
想要使用别的包中的类，需要import导入；同一个包下类不需要导入；`java.lang`包下所有类系统自动导入，不需要手写import。
```java
package com.example.util; // 包定义
public class MyUtil {
    public void printMsg() {
        System.out.println("工具类方法");
    }
}
// 在另一个类中使用：
// import com.example.util.MyUtil;
// MyUtil util = new MyUtil();
```

## 四、泛型、集合与异常处理
### 1. 泛型定义使用
泛型叫做参数化类型，把类、方法里面的数据类型，推迟到创建对象、调用方法的时候再指定。
核心价值：编译期做类型校验，避免运行时强制类型转换抛出`ClassCastException`类型转换异常。
```java
// 定义泛型类
public class Box<T> {
    private T item;
    public void setItem(T item) { this.item = item; }
    public T getItem() { return item; }
}
// 使用泛型类（限定类型为String）
Box<String> stringBox = new Box<>();
stringBox.setItem("Java泛型");
```
#### 1.1 泛型通配符
> 🎯【面试题】讲一下泛型通配符?、? extends T、? super T，什么是PECS原则？
> 参考答案：
> 1. `<?>`无边界通配符，代表任意未知类型；
> 2. `<? extends T>`上界通配符，代表T或者T的子类，适合读取数据，也就是生产者；
> 3. `<? super T>`下界通配符，代表T或者T的父类，适合写入数据，也就是消费者。
> PECS原则全称Producer Extends，Consumer Super。生产者（向外拿数据）用extends；消费者（往里存数据）用super。
> 泛型只存在编译期，运行时会发生泛型擦除，字节码当中会把泛型信息抹去，替换为上界Object。

### 2. 集合类 (Java Collections Framework)
Java集合框架分为两大分支：Collection单列集合（存一个个对象）；Map双列集合（存储key‑value键值对）。
```text
Collection
├── List
│   ├── ArrayList
│   └── LinkedList
├── Set
│   ├── HashSet
│   ├── LinkedHashSet
│   └── TreeSet
└── Queue
    ├── PriorityQueue
    └── Deque
        └── ArrayDeque
Map
├── HashMap
├── LinkedHashMap
├── TreeMap
├── Hashtable
└── ConcurrentHashMap
```
#### 2.1 Collection 接口（单列集合）
##### List（有序、元素可重复）
**ArrayList**
底层是动态Object数组。
随机访问get(index)时间复杂度O(1)；中间插入删除需要移动数组元素，时间复杂度O(n)；尾部追加效率高，数组满了触发扩容。
JDK8无参构造不会直接创建长度10数组，第一次add添加元素的时候初始化容量10；每次扩容为原容量1.5倍。线程不安全。适合查询多、中间增删少的业务场景。

**LinkedList**
底层双向链表，每个节点保存prev前驱、next后继指针。
按索引查询需要遍历链表，get(index)性能O(n)；如果已经定位到目标节点，插入删除只修改指针，O(1)。但是通过索引做增删，需要先遍历定位节点，整体不一定快。
同时实现List和Deque接口，可以作为普通列表、队列、双端队列使用。线程不安全。适合频繁首尾增删的场景。

> 🎯【面试题】ArrayList和LinkedList区别？
> 参考答案：
> ArrayList底层动态数组，随机读取速度快，内存连续，CPU缓存友好；中间插入删除需要移动大量元素，开销大。扩容会创建新数组拷贝。
> LinkedList底层双向链表，内存分散，随机读取慢；如果已经定位节点，增删改指针很快；但是根据索引做增删，要遍历找节点，性能不一定优于ArrayList。内存开销更大，每个节点要存前后指针。
> 总结：查询多优先ArrayList；频繁首尾增删可以用LinkedList；不要笼统说LinkedList增删一定快。

| 对比    | ArrayList | LinkedList |
| ----- | --------- | ---------- |
| 底层    | 动态数组      | 双向链表       |
| 随机访问  | 快 O(1)    | 慢 O(n)     |
| 中间插入  | O(n)      | 定位后 O(1)   |
| 中间删除  | O(n)      | 定位后 O(1)   |
| 内存占用  | 较低        | 较高         |
| 缓存友好性 | 好         | 差          |
| 常见使用  | 查询多       | 需要频繁首尾操作   |

##### Set（元素不可重复）
- HashSet：底层封装HashMap，存入的元素作为map的key。依靠hashCode()+equals()判断元素是否重复。无序，允许一个null元素。线程不安全。
- LinkedHashSet：底层基于LinkedHashMap，保证元素插入顺序，不允许重复。
- TreeSet：底层红黑树，元素自动排序，支持自定义比较器；增删查询O(log n)。

#### 2.2 Map 接口（双列集合，键值对 K‑V）
##### HashMap
> 🎯【面试题】HashMap底层结构(JDK8)？put流程？默认参数？为什么容量是2的幂？hash扰动？树化条件？扩容机制？为什么key要重写equals和hashCode？
> 参考答案：
> JDK8底层结构：数组 + 链表 + 红黑树。
> 默认初始容量16，负载因子0.75；扩容阈值=容量*负载因子，超过阈值触发扩容，每次扩容扩大2倍。0.75是空间利用率和哈希冲突概率之间的折中。
> 数组下标计算使用`(n‑1) & hash`，只有数组长度是2的n次幂，位运算才能等价取模，运算速度比%更快。
> hash扰动`h ^ (h>>>16)`，把对象hashCode高位混合到低位，减少哈希冲突。
> put流程：先计算key扰动后的hash值，算出数组下标；判断桶位置有没有元素；桶为空直接存放；桶不为空，对比hash、equals判断key是否完全相同，如果key一样直接覆盖value；不相同就挂链表；链表长度达到8，并且数组容量>=64，链表转为红黑树；数组小于64优先扩容而不是树化。
> 扩容的时候旧数组元素要么保留原下标，要么偏移oldCap，不用重新完整计算hash。
> 如果两个对象equals相等，hashCode必须相等，否则存HashMap的时候会分到不同桶，出现key重复存储的bug。

> 🎯【面试题】HashMap、LinkedHashMap、TreeMap区别？
> 参考答案：
> HashMap无序；LinkedHashMap底层HashMap加上双向链表，可以保存插入顺序，也可以开启访问顺序，用来实现LRU缓存；TreeMap底层红黑树，key自动排序，可以自定义Comparator。

##### LinkedHashMap
底层HashMap+双向链表，可以维护插入顺序；构造方法第三个参数传true，开启访问顺序，最近访问的元素放到链表末尾，可实现LRU缓存。

##### TreeMap
底层红黑树，key有序，增删查O(log n)。key可以实现Comparable，或者传入外部比较器。

##### ConcurrentHashMap
> 🎯【面试题】ConcurrentHashMap怎么保证线程安全？为什么不允许null key、null value？和HashMap对比？
> 参考答案：
> JDK7使用Segment分段锁；JDK8废弃Segment，底层数组链表红黑树；采用CAS + synchronized锁单个桶节点，锁粒度变细，并发性能更好。
> put核心流程：计算hash；数组未初始化则CAS初始化；目标桶为空直接CAS插入；桶不为空，synchronized锁住桶头节点；遍历链表或者红黑树做更新或新增，满足条件执行树化。
> get读取依靠内存可见性，一般不需要加锁。
> 不允许存放null key、null value。因为多线程场景get返回null，分不清是key不存在，还是value本身存的null，无法区分语义。
> 注意：`containsKey + put`这种组合不具备原子性，业务复合操作要使用putIfAbsent、compute、merge。
> HashMap线程不安全，可以存一个null key；ConcurrentHashMap线程安全，不能存null键值。

| 对比         | HashMap      | ConcurrentHashMap  |
| ---------- | ------------ | ------------------ |
| 线程安全       | ❌            | ✅                  |
| null Key   | ✅            | ❌                  |
| null Value | ✅            | ❌                  |
| Java 8结构   | 数组+链表+红黑树    | 数组+链表+红黑树          |
| 并发控制       | 无            | CAS + synchronized |
| 使用场景       | 单线程/外部保证线程安全 | 多线程并发              |

##### CopyOnWriteArrayList
> 🎯【面试题】CopyOnWriteArrayList原理以及适用场景？
> 参考答案：核心是写时复制思想。读直接读取当前数组；写操作会复制一份全新数组，修改完成后替换数组引用。读操作不加锁，写操作加锁。读性能优秀，但写操作开销大，占用额外内存。适合**读多写少**场景，比如配置列表、监听器集合、白名单；不适合高频写入业务。

#### 2.3 常用核心方法
**Collection通用方法**
add、addAll添加元素；remove、removeAll、retainAll、clear做删除；size、isEmpty、contains做判断；toArray转数组；iterator获取迭代器。

**Map特有方法**
put、putIfAbsent存入键值对；get、getOrDefault获取值；keySet获取全部key集合；values获取全部value集合；entrySet拿到全部键值对对象。
> 开发遍历Map优先用entrySet，直接拿到key和value；不要循环keySet再get(key)，会多一次查询开销。
```java
//推荐写法
for (Map.Entry<String, String> entry : map.entrySet()) {
    System.out.println(entry.getKey() + ":" + entry.getValue());
}
```

#### 集合总览对比表
| 集合                  | 底层结构          | 是否有序    | 是否重复    | 线程安全 |
| ------------------- | ------------- | ------- | ------- | ---- |
| `ArrayList`         | 动态数组          | ✅       | ✅       | ❌    |
| `LinkedList`        | 双向链表          | ✅       | ✅       | ❌    |
| `HashSet`           | HashMap       | ❌       | ❌       | ❌    |
| `LinkedHashSet`     | LinkedHashMap | 插入顺序    | ❌       | ❌    |
| `TreeSet`           | 红黑树           | 排序      | ❌       | ❌    |
| `HashMap`           | 数组+链表+红黑树     | ❌       | Key 不重复 | ❌    |
| `LinkedHashMap`     | HashMap+链表    | 插入/访问顺序 | Key 不重复 | ❌    |
| `TreeMap`           | 红黑树           | Key 排序  | Key 不重复 | ❌    |
| `ConcurrentHashMap` | 数组+链表+红黑树     | ❌       | Key 不重复 | ✅    |
| `CopyOnWriteArrayList` | 数组（写时复制） | ✅ | ✅ | ✅ |

#### 代码示例
```java
import java.util.*;
public class TestCollection {
    public static void main(String[] args) {
        // 1. List 操作
        List<String> list = new ArrayList<>();
        list.add("张三");
        list.add("李四");
        System.out.println("集合大小: " + list.size()); // 2
        System.out.println("索引1元素: " + list.get(1)); // 李四
        list.remove("张三");
        // 2. Set 操作
        Set<String> set = new HashSet<>();
        set.add("Java");
        set.add("Java");
        set.add("MySQL");
        System.out.println(set); // Java 只保留一个
        // 3. Map 操作
        Map<String, String> map = new HashMap<>();
        map.put("001", "Java");
        map.put("002", "MySQL");
        System.out.println("查询值: " + map.get("001")); // Java
        // 4. 遍历 Map
        for (Map.Entry<String, String> entry : map.entrySet()) {
            System.out.println(entry.getKey() + ":" + entry.getValue());
        }
    }
}
```

### 3. 异常处理两种方法
Java异常顶层父类Throwable，分为Error和Exception。
Error是系统级严重错误，程序无法处理；Exception是业务程序可以捕获处理的异常。
Exception分为两类：
1. **运行时异常RuntimeException（非受检异常）**：编译器不强制捕获，代码运行才会抛出；
2. **编译时受检异常CheckedException**：编译器强制要求处理，要么try‑catch捕获，要么方法上throws声明抛出。

处理异常两种方式：
1. try‑catch‑finally：直接捕获异常，在本地处理；
2. throw手动抛出异常对象；throws写在方法签名，声明这个方法可能向外抛出异常，交给调用方处理。

```java
public class TestException1 {
    public static void main(String[] args) {
        try {
            int result = 10 / 0; // 模拟算术异常
        } catch (ArithmeticException e) {
            System.out.println("捕获到算术异常: / by zero");
            e.printStackTrace(); // 打印异常栈信息
        } finally {
            System.out.println("finally块：无论是否发生异常都会执行，常用于资源释放");
        }
    }
}
```

```java
public class TestException2 {
    // 使用 throws 在方法签名处声明抛出异常
    public static void checkAge(int age) throws IllegalArgumentException {
        if (age < 0) {
            // 使用 throw 主动抛出一个异常对象
            throw new IllegalArgumentException("年龄不能为负数");
        }
        System.out.println("年龄合法");
    }
    public static void main(String[] args) {
        try {
            checkAge(-1);
        } catch (IllegalArgumentException e) {
            System.out.println("处理异常: " + e.getMessage());
        }
    }
}
```

> 🎯【面试题】throw和throws区别？try‑with‑resources是什么？finally一定执行吗？
> 参考答案：
> throw是方法内部手动抛出一个异常实例对象；throws写在方法签名后面，声明该方法有可能抛出哪些异常，提醒调用者处理。
> try‑with‑resources是JDK7提供语法，实现AutoCloseable接口的资源可以写在try()括号里面，代码执行结束自动关闭资源，不用手写finally关闭流、连接。
> finally大部分场景会执行；如果JVM直接退出System.exit(0)，finally不会执行。

## 五、输入输出文件流（I/O）
### 1. 基本分类
按照流向区分：输入流负责读数据；输出流负责写数据。
按照处理单位区分：
1. **字节流**：InputStream、OutputStream作为顶层抽象，处理二进制字节，图片、视频、文件都可以操作；
2. **字符流**：Reader、Writer顶层抽象，专门处理文本字符，内部自带编码解码。

### 2. 文件流与管道流等概念
- File文件流：FileInputStream/FileOutputStream、FileReader/FileWriter，直接对接磁盘文件；
- Buffered缓冲流：包装原始流，内部开辟缓冲区，减少磁盘IO次数，提升读写性能；
- Data数据流：可以直接读写Java基础数据类型；
- Piped管道流：线程之间数据通信，一个线程写管道输出流，另一个线程读取管道输入流。

### 3. BIO 与 NIO
> 🎯【面试题】BIO和NIO区别？
> 参考答案：
> BIO传统阻塞IO，读写的时候线程阻塞等待IO完成，一个连接对应一个线程，并发能力弱。
> NIO是非阻塞IO，核心三大组件Buffer缓冲区、Channel通道、Selector选择器。一个Selector线程可以监控大量Channel的IO事件，实现单线程处理大量连接，提升并发能力。

> 🎯【面试题】简单说下Reactor模型、零拷贝？
> 参考答案：Reactor是高性能IO设计思想，客户端事件交给Selector监听，事件到来之后分发到对应的Handler去处理，Netty底层就是基于Reactor模式。
> 零拷贝目标是减少内核态‑用户态之间数据拷贝次数、减少上下文切换；Java中`FileChannel.transferTo`可以使用操作系统零拷贝能力，是否真正零拷贝依赖操作系统版本。

### I/O流操作代码示例
```java
import java.io.*;
public class StudentScoreIO {
    public static void main(String[] args) {
        String fileName = "scores.txt";
        // 1. 使用 PrintWriter 写入学生成绩（自动换行、格式化输出）
        try (PrintWriter pw = new PrintWriter(new FileWriter(fileName))) {
            pw.println("张三,85");
            pw.println("李四,90");
            pw.println("王五,78");
        } catch (IOException e) {
            e.printStackTrace();
        }
        // 2. 使用 BufferedReader 逐行读取并解析
        try (BufferedReader br = new BufferedReader(new FileReader(fileName))) {
            String line;
            int total = 0;
            int count = 0;
            System.out.println("=== 学生成绩清单 ===");
            while ((line = br.readLine()) != null) {
                String[] parts = line.split(",");
                String name = parts[0];
                int score = Integer.parseInt(parts[1]);
                total += score;
                count++;
                System.out.println(name + ": " + score + " 分");
            }
            double avg = (double) total / count;
            System.out.println("====================");
            System.out.printf("平均分: %.1f 分%n", avg);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
/* 输出结果：
=== 学生成绩清单 ===
张三: 85 分
李四: 90 分
王五: 78 分
====================
平均分: 84.3 分
*/
```

## 六、枚举与程序初始化顺序
### 1. 枚举类型的定义方式和使用
enum定义枚举，本质是继承`java.lang.Enum`的特殊类。适合定义固定有限集合，比如状态码、星期。枚举可以在switch中使用，也可以做单例。
```java
public class TestEnum {
    // 定义枚举
    public enum Day {
        MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
    }
    public static void main(String[] args) {
        Day today = Day.FRIDAY;
        // switch 中使用枚举
        switch (today) {
            case FRIDAY:
                System.out.println("周五了，准备放假！");
                break;
            default:
                System.out.println("搬砖中...");
                break;
        }
    }
}
```
### 2. 程序初始化顺序
> 🎯【面试题】创建子类对象的时候，父子类初始化执行顺序？
> 参考答案：
> 1. 父类静态变量、静态代码块（类加载执行，只执行一次）
> 2. 子类静态变量、静态代码块
> 3. 父类普通成员变量、非静态代码块
> 4. 父类构造方法
> 5. 子类普通成员变量、非静态代码块
> 6. 子类构造方法

## 七、多线程与并发编程
### 1. 线程与进程
进程：操作系统资源分配最小单位，程序一次运行实例，每个进程拥有独立内存空间。
线程：CPU调度执行最小单位。一个进程内部可以包含多个线程；同一个进程内线程共享堆、方法区；但是每个线程拥有独立虚拟机栈、程序计数器。
多线程优点：充分利用CPU，提升程序响应速度。
多线程带来问题：线程安全（多线程并发修改共享变量）、死锁。

> 🎯【面试题】synchronized和ReentrantLock区别？volatile作用？volatile为什么不能保证原子性？
> 参考答案：
> synchronized是隐式锁，自动加锁释放锁，可修饰方法、代码块；底层JVM实现。
> ReentrantLock是API层面显式锁，需要手动lock()、unlock()，支持公平锁、tryLock非阻塞抢锁、锁中断，功能更丰富。
> volatile修饰变量，保证多线程可见性，禁止指令重排序；但是不能保证原子性，像i++这种复合操作，volatile无法保证线程安全。

### 2 Java内存模型 JMM & happens‑before
> 🎯【面试题】什么是JMM，说说happens‑before规则？
> 参考答案：JMM是Java语言层面抽象的内存模型，不是JVM硬件内存。规定多线程读写共享变量的可见性、有序性。模型抽象出主内存存放共享变量，每个线程拥有自己的工作内存（变量副本），线程不能直接操作别的线程工作内存。
> happens‑before是一套规则，不用依靠同步也可以保证部分可见性：
> 1. 程序次序：同一个线程内前面操作happens‑before后面；
> 2. 监视器锁：解锁 happens‑before 后续加锁；
> 3. volatile写 happens‑before 后续读；
> 4. start() happens‑before 线程内操作；
> 5. 线程全部操作 happens‑before 其他线程感知线程终止；
> 6. 具备传递性。

### 3 synchronized深入
synchronized可以修饰实例方法（锁this对象）、静态方法（锁Class对象）、同步代码块（锁指定对象）。字节码依靠`monitorenter`、`monitorexit`，保证原子性、可见性、锁相关有序性。
> 🎯【面试题】synchronized为什么支持可重入？
> 参考答案：同一个线程拿到对象监视器锁之后，再次进入同步块不会阻塞；对象头会记录持有锁的线程、锁计数，进入计数+1，退出计数‑1，计数归零锁才释放。

### 4 volatile深入
volatile只能保证可见性、禁止指令重排序，**不能保证复合操作原子性**，例如count++分为读取、计算、写回三步，多线程依然会丢失更新。底层依靠内存屏障限制CPU、编译器重排序。

### 5 CAS、ABA、原子类
> 🎯【面试题】什么是CAS？ABA问题怎么解决？
> 参考答案：CAS即比较并交换，无锁乐观操作。内存值V、预期值A、新值B；只有V等于A，才把V更新为B，否则失败重试。Atomic系列原子类底层大量使用CAS。
> 缺点：自旋消耗CPU；只能保证单个变量；存在ABA问题。线程读到A，中间被改成B又改回A，CAS误认为没有修改。
> ABA解决方案：带上版本号，使用`AtomicStampedReference`。

常用原子类：`AtomicInteger`、`AtomicLong`、`AtomicReference`、`AtomicStampedReference`。注意原子类只能保证单个变量原子，多个变量业务一致性仍然需要锁。

### 6 AQS
> 🎯【面试题】简单介绍AQS？
> 参考答案：AQS是AbstractQueuedSynchronizer，并发包的底层同步框架。核心三要素：state同步状态、FIFO等待队列、CAS、LockSupport。分为独占模式（ReentrantLock）和共享模式（CountDownLatch、Semaphore）。获取资源失败的线程进入等待队列挂起。

### 7 ReentrantReadWriteLock
读写锁：读锁共享，多个线程可以同时读；写锁完全互斥。适合读多写少场景。

### 8 ThreadLocal
> 🎯【面试题】ThreadLocal原理，为什么必须调用remove()？
> 参考答案：ThreadLocal为每一个线程维护独立数据副本；数据存放在当前线程内部`ThreadLocalMap`，key是ThreadLocal的弱引用。
> 线程池场景线程会复用，如果不执行remove，旧的业务数据残留，会造成数据污染；同时存在内存泄漏风险。

### 7.4 线程池 ⭐⭐⭐⭐⭐
> 🎯【面试题】ThreadPoolExecutor七大参数，工作流程？四种拒绝策略？为什么不推荐Executors工厂？
> 参考答案：
> 七大参数：corePoolSize核心线程数、maximumPoolSize最大线程数、keepAliveTime非核心空闲存活时间、unit时间单位、workQueue阻塞队列、threadFactory线程工厂、handler拒绝策略。
> 执行流程：提交任务 → 小于核心线程数创建核心线程；核心线程满，任务进队列；队列满，没到最大线程数创建非核心线程；达到最大线程数执行拒绝策略。口诀：核心线程→队列→非核心线程→拒绝。
> 拒绝策略：
> - AbortPolicy（默认）抛出RejectedExecutionException；
> - CallerRunsPolicy：提交任务的线程自己运行任务，起到反压；
> - DiscardPolicy：直接丢弃任务无异常；
> - DiscardOldestPolicy：丢弃队列最老任务，重新提交。
> Executors工具类封装的线程池隐藏参数：FixedThreadPool、SingleThreadExecutor使用无界队列，任务堆积会OOM；CachedThreadPool最大线程无上限，可能创建大量线程耗尽资源。生产建议直接new ThreadPoolExecutor，明确配置全部参数。

> 🎯【面试题】execute()和submit()区别；shutdown与shutdownNow；线程池参数怎么设置？
> 参考答案：execute接收Runnable，无返回；submit返回Future对象，可以捕获异常获取返回结果。
> shutdown不再接收新任务，已提交任务全部执行完；shutdownNow尝试中断正在执行任务，返回未执行任务集合，中断不一定生效。
> CPU密集型线程数接近CPU核心数；IO密集型可以设置大于CPU核心数；不要死记公式，结合业务耗时、压测调优。

### 7.5 并发工具类
> 🎯【面试题】CountDownLatch、CyclicBarrier、Semaphore三者区别？
> 参考答案：
> CountDownLatch：一个或者多个线程等待其他N个线程完成，计数器只能递减，不能重置复用；
> CyclicBarrier：N个线程互相等待全部到达屏障点之后再继续执行，计数器可以循环复用；
> Semaphore：控制同时访问资源的线程许可数量，做限流。

## 八、JVM
### 1. JVM 运行时内存区域
> 🎯【面试题】JVM运行时数据区分为哪几块？哪些线程私有哪些共享？
> 参考答案：
> 线程私有：程序计数器、虚拟机栈、本地方法栈；
> 线程共享：堆、方法区。
> 程序计数器：记录当前线程执行字节码行号，唯一一个没有OOM的区域。
> 虚拟机栈：每个方法调用生成栈帧，保存局部变量、操作数栈、动态链接、返回地址；栈深度过大抛出StackOverflowError。
> 本地方法栈：给native本地方法服务。
> 堆：所有对象实例分配在这里，GC主要回收堆内存；内存耗尽抛出OOM。
> 方法区：存放类元信息、常量池，JDK8元空间实现，使用本地内存。

> 🎯【面试题】栈帧里面包含哪些内容？
> 参考答案：一次Java方法调用对应一个栈帧；栈帧包含局部变量表、操作数栈、动态链接、方法返回地址。

### 2. 堆与栈的区别
栈线程私有，保存栈帧、局部变量；堆线程共享，存放对象。栈溢出StackOverflowError；堆内存不足OutOfMemoryError。
> 补充：不要说“基本类型一定放在栈，对象一定放在堆”。局部变量基本类型存在栈帧；对象成员变量随对象在堆；JIT逃逸分析可以做对象栈上分配优化。

### 2.1 对象内存布局（HotSpot）
对象分为三部分：对象头（MarkWord标记字、Klass指针）、实例数据、对齐填充；数组对象额外保存数组长度。对象分配会涉及TLAB本地线程分配缓冲，提升分配性能。

### 3. 对象创建过程
new对象完整流程：类加载校验 → 分配内存空间 → 内存赋零值 → 设置对象头信息 → 执行构造方法 → 返回对象引用。

### 4. 类加载过程
> 🎯【面试题】Java类加载分为哪几步？双亲委派模型是什么？什么场景要打破双亲委派？
> 参考答案：加载、验证、准备、解析、初始化。
> 加载读取class字节码；验证校验字节码合法性；准备给静态变量分配内存赋默认零值；解析符号引用转为直接引用；初始化执行静态代码块，给静态变量赋值。
> 双亲委派：类加载收到请求，优先向上委托父加载器去加载；父加载器加载失败，自己再加载。好处：保护核心JDK类，防止篡改，避免类重复加载。
> 打破双亲委派场景：Tomcat容器、JDBC‑SPI、插件化框架，需要父加载器的代码使用子加载器实现类。

### 5. 类加载器
启动类加载器Bootstrap ClassLoader；平台类加载器Platform ClassLoader；应用程序类加载器Application ClassLoader。

### 7. 垃圾回收与GC Roots
> 🎯【面试题】什么是GC Roots，可达性分析？Java四种引用？
> 参考答案：JDK使用可达性分析判断对象存活。从GC Roots作为起点向下扫描引用链，如果对象到GC Roots没有任何引用链相连，判定为垃圾对象。
> GC Roots包含：虚拟机栈局部变量引用；静态变量引用；常量引用；JNI本地引用。
> 四种引用：
> 1. 强引用：普通new，只要强引用存在不会回收；
> 2. 软引用：内存不足的时候才回收；
> 3. 弱引用：只要发生GC就回收；
> 4. 虚引用：仅用于感知对象回收，必须配合ReferenceQueue。

### 8. 垃圾回收算法
标记清除：标记垃圾直接回收，产生内存碎片；
复制算法：把存活对象复制到新区域，适合新生代对象存活率低；
标记整理：标记存活对象，全部向一侧移动，消除碎片；
分代收集：把堆分为新生代老年代，不同区域使用不同回收算法。

### 9. 新生代与老年代
新生代存放刚创建对象，对象生命周期短，频繁MinorGC；对象熬过多次GC晋升到老年代。MajorGC老年代回收；FullGC整堆回收，开销很大。

### 10. G1 & ZGC
> 🎯【面试题】G1收集器特点？ZGC？
> 参考答案：G1把堆切分成很多大小相等Region，根据预期停顿时间，优先回收收益高的Region；有RememberedSet处理跨Region引用；适合大内存堆，追求可控停顿时间。
> ZGC是低延迟收集器，大量并发阶段，STW停顿极低，面向大堆低延迟业务。

### 11. 常见JVM异常
StackOverflowError栈溢出；OutOfMemoryError内存溢出；内存泄漏对象无用但是仍被引用，GC无法回收。

## 九、Java 8
1. Lambda表达式，简化函数式接口匿名内部类写法。
2. 函数式接口：只有一个抽象方法，@FunctionalInterface标记；常用Predicate、Function、Consumer、Supplier。
3. Stream流式操作：filter过滤、map转换、flatMap拆分、distinct去重、sorted排序、limit截断、collect收集、reduce聚合。
> 🎯【面试题】Stream中间操作和终止操作？map与flatMap区别？parallelStream注意点？
> 参考答案：中间操作filter/map/flatMap等属于惰性求值，不会立刻执行；必须调用终止操作（collect、forEach、count）才真正执行。map是一对一转换；flatMap把每个元素转成流再合并，用于一对多场景。parallelStream并行流不一定更快，小数据量任务线程调度开销可能大于计算收益。

> 🎯【面试题】CompletableFuture的常用方法，join和get区别？
> 参考答案：CompletableFuture实现Future+CompletionStage，编排多个异步任务。thenApply转换、thenAccept消费、thenRun无参执行；thenCompose处理依赖异步任务；thenCombine组合两个独立任务；allOf等待全部完成，anyOf任意一个完成。get抛出受检异常必须捕获；join不抛受检异常，异常包装为CompletionException。不指定线程池默认使用ForkJoinPool，阻塞IO业务建议自定义业务线程池隔离。

4. Optional容器类，用来包装可为null的值，减少空指针。
5. 方法引用`::`，简化lambda写法。

### Java9‑Java21新特性简要
- Java9：模块化、Stream增强
- Java10：var局部变量类型推断
- Java11：标准HttpClient
- Java14：switch表达式
- Java16：instanceof模式匹配
- Java17：record、密封类sealed class
- Java21：虚拟线程Virtual Thread，适合大量IO阻塞任务；虚拟线程不会加速CPU计算，只是降低阻塞任务线程成本。

## 十、反射与注解
### 1. 反射
> 🎯【面试题】什么是反射？获取Class对象有几种方式？
> 参考答案：反射就是运行时动态获取类的全部信息，并且动态创建对象、调用方法、修改属性。Spring IOC大量依赖反射。
> 获取Class三种方式：对象.getClass()；类名.class；Class.forName("全限定类名")。

### 2. 注解
注解是元数据，给类、方法、变量附加标记信息。内置注解@Override、@Deprecated、@SuppressWarnings；框架注解@Component、@Autowired、@Transactional。

### 3. 元注解
用来修饰注解的注解：
@Target：限定注解写在类/方法/字段等什么位置；
@Retention：设置注解保留到源码、class字节码、运行期；
@Documented：是否输出到javadoc文档；
@Inherited：子类是否继承父类的该注解。

### 4 代理模式基础
> 🎯【面试题】JDK动态代理与CGLIB代理区别？Spring AOP什么时候选择哪一种？
> 参考答案：JDK动态代理基于接口，只能代理实现接口的类；CGLIB继承目标类生成子类字节码实现代理，可以代理没有实现接口的类。Spring AOP：目标对象实现接口优先JDK代理；否则使用CGLIB。

### 5 常见设计模式简要
> 🎯【面试题】单例模式有哪些实现？双重检查锁为什么要volatile？
> 参考答案：饿汉式、懒汉式、双重检查锁DCL、静态内部类、枚举单例。双重检查锁volatile防止对象初始化指令重排序，避免拿到半初始化对象。
> 其他高频模式：工厂模式、模板方法、策略模式、责任链、观察者模式；Spring框架大量使用这些模式。

## 十一、Java 基础高频八股总复习清单
