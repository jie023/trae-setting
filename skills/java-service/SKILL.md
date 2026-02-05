---
name: "java-service"
description: "Provides Java Service layer standards including interface specifications, implementation patterns, method naming, transaction management, and dependency injection. Invoke when generating Service files or ServiceImpl classes."
---

# Java Service层规范

本技能提供Java Service层的开发规范，包括Service接口规范、ServiceImpl实现类规范、方法命名规范、事务管理规范、依赖注入规范和异常处理规范。

## Service接口规范

### 类命名规范
- 类名：业务模块名 + Service，如AdminDepartmentService
- 注释：使用/** */格式，包含类的用途、作者和创建日期

### 方法命名规范

| 操作类型 | 命名模式 | 示例 |
|----------|----------|------|
| 保存 | save + 业务模块名 | saveAdminDepartment |
| 修改 | update + 业务模块名 | updateAdminDepartment |
| 修改状态 | update + 业务模块名 + State | updateAdminDepartmentState |
| 删除 | delete + 业务模块名 + By + 主键名 | deleteAdminDepartmentByAdminDepartmentId |
| 查询单个（DO） | get + 业务模块名 + By + 查询条件 | getAdminDepartmentByAdminDepartmentId |
| 查询单个（VO） | get + 业务模块名 + Show + By + 查询条件 | getAdminDepartmentShowByAdminDepartmentId |
| 查询列表（DO） | list + 业务模块名 + By + 查询条件 | listAdminDepartmentByAdminDepartmentSuperiorId |
| 查询列表（VO） | list + 业务模块名 + Show + By + 查询条件 | listAdminDepartmentShowByAdminDepartmentSuperiorId |
| 分页查询 | list + 业务模块名 + TableShow | listAdminDepartmentTableShow |

### 方法签名模板

```java
// 保存方法
void save业务模块名(业务模块名DataSaveVO 业务模块名DataSaveVO);

// 修改方法
void update业务模块名(业务模块名DataSaveVO 业务模块名DataSaveVO);

// 修改状态方法
void update业务模块名State(业务模块名DataIdAndStateVO 业务模块名DataIdAndStateVO);

// 删除方法
void delete业务模块名By主键名(String 主键名);

// 查询单个（DO）
业务模块名DO get业务模块名By主键名(String 主键名);

// 查询单个（VO）
业务模块名DataShowVO get业务模块名ShowBy主键名(String 主键名);

// 查询列表（DO）
List<业务模块名DO> list业务模块名By查询条件(String 查询条件);

// 查询列表（VO）
List<业务模块名DataShowVO> list业务模块名ShowBy查询条件(String 查询条件);

// 分页查询
TableShowDataVO list业务模块名TableShow(TableConditionDataVO tableConditionDataVO);
```

### 注释规范
```java
/**
 * 保存管理员部门表
 *
 * @param adminDepartmentDataSaveVO 管理员部门表
 */
void saveAdminDepartment(AdminDepartmentDataSaveVO adminDepartmentDataSaveVO);
```

## ServiceImpl实现类规范

### 类命名规范
- 类名：业务模块名 + ServiceImpl，如AdminDepartmentServiceImpl
- 注释：使用/** */格式，包含类的用途、作者和创建日期

### 注解规范
```java
@Service
public class 业务模块名ServiceImpl implements 业务模块名Service {
```

### 依赖注入规范
```java
@Autowired
private 业务模块名Dao 业务模块名Dao;

@Autowired
private 其他Dao 其他Dao;
```

### 事务管理规范
```java
@Override
@Transactional
public void save业务模块名(业务模块名DataSaveVO 业务模块名DataSaveVO) {
    // 业务逻辑
}
```
- 对于涉及数据库修改的方法（保存、修改、删除），添加@Transactional注解
- 事务默认传播行为：PROPAGATION-REQUIRED

### 参数和返回值规范

| 操作类型 | 参数类型 | 返回值类型 |
|----------|----------|------------|
| 保存 | 业务模块名DataSaveVO | void |
| 修改 | 业务模块名DataSaveVO | void |
| 修改状态 | 业务模块名DataIdAndStateVO | void |
| 删除 | String（主键） | void |
| 查询单个（DO） | String（主键） | 业务模块名DO |
| 查询单个（VO） | String（主键） | 业务模块名DataShowVO |
| 查询列表（DO） | String（查询条件） | List<业务模块名DO> |
| 查询列表（VO） | String（查询条件） | List<业务模块名DataShowVO> |
| 分页查询 | TableConditionDataVO | TableShowDataVO |

- 参数优先使用VO类（如AdminDepartmentDataSaveVO）
- 返回值优先使用VO类（如AdminDepartmentDataShowVO）
- 列表查询返回List<VO类>或JSONArray
- 分页查询返回TableShowDataVO

### 异常处理规范
```java
try {
    // 业务逻辑
} catch (Exception e) {
    LOG.error(e.getMessage());
    throw new RuntimeException("操作失败");
}
```
- 对可能为空的对象进行空值检查
- 空值处理：返回null或直接返回，不抛出异常
- 使用try-catch捕获异常并记录日志

### 资源清理规范
```java
public void save业务模块名(业务模块名DataSaveVO 业务模块名DataSaveVO) {
    业务模块名DO 业务模块名DO = new 业务模块名DO();
    // 业务逻辑
    业务模块名DO = null;
    业务模块名DataSaveVO = null;
}
```
- 在方法结束前将局部对象置为null，帮助垃圾回收

## 调用时机

当用户需要生成以下内容时，应调用此技能：
- Java Service接口文件
- Java ServiceImpl实现类文件
- Service层方法定义
- Service层代码规范咨询
- 事务管理规范咨询
