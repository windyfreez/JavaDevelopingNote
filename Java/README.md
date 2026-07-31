# 📚 Java 语言基础

## 一、 Java 基础语法

### 1. 标识符的定义规则

* **组成范围**：由英文字母（A-Z, a-z）、数字（0-9）、下划线 `_` 和美元符号 `$` 组成。
* **命名规则**：
* 不能以数字开头。
* 严格区分大小写。
* 不能使用 Java 中的关键字（如 `public`, `class`, `int` 等）和保留字。
* 遵循驼峰命名法（类名首字母大写，变量/方法首字母小写，常量全大写）。



### 2. 基本数据类型四类八种

* **整数型**：`byte` (1字节, 8位), `short` (2字节), `int` (4字节), `long` (8字节)
* **浮点型**：`float` (4字节, 单精度), `double` (8字节, 双精度)
* **字符型**：`char` (2字节, 存放单个字符)
* **布尔型**：`boolean` (1位, 值为 `true` 或 `false`)

### 3. 复合数据类型

* 也称引用数据类型。包括：**类 (Class)**、**接口 (Interface)**、**数组 (Array)**、**枚举 (Enum)**。
* 复合数据类型变量在内存中存储的是对象在堆内存中的**内存地址（引用）**，而不是直接存储数据本身。

### 4. 程序流控制语句

* **顺序结构**：代码自上而下逐行执行。
* **分支结构**：`if...else`, `switch`。
* **循环结构**：`for`, `while`, `do...while`。
* **异常处理结构**： 后面会具体叙述。

### 5. switch 语句和 if-else 语句的区别，能不能互换使用

* switch语句可以用ifelse来实现其功能
* **区别**：但在某些情况下，使用switch结构更简单，代码可读性强，而且程序的执行效率也得到提高

### 6. 增强型 for 循环

* **用途**：用于遍历数组或某些集合（如 `List`, `Set`），语法简洁，隐藏了迭代器细节。
* **代码**：

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

* **String**：字符串常量，**不可变**。每次修改（如拼接）都会生成新的 String 对象，效率低。
* **StringBuilder**：字符串变量，**可变**，**非线程安全**。单线程环境下拼接效率高。
* **StringBuffer**：字符串变量，**可变**，**线程安全**（方法带 `synchronized`）。多线程环境下使用，效率略低于 `StringBuilder`。

* **代码**：

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

---

## 二、 面向对象基础

### 1. 封装、继承、多态定义

* **封装 (Encapsulation)**：隐藏对象的属性和实现细节，仅对外提供公共的访问方式（通过 `private` 属性配合 `getter/setter` 方法）。提高安全性和复用性。
* **继承 (Inheritance)**：子类继承父类的属性和方法，体现类与类之间的 `is-a` 关系。提高了代码的复用性，是多态的前提。
* **多态 (Polymorphism)**：同一操作/方法作用于不同的对象，可以有不同的解释和执行结果（即父类引用指向子类对象）。

### 2. final 关键字

* **修饰类**：该类不能被继承（如 `String`, `Math`）。
* **修饰方法**：该方法不能被子类重写。
* **修饰变量**：变为**常量**，只能被赋值一次。如果修饰引用类型变量，引用地址不可变，但其指向的对象内容可以改变。

### 3. 方法重载是什么？规则？

* **是什么**：在**同一个类**中，允许存在一个以上的同名方法，只要它们的**参数列表不同**。
* **规则（两同一不同）**：
* 方法名**相同**。
* 参数列表**不同**（参数个数不同、类型不同、顺序不同）。
* **与返回值类型、修饰符无关**。


* **代码**：

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

* **是什么**：在**子父类**中，子类拥有与父类方法签名完全相同的方法，当子类对象调用该方法时，会执行子类重写后的逻辑。
* **规则**：
* 方法名、参数列表必须**完全相同**。
* 子类重写方法的返回值和父类方法的返回值**必须相同**；
* 子类重写方法的访问权限相较于父类方法**不能缩小**；
* 子类重写方法相较于父类方法**不能抛出新的异常**。
> 注意：①被`final`修饰的方法不能被重写；
> ②私有方法（`private`）和静态方法（`static`）不能被重写。


* **代码**：

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

* **是什么**：一种特殊的方法，**方法名必须与类名完全相同**，且**没有返回值类型**（连 `void` 都不写）。
* **作用**：在创建对象时使用 `new` 关键字自动调用，主要用于对对象的数据（属性）进行初始化。如果没有显式定义，系统会提供一个无参构造；一旦定义了有参构造，默认无参构造将被覆盖（建议手写无参构造）。
* **代码**：

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

