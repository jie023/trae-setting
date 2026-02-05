---
name: "p3c-oop-standards"
description: "Provides OOP programming standards including method overriding, equals/hashCode, wrapper types, serialization, and access control. Invoke when user asks about OOP best practices or class design patterns."
---

# P3C OOP编程规范

本技能提供阿里巴巴Java开发手册中的OOP相关规范。

## 静态访问

1. 【强制】避免通过对象引用访问静态变量或静态方法，直接用类名访问

## 方法覆写

2. 【强制】所有覆写方法必须加@Override注解

## 可变参数

3. 【强制】相同参数类型、相同业务含义才可使用可变参数，避免使用Object
   - 可变参数必须放置在参数列表最后

## 接口稳定性

4. 【强制】外部调用或二方库依赖的接口不允许修改方法签名，过时接口加@Deprecated注解

## 过时方法

5. 【强制】不能使用过时的类或方法

## equals方法

6. 【强制】Object的equals方法易抛空指针，应使用常量或确定有值的对象调用equals
   - 正例："test".equals(object);
   - 反例：object.equals("test");
   - 推荐使用java.util.Objects#equals

## 包装类比较

7. 【强制】相同类型包装类对象之间值的比较，全部使用equals方法

## 基本类型与包装类型

8. 关于基本数据类型与包装数据类型的使用标准：
   - 【强制】所有POJO类属性必须使用包装数据类型
   - 【强制】RPC方法返回值和参数必须使用包装数据类型
   - 【推荐】所有局部变量使用基本数据类型

## POJO默认值

9. 【强制】定义DO/DTO/VO等POJO类时，不要设定任何属性默认值

## 序列化

10. 【强制】序列化类新增属性时，请不要修改serialVersionUID字段

## 构造方法

11. 【强制】构造方法中禁止加入任何业务逻辑，初始化逻辑放在init方法中

## toString方法

12. 【强制】POJO类必须写toString方法，继承POJO类时在前面加super.toString()

## split方法

13. 【推荐】使用索引访问String的split方法得到的数组时，需做最后一个分隔符后有无内容的检查

## 方法顺序

14. 【推荐】多个构造方法或同名方法应按顺序放置在一起

15. 【推荐】类内方法定义顺序：公有或保护方法 > 私有方法 > getter/setter方法

## setter/getter

16. 【推荐】setter方法中参数名称与类成员变量名称一致，getter/setter方法中不要增加业务逻辑

## 字符串连接

17. 【推荐】循环体内字符串连接使用StringBuilder的append方法

## final关键字

18. 【推荐】final关键字使用场景：
    - 不允许被继承的类
    - 不允许修改引用的域对象
    - 不允许被重写的方法
    - 不允许运行过程中重新赋值的局部变量
    - 避免上下文重复使用变量

## clone方法

19. 【推荐】慎用Object的clone方法，默认是浅拷贝

## 访问控制

20. 【推荐】类成员与方法访问控制从严：
    - 不允许外部直接new创建对象，构造方法必须是private
    - 工具类不允许有public或default构造方法
    - 类非static成员变量与子类共享，必须是protected
    - 类非static成员变量仅在本类使用，必须是private
    - 类static成员变量仅在本类使用，必须是private
    - 若是static成员变量，必须考虑是否为final
    - 类成员方法只供类内部调用，必须是private
    - 类成员方法只对继承类公开，限制为protected
