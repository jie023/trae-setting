---
name: "p3c-exception-logging"
description: "Provides exception handling and logging standards including NPE prevention, try-catch-finally best practices, SLF4J usage, and log management. Invoke when user asks about exception handling or logging best practices."
---

# P3C 异常与日志规范

本技能提供阿里巴巴Java开发手册中的异常处理和日志相关规范。

## 异常处理

### 异常捕获原则

1. 【强制】不要通过catch方式处理可预检查的RuntimeException（如NullPointerException、IndexOutOfBoundsException）
   - 正例：`if (obj != null) {...}`
   - 反例：`try { obj.method() } catch (NullPointerException e) {…}`

2. 【强制】异常不要用来做流程控制、条件控制

3. 【强制】catch时区分稳定代码和非稳定代码，对非稳定代码尽可能区分异常类型

### 异常处理要求

4. 【强制】捕获异常是为了处理它，不要捕获却什么都不处理

5. 【强制】try块在事务代码中，catch异常后需要回滚事务时必须手动回滚

6. 【强制】finally块必须对资源对象、流对象进行关闭，有异常也要做try-catch

7. 【强制】不要在finally块中使用return

8. 【强制】捕获异常与抛异常必须完全匹配，或捕获异常是抛异常的父类

### 返回值规范

9. 【推荐】方法返回值可以为null，不强制返回空集合或空对象，必须添加注释说明null情况

### NPE防护

10. 【推荐】防止NPE产生的场景：
    - 基本数据类型return包装对象时自动拆箱
    - 数据库查询结果可能为null
    - 集合元素即使isNotEmpty也可能为null
    - 远程调用返回对象必须进行空指针判断
    - Session中获取的数据建议NPE检查
    - 级联调用obj.getA().getB().getC()
    - 推荐使用JDK8的Optional类

### 自定义异常

11. 【推荐】区分unchecked/checked异常，避免直接抛出new RuntimeException()，不允许抛出Exception或Throwable

### 错误码与异常

12. 【参考】外部http/api接口使用错误码；应用内部推荐异常抛出；跨应用RPC优先使用Result方式

### DRY原则

13. 【参考】避免重复代码（DRY原则），必要时抽取共性方法或抽象公共类

## 日志规约

### 日志框架

1. 【强制】应用中不可直接使用日志系统（Log4j、Logback）中的API，应依赖使用SLF4J中的API
   ```java
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   private static final Logger logger = LoggerFactory.getLogger(Abc.class);
   ```

### 日志保存

2. 【强制】日志文件推荐至少保存15天

### 日志命名

3. 【强制】扩展日志命名：appName_logType_logName.log
   - logType：stats/monitor/visit等
   - logName：日志描述
   - 正例：mppserver_monitor_timeZoneConvert.log

### 日志输出

4. 【强制】trace/debug/info级别日志必须使用条件输出或占位符方式
   - 正例（条件）：
     ```java
     if (logger.isDebugEnabled()) {
         logger.debug("Processing trade with id: " + id);
     }
     ```
   - 正例（占位符）：
     ```java
     logger.debug("Processing trade with id: {} ", id);
     ```

5. 【强制】避免重复打印日志，在log4j.xml中设置additivity=false

6. 【强制】异常信息应包括案发现场信息和异常堆栈信息

### 日志级别

7. 【推荐】生产环境禁止输出debug日志；有选择地输出info日志；谨慎使用warn级别

8. 【推荐】使用warn日志级别记录用户输入参数错误，非必要不使用error级别

## 其他最佳实践

### 正则表达式

1. 【强制】利用正则表达式预编译功能，不要在方法体内定义Pattern

### 模板引擎

2. 【强制】velocity调用POJO类属性时直接使用属性名，模板引擎会自动调用getXxx()

3. 【强制】后台输送给页面的变量必须加$!{var}

### 随机数

4. 【强制】Math.random()返回double类型，范围0≤x<1，获取整数随机数使用Random对象的nextInt或nextLong方法

### 时间获取

5. 【强制】获取当前毫秒数：`System.currentTimeMillis();` 而不是 `new Date().getTime();`

### 视图模板

6. 【推荐】不要在视图模板中加入任何复杂的逻辑

### 数据结构

7. 【推荐】任何数据结构的构造或初始化都应指定大小

### 代码清理

8. 【推荐】及时清理不再使用的代码段或配置信息
