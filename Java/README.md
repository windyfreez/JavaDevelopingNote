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
37. 线程中断机制 interrupt？
38. ThreadLocal 内存泄漏细节与跨线程传递（TTL）？
39. ReentrantLock 的 lock/tryLock/lockInterruptibly 区别？
40. 乐观锁（版本号）更新失败后怎么处理？
41. 五种内置线程池与阻塞队列选择？
42. Minor GC、Major GC、Full GC 区别？Full GC 触发场景？
43. 内存泄漏和内存溢出的区别？

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
52. 循环中字符串拼接为什么必须用 StringBuilder？
53. 浮点数比较陷阱（0.1+0.2 != 0.3）？
54. 泛型擦除对运行时的影响（instanceof、反射绕过）？
55. 遍历集合时如何安全删除元素？
56. HashMap 多线程下的问题与初始化容量预估？
57. HashMap 按 Key / 按 Value 排序？
58. ConcurrentHashMap 的 get 为什么不需要加锁？
59. NIO 三大组件（Buffer/Channel/Selector）？
60. 读写锁细节：锁降级、锁升级与 StampedLock？
61. CountDownLatch 底层原理与使用陷阱？
62. 堆内存分代结构（Eden/Survivor 比例、对象晋升规则）？
63. 垃圾收集器演进：CMS 缺陷、G1 与 ZGC？
64. JVM 内存区域和 JMM 的区别？
65. JDBC 如何打破双亲委派（线程上下文类加载器）？
66. 反射核心 API、setAccessible 与性能优化？
67. 单例模式五种写法对比？
68. 责任链、策略、代理与适配器模式的区别？

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
### 2. 基本数据类型**四类八种**
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

### 13. 数组（Array）详解
数组是存储**同类型元素**的固定长度容器，下标从0开始，创建后长度不可变。
- 三种创建方式：`int[] a = new int[3];`、`int[] b = {1,2,3};`、`int[] c = new int[]{1,2,3};`
- 基本类型数组默认值：int→0、double→0.0、boolean→false、char→'\u0000'；引用类型数组默认`null`；
- 数组是对象，`length`是属性不是方法；越界访问抛出`ArrayIndexOutOfBoundsException`；
- 常用工具类`Arrays`：`toString`、`sort`、`binarySearch`（二分查找，要求先排序）、`copyOf`扩容复制、`fill`填充、`equals`比较。

> 🎯【面试题】数组和集合的区别？
> 参考答案：数组长度固定，创建后不能扩容；集合长度动态可变，会自动扩容。数组可以存储基本类型和引用类型；集合只能存储引用类型（基本类型自动装箱）。数组访问效率高、内存连续；集合功能更丰富，提供增删改查、排序、去重等API。业务开发绝大多数场景使用集合。

> 补充：`Arrays.asList()`返回的是**固定大小**列表，不能执行add/remove，否则抛`UnsupportedOperationException`。

### 14. 字符串常量池与 String 不可变原理
> 🎯【面试题】String 为什么不可变？有什么好处？
> 参考答案：String类被`final`修饰，类不能被继承；内部字符数组被`final`修饰，并且**没有提供任何修改字符数组的入口**（concat、replace、substring等所有"修改"操作都会返回新对象）。不可变的好处：1. 字符串常量池可以安全复用同一对象，节省内存；2. 作为HashMap的key安全，hashCode可以缓存，不会因内容变化导致哈希错乱；3. 线程安全，多线程共享无需加锁。

**字符串常量池**（JDK7及之后位于堆中，之前位于方法区PermGen）：
- 双引号字面量直接去常量池查找，存在则复用，不存在则创建入池；
- `new String("abc")`：在堆中新建一个对象（不放入常量池），同时"abc"字面量会在常量池创建；
- `intern()`：手动把字符串加入常量池，若池中已有相同内容则直接返回池中引用。

```java
String s1 = "abc";                 // 常量池对象
String s2 = "abc";                 // 复用常量池对象
System.out.println(s1 == s2);      // true
String s3 = new String("abc");     // 堆上新对象
System.out.println(s1 == s3);      // false
System.out.println(s1 == s3.intern()); // true，intern返回常量池引用
```

### 15. BigDecimal 精度问题（金额计算必用）
> 🎯【面试题】float和double为什么会有精度问题？金额计算怎么保证精度？
> 参考答案：二进制无法精确表示所有十进制小数（例如0.1在二进制中是无限循环小数），float/double按二进制浮点数存储，运算会产生精度丢失。金额等精确计算必须使用BigDecimal，并且**必须用字符串构造**：`new BigDecimal("0.1")`，不能`new BigDecimal(0.1)`（传入double本身已经失真）。

- 常用运算：`add`加、`subtract`减、`multiply`乘、`divide`除（**除法必须指定精度和舍入模式**，否则除不尽抛`ArithmeticException`）；
- 比较大小用`compareTo`，不要用`equals`（equals会比较精度标度，`1.0`和`1.00`不相等）；
- 保留小数：`setScale(2, RoundingMode.HALF_UP)`四舍五入；
- 开发规范：数据库金额字段用`DECIMAL`，实体类用`BigDecimal`；JSON序列化注意精度丢失问题（前端可传字符串）。

### 16. 循环中字符串拼接的选择
> 🎯【面试题】循环里拼接字符串为什么不能用`+`？应该怎么做？
> 参考答案：
> 1. 循环内写`s += "a"`，编译器虽然会优化成StringBuilder，但优化范围只有**单行**：StringBuilder是在循环体内部每次重新new的，循环1万次就创建1万个StringBuilder和1万个String临时对象，堆压力巨大，频繁触发GC；
> 2. 正确做法：循环外创建**一个**StringBuilder，循环内只调用`append`，结束后`toString`；如果能预估长度，构造时直接传初始容量（如`new StringBuilder(10000)`），避免内部数组反复扩容；
> 3. 单线程场景一律用StringBuilder；只有多线程共享同一个字符串缓冲区（实际极少见）才考虑StringBuffer；
> 4. 补充：Java9起`+`拼接改用`StringConcatFactory`（invokedynamic动态生成拼接逻辑），非循环场景效率有提升，但循环内拼接依然要显式使用StringBuilder。
```java
// ❌ 错误：每次循环都 new StringBuilder() + toString()
String result = "";
for (int i = 0; i < 10000; i++) {
    result += i;
}
// ✅ 正确：循环外建一个 StringBuilder
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 10000; i++) {
    sb.append(i);
}
String finalResult = sb.toString();
```

