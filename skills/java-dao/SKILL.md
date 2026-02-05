---
name: "java-dao"
description: "Provides Java DAO layer standards including interface specifications, MyBatis XML templates, method naming, parameter annotations, and CRUD operation patterns. Invoke when generating DAO files or MyBatis mappers."
---

# Java DAO层规范

本技能提供Java DAO层的开发规范，包括Dao接口规范、MyBatis XML实现模板、方法命名规范、参数注解规范和CRUD操作模式。

## Dao接口规范

### 方法命名规范

| 操作类型 | 命名模式 | 示例 |
|----------|----------|------|
| 保存 | save实体类名 | saveAdminDepartment |
| 修改 | update实体类名 | updateAdminDepartment |
| 删除（软删除） | delete实体类名By主键 | deleteAdminDepartmentByAdminDepartmentId |
| 查询单个（主键） | get实体类名By主键 | getAdminDepartmentByAdminDepartmentId |
| 查询列表（表格展示） | list实体类名TableShow | listAdminDepartmentTableShow |
| 查询数量 | get实体类名TableCount | getAdminDepartmentTableCount |

### 方法签名模板

```java
// 保存操作
void save实体类名(实体类名DO 实体类名DO);

// 修改操作
void update实体类名(实体类名DO 实体类名DO);

// 删除操作（软删除）
void delete实体类名By实体类名Id(@Param("实体类名Id") String 实体类名Id);

// 查询单个（主键）
实体类名DO get实体类名By实体类名Id(@Param("实体类名Id") String 实体类名Id);

// 查询列表（表格展示）
List<Map<String, Object>> list实体类名TableShow(TableConditionDataVO tableConditionDataVO);

// 查询数量
int get实体类名TableCount(TableConditionDataVO tableConditionDataVO);
```

### 参数注解规范
- 所有参数必须使用@Param注解明确参数名称
- 复杂查询条件使用TableConditionDataVO封装

### 返回类型规范

| 操作类型 | 返回类型 | 说明 |
|----------|----------|------|
| 保存 | void | 无返回值 |
| 修改 | void | 无返回值 |
| 删除 | void | 无返回值 |
| 查询单个 | 实体类名DO | 返回对应的DO对象 |
| 查询列表 | List<Map<String, Object>> | 返回Map列表，便于表格展示 |
| 查询数量 | int | 返回记录数量 |

### 类注释规范
```java
/**
 * 表功能描述
 *
 * @Author: 作者
 * @Date: 创建日期
 */
@Mapper
@Component
public interface 实体类名Dao {
```

## MyBatis XML实现规范

### 保存操作(insert)
```xml
<insert id="save实体类名" parameterType="实体类名DO全路径">
    INSERT INTO 表名
    (字段1, 字段2, ..., delete_state, create_time, update_time, delete_time)
    VALUES
    (#{字段1}, #{字段2}, ..., 0, now(), null, null)
</insert>
```

### 修改操作(update)
```xml
<update id="update实体类名" parameterType="实体类名DO全路径">
    UPDATE 表名 SET
        字段1 = #{字段1},
        字段2 = #{字段2},
        ...
    WHERE delete_state = 0
    AND 主键 = #{主键,jdbcType=VARCHAR}
</update>
```

### 删除操作(软删除)
```xml
<delete id="delete实体类名By实体类名Id">
    UPDATE 表名 SET
        delete_state = 1,
        delete_time = now()
    WHERE delete_state = 0
    AND 主键 = #{主键,jdbcType=VARCHAR}
</delete>
```

### 查询操作(select)

#### 单个查询
```xml
<select id="get实体类名By实体类名Id" resultType="实体类名DO全路径">
    SELECT
        字段1 as 属性1,
        字段2 as 属性2,
        ...
    FROM 表名
    WHERE delete_state = 0
    AND 主键 = #{主键,jdbcType=VARCHAR}
    LIMIT 1
</select>
```

#### 表格展示查询
```xml
<select id="list实体类名TableShow" resultType="java.util.Map" parameterType="TableConditionDataVO全路径">
    SELECT
        字段1 as 属性1,
        字段2 as 属性2,
        ...
    FROM 表名
    WHERE delete_state = 0
    <if test="data!=null">
        <if test="data.字段名!=null and data.字段名!=''">
            AND 字段名 = #{data.字段名,jdbcType=VARCHAR}
        </if>
    </if>
    ORDER BY 排序字段 DESC
    <if test="rows!=null and rows!=0">
        LIMIT #{offset,jdbcType=INTEGER},#{rows,jdbcType=INTEGER}
    </if>
</select>
```

#### 数量查询
```xml
<select id="get实体类名TableCount" resultType="java.lang.Integer" parameterType="TableConditionDataVO全路径">
    SELECT
        COUNT(主键)
    FROM 表名
    WHERE delete_state = 0
    <if test="data!=null">
        <if test="data.字段名!=null and data.字段名!=''">
            AND 字段名 = #{data.字段名,jdbcType=VARCHAR}
        </if>
    </if>
</select>
```

### XML注释规范
在XML文件中添加注释，说明SQL语句的功能、参数、返回值等

## 通用规范

### 逻辑删除
- 所有删除操作采用软删除，更新delete_state字段为1
- 所有查询必须包含`WHERE delete_state = 0`条件
- delete_state：0表示正常，1表示已删除

### 时间字段

| 字段名 | 类型 | 说明 | 默认值 |
|--------|------|------|--------|
| create_time | datetime | 创建时间 | now() |
| update_time | datetime | 更新时间 | 修改时更新 |
| delete_time | datetime | 删除时间 | 软删除时更新为now() |

### 查询条件
- 所有查询必须包含`WHERE delete_state = 0`条件
- 单个查询使用LIMIT 1限制结果
- 列表查询添加合适的排序
- 使用动态SQL处理可选查询条件

### 命名规范
- Java属性使用驼峰命名法：adminDepartmentId
- 数据库字段使用下划线命名法：admin_department_id
- 属性与字段映射使用`as`关键字：字段名 as 属性名

### 参数类型
- 字符串参数需指定`jdbcType=VARCHAR`
- 整数参数需指定`jdbcType=INTEGER`
- 示例：`#{主键,jdbcType=VARCHAR}`

### 注释生成
- 生成标准的Java注释，包含类注释、方法注释、参数注释、返回类型注释
- XML注释：在XML文件中添加注释，说明SQL语句的功能、参数、返回值等

## 调用时机

当用户需要生成以下内容时，应调用此技能：
- Java DAO接口文件
- MyBatis XML映射文件
- DAO层方法定义
- CRUD操作的SQL实现
- DAO层代码规范咨询
