---
title: 下拉框规范
impact: MEDIUM
impactDescription: 下拉框配置不当会影响用户体验和数据正确性
type: best-practice
tags: [vue3, element-plus, form, select]
appliesTo: [DataTable, DataUpdate]
---

# 下拉框规范

**Impact: MEDIUM** - 下拉框必须正确配置属性，并处理清空时的 null 值问题。

## Task Checklist

- [ ] 必须添加 `clearable` 属性
- [ ] 动态数据下拉框添加 `filterable` 属性
- [ ] 静态字典数据可以不加 `filterable`
- [ ] 必须处理清空时的 null 值（使用 `@change` 事件）

## filterable 使用规则

| 数据类型 | filterable | 说明 |
|:--------|:----------|:----|
| 动态数据 | ✅ 必须 | 用户、部门、分类等后台接口数据 |
| 静态字典 | ❌ 可选 | 状态、启用/禁用等固定选项 |

**Incorrect:**
```vue
<!-- ❌ 错误：动态数据没加 filterable -->
<el-select v-model="formData.userID" clearable>
    <el-option v-for="item in userList" :key="item.id" :label="item.name" :value="item.id" />
</el-select>

<!-- ❌ 错误：缺少 clearable -->
<el-select v-model="formData.status">
    <el-option label="启用" value="1" />
    <el-option label="禁用" value="0" />
</el-select>

<!-- ❌ 错误：没有处理 null 值 -->
<el-select v-model="formData.type" clearable>
    <el-option v-for="item in typeList" :key="item.id" :label="item.name" :value="item.id" />
</el-select>
```

**Correct:**
```vue
<!-- ✅ 正确：动态数据下拉框（有 filterable + null 处理） -->
<el-select
    v-model="formData.userID"
    placeholder="请选择用户"
    clearable
    filterable
    @change="val => formData.userID = val ?? ''"
>
    <el-option
        v-for="item in userList"
        :key="item.id"
        :label="item.name"
        :value="item.id"
    />
</el-select>

<!-- ✅ 正确：静态字典下拉框（无 filterable + null 处理） -->
<el-select
    v-model="formData.status"
    placeholder="请选择状态"
    clearable
    @change="val => formData.status = val ?? ''"
>
    <el-option label="启用" value="1" />
    <el-option label="禁用" value="0" />
</el-select>
```

## 搜索栏下拉框

搜索栏中的下拉框需要在 `@change` 中同时处理 null 值和触发搜索：

```vue
<el-select
    v-model="tableSelectData.typeID"
    placeholder="类型"
    @change="
        val => {
            tableSelectData.typeID = val ?? ''
            handleSearch()
        }
    "
    clearable
    filterable
>
    <el-option v-for="item in typeList" :key="item.value" :label="item.label" :value="item.value" />
</el-select>
```