### 17. 浮点数比较陷阱与 double/BigDecimal 的选择
> 🎯【面试题】`0.1 + 0.2 == 0.3` 的结果是什么？浮点数判断相等怎么做？
> 参考答案：false。double是IEEE 754二进制浮点数，0.1在二进制中是无限循环小数只能近似存储，运算误差会累积，实际得到`0.30000000000000004`。
> 浮点数判断相等不能用`==`，应该用误差范围（epsilon）：`Math.abs(x - y) < 1e-10`。
> double和BigDecimal怎么选：
> 1. 科学计算、图形渲染、性能敏感的高频计算用double——硬件指令支持，比BigDecimal快10~100倍；
> 2. 金额等精确计算一律用BigDecimal——金融系统宁可慢也要准；
> 3. `BigDecimal.valueOf(double)`虽然比`new BigDecimal(0.1)`好一点，但仍会带入double的原始误差，最稳妥的还是字符串构造`new BigDecimal("0.1")`。
> ⚠️注意：BigDecimal是对象，内存开销大、运算慢，不要在循环统计均值等非精确场景滥用；BigDecimal可以为null，从数据库取值判空再运算。

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

### 8. 内部类详解
定义在类内部的类叫内部类，分为四种：
1. **成员内部类**：定义在类内部、方法外。持有外部类引用（Outer.this），可以访问外部类所有成员（包括private）；创建方式`外部类.new 内部类()`，必须先有外部类对象；
2. **静态内部类**：用`static`修饰，不持有外部类引用，只能访问外部类静态成员；创建方式`new 外部类.内部类()`，不需要外部类对象；
3. **局部内部类**：定义在方法内部，作用域只限当前方法；
4. **匿名内部类**：没有类名的局部内部类，常用于接口/抽象类的一次性实现，是Lambda表达式的"前身"。

> 🎯【面试题】局部内部类和匿名内部类访问外部局部变量，为什么要求变量是final的？
> 参考答案：局部变量存在栈上，方法执行完毕就销毁；而内部类对象可能逃逸到堆中长期存活。为了内部类对象在方法结束后仍能使用该变量，编译器会把局部变量复制一份到内部类对象中；如果变量后续被修改，内外两份数据会不一致，所以要求变量不可变（final或effectively final），保证复制值与原值一致。

### 9. 对象排序：Comparable 与 Comparator
> 🎯【面试题】Comparable和Comparator的区别？
> 参考答案：Comparable是"自然排序"，定义在实体类内部，实现`compareTo`方法，类自己具备比较能力，一个类只能有一种自然排序；Comparator是"外部比较器"，单独定义比较规则，不侵入实体类，可以为同一个类定义多种排序规则。优先使用Comparator，符合开闭原则，避免修改实体类。

```java
// Comparable：类内部实现自然排序（按年龄升序）
class User implements Comparable<User> {
    private int age;
    @Override
    public int compareTo(User o) { return Integer.compare(this.age, o.age); }
}

// Comparator：外部比较器（按姓名降序）
Comparator<User> byNameDesc = (u1, u2) -> u2.getName().compareTo(u1.getName());
Collections.sort(list, byNameDesc);
list.stream().sorted(Comparator.comparing(User::getAge).reversed());
```
- 返回负数表示小于、0相等、正数大于；**注意不要写`this.age - o.age`，可能整数溢出**，推荐`Integer.compare`；
- TreeSet / TreeMap / Collections.sort / Stream.sorted 都依赖这两个接口。

### 10. 深拷贝与浅拷贝（clone）
> 🎯【面试题】浅拷贝和深拷贝的区别？如何实现深拷贝？
> 参考答案：浅拷贝只复制对象本身，对象内部引用类型的成员变量仍然指向同一个对象；深拷贝连内部引用对象也一起复制，得到完全独立的新对象。Object.clone()默认是浅拷贝，并且类必须实现`Cloneable`标记接口，否则抛`CloneNotSupportedException`。

- 浅拷贝：重写`clone()`，默认`super.clone()`即可；
- 深拷贝实现方式：1. 在clone()里手动new内部对象逐个复制；2. 对象流序列化反序列化（要求所有成员实现Serializable）；3. 第三方工具（JSON序列化、BeanUtils.copyProperties仅浅拷贝）；
- 注意：`Arrays.copyOf`、`list.addAll`等都是浅拷贝，修改元素内容会互相影响；String/包装类不可变，浅拷贝也不会出问题。

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

##### HashMap 为什么使用红黑树（树化原因）
> 🎯【面试题】HashMap 为什么要用红黑树？链表也能存元素，为什么还要转红黑树？
> 参考答案：
> 根本原因是**防止哈希冲突严重时链表查询性能退化**。正常情况下靠 hash 扰动和取模散列，元素落桶均匀，链表很短，桶内查询接近 O(1)；但若 hashCode 设计糟糕、大量 key 挤进同一个桶，链表会越挂越长，桶内查找只能从头到尾遍历，最坏退化为 **O(n)**。
> 红黑树是**自平衡二叉搜索树**，插入、删除、查找复杂度都能稳定在 **O(log n)**。极端冲突下桶里即使堆积上万节点，红黑树查找也只有十几层，等于给最坏情况兜底。
> 为什么不一开始就用树？因为**树节点内存占用约为链表节点的两倍**，还要维护平衡、频繁旋转，正常短链表场景直接转树反而浪费。所以 JDK8 采用折中策略：**链表长度 ≥ 8 且数组容量 ≥ 64 才转树；数组容量不足 64 时优先扩容**——容量太小时冲突源于桶太少，扩容让元素重新散列更划算。
> 补充：官方注释按泊松分布估算，默认负载因子 0.75、随机 hashCode 下桶内链表长度达到 8 的概率约为 0.00000006，几乎不可能，因此 8 是"冲突已很严重"的经验阈值；不用 AVL 树是因为红黑树放宽了严格平衡要求、旋转次数更少，更适合 HashMap 频繁增删的写入场景。

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

#### 2.4 迭代器与 fail-fast / fail-safe 机制
Iterator迭代器三个核心方法：`hasNext()`是否有下一个、`next()`获取下一个元素、`remove()`删除当前元素。
- 增强for循环底层就是Iterator，遍历过程中直接调用集合的add/remove会抛`ConcurrentModificationException`并发修改异常；
- 正确删除方式：迭代器自己的`iterator.remove()`（ArrayList）或`removeIf`。

> 🎯【面试题】什么是fail-fast？什么是fail-safe？
> 参考答案：fail-fast快速失败，是ArrayList、HashMap等普通集合的机制。集合内部维护`modCount`修改计数器，迭代器创建时记录初始modCount，每次next()都会校验，一旦发现modCount变化立即抛ConcurrentModificationException，防止遍历过程数据错乱。fail-safe安全失败，是CopyOnWriteArrayList、ConcurrentHashMap等并发容器的机制，遍历的是原数组快照（或弱一致性迭代器），遍历过程允许并发修改不抛异常，但读取到的可能不是最新数据。

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

