---
name: "java-do"
description: "Provides database type to Java type mapping standards for DO (Data Object) generation. Includes type conversion rules and special field handling. Invoke when generating DO files from SQL table structures."
---

# Java DO映射规范

本技能提供数据库类型到Java类型的映射规范，用于将SQL表结构转换为Java DO文件。

## 数据库类型映射为Java类型

### 基本类型映射
| 数据库类型 | Java类型 | 说明 |
|-----------|----------|------|
| varchar | String | 可变长度字符串 |
| char | String | 定长字符串 |
| int | Integer | 整数（推荐使用包装类） |
| tinyint | Integer | 小整数 |
| smallint | Integer | 短整数 |
| bigint | Long | 长整数 |
| decimal | BigDecimal | 精确小数 |
| float | Float | 浮点数（不推荐） |
| double | Double | 双精度浮点数（不推荐） |
| datetime | Date | 日期时间 |
| date | Date | 日期 |
| time | Date | 时间 |
| timestamp | Date | 时间戳 |
| text | String | 长文本 |
| longtext | String | 超长文本 |
| blob | byte[] | 二进制数据 |

### 类型映射注意事项
1. **包装类优先**：基本数据类型优先使用包装类（如Integer代替int），避免null值问题
2. **精度要求**：涉及金额、精度要求的字段必须使用BigDecimal，禁止使用float和double
3. **时间类型**：日期时间相关字段统一使用Date类型
4. **大文本处理**：text、longtext、clob字段映射为String类型

## 特殊场景处理

### 复合主键
- 使用多个字段组合表示
- 在注释中标注复合主键关系

### 逻辑删除字段 *** 强制执行 ***
- is_deleted、delete_state字段不生成到DO中

### 枚举类型
- 映射为String或Integer类型
- 在注释中说明枚举值含义

### JSON字段
- 映射为String类型
- 在注释中说明JSON结构

### 大文本字段
- text、longtext、clob映射为String类型
- 在注释中说明大文本用途

### 自增字段 *** 强制执行 ***
- 自增的id字段不生成到DO中

### 时间戳字段 *** 强制执行 ***
- create_time、update_time、delete_time等时间字段不生成到DO中

## 类结构规范

### 标准类结构
```java
package com.weetion.serverModule.业务模块.pojo.DO;

import lombok.Data;

/**
 * 表功能描述
 *
 * @Author: 作者
 * @Date: 日期
 */
@Data
@ToString
public class 表名DO {
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

## 字段注释规范

### 注释要求
1. **必须注释**：所有字段都必须有注释
2. **中文注释**：使用中文说明字段用途
3. **业务含义**：说明字段的业务含义和有效值范围
4. **特殊规则**：对复杂业务逻辑添加详细的实现说明
5. **术语一致**：保持术语一致性，统一使用项目约定词汇

### 注释示例
```java
private String roleAuthorityId;//角色权限id,唯一索引,uuid
private String lesseeId;//租户id
private String roleId;//角色id,唯一索引,uuid
private String roleAuthorityModuleNO;//租户管理组权限模块编号
private String roleAuthorityAlias;//租户管理组权限别名
```

## 质量检查

### 类型映射检查
- 验证数据库类型映射到Java类型的准确性
- 检查是否使用了合适的包装类
- 确认精度要求字段使用了BigDecimal

### 注释完整性检查
- 确认所有字段都有中文注释
- 验证注释的准确性和完整性
- 检查注释是否符合项目规范

## 最佳实践

1. **统一管理**：将常用的类型映射定义为标准规范
2. **明确注释**：字段注释要明确指出业务含义
3. **合理命名**：遵循驼峰命名法，保持命名一致性
4. **类型选择**：根据业务需求选择合适的Java类型
5. **测试覆盖**：确保生成的DO类能够正常使用
