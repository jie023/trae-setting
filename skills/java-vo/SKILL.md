---
name: "java-vo"
description: "Provides Java VO (Value Object) generation standards including VO classification, naming conventions, class structure, field specifications, and annotation usage. Invoke when generating VO classes or converting DO to VO."
---

# Java VO映射规范

本技能提供Java VO（Value Object）的生成规范，包括VO分类、命名规范、类结构、字段规范和注解使用。

## VO文件分类

### 1. 业务DataIdVO
- 用途：仅包含业务主键Id，用于简单的Id传递和验证
- 示例：AdminDepartmentDataIdVO、MemberFeedbackDataIdVO
- 特点：结构简单，通常只有一个Id字段

### 2. 业务DataSaveVO
- 用途：用于接收前端提交的业务数据，进行数据保存或更新
- 示例：AdminDepartmentDataSaveVO、MemberFeedbackDataSaveVO
- 特点：包含完整的业务字段，支持JSR-380验证注解

### 3. 业务DataShowVO
- 用途：用于向前端展示业务数据，包含完整的业务信息
- 示例：AdminDepartmentDataShowVO、MemberFeedbackDataShowVO
- 特点：包含所有需要展示的字段，可能关联其他DTO

## 命名规范

### 类名规范
- 使用大驼峰命名法
- 格式：业务名称+Data+功能(Id、Save、Show) + VO结尾
- 示例：
  - AdminDepartmentDataIdVO
  - AdminDepartmentDataSaveVO
  - AdminDepartmentDataShowVO

### 字段名规范
- 使用小驼峰命名法
- 示例：adminDepartmentId、adminDepartmentName、adminDepartmentOrder

## 类结构规范

### 标准类结构
```java
package com.weetion.serverModule.业务模块.pojo.VO.业务功能文件夹;

import javax.validation.constraints.*;
import lombok.Data;

/**
 * 类功能描述
 *
 * @Author: 作者
 * @Date: 日期
 */
@Data
@ToString
public class 业务DataSaveVO {
    // 字段定义
}
```

### Lombok注解规范
- 优先使用Lombok注解自动生成getter、setter和toString方法
- 详细规范请参考java-common-standards技能
- 常用注解：
  - @Data：自动生成getter、setter、toString、equals、hashCode方法
  - @Getter：自动生成getter方法
  - @Setter：自动生成setter方法
  - @ToString：自动生成toString方法

## 字段规范

### 基本类型
- 基本数据类型优先使用包装类（如Integer代替int）
- 示例：
```java
private Integer adminDepartmentOrder; // 推荐
private int adminDepartmentOrder; // 不推荐
```

### 集合类型
- 使用泛型指定集合元素类型
- 示例：
```java
private List<String> adminDepartmentList; // 推荐
private List adminDepartmentList; // 不推荐
```

### 依赖管理
- 优先使用项目内部的DTO和VO
- 避免循环依赖
- 示例：
```java
private AdminDepartmentDataShowVO superiorDepartment; // 推荐
private Object superiorDepartment; // 不推荐
```

## 注释规范

### 类注释
- 使用/** */格式
- 包含类的用途、作者和创建日期
- 示例：
```java
/**
 * 管理员部门表
 *
 * @Author: 吕旭龙
 * @Date: 2022-11-23
 */
```

### 字段注释
- 使用//格式
- 说明字段的业务含义
- 示例：
```java
private String adminDepartmentId; //管理员部门Id
private String adminDepartmentName; //管理员部门名称
```

### 注释语言
- 统一使用中文
- 避免歧义和错别字
- 保持术语一致性

## 验证注解使用

### 基本验证注解
- @NotBlank：非空字符串验证
- @NotNull：非空对象验证
- @Min/@Max：数值范围验证
- @Size：长度验证
- @Email：邮箱格式验证
- @Pattern：自定义正则验证

### 注解组合
- 可以组合使用多个验证注解
- 示例：
```java
@NotBlank(message = "请输入用户名")
@Size(min = 6, max = 20, message = "用户名长度必须在6-20位之间")
private String username;
```

### 错误消息
- message属性必须使用中文
- 消息要简洁明了，便于用户理解
- 示例：
```java
@NotBlank(message = "请输入用户名") // 推荐
@NotBlank(message = "username is required") // 不推荐
```

## 最佳实践

1. **统一命名**：遵循驼峰命名法，保持命名一致性
2. **包装类优先**：基本数据类型优先使用包装类
3. **泛型使用**：集合类型必须使用泛型
4. **避免循环**：避免VO之间的循环依赖
5. **注释完整**：所有字段必须有中文注释
6. **验证注解**：合理使用验证注解，提供友好的错误提示
7. **toString方法**：重写toString方法便于调试