### 4. 泛型擦除对运行时的影响（instanceof、反射绕过）
> 🎯【面试题】为什么 `obj instanceof List<String>` 编译报错？运行时还能拿到泛型类型吗？
> 参考答案：
> 泛型只存在于编译期，运行时发生类型擦除：`List<String>` 擦成原始类型 List，带通配符上界的擦成上界。所以 JVM 根本不知道这个 List 原来装的是什么，对不存在的信息做运行时判断会直接编译报错，绝大多数情况下不能通过 instanceof 检查具体泛型类型。
> 合法写法：只能用无界通配符 `obj instanceof List<?>`（或原始类型 List）判断"外壳"，无法判断元素类型。
> 要在运行时校验元素类型有两种思路：
> 1. 先 `instanceof List<?>` 判断是 List，再遍历元素逐个 `element instanceof String` 校验；
> 2. Type Token 模式：把 `Class<T>` 作为参数显式传进来，用 `type.isInstance(obj)` 判断、`type.cast(obj)` 安全强转。
> 框架怎么拿到泛型：局部变量、实例字段的泛型被擦除了，但**类声明上的父类/接口泛型签名（Signature）会保留在字节码常量池**。Jackson、Fastjson 反序列化 `List<User>` 就是靠 `new TypeReference<List<User>>(){}` 匿名子类 + 反射 `getGenericSuperclass()` 把泛型信息"抠"出来——这就是匿名内部类写法能绕过擦除的原因。

### 5. 遍历集合时如何安全删除元素？
> 🎯【面试题】遍历 ArrayList 的过程中要删除元素，怎么做才正确？
> 参考答案：
> 1. 增强 for 循环里直接 list.remove()：❌ 必抛 ConcurrentModificationException。foreach 底层是 Iterator，迭代器内部维护 expectedModCount，集合自身的 remove 只更新 modCount，下次调用 next() 时发现两者不一致，触发 fail-fast；
> 2. 普通 for 正向遍历删除：⚠️ 不会报错但会**漏删**——删除后后面的元素整体前移一位，循环 i++ 正好跳过了紧邻的下一个元素；修正方式是从后往前遍历；
> 3. 迭代器 it.remove()：✅ 安全，它删除元素后会同步更新 expectedModCount；
> 4. `list.removeIf(条件)`（Java 8）：🏆 最推荐，声明式一行搞定，底层已处理好遍历删除逻辑；
> 5. 换成 CopyOnWriteArrayList：✅ 遍历的是原数组快照，foreach 中删除也不报错，但每次写都复制数组，仅适合读多写少。

```java
// 推荐：Java8 removeIf
list.removeIf(s -> "b".equals(s));
// 传统：迭代器删除（注意调用的是 iterator 的 remove，不是 list 的 remove）
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    if ("b".equals(it.next())) {
        it.remove();
    }
}
```

### 6. HashMap 多线程问题与初始容量预估
> 🎯【面试题】HashMap 在多线程下会出什么问题？为什么建议预估容量创建？
> 参考答案：
> 多线程问题：JDK7 头插法在并发扩容时可能让链表形成**环形结构**，后续 get 陷入死循环 CPU 100%；JDK8 改为尾插法解决了成环问题，但并发 put 仍会**丢失数据**（两个线程同时写同一个桶相互覆盖）、size 不准确。多线程场景直接用 ConcurrentHashMap，不要用 Collections.synchronizedMap（整表一把锁，性能差）。
> 容量预估：默认容量 16、负载因子 0.75，元素增多会多次触发扩容 rehash 拷贝，产生性能抖动。已知要存 N 个元素时，按 `(N / 0.75) + 1` 预估初始容量（Alibaba 规范口径）直接 `new HashMap<>(capacity)`，避免反复扩容。

### 7. HashMap 按 Key / 按 Value 排序
> 🎯【面试题】HashMap 天生无序，怎么按 Key 排序？怎么按 Value 排序？
> 参考答案：
> 按 Key 排序：直接 `new TreeMap<>(map)`，TreeMap 底层红黑树自动按 Key 排序，传自定义 Comparator 可控制升降序。
> 按 Value 排序：没有哪种 Map 是按 Value 组织数据的，标准套路是 entrySet 转 List → 排序 → 收集回 **LinkedHashMap**（靠内部双向链表保持插入顺序）。

```java
// 按 Key 排序
Map<String, Integer> sortedByKey = new TreeMap<>(map);
// 按 Value 排序（Java8 Stream，收集时必须显式指定 LinkedHashMap，否则顺序丢失）
Map<String, Integer> sortedByValue = map.entrySet().stream()
        .sorted(Map.Entry.<String, Integer>comparingByValue().reversed())
        .collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue,
                (o, n) -> o, LinkedHashMap::new));
```

> ⚠️注意：不要给 TreeMap 传按 Value 比较的 Comparator 来实现按 Value 排序——TreeMap 的去重也依赖 Comparator，两个 Entry 的 Value 相等会被判定为"相同 Key"直接覆盖，导致数据丢失。

### 8. ConcurrentHashMap 的 get 为什么不需要加锁？
> 🎯【面试题】ConcurrentHashMap 的 get 完全无锁，靠什么保证线程安全？
> 参考答案：Node 节点的 val 和 next 指针都用 **volatile** 修饰，写线程修改后对读线程立即可见，get 靠 volatile 可见性直接读到最新值，全程无锁，这也是它读性能高的核心原因。
> 补充两个细节：get 读到的桶头节点 hash 为 -1（ForwardingNode）说明正在扩容，会顺着它去新数组读；put 时桶为空用 CAS 无锁插入，桶不为空才 synchronized 锁住桶头节点——锁粒度从 JDK7 的 Segment 段细化到了单个桶。

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

### 4. NIO 三大组件（Buffer / Channel / Selector）
> 🎯【面试题】说说 NIO 三大核心组件的作用？NIO 是同步还是异步？
> 参考答案：
> 1. **Buffer 缓冲区**：NIO 中所有数据读写都必须经过 Buffer，本质是数组（ByteBuffer 最常用），内部用 position、limit、capacity 三个指针跟踪读写状态；写模式切换到读模式要调用 flip()；
> 2. **Channel 通道**：双向管道，既能读也能写（BIO 的流是单向的），数据在 Channel 与 Buffer 之间搬运：channel.read(buf)、channel.write(buf)；常见实现 FileChannel、SocketChannel、ServerSocketChannel、DatagramChannel；
> 3. **Selector 选择器**：NIO 的灵魂，单线程监控多个 Channel 的 IO 事件（ACCEPT、CONNECT、READ、WRITE）。Channel 向 Selector 注册感兴趣的事件，select() 轮询返回就绪事件的 SelectionKey 集合，线程再逐个处理——这就是 IO 多路复用，一个线程管理成千上万连接。
> NIO 是**同步非阻塞**：线程仍要主动调用 select() 询问就绪状态（同步）；没有就绪事件时线程不会被卡死（非阻塞）。
- 生产上很少直接写原生 NIO API（半包黏包、断连重连处理繁琐），主流用 Netty 封装；Tomcat8+ 默认 NIO 模式，Kafka、Zookeeper 通信层也基于 NIO。

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

