---
name: "java-validation-annotations"
description: "Provides JSR-380 validation annotation standards for Java fields including @Pattern, @NotBlank, @NotNull. Invoke when generating VO classes or adding validation to Java fields."
---

# Java验证注解规范

本技能提供Java字段验证注解的标准规范，包括常用的@Pattern正则表达式验证和其他JSR-380验证注解。

## 常用验证注解

### @Pattern正则表达式验证

| 验证类型 | 正则表达式 | 用途 | 示例 |
|----------|-----------|------|------|
| 整数或小数 | DataPattern.DECIMAL | 验证字段必须是整数或小数 | 123、12.34、-56.78 |
| 数字和字母 | DataPattern.NUMBER_LETTER | 验证字段只能包含数字和字母 | abc123、XYZ789 |
| 纯数字 | DataPattern.NUMBER | 验证字段只能是数字 | 123、456、789 |
| 日期格式 | DataPattern.DATE2 | 验证日期格式（yyyy-MM-dd） | 2024-01-15 |
| 时间格式 | DataPattern.DATE_TIME1 | 验证时间格式（yyyy-MM-dd HH:mm:ss） | 2024-01-15 14:30:00 |
| 手机号 | DataPattern.MOBILE_PHONE | 验证中国大陆手机号格式 | 13800138000 |
| 固话 | DataPattern.FIXED_PHONE | 验证中国大陆固定电话格式 | 010-12345678 |
| 邮箱 | DataPattern.EMAIL | 验证邮箱地址格式 | user@example.com |
| 身份证 | DataPattern.ID_CARD | 验证中国大陆身份证号码格式 | 110101199001011234 |

### 其他常用验证注解

| 注解 | 用途 | 适用场景 |
|------|------|----------|
| @NotBlank | 验证字符串不能为null或空字符串 | 必填的字符串字段 |
| @NotNull | 验证对象不能为null | 必填的对象字段（包括包装类型） |
| @Min/@Max | 验证数值在指定范围内 | 年龄、数量、金额等有范围要求的字段 |
| @Size | 验证字符串、集合或数组的长度 | 密码、用户名、备注等有长度要求的字段 |
| @Email | 快速验证邮箱格式（不依赖DataPattern） | 简单的邮箱验证需求 |
| @Pattern | 自定义正则表达式验证 | 特殊的格式要求 |

## 验证注解使用规范

### 注解位置
- 验证注解应放在字段声明上方
- 多个注解可以叠加使用

### 错误消息
- message属性：必须提供中文错误提示
- 消息规范：简洁明了，便于用户理解
- 示例："请输入用户名"、"手机号格式不正确"

### 分组验证
- 根据不同场景应用不同的验证规则
- 适用于创建和更新场景验证规则不同

### 组合验证
- 同时应用多个验证规则
- 适用于需要多重验证的字段

## 注意事项

1. **空值处理**：@NotBlank和@NotNull的区别
   - @NotBlank：字符串不能为null或空字符串
   - @NotNull：对象不能为null

2. **正则表达式**：使用DataPattern常量
   - 便于统一管理和修改
   - 避免正则表达式重复定义

3. **错误消息**：必须使用中文
   - 便于前端展示
   - 提升用户体验

4. **验证顺序**：按业务逻辑顺序排列
   - 先验证必填项
   - 再验证格式要求
   - 最后验证业务规则

5. **性能考虑**：复杂正则可能影响性能
   - 避免过于复杂的正则表达式
   - 考虑使用专门的验证库

## 最佳实践

1. **统一管理**：将常用的正则表达式定义为常量（如DataPattern）
2. **明确提示**：错误消息要明确指出问题所在
3. **合理分组**：使用验证分组处理不同场景
4. **组合使用**：合理组合多个验证注解
5. **测试覆盖**：确保所有验证规则都有对应的测试用例