| 修饰符 | 当前类 (Class) | 同一包内 (Package) | 子类 (Subclass) | 其他包 (World) |
| --- | --- | --- | --- | --- |
| **public** | √ | √ | √ | √ |
| **protected** | √ | √ | √ | × |
| **default** | √ | √ | × | × |
| **private** | √ | × | × | × |

 *(注：default表示不写任何修饰符)* 

### 7. 类多重继承如何实现

* **机制**：Java 类**不支持多重继承**（即一个类不能有多个直接父类）
* **实现**：可以通过**实现多个接口 (Interface)** 来达到类似多重继承的效果。
* **代码**：

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

---

## 三、 面向对象高级特性与多态

### 1. 编译时多态？运行时多态？

* **多态的条件**：继承、重写、向上转型。

* **编译时多态（静态多态）**：指**方法重载 (Overload)**。在编译期，编译器根据调用时传入的参数类型和个数决定绑定哪个具体的方法。
* **运行时多态（动态多态）**：指**方法重写 (Override)**。在编译期，引用变量只能调用父类的方法；在运行期，JVM 根据实际引用的对象类型动态调用对应子类的方法。
* **条件**：① 继承关系；② 方法重写；③ 父类引用指向子类对象 (`Parent p = new Child();`)。
* **代码**：

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

* **作用**：用于判断一个引用类型变量所指向的对象是否是一个特定类（或其子类、实现类）的实例。返回布尔值 `true` 或 `false`。常用于向下转型前防错。
* **代码**：

```java
Animal animal = new Dog();
if (animal instanceof Dog) {
    Dog dog = (Dog) animal; // 向下转型安全
    System.out.println("确实是狗");
}

```

### 3. 类变量（静态变量）与类方法（静态方法）

* 用 `static` 关键字修饰。
* **特点**：
* 属于类，不属于单个对象，被该类的所有对象**共享**。
* 在类加载时（早于对象创建）就加载到内存的方法区中。
* 推荐使用 **`类名.静态变量`** 或 **`类名.静态方法()`** 的方式进行访问。
* 静态方法中**不能**直接访问非静态的成员变量或非静态方法（因为非静态依赖具体对象）。


* **代码**：

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

* **要点**：
* 使用 `interface` 关键字定义。
* 接口中的变量默认都是 `public static final`（常量）。
* 接口中的方法在 Java 8 之前默认是 `public abstract`（抽象方法）。Java 8 允许有 `default`（默认）方法和 `static`（静态）方法。
* 非抽象类实现接口必须实现接口中的**所有抽象方法**，使用 `implements` 关键字。


* **代码**：

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

* **定义**：使用 `abstract` 关键字修饰的类。不能被实例化，只能被继承。
* **特点**：
  * 可以包含**抽象方法**（只有声明，没有实现）和**非抽象方法**（有完整实现）。
  * 可以包含成员变量、构造方法（供子类调用）。
  * 子类继承抽象类时，**必须实现所有抽象方法**，除非子类也是抽象类。
* **与接口的区别**：
  * 抽象类是 `is-a` 关系（模板），接口是 `can-do` 关系（能力契约）。
  * 一个类只能**单继承**抽象类，但可以实现**多个**接口。

* **代码**：

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

* **定义**：包（Package）用于管理类文件，避免类名冲突，类似于文件夹。定义包的语句 `package packageName;` **必须**放在 Java 源文件的第一行非注释代码。
* **使用**：若要使用其他包中的类，必须使用 `import` 关键字导入（如 `import java.util.Scanner;`）。同包下的类无需导入。位于 `java.lang` 包下的核心类（如 `String`, `System`）由系统默认自动导入。
* **代码**：

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

---

## 四、 泛型、集合与异常处理

### 1. 泛型定义使用

* **定义**：泛型（Generics）即“参数化类型”，将类型明确的工作推迟到创建对象或调用方法时进行。
* **作用**：提供编译时类型安全检测机制，避免向下转型时出现 `ClassCastException`（强制类型转换异常）。
* **代码**：

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

### 2. 集合类 (Java Collections Framework)

![alt text](image4.2.png)
#### A. Collection 接口（单列集合）

* **List（有序、可重复）**
* `ArrayList`：底层数组实现。**特点**：随机访问（查询）快，增删元素时需移动大量数据，效率较低。
* `LinkedList`：底层双向链表实现。**特点**：增删元素只需修改指针，效率极高；查询需从头遍历，效率较低。


* **Set（无序、不可重复）**
* `HashSet`：底层基于 `HashMap` 实现。**特点**：通过 `hashCode()` 和 `equals()` 方法保证元素唯一性，无序。



#### B. Map 接口（双列集合，键值对 K-V）

* `HashMap`：底层哈希表实现。
* **特点**：键（Key）唯一，值（Value）可重复。允许一个 `null` 键。通过 Key 的 `hashCode()` 计算存储位置。