### 3.1 synchronized 锁升级（偏向锁、轻量级锁、重量级锁）
> 🎯【面试题】synchronized的锁升级过程？为什么要有锁升级？
> 参考答案：JDK6之后synchronized做了锁优化，锁存在"无锁→偏向锁→轻量级锁→重量级锁"的升级过程（面试口径：锁只能升级、不轻易降级）：
> 1. **偏向锁**：只有一个线程反复进入同步块时，把线程ID记录在对象头MarkWord里，后续该线程再次进入无需CAS，性能最高；
> 2. **轻量级锁**：出现第二个线程竞争时，偏向锁撤销升级为轻量级锁，通过CAS自旋尝试获取锁，自旋占用CPU但避免线程阻塞，适合锁竞争不激烈、持锁时间短的场景；
> 3. **重量级锁**：自旋超过阈值（或自旋线程过多）升级为重量级锁，依赖操作系统互斥量（monitor），未抢到锁的线程进入阻塞队列，涉及用户态内核态切换，开销最大。
> 锁状态记录在对象头**MarkWord**中；可用`-XX:-UseBiasedLocking`关闭偏向锁。

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

### 7.6 线程的创建方式（Callable 与 FutureTask）
> 🎯【面试题】创建线程有哪几种方式？
> 参考答案：四种方式——1. 继承Thread类重写run()；2. 实现Runnable接口；3. 实现Callable接口配合FutureTask，能返回结果、能抛异常；4. 线程池ExecutorService提交任务。推荐实现接口和线程池：Java单继承，继承Thread会占用继承位；接口方式解耦便于复用；线程池统一管理线程生命周期，避免频繁创建销毁。

```java
// 方式三：Callable + FutureTask，可以获取返回值
Callable<Integer> task = () -> {
    Thread.sleep(1000);
    return 100;
};
FutureTask<Integer> futureTask = new FutureTask<>(task);
new Thread(futureTask, "compute-thread").start();
Integer result = futureTask.get(); // 阻塞等待结果
```
- `get()`会阻塞当前线程直到任务完成；也可`get(timeout, unit)`限时等待，超时抛TimeoutException；
- 守护线程（daemon）：`thread.setDaemon(true)`，守护线程服务于其他线程，主线程结束守护线程随之结束，GC线程就是典型守护线程。

### 7.7 线程的生命周期与状态
> 🎯【面试题】线程有哪几种状态？状态之间如何流转？
> 参考答案：Java线程共6种状态（Thread.State枚举）：
> 1. **NEW新建**：new出Thread对象，还没调用start()；
> 2. **RUNNABLE就绪/运行**：调用start()后进入，等待CPU调度执行（Runnable同时包含就绪和运行两种OS状态）；
> 3. **BLOCKED阻塞**：等待获取monitor锁进入同步块/方法时阻塞；
> 4. **WAITING等待**：无期限等待，`wait()`、`join()`、`LockSupport.park()`进入，需要其他线程唤醒；
> 5. **TIMED_WAITING计时等待**：有期限等待，`sleep(ms)`、`wait(timeout)`、`join(ms)`进入，时间到自动唤醒；
> 6. **TERMINATED终止**：run()正常执行完毕或抛出未捕获异常。

状态流转口诀：NEW调start进RUNNABLE；抢锁失败进BLOCKED；wait/join/park进WAITING；sleep/带参wait进TIMED_WAITING；notify/notifyAll/unpark/超时回到RUNNABLE；run结束进TERMINATED。

> ⚠️注意：调用`start()`才是启动新线程；直接调用`run()`只是在当前线程执行普通方法，不会创建新线程。

### 7.8 sleep、wait、join 的区别与线程通信
> 🎯【面试题】sleep()和wait()有什么区别？
> 参考答案：
> 1. 所属类不同：sleep是Thread的静态方法；wait是Object的实例方法；
> 2. 锁的释放：sleep不释放已持有的锁；wait会释放对象锁，让其他线程进入同步块；
> 3. 使用位置：sleep可以在任何地方使用；wait必须配合synchronized同步块使用，否则抛IllegalMonitorStateException；
> 4. 唤醒方式：sleep到时间自动唤醒；wait需要notify/notifyAll唤醒或带超时时间；
> 5. join()：Thread的方法，当前线程等待调用join的线程执行完毕，如`t.join()`主线程等t线程结束；join(ms)限时等待。

> 🎯【面试题】wait/notify实现线程通信的原理？
> 参考答案：wait/notify必须成对出现在synchronized同步代码块中。线程执行wait()会释放对象锁并进入该对象的等待集（WaitSet）；其他线程拿到锁后执行notify()唤醒等待集中的一个线程（notifyAll唤醒全部），被唤醒线程需要重新竞争对象锁才能继续执行。经典应用：生产者-消费者模型；现代开发推荐使用`BlockingQueue`（ArrayBlockingQueue/LinkedBlockingQueue）替代手写wait/notify，更安全简洁。

### 7.9 死锁（Deadlock）
> 🎯【面试题】什么是死锁？产生的四个必要条件？如何避免？
> 参考答案：死锁是多个线程互相持有对方需要的锁、互相等待对方释放，导致所有线程都无法继续执行的现象。四个必要条件：互斥（资源只能被一个线程占用）、请求与保持（持有资源的同时请求新资源）、不可剥夺（资源只能主动释放）、循环等待（形成环路）。避免死锁的思路：破坏任意一个条件即可，工程上最常用的是**破坏循环等待——所有线程按同一固定顺序加锁**（如统一先锁A再锁B）；同时缩短持锁时间、减少锁粒度、使用tryLock超时抢锁。

- 排查命令：`jps`查进程号 → `jstack <pid>`查看线程栈，出现"Found one Java-level deadlock"即死锁，能看到互相等待的锁和线程；
- 实际业务中死锁往往来自**锁顺序不一致**（A线程先锁A再锁B，B线程先锁B再锁A）。

### 7.10 线程中断机制（interrupt）
> 🎯【面试题】调用 interrupt() 会强制停止线程吗？isInterrupted() 和 Thread.interrupted() 有什么区别？
> 参考答案：
> interrupt() 是**协作式**中断，不是强制命令，更不是"杀死线程"。它只是给目标线程打一个中断标记（标志位置 true），线程停不停、什么时候停，完全由线程自己的代码逻辑决定。已废弃的 stop() 才是暴力终止，会导致锁不释放、数据错乱。
> 三个核心方法：
> 1. interrupt()：实例方法，设置目标线程中断标志位为 true；
> 2. isInterrupted()：实例方法，判断标志位，**不清除**；
> 3. Thread.interrupted()：静态方法，判断**当前线程**标志位并**清除**（重置回 false）。
> 两种不同的表现：
> 1. 线程正常运行中：interrupt() 只把标志位置 true，线程如果不去检查就完全没影响，必须在循环条件里主动判断 `!Thread.currentThread().isInterrupted()`；
> 2. 线程阻塞中（sleep/wait/join）：interrupt() 会让线程立刻抛出 InterruptedException，并且 JVM 自动把中断标志位**重置为 false**——这是为了给线程一个复原的机会。

