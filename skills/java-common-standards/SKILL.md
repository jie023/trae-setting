---
name: "java-common-standards"
description: "Provides common Java coding standards including hutool tool usage and Lombok annotations. Invoke when generating Java code (DO, VO, Service, Controller) or applying coding standards."
---

# Java通用编码规范

本技能提供Java通用编码规范，包括工具类使用规范和Lombok注解使用规范，适用于所有Java类的生成。

## Java工具类使用规范 *** 强制执行 ***

### hutool工具类优先使用
- **优先使用hutool工具类**：在生成Java代码时，对于字符串处理、日期时间、集合操作、IO操作、加密解密等常用功能，优先使用hutool工具类
- **使用原则**：避免重复造轮子，优先使用hutool提供的成熟工具方法，提高代码质量和开发效率

### hutool常用工具类
- **字符串处理**：StrUtil、StringUtils
- **日期时间**：DateUtil、DateTime
- **集合操作**：CollUtil、ListUtil、MapUtil
- **IO操作**：IoUtil、FileUtil
- **加密解密**：SecureUtil、DigestUtil
- **对象转换**：BeanUtil、Convert
- **数字处理**：NumberUtil、MathUtil

### 使用示例
```java
// 字符串判空
if (StrUtil.isNotBlank(str)) {
    // 处理逻辑
}

// 日期格式化
String dateStr = DateUtil.format(new Date(), "yyyy-MM-dd HH:mm:ss");

// 集合判空
if (CollUtil.isNotEmpty(list)) {
    // 处理逻辑
}

// 对象属性拷贝
BeanUtil.copyProperties(source, target);
```

## Lombok注解使用规范 *** 强制执行 ***

### Lombok注解优先使用
- **优先使用Lombok注解**：在生成Java代码时，对于getter/setter方法和toString方法，优先使用Lombok注解
- **使用原则**：避免手动编写重复的样板代码，优先使用Lombok提供的注解，提高代码简洁性和可维护性

### 常用注解
- **@Data**：自动生成getter、setter、toString、equals、hashCode方法
- **@Getter**：自动生成getter方法
- **@Setter**：自动生成setter方法
- **@ToString**：自动生成toString方法

### 注解使用示例
```java
import lombok.Data;
import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

// 使用@Data注解（推荐）
@Data
@ToString
public class UserVO {
    private String userId;
    private String userName;
}

// 单独使用注解
@Getter
@Setter
@ToString
public class UserVO {
    private String userId;
    private String userName;
}
```

### 注意事项
- 不使用@NoArgsConstructor和@AllArgsConstructor注解
- 优先使用@Data注解，需要时可以单独使用@Getter、@Setter、@ToString
- 确保项目中已添加Lombok依赖

## 最佳实践

1. **工具类选择**：优先使用hutool工具类，其次考虑Apache Commons、Guava等
2. **注解使用**：合理使用Lombok注解，避免过度使用
3. **代码简洁**：减少样板代码，提高代码可读性
4. **性能考虑**：选择性能最优的工具类方法
5. **异常处理**：使用工具类时注意异常处理机制

## 适用范围

本规范适用于以下Java类的生成：
- DO（Data Object）
- VO（Value Object）
- Service接口和实现类
- Controller类
- 其他业务类

## 调用时机

在以下情况下调用本技能：
- 生成新的Java类时
- 重构现有Java代码时
- 应用编码规范时
- 代码审查时