#### C. 常用核心方法 (Methods)

* **增（添加元素）**
  * `add(E e)` — 向集合中添加单个元素。
  * `addAll(Collection c)` — 将指定集合中的所有元素批量添加到当前集合。
  * `put(K k, V v)` — **Map 特有**，存入一个键值对；若键已存在则覆盖原值。

* **删（删除元素）**
  * `remove(Object o)` — 删除集合中第一个匹配的元素。
  * `removeAll(Collection c)` — 删除当前集合中与指定集合的**交集元素**（即移除所有在参数集合中出现的元素）。
  * `retainAll(Collection c)` — **仅保留**当前集合中与指定集合的交集元素（即删除不在参数集合中的所有元素，求交集）。
  * `clear()` — 清空集合中的所有元素。

* **查（查询状态）**
  * `size()` — 返回集合中元素的个数。
  * `isEmpty()` — 判断集合是否为空，为空返回 `true`。
  * `contains(Object o)` — 判断集合是否包含指定元素。
  * `containsAll(Collection c)` — 判断当前集合是否包含指定集合中的**所有**元素。

* **取（获取元素）**
  * `get(int index)` — **List 特有**，根据索引获取元素。
  * `get(Object key)` — **Map 特有**，根据键获取对应的值。
  * `toArray()` — 将集合转换为数组后返回。

* **遍历**
  * `iterator()` — 获取迭代器，用于遍历 Collection 及其子接口（List、Set）。
  * `keySet()` — **Map 特有**，获取所有键的集合，再通过键遍历取值。
  * `entrySet()` — **Map 特有**，获取所有键值对（`Map.Entry`）的集合，推荐用于遍历 Map。

#### D. 代码示例

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

        // 2. Map 操作
        Map<String, String> map = new HashMap<>();
        map.put("001", "Java"); // 存入键值对
        System.out.println("查询值: " + map.get("001")); // Java
        
        // 3. 遍历 Map (考点：entrySet)
        for (Map.Entry<String, String> entry : map.entrySet()) {
            System.out.println(entry.getKey() + ":" + entry.getValue());
        }
    }
}