> ⚠️注意：catch 到 InterruptedException 后不要吞掉。抛异常时标志位已被清除，如果当前方法决定不往外抛，应再次调用 `Thread.currentThread().interrupt()` 恢复中断状态，否则中断信号"消失"，上层调用方（比如线程池的 shutdownNow）感知不到线程曾被中断，该停的时候停不下来。

### 7.11 ThreadLocal 内存泄漏细节与跨线程传递（TTL）
> 🎯【面试题】ThreadLocal 的 key 是弱引用，为什么还会内存泄漏？泄漏的到底是谁？
> 参考答案：
> 泄漏的不是 key（ThreadLocal 对象），而是 Entry 里的 **value**。强引用链：Thread → ThreadLocalMap → Entry → value，线程池核心线程长期存活，这条链就一直在。
> 泄漏过程：外部不再持有 ThreadLocal 的强引用后，下一次 GC 弱引用 key 被回收变成 null；但 value 是强引用仍然挂在 Entry 上，而程序已经无法通过 key 找到它，这块内存就成了"幽灵对象"，占着坑却访问不到。
> 为什么设计成弱引用：弱引用其实是在**减轻**泄漏——如果 key 是强引用，线程不销毁，ThreadLocal 和 value 就永远不回收；弱引用至少保证 ThreadLocal 对象本身能被回收，而且 set()/get()/rehash() 时会顺带探测清理 key 为 null 的 Entry。但这种清理是被动的，之后再也不调用就漏了，真正的锅是业务代码没闭环。
> 规避：用完必须 remove()，配合 try-finally；Web 场景在拦截器 afterCompletion 里统一清理。

```java
private static final ThreadLocal<User> USER_HOLDER = new ThreadLocal<>();
public void process() {
    try {
        USER_HOLDER.set(currentUser);
        doBusiness();
    } finally {
        USER_HOLDER.remove(); // 线程池复用场景的救命稻草
    }
}
```

> 🎯【面试题】ThreadLocal 怎么跨线程传递？InheritableThreadLocal 为什么在线程池下失效？
> 参考答案：
> 普通 ThreadLocal 天然不能跨线程。InheritableThreadLocal 在 new Thread() 时把父线程的变量复制给子线程，但只在**线程创建那一刻**同步一次——线程池的线程是预先创建好、反复复用的，不会重新走 init 逻辑，所以拿到的永远是线程"出生"时的旧值，感知不到后续每次提交任务时主线程的最新上下文。
> 标准解法是阿里开源的 TransmittableThreadLocal（TTL）：任务**提交时**抓取（Copy）当前线程变量快照打包进增强的 Runnable，**执行时**注入（Replay）到池化线程，**执行完**恢复（Restore）原现场，避免污染下一个任务。用法：TtlExecutors.getTtlExecutorService() 包装线程池，或 JVM 启动时挂载 TTL Java Agent 自动字节码增强，业务零侵入。典型场景：链路追踪 TraceID 透传、登录用户上下文传递。

### 7.12 ReentrantLock 的 lock / tryLock / lockInterruptibly 区别
> 🎯【面试题】三种获取锁的方式有什么区别？各适用什么场景？
> 参考答案：
> 1. lock()：死等派——拿不到锁就永久阻塞，**不响应中断**（等待中被 interrupt() 不会抛异常，拿到锁后才补上中断标记）；适合最常规的同步场景；
> 2. tryLock()：急性子——无参版本探测一下，锁空闲立刻拿走返回 true，被占立刻返回 false 绝不等待；带参版本 tryLock(time, unit) 限时等待，超时返回 false，等待期间**响应中断**；是避免死锁的利器（互相等待时超时一方主动放弃，打破循环等待）；
> 3. lockInterruptibly()：可唤醒派——阻塞等待但积极响应中断，被 interrupt() 立刻放弃排队并抛 InterruptedException；适合可能长时间等待、允许被取消的任务。
> 加分点：无参 tryLock() 会"插队"破坏公平性——即使创建的是公平锁，只要锁空闲它就直接抢走，不管排队队列里有没有人在等。

```java
// tryLock 标准范式：必须 if 判断返回值，只有拿到锁才允许 unlock
if (lock.tryLock()) {
    try {
        // 业务逻辑
    } finally {
        lock.unlock();
    }
} else {
    // 没拿到锁的降级/兜底逻辑
}
```

> ⚠️注意：tryLock() 返回 false 时绝不能执行 unlock()，会抛 IllegalMonitorStateException——不能释放不属于你的锁。

### 7.13 乐观锁更新失败后怎么处理？
> 🎯【面试题】数据库乐观锁版本号冲突、CAS 更新失败之后，工程上怎么处理？
> 参考答案：
> 版本号变了说明"撞车"了，判断依据是 `UPDATE t SET val=?, version=version+1 WHERE id=? AND version=?` 的**受影响行数为 0**。两种处理策略：
> 1. 直接失败（Fail Fast）：放弃本次修改抛出业务异常，交给上层或用户决定是否重试。适合用户强交互场景（秒杀、抢购），前端提示"系统繁忙请重试"，保护后端不被重试风暴打挂；
> 2. 自旋重试（Spin Retry）：循环里**重新读取最新数据和版本号**再更新，直到成功或达到最大重试次数。适合系统内部自动化任务（定时任务、MQ 消费状态更新）。
> 两个必须：重试必须设置上限（一般 3~5 次），否则高并发下大量失败线程同时疯狂查库抢版本，CPU 飙升、数据库连接池被打爆；重试前加随机休眠（Backoff 策略，如 sleep 10~50ms）错开重试时间，避免活锁。
> 延伸：数据库版本号严格递增（version = version + 1），永远不可能 1→2→1 回退，天然没有 CAS 的 ABA 问题。

### 7.14 五种内置线程池与阻塞队列选择
> 🎯【面试题】Executors 提供了哪几种内置线程池？各自的隐患？线程池的阻塞队列怎么选？
> 参考答案：
> 五种内置线程池：
> 1. FixedThreadPool：定长，核心线程数=最大线程数；底层**无界** LinkedBlockingQueue，任务堆积会 OOM；
> 2. CachedThreadPool：核心 0、最大 Integer.MAX_VALUE，空闲 60 秒回收；短时间涌入大量任务会创建海量线程，CPU 100% 或 OOM；
> 3. SingleThreadExecutor：单线程保证任务顺序执行，线程挂了会自动补一个；底层同样无界队列，会 OOM；
> 4. ScheduledThreadPool：定时、周期性任务，替代 Timer；⚠️ 周期任务抛异常且未捕获，后续调度会**静默停止**，任务体必须 try-catch；
> 5. WorkStealingPool（JDK8）：基于 ForkJoinPool 的工作窃取线程池，每个线程有自己的双端队列，干完自己的活去偷别人的，适合大任务拆分、任务耗时差异大的并行计算。
> 阻塞队列选择（线程池调优核心）：
> 1. LinkedBlockingQueue：业务首选，但**必须手动指定容量**（无参构造是 Integer.MAX_VALUE），队列满触发拒绝策略保护系统；
> 2. ArrayBlockingQueue：数组结构、容量固定、单锁实现，适合负载平稳、严格限制资源占用的场景；
> 3. SynchronousQueue：容量为 0，一进一出直接交接、不存储任务，配合很大的 maximumPoolSize 追求极致响应（CachedThreadPool 底层就是它）；
> 4. PriorityBlockingQueue：按优先级出队（注意是无界队列），VIP 请求优先、紧急报警优先场景；
> 5. DelayQueue：延迟到期才能取出，定时/延迟任务。
> 生产模板：手动 `new ThreadPoolExecutor(4, 8, 60, SECONDS, new LinkedBlockingQueue<>(500), 具名ThreadFactory, CallerRunsPolicy)`——有界队列 + 具名线程 + CallerRunsPolicy 天然背压（提交线程自己干活，自动减速）。

