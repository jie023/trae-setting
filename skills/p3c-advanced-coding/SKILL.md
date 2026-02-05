---
name: "p3c-advanced-coding"
description: "Provides advanced coding standards including collection handling, concurrency, control flow, and thread safety. Invoke when user asks about multithreading, collections, or complex control logic."
---

# P3C 高级编程规范

本技能提供阿里巴巴Java开发手册中的高级编程相关规范，包括集合处理、并发处理和控制语句。

## 集合处理

### hashCode和equals
1. 【强制】关于`hashCode`和`equals`的处理，遵循如下规则：
   - 只要重写`equals`，就必须重写`hashCode`
   - 因为Set存储的是不重复的对象，依据`hashCode`和`equals`进行判断，所以Set存储的对象必须重写这两个方法
   - 如果自定义对象作为Map的键，那么必须重写`hashCode`和`equals`

### subList使用
2. 【强制】ArrayList的subList结果不可强转成ArrayList，否则会抛出ClassCastException异常
3. 【强制】在subList场景中，**高度注意**对原集合元素个数的修改，会导致子列表的遍历、增加、删除均会产生ConcurrentModificationException 异常

### 集合转数组
4. 【强制】使用集合转数组的方法，必须使用集合的toArray(T[] array)，传入的是类型完全一样的数组，大小就是`list.size()`

### 数组转集合
5. 【强制】使用工具类Arrays.asList()把数组转换成集合时，不能使用其修改集合相关的方法，它的add/remove/clear方法会抛出UnsupportedOperationException异常

### 泛型通配符
6. 【强制】泛型通配符`<? extends T>`来接收返回的数据，此写法的泛型集合不能使用add方法，而`<? super T>`不能使用get方法，作为接口调用赋值时易出错

### foreach循环
7. 【强制】不要在foreach循环里进行元素的remove/add操作。remove元素请使用Iterator方式，如果并发操作，需要对Iterator对象加锁

### Comparator规范
8. 【强制】在JDK7版本及以上，`Comparator`要满足如下三个条件，不然`Arrays.sort`，`Collections.sort`会报IllegalArgumentException异常
   - x，y的比较结果和y，x的比较结果相反
   - x>y，y>z，则x>z
   - x=y，则x，z比较结果和y，z比较结果相同

### 集合初始化
9. 【推荐】集合初始化时，指定集合初始值大小

### Map遍历
10. 【推荐】使用entrySet遍历Map类集合KV，而不是keySet方式进行遍历

### Map的null值
11. 【推荐】高度注意Map类集合K/V能不能存储null值的情况

| 集合类            | Key          | Value        | Super       | 说明                   |
|-------------------|--------------|--------------|-------------|------------------------|
| Hashtable         | 不允许为null | 不允许为null | Dictionary  | 线程安全               |
| ConcurrentHashMap | 不允许为null | 不允许为null | AbstractMap | 锁分段技术（JDK8:CAS）  |
| TreeMap           | 不允许为null | 允许为null   | AbstractMap | 线程不安全             |
| HashMap           | 允许为null   | 允许为null   | AbstractMap | 线程不安全             |

### 集合有序性
12. 【参考】合理利用好集合的有序性(sort)和稳定性(order)，避免集合的无序性(unsort)和不稳定性(unorder)带来的负面影响

### Set去重
13. 【参考】利用Set元素唯一的特性，可以快速对一个集合进行去重操作，避免使用List的contains方法进行遍历、对比、去重操作

## 并发处理

### 单例线程安全
1. 【强制】获取单例对象需要保证线程安全，其中的方法也要保证线程安全

### 线程命名
2. 【强制】创建线程或线程池时请指定有意义的线程名称，方便出错时回溯

### 线程池使用
3. 【强制】线程资源必须通过线程池提供，不允许在应用中自行显式创建线程
4. 【强制】线程池不允许使用Executors去创建，而是通过ThreadPoolExecutor的方式，这样的处理方式让写的同学更加明确线程池的运行规则，规避资源耗尽的风险

### SimpleDateFormat线程安全
5. 【强制】SimpleDateFormat 是线程不安全的类，一般不要定义为static变量，如果定义为static，必须加锁，或者使用DateUtils工具类

