---
name: "java-controller"
description: "Provides Java Controller layer standards including RESTful API design, request mapping, parameter validation, and response handling. Invoke when generating Controller files or RESTful API endpoints."
---

# Java Controller层规范

本技能提供Java Controller层的开发规范，包括Controller类规范、RESTful API规范、参数验证规范、返回值处理规范和异常处理规范。

## Controller类规范

### 类命名规范
- 类名：业务模块名 + Controller，如AdminDepartmentController
- 注释：使用/** */格式，包含类的用途、作者和创建日期

### 注解规范
```java
@RestController
@RequestMapping("/模块路径/业务模块")
public class 业务模块名Controller {
```
- @RestController：标记为RESTful控制器
- @RequestMapping：指定控制器的基础URL路径

### 依赖注入规范
```java
@Autowired
private 业务模块名Service 业务模块名Service;

@Autowired
private 其他Service 其他Service;
```
- 使用@Autowired注解注入Service层依赖
- 依赖字段使用private修饰

### 日志记录规范
```java
private static final Logger LOG = LoggerFactory.getLogger(业务模块名Controller.class);
```
- 使用SLF4J日志框架
- 日志记录器使用private static final修饰

## RESTful API规范

### HTTP方法规范

| 操作类型 | HTTP方法 | URL路径 | 示例 |
|----------|----------|---------|------|
| 保存 | POST | /Save业务模块名 | /SaveAdminDepartment |
| 修改 | POST | /Update业务模块名 | /UpdateAdminDepartment |
| 删除 | POST | /Delete业务模块名 | /DeleteAdminDepartment |
| 获取单个 | POST | /GetOne业务模块名 | /GetOneAdminDepartment |
| 获取全部（带分页） | POST | /GetAll业务模块名Table | /GetAllAdminDepartmentTable |

### 方法注解规范
```java
@PostMapping("/Save业务模块名")
@ResponseBody
public BaseRetuenDataVO save业务模块名(@RequestBody @Valid 业务模块名DataSaveVO 业务模块名DataSaveVO) {
    // 业务逻辑
}
```
- @PostMapping：指定HTTP POST请求
- @ResponseBody：指定返回JSON格式数据
- @RequestBody：指定请求体参数
- @Valid：启用参数验证

### 方法命名规范

| 操作类型 | 方法命名 | 示例 |
|----------|----------|------|
| 保存 | save + 业务模块名 | saveAdminDepartment |
| 修改 | update + 业务模块名 | updateAdminDepartment |
| 删除 | delete + 业务模块名 | deleteAdminDepartment |
| 获取单个 | getOne + 业务模块名 | getOneAdminDepartment |
| 获取全部 | getAll + 业务模块名 | getAllAdminDepartment |

### 注释规范
```java
/**
 * 保存管理员部门
 *
 * @param adminDepartmentDataSaveVO 管理员部门
 */
@PostMapping("/SaveAdminDepartment")
@ResponseBody
public BaseRetuenDataVO saveAdminDepartment(@RequestBody @Valid AdminDepartmentDataSaveVO adminDepartmentDataSaveVO) {
    // 业务逻辑
}
```
- 使用/** */格式
- 包含方法的用途、参数说明和返回值说明

## 参数验证规范

### 验证注解使用
```java
@PostMapping("/Save业务模块名")
@ResponseBody
public BaseRetuenDataVO save业务模块名(@RequestBody @Valid 业务模块名DataSaveVO 业务模块名DataSaveVO) {
    // 业务逻辑
}
```
- 使用@Valid注解启用参数验证
- 验证规则在VO类中使用JSR-380验证注解定义

### 参数类型规范

| 操作类型 | 参数类型 | 示例 |
|----------|----------|------|
| 保存 | 业务模块名DataSaveVO | AdminDepartmentDataSaveVO |
| 修改 | 业务模块名DataSaveVO | AdminDepartmentDataSaveVO |
| 删除 | 业务模块名DataIdVO | AdminDepartmentDataIdVO |
| 获取单个 | 业务模块名DataIdVO | AdminDepartmentDataIdVO |
| 获取全部（带分页） | TableConditionDataVO | TableConditionDataVO |

## 返回值处理规范

### 返回值类型
```java
public BaseRetuenDataVO save业务模块名(@RequestBody @Valid 业务模块名DataSaveVO 业务模块名DataSaveVO) {
    BaseRetuenDataVO baseRetuenDataVO;
    // 业务逻辑
    return baseRetuenDataVO;
}
```
- 统一返回BaseRetuenDataVO类型
- 使用ResultUtil工具类生成返回值

### 返回值处理
```java
// 成功返回
baseRetuenDataVO = ResultUtil.success("操作成功", 数据对象);

// 失败返回
baseRetuenDataVO = ResultUtil.error("操作失败");
```
- 使用ResultUtil.success()方法生成成功返回值
- 使用ResultUtil.error()方法生成失败返回值
- 成功返回可以包含数据对象

## 异常处理规范

### try-catch异常处理
```java
try {
    // 业务逻辑
    baseRetuenDataVO = ResultUtil.success("操作成功");
} catch (Exception e) {
    LOG.error(e.getMessage());
    baseRetuenDataVO = ResultUtil.error("操作失败，请您重新操作");
}
```
- 使用try-catch捕获异常
- 使用LOG.error()记录异常日志
- 返回友好的错误消息给用户

### 空值检查
```java
业务模块名DO 业务模块名DO = 业务模块名Service.get业务模块名By主键名(主键名);
if (业务模块名DO == null) {
    baseRetuenDataVO = ResultUtil.error("此数据不存在，请刷新后重试");
    业务模块名DataSaveVO = null;
    return baseRetuenDataVO;
}
```
- 对可能为空的对象进行空值检查
- 空值时返回友好的错误消息

### 资源清理
```java
public BaseRetuenDataVO save业务模块名(@RequestBody @Valid 业务模块名DataSaveVO 业务模块名DataSaveVO) {
    BaseRetuenDataVO baseRetuenDataVO;
    // 业务逻辑
    业务模块名DataSaveVO = null;
    业务模块名DO = null;
    return baseRetuenDataVO;
}
```
- 在方法结束前将局部对象置为null，帮助垃圾回收

## 调用时机

当用户需要生成以下内容时，应调用此技能：
- Java Controller文件
- RESTful API接口
- Controller层方法定义
- Controller层代码规范咨询
- RESTful API设计咨询