### 7.15 读写锁细节：锁降级、锁升级与 StampedLock
> 🎯【面试题】ReentrantReadWriteLock 怎么用一个 state 同时表示读锁和写锁？什么是锁降级？
> 参考答案：
> AQS 的 state 是 32 位 int，读写锁把它**按位切分**：高 16 位记录读锁状态，低 16 位记录写锁状态。获取写锁要求高低位都没被别人占用；获取读锁只看低 16 位（有人在写且不是自己就失败），成功则高位加 1。
> **锁降级**（支持）：持有写锁 → 再获取读锁 → 释放写锁。典型场景：刚写完缓存马上要读自己刚写的数据，先拿读锁再放写锁，防止释放写锁的间隙被别人改了导致读到脏数据。
> **锁升级**（不支持）：持有读锁时直接申请写锁会造成死锁——你在等其他读线程释放读锁，别的读线程也在等你释放读锁。
> 写饥饿问题：读极其频繁时读锁源源不断，写线程可能一直抢不到写锁。JDK8 引入 **StampedLock** 缓解：提供"乐观读"——读时不加锁，读完校验邮戳（stamp）确认期间没有写入，失败再升级为悲观读锁重读，吞吐更高，缓解写饥饿。

### 7.16 CountDownLatch 底层原理与使用陷阱
> 🎯【面试题】CountDownLatch 底层怎么实现的？使用时有哪些坑？
> 参考答案：
> 底层就是 AQS **共享模式**：构造参数直接写入 state；countDown() 通过 CAS 把 state 减 1；await() 检查 state 不为 0 就挂进 AQS 等待队列；state 减到 0 时唤醒全部等待线程（共享锁特性，一次性唤醒所有）。
> 两大经典场景：一等多——接口聚合并行查 3 个下游，主线程 await 等 3 个子任务都 countDown；多等一——压测发令枪，N 个线程 await 等主线程一次 countDown 同时起跑。
> 两个致命坑：
> 1. 子线程抛异常导致 countDown() 没执行，state 永远不到 0，主线程 await() **永久阻塞**——countDown 必须放 finally 块；
> 2. await() 必须用带超时的 `await(timeout, unit)` 兜底，超时走降级逻辑，别裸等。
> 对比记忆：CountDownLatch 一次性，减到 0 不能重置；要循环复用用 CyclicBarrier（基于 ReentrantLock + Condition 实现，支持 reset）。

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

### 12. JVM 常用调优参数与 OOM 排查
**常用启动参数（生产必须显式配置）：**
```bash
-Xms2g          # 初始堆大小（建议与-Xmx一致，避免扩容抖动）
-Xmx2g          # 最大堆大小
-Xmn512m        # 新生代大小
-Xss512k        # 每个线程栈大小
-XX:MetaspaceSize=256m
-XX:MaxMetaspaceSize=512m   # 元空间上限，防止类加载过多撑爆本地内存
-XX:+UseG1GC                # 使用G1收集器
-XX:MaxGCPauseMillis=200    # G1期望最大停顿时间
-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/data/dump/  # OOM自动导出堆快照
-XX:+PrintGCDetails -Xloggc:/data/logs/gc.log                  # 打印GC日志
```

> 🎯【面试题】线上OOM怎么排查？
> 参考答案：1. 先看报错类型：堆OOM（对象太多/内存泄漏）、元空间OOM（动态生成类过多，如反射/CGLIB）、栈溢出（递归过深）；2. 用`jps`定位进程，`jstack`看线程栈，`jmap -heap <pid>`看堆使用，`jmap -dump:format=b,file=heap.hprof <pid>`导出堆快照；3. 用MAT（Memory Analyzer）分析hprof文件，找占用内存最大的对象（Dominator Tree），追踪GC Roots引用链定位泄漏点。常见泄漏：静态集合缓存不清理、ThreadLocal不remove、连接/流未关闭、监听器未注销。

### 13. Minor GC、Major GC、Full GC 与 Full GC 触发场景
> 🎯【面试题】三种 GC 的区别？Full GC 什么情况下触发？
> 参考答案：
> 1. **Minor GC（Young GC）**：只回收新生代（Eden+S0+S1），Eden 满触发，频率高、速度快；
> 2. **Major GC**：回收老年代，老年代空间不足触发；严格说只有 CMS 等部分收集器有单独的 Major GC 概念，很多工具和日志里把它和 Full GC 混用，面试口径：**Major GC 侧重老年代，Full GC 是全堆**；
> 3. **Full GC**：清理新生代 + 老年代 + 元空间，频率低、STW 长、性能影响最大。
> Full GC 常见触发场景：
> 1. 老年代空间不足：对象晋升时放不下，或大对象直接进老年代放不下；
> 2. 元空间不足：类加载过多（反射、动态代理、CGLIB 滥用），触发 Full GC 尝试卸载无用类；
> 3. 代码调用 System.gc()：只是"建议"JVM 回收，默认通常触发 Full GC，生产建议 -XX:+DisableExplicitGC 禁掉；
> 4. 空间分配担保失败：Minor GC 前检查历次晋升老年代的平均大小大于老年代剩余连续空间，直接放弃 Minor GC 改为 Full GC；
> 5. CMS 并发清理期间用户线程产生浮动垃圾塞满老年代，发生 Concurrent Mode Failure，退化 Serial Old 单线程回收，STW 长达数秒。
> 补充：GC 核心环节伴随 STW（Stop The World），期间业务线程全部暂停；排查频繁 Full GC：jmap 导堆快照 → MAT 分析大对象和引用链 → 检查分代比例、元空间大小、内存泄漏、违规 System.gc()。