### 锁性能
6. 【强制】高并发时，同步调用应该去考量锁的性能损耗。能用无锁数据结构，就不要用锁；能锁区块，就不要锁整个方法体；能用对象锁，就不要用类锁

### 死锁预防
7. 【强制】对多个资源、数据库表、对象同时加锁时，需要保持一致的加锁顺序，否则可能会造成死锁

### 并发更新
8. 【强制】并发修改同一记录时，避免更新丢失，需要加锁。要么在应用层加锁，要么在缓存加锁，要么在数据库层使用乐观锁，使用version作为更新依据

### 定时任务
9. 【强制】多线程并行处理定时任务时，Timer运行多个TimeTask时，只要其中之一没有捕获抛出的异常，其它任务便会自动终止运行，使用`ScheduledExecutorService`则没有这个问题

### CountDownLatch
10. 【推荐】使用`CountDownLatch`进行异步转同步操作，每个线程退出前必须调用countDown方法，线程执行代码注意catch异常，确保countDown方法被执行到，避免主线程无法执行至await方法，直到超时才返回结果

### Random使用
11. 【推荐】避免Random实例被多线程使用，虽然共享该实例是线程安全的，但会因竞争同一seed 导致的性能下降

### 双重检查锁
12. 【推荐】在并发场景下，通过双重检查锁（double-checked locking）实现延迟初始化的优化问题隐患(可参考 The "Double-Checked Locking is Broken" Declaration)，推荐解决方案中较为简单一种（适用于JDK5及以上版本），将目标属性声明为 volatile型

### volatile
13. 【参考】volatile解决多线程内存不可见问题。对于一写多读，是可以解决变量同步问题，但是如果多写，同样无法解决线程安全问题

### HashMap并发
14. 【参考】HashMap在容量不够进行resize时由于高并发可能出现死链，导致CPU飙升，在开发过程中可以使用其它数据结构或加锁来规避此风险

### ThreadLocal
15. 【参考】`ThreadLocal`无法解决共享对象的更新问题，`ThreadLocal`对象建议使用static修饰

## 控制语句

### switch语句
1. 【强制】在一个switch块内，每个case要么通过break/return等来终止，要么注释说明程序将继续执行到哪一个case为止；在一个switch块内，都必须包含一个default语句并且放在最后，即使空代码

### 大括号使用
2. 【强制】在if/else/for/while/do语句中必须使用大括号。即使只有一行代码，避免采用单行的编码方式：`if (condition) statements;`

### 高并发判断
3. 【强制】在高并发场景中，避免使用"等于"判断作为中断或退出的条件

### if-else优化
4. 【推荐】表达异常的分支时，少用if-else方式，这种方式可以改写成：
    ```java
    if (condition) {
        ...
        return obj;
    }
    // 接着写else的业务逻辑代码;
    ```
   - 说明：如果非得使用if()...else if()...else...方式表达逻辑，【强制】避免后续代码维护困难，请勿超过3层

### 条件判断优化
5. 【推荐】除常用方法（如getXxx/isXxx）等外，不要在条件判断中执行其它复杂的语句，将复杂逻辑判断的结果赋值给一个有意义的布尔变量名，以提高可读性

### 循环性能
6. 【推荐】循环体中的语句要考量性能，以下操作尽量移至循环体外处理，如定义对象、变量、获取数据库连接，进行不必要的try-catch操作

### 取反逻辑
7. 【推荐】避免采用取反逻辑运算符

### 接口入参保护
8. 【推荐】接口入参保护，这种场景常见的是用作批量操作的接口

### 参数校验
9. 【参考】下列情形，需要进行参数校验：
   - 调用频次低的方法
   - 执行时间开销很大的方法
   - 需要极高稳定性和可用性的方法
   - 对外提供的开放接口，不管是RPC/API/HTTP接口
   - 敏感权限入口

10. 【参考】下列情形，不需要进行参数校验：
    - 极有可能被循环调用的方法
    - 底层调用频度比较高的方法
    - 被声明成private只会被自己代码所调用的方法
