---
name: "p3c-security-rules"
description: "Provides security coding standards including authentication, data masking, SQL injection prevention, CSRF protection, and input validation. Invoke when user asks about security best practices or vulnerability prevention."
---

# P3C 安全规约

本技能提供阿里巴巴Java开发手册中的安全相关规范。

## 权限控制

1. 【强制】隶属于用户个人的页面或者功能必须进行权限控制校验
   - 防止没有做水平权限校验就可随意访问、修改、删除别人的数据

## 数据脱敏

2. 【强制】用户敏感数据禁止直接展示，必须对展示数据进行脱敏
   - 个人手机号码显示为:158****9119，隐藏中间4位

## SQL注入防护

3. 【强制】用户输入的SQL参数严格使用参数绑定或者METADATA字段值限定，防止SQL注入，禁止字符串拼接SQL访问数据库

## 参数验证

4. 【强制】用户请求传入的任何参数必须做有效性验证
   - 忽略参数校验可能导致：
     - page size过大导致内存溢出
     - 恶意order by导致数据库慢查询
     - 任意重定向
     - SQL注入
     - 反序列化注入
     - 正则输入源串拒绝服务ReDoS

## XSS防护

5. 【强制】禁止向HTML页面输出未经安全过滤或未正确转义的用户数据

## CSRF防护

6. 【强制】表单、AJAX提交必须执行CSRF安全过滤
   - CSRF(Cross-site request forgery)跨站请求伪造是一类常见编程漏洞

## 防重放限制

7. 【强制】在使用平台资源（短信、邮件、电话、下单、支付）时，必须实现正确的防重放限制
   - 数量限制、疲劳度控制、验证码校验，避免被滥刷导致资损

## 内容风控

8. 【推荐】发贴、评论、发送即时消息等用户生成内容的场景必须实现防刷、文本内容违禁词过滤等风控策略