### 14. 内存泄漏 vs 内存溢出
> 🎯【面试题】内存泄漏和内存溢出是一回事吗？什么关系？
> 参考答案：
> **内存泄漏（Memory Leak）**：对象逻辑上已经没用了，但被失效的强引用链牵着，GC 无法回收——"占着茅坑不拉屎"，内存占用呈阶梯状上升。
> **内存溢出（OOM）**：申请内存时空间不够，GC 后依然不足——"茅坑不够用了"，程序直接抛 Error 崩溃。
> 关系：内存泄漏持续堆积是内存溢出的常见**诱因**；但 OOM 不一定是泄漏，也可能是堆参数太小、瞬时大对象太多。
> 常见泄漏场景：静态集合只 add 不 remove、ThreadLocal 不 remove（线程池复用）、数据库连接/IO 流未 close、非静态内部类隐式持有外部类、监听器未注销。
> 常见 OOM 类型：`OutOfMemoryError: Java heap space`（堆溢出）、`OutOfMemoryError: Metaspace`（动态生成类过多）、`StackOverflowError`（递归过深，栈溢出）。
> 排查难度：泄漏更难——OOM 当场崩溃有报错线索；泄漏早期无感，只能靠监控内存曲线 + 堆快照引用链定位。

### 15. 堆内存分代结构与对象晋升规则
> 🎯【面试题】堆怎么划分？Eden 和 Survivor 比例是多少？对象什么时候晋升老年代？
> 参考答案：
> 堆 = 新生代 + 老年代，默认比例 1:2（-XX:NewRatio=2）；新生代内部 Eden : S0 : S1 = 8 : 1 : 1（-XX:SurvivorRatio=8）。
> 分代依据是"绝大多数对象朝生夕灭"的经验法则：新生代存活率低用复制算法，老年代存活率高用标记整理/清除。
> Survivor 分两块的原因：复制算法每次把 Eden + 一个 Survivor 的存活对象复制到另一个空的 Survivor，保证空间连续无碎片，两个 Survivor（From/To）交替使用。
> 晋升老年代的规则：
> 1. 年龄达标：对象每熬过一次 Minor GC 年龄 +1，默认 15 次晋升（-XX:MaxTenuringThreshold=15，因为对象头 MarkWord 里年龄字段只有 4 位，最大就是 15）；
> 2. 大对象直接进老年代（-XX:PretenureSizeThreshold），避免在 Survivor 之间来回复制消耗性能；
> 3. 动态年龄判断：Survivor 中同龄对象总大小超过 Survivor 空间一半，该年龄及以上的对象直接晋升；
> 4. Minor GC 后存活对象放不进 Survivor，靠空间分配担保直接进老年代。
> ⚠️注意：JDK8 起方法区实现为元空间，使用本地内存，**不属于堆**，不要说"方法区在堆里"；G1 下逻辑分代、物理不分代，堆是等大的 Region。

### 16. 垃圾收集器演进：Serial → Parallel → CMS → G1 → ZGC
> 🎯【面试题】垃圾收集器是怎么演进的？CMS 有什么缺陷为什么被移除？
> 参考答案：演进主线：吞吐量优先 → 响应时间优先 → 停顿可预测 → 超低延迟。
> 1. Serial / ParNew：单线程 / 多线程，回收全程 STW；
> 2. Parallel Scavenge / Parallel Old：吞吐量优先，JDK8 默认组合；
> 3. CMS（并发标记清除）：首次实现 GC 线程与用户线程并发，STW 大幅缩短。三大硬伤：标记清除产生**内存碎片**，大对象找不到连续空间触发 Concurrent Mode Failure，退化为 Serial Old 单线程全堆回收（数秒级 STW，生产灾难）；并发阶段产生**浮动垃圾**，需要预留空间，内存利用率低；对 CPU 资源敏感。JDK9 废弃、JDK14 移除；
> 4. G1（JDK9 起默认）：堆切成约 2048 个等大 Region，逻辑分代物理不分代；Region 间局部复制算法基本解决碎片；**停顿预测模型**，-XX:MaxGCPauseMillis 指定期望停顿，优先回收垃圾占比最高的 Region（Garbage First 得名）；Mixed GC 混合回收新生代 + 部分老年代 Region；Remembered Set 维护跨 Region 引用，代价约 10%~20% 堆内存开销。适合 4G~32G 大内存；
> 5. ZGC / Shenandoah（JDK11+）：染色指针 + 读屏障，几乎所有阶段与用户线程并发，停顿不随堆增大而增长，目标亚毫秒级，面向 TB 级超大堆低延迟场景。
> 选型口径：<4G 用 Parallel 或 G1 都行；4G~32G 首选 G1；更大堆或金融级低延迟场景 ZGC。
> ⚠️注意：G1 也有 STW（初始标记、最终标记等阶段），不是全程并发；做到接近全程并发的目前是 ZGC。

### 17. JVM 内存区域和 JMM 的区别
> 🎯【面试题】JMM 和 JVM 运行时数据区是一回事吗？
> 参考答案：不是一回事，考察维度完全不同：
> **JVM 运行时数据区**：内存分配的**物理布局**，回答"数据存在哪、哪块会 OOM"——堆、虚拟机栈、方法区（元空间）、程序计数器等，关注 GC 和内存划分。
> **JMM（Java 内存模型）**：线程通信的**抽象规范**，回答"多线程变量怎么同步"——抽象出主内存、工作内存，定义 happens-before 规则，保证并发三特性（原子性、可见性、有序性），关注并发安全和指令重排。
> 类比：JVM 内存区域是办公楼的楼层分布；JMM 是员工协作的规章制度。
> 联系：JMM 的主内存可类比堆（存对象实例数据），工作内存类比 CPU 缓存/寄存器——只是抽象概念并不真实存在；JMM 的目的是屏蔽不同硬件平台的内存访问差异。
> ⚠️注意：别说"JMM 的主内存就是堆"。关联高频考点：volatile 的可见性/有序性（内存屏障）、happens-before 规则。

### 18. JDBC 如何打破双亲委派（线程上下文类加载器）
> 🎯【面试题】DriverManager 由 Bootstrap 加载，为什么能加载到 classpath 里的 MySQL 驱动？
> 参考答案：
> 矛盾：DriverManager、Connection 等核心类在 JDK 核心库中，由 Bootstrap ClassLoader 加载；MySQL 驱动实现类在项目 classpath，由 Application ClassLoader 加载。双亲委派是"子委托父"，**父加载器看不到子加载器加载的类**——按正常委派，Bootstrap 根本找不到驱动实现类。
> 解决方案：线程上下文类加载器（TCCL）。每个线程自带 contextClassLoader 属性，默认设置为 Application ClassLoader。JDK 核心代码不用"自己"的加载器，而是借道线程上下文实现**逆向加载**：
> 1. 调用 DriverManager.getConnection(url)，内部触发 SPI：ServiceLoader.load(Driver.class)；
> 2. ServiceLoader 取 `Thread.currentThread().getContextClassLoader()`（即 AppClassLoader）；
> 3. 用它读取 META-INF/services/java.sql.Driver 配置文件，加载并实例化 com.mysql.cj.jdbc.Driver；
> 4. 驱动类静态代码块把自己注册进 DriverManager。
> 本质：让父加载器"反向使用"子加载器加载的类，破坏了双亲委派，是 SPI 机制的标准套路；同类场景还有 JNDI、Tomcat、Spring 框架加载用户 Bean。JDBC 4.0 起无需手写 `Class.forName("com.mysql.jdbc.Driver")`，SPI 自动完成驱动注册。

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
6. 日期时间 API（LocalDate / LocalDateTime / Duration / Period）
> 🎯【面试题】为什么用新日期时间API替代Date和Calendar？
> 参考答案：旧的Date/Calendar存在严重设计缺陷：可变对象（多线程共享不安全）、月份从0开始、SimpleDateFormat线程不安全、设计混乱（Date既表示日期又表示时间戳）。Java8引入`java.time`包：所有对象不可变、线程安全；LocalDate日期、LocalTime时间、LocalDateTime日期时间、Instant时间戳、Duration秒级时长、Period天级时长。