```

---



### 3. 异常处理两种方法

* Java 中的异常体系：`Throwable` -> `Error`（严重错误，程序无法处理） 和 `Exception`（异常，程序可处理）。
* `Exception` 分为：**运行时异常（RuntimeException / Unchecked）** 和 **编译时异常（Checked Exception，必须显式捕获或抛出）**。

#### 方法一：try-catch-finally 语句捕获并处理异常

* **代码**：

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

#### 方法二：将方法中的异常抛出 (`throws / throw`)

* **代码**：

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

---

## 五、 输入输出文件流 (I/O)

### 1. 基本分类

* **流向**：**输入流**（读 Read/Input）、**输出流**（写 Write/Output）。
* **操作单位**：**字节流**（操作二进制数据，1字节，基类 InputStream/OutputStream）和 **字符流**（操作文本字符，2字节，基类 Reader/Writer）。

### 2. 文件流与管道流等概念

* **文件流 (File Streams)**：直接连接到文件的流，用于读写磁盘文件（如 `FileInputStream`, `FileOutputStream`, `FileReader`, `FileWriter`）。
* **缓存流 (Buffered Streams)**：对基础流进行包装，带内部缓冲区（如 `BufferedInputStream`, `BufferedReader`）。读写数据时减少实际物理磁盘读写次数，极大提高 I/O 效率。
* **数据流 (Data Streams)**：允许按机器无关的方式读写 Java 基本数据类型（如 `DataInputStream`, `DataOutputStream`）。
* **管道流 (Piped Streams)**：用于线程间的直接数据通信（一个线程向管道输出流写，另一个线程从管道输入流读，如 `PipedInputStream`, `PipedOutputStream`）。

### 3. I/O 流操作代码

* **代码（文件字符流读写示例）**：

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
            // PrintWriter 在 println 后若开启 autoFlush 会自动刷新，
            // 此处 try-with-resources 关闭时也会刷新缓冲区
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

---

## 六、 枚举与程序初始化顺序

### 1. 枚举类型的定义方式和使用

* **定义**：使用 `enum` 关键字定义一组有限的具名常量。枚举本质上是一个继承自 `java.lang.Enum` 的特殊类。
* **使用**：常用于状态机、单例模式或定义固定常量集合（如星期、季节、方向）。
* **代码**：

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

* 当创建一个子类对象时，类加载及对象初始化的执行顺序如下：
1. **父类静态**变量和**静态**代码块（按代码先后顺序，仅在类加载时执行一次）。
2. **子类静态**变量和**静态**代码块。
3. **父类非静态**变量和**非静态**代码块。
4. **父类构造**方法。
5. **子类非静态**变量和**非静态**代码块。
6. **子类构造**方法。



---

## 七、 多线程与图形用户界面 (GUI)

### 1. 线程与进程

* **进程**：程序的一次执行过程，是系统分配资源（内存、CPU时间片等）的**基本单位**。
* **线程**：进程内部的一条执行路径，是 CPU 调度和执行的**基本单位**。
* **区别**：一个进程可以包含多个线程。进程间内存相互独立，同一进程内的线程共享堆和方法区资源，但有各自独立的栈空间。
* **多线程特点与优点**：
* **特点**：并发执行、随机性（抢占式调度）。
* **优点**：提高 CPU 利用率，改善程序的响应速度（如后台下载、前台界面流畅）。


* **多线程问题与解决**：
* **问题**：线程安全问题（多个线程同时操作共享数据导致数据混乱）、死锁（互相等待锁释放导致程序挂起）。
* **解决**：使用**同步机制**（如 `synchronized` 关键字或显式锁 `ReentrantLock`），遵守锁的申请顺序及设置超时时间预防死锁。


* **线程锁的功能**：
* `synchronized`：隐式锁，修饰方法或代码块，保证同一时刻最多只有一个线程执行该段代码（原子性、可见性），自动加锁和释放。
* `ReentrantLock`：显式锁，提供更灵活的操作（如公平锁、尝试非阻塞获取锁 `tryLock()`、中断响应），必须手动 `lock()` 和 `unlock()`（放入 `finally` 块中）。
* `volatile`：轻量级同步机制，保证变量在多个线程间的**可见性**，禁止指令重排，**不保证原子性**。



### 2. Java GUI 编程 (AWT 与 Swing)

* **AWT 与 Swing 多层架构**：
* **AWT** (Abstract Window Toolkit)：重量级组件。它依赖底层操作系统的窗口组件，功能有限，但平台一致性较差。
* **Swing**：轻量级组件。完全由纯 Java 写成，构建在 AWT 架构之上，提供更丰富的组件和更强的跨平台外观（MVC 架构分离）。


* **GUI 事件处理模型（三要素）**：
* **事件源 (Event Source)**：发生事件的组件（如按钮、文本框）。
* **事件对象 (Event Object)**：封装了事件发生时的信息和状态（如 `ActionEvent`）。
* **监听器 (Listener)**：负责接收事件对象并执行相应的处理代码（如 `ActionListener`）。


* **处理步骤**：
1. 事件源组件注册监听器（如 `button.addActionListener(listener);`）。
2. 编写监听器类实现对应的事件接口，重写处理方法。
3. 触发事件时，监听器自动调用方法处理。


* **监听器定义方式**：
* **内部类**：直接在 GUI 类中定义一个内部类实现监听接口。
* **匿名内部类**：在注册监听器时直接 `new Interface() { @Override ... }`（简便、常用于小型代码）。
* **外部类**：单独定义一个类实现监听器接口。
* **当前类本身**：主窗口类直接实现 Listener 接口（代码集中）。


* **Layout（布局管理器）**：
* **是什么**：用于自动管理容器中各个组件的位置和大小，避免硬编码坐标。
* **处理不同布局**：
* `FlowLayout`（流式布局）：组件像水流一样从左到右排列，换行，常用于面板（Panel）。
* `BorderLayout`（边界布局）：分为东、西、南、北、中五个区域，顶层容器默认布局。
* `GridLayout`（网格布局）：将容器分割为规则的行列表格。
* 容器可以嵌套使用不同的布局，通过混合组件布局构建复杂的图形界面。

### 3. 线程的生命周期与状态

* **新建 (New)**：线程对象被创建，尚未调用 `start()`。
* **就绪 (Runnable)**：调用 `start()` 后，线程进入就绪队列，等待 CPU 调度。
* **运行 (Running)**：获得 CPU 时间片，正在执行 `run()` 方法。
* **阻塞/等待 (Blocked / Waiting / Timed_Waiting)**：
  * 调用 `sleep()`、`join()`、`wait()` 等方法会进入等待/阻塞状态。
  * 等待锁资源时进入 `Blocked` 状态。
* **死亡 (Terminated)**：`run()` 方法执行完毕，或发生未捕获异常，线程结束。

* **状态转换图简要说明**：
  * `New` → `start()` → `Runnable`
  * `Runnable` ←→ `Running`（CPU 调度）
  * `Running` → `wait()` / `sleep()` / `join()` / 等待锁 → `Waiting` / `Timed_Waiting` / `Blocked`
  * `Waiting` / `Blocked` → 唤醒 / 获取锁 → `Runnable`
  * `Running` → `run()` 结束 / 异常 → `Terminated`