```java
LocalDateTime now = LocalDateTime.now();                 // 当前时间
LocalDateTime time = LocalDateTime.of(2026, 8, 25, 10, 30); // 指定时间
LocalDateTime tomorrow = now.plusDays(1);                // 加一天，返回新对象
String str = now.format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));
LocalDateTime parsed = LocalDateTime.parse("2026-08-25 10:30:00",
        DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));
System.out.println(Duration.between(now, tomorrow).toHours()); // 时长
```
> 注意：格式化统一使用线程安全的`DateTimeFormatter`（不可变）；不要多线程共享`new SimpleDateFormat()`。

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

### 6. 反射核心 API、setAccessible 与性能优化
> 🎯【面试题】反射常用 API 有哪些？setAccessible(true) 是干什么的？反射性能差在哪、怎么优化？
> 参考答案：
> 核心 API（java.lang.reflect 包）：Class 反射入口；Field 字段；Method 方法；Constructor 构造器。常用操作：`clazz.getDeclaredConstructor().newInstance()` 创建实例（Class.newInstance() 已过时）；`method.invoke(obj, 参数)` 调用方法；`field.get/set` 读写字段。
> 注意区分：getMethod 只能拿 public 方法（含继承的）；getDeclaredMethod 拿本类声明的所有方法（含 private，但不含父类的）。
> setAccessible(true)：绕过 Java 语言级访问检查，让反射可以访问 private 成员。Spring 依赖注入私有字段、Jackson 序列化私有字段都靠它，代价是破坏封装，一般只在框架内部使用。
> 性能问题：反射调用要经过方法查找、访问安全检查、参数装箱，且 JIT 难以内联，比直接调用慢几十倍。优化手段：
> 1. 缓存 Method/Field/Class 对象，框架启动时解析一次复用（Spring 的 Bean 元信息就是这样缓存的）；
> 2. setAccessible(true) 顺带跳过访问检查；
> 3. JDK9+ 可用 MethodHandle/VarHandle 替代部分反射，对 JIT 更友好。
> ⚠️注意：反射是框架基石——Spring 扫描 @Service 后反射创建 Bean、@Autowired 反射注入、Spring MVC 反射调用 @RequestMapping 方法、MyBatis 反射映射结果集，理解反射才能看懂框架源码。

### 7. 单例模式五种写法与 DCL 细节
> 🎯【面试题】手写单例模式有哪几种写法？各有什么优缺点？
> 参考答案：饿汉式、懒汉式、DCL 双重检查锁、静态内部类、枚举五种：
> 1. 饿汉式：类加载即创建静态实例，靠类加载机制保证线程安全；不支持懒加载，实例一直占内存；
> 2. 懒汉式：第一次调用才创建，实现懒加载，但多线程下会创建多个实例，线程不安全；方法上加 synchronized 的版本安全但每次获取实例都要抢锁，性能差；
> 3. DCL 双重检查锁：两次判空 + synchronized + volatile，兼顾懒加载和性能，推荐；
> 4. 静态内部类：第一次调用 getInstance 才加载 Holder 类，由 JVM 类加载保证线程安全，天然懒加载，代码优雅，推荐；
> 5. 枚举：《Effective Java》最推荐，天然线程安全、绝对防止多次实例化，还能**防御反射和序列化破坏**——反射 newInstance 枚举会直接抛 IllegalArgumentException，枚举反序列化也不会生成新对象。
> DCL 细节：`new Singleton()` 不是原子操作，分三步——分配内存、初始化对象、引用指向内存。volatile 禁止后两步指令重排，否则线程 A 执行到"引用已赋值但对象还没初始化完"时，线程 B 第一次判空非 null 直接返回，拿到**半初始化对象**。

```java
// DCL 双重检查锁
public class Singleton {
    private static volatile Singleton instance; // volatile 防止指令重排
    private Singleton() {}
    public static Singleton getInstance() {
        if (instance == null) {                 // 第一次判空：避免不必要的加锁
            synchronized (Singleton.class) {
                if (instance == null) {         // 第二次判空：保证只创建一个实例
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
// 静态内部类
public class Singleton2 {
    private Singleton2() {}
    private static class Holder {
        private static final Singleton2 INSTANCE = new Singleton2();
    }
    public static Singleton2 getInstance() { return Holder.INSTANCE; }
}
```

| 写法 | 线程安全 | 懒加载 | 防反射/序列化破坏 | 备注 |
| ---- | ------ | ------ | ---------------- | ---- |
| 饿汉式 | ✅ | ❌ | ❌ | 简单，必用场景 |
| 懒汉式 | ❌ | ✅ | ❌ | 多线程不可用 |
| DCL | ✅ | ✅ | ❌ | 注意 volatile |
| 静态内部类 | ✅ | ✅ | ❌ | 推荐写法 |
| 枚举 | ✅ | ❌ | ✅ | 最安全 |

### 8. 责任链、策略、代理与适配器模式的区别
> 🎯【面试题】责任链和策略模式有什么区别？代理和适配器怎么区分？
> 参考答案：
> **责任链模式**：多个处理器串成一条链，请求沿链传递，能处理就处理，不能就传给下一个；每个处理器持有下一个处理器的引用，发送者不关心最终谁处理。典型应用：Servlet Filter 过滤器链、Spring Security 鉴权链、Netty Pipeline、网关的多层校验。
> **策略模式**：定义一组可互相替换的算法，客户端**选定其中一个**执行，用来消除大量 if-else；各策略平级、互不感知。典型应用：支付方式选择、促销折扣计算，ThreadPoolExecutor 的四种拒绝策略本身就是策略模式。
> 一句话区分：策略是"横向选一个执行"；责任链是"纵向传递，可能一个处理、多个处理，甚至没人处理"。
> **代理模式**：代理类和目标类实现**同一个接口**，不改变接口，在调用前后增加控制逻辑（日志、事务、权限、缓存、延迟加载）。Spring AOP 本质就是动态代理。
> **适配器模式**：把一个类的接口**转换**成客户端期望的另一个接口，两边接口不同，解决"接口不兼容无法协作"的问题。典型应用：InputStreamReader 把 InputStream 字节流适配成 Reader 字符流、老系统接口改造。
> 一句话区分：代理是"接口相同做增强"，适配器是"接口不同做转换"。

## 十一、Java 基础高频八股总复习清单
