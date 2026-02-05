---
title: 表单组件值重置（防 null）
impact: HIGH
impactDescription: 表单清空后值为 null 会导致后端接口报错或数据异常
type: gotcha
tags: [vue3, element-plus, form, null-prevention]
appliesTo: [DataTable, DataUpdate]
---

# 表单组件值重置（防 null）

**Impact: HIGH** - Element Plus 的 select、date-picker 等组件清空后值会变为 `null`，必须通过 `@change` 事件重置为合适的空值。

> ⚠️ **CRITICAL WARNING**:
>
> ## 空值处理必须写在模板层（@change 属性中）！
>
> **禁止在 JavaScript 函数中处理空值，必须在 HTML 模板的 `@change` 属性中处理！**
>
> 即使有联动逻辑，空值处理也要在模板层完成，联动函数只处理业务逻辑。

## Task Checklist

- [ ] **空值处理必须在模板层（@change 属性中），禁止在 JavaScript 函数中处理**
- [ ] 所有可清空组件必须添加 `@change` 事件处理 null 值
- [ ] 单选组件重置为 `''`（空字符串）
- [ ] 范围选择组件重置为 `[]`（空数组）

## 重置规则

| 组件类型 | 模式 | 重置值 |
|:--------|:----|:------|
| `el-select` / `el-select-v2` | 单选 | `''` (空字符串) |
| `el-cascader` | 单选 | `''` (空字符串) |
| `el-date-picker` / `el-time-picker` | 单个日期/时间 | `''` (空字符串) |
| `el-date-picker` / `el-time-picker` | 范围日期/时间 | `[]` (空数组) |

**Incorrect:**
```vue
<!-- ❌ 错误：缺少 @change 处理，清空后值为 null -->
<el-select v-model="formData.type" clearable>
    <el-option label="类型A" value="1" />
</el-select>

<el-date-picker v-model="formData.date" clearable />

<el-date-picker v-model="formData.dateRange" type="daterange" clearable />
```

```javascript
// ❌❌❌ 严重错误：在 JavaScript 函数中处理空值 ❌❌❌
// 空值处理必须在模板层，不能在函数中处理！
const handleUserChange = val => {
    formData.userName = val ?? ''  // ❌ 应该在模板层处理
    const user = userList.find(item => item.value === val)
    if (user) {
        formData.jobNum = user.jobNum
    } else {
        formData.jobNum = ''
    }
}
```

**Correct:**
```vue
<!-- ✅ 正确：单选 Select -->
<el-select
    v-model="formData.type"
    clearable
    @change="val => formData.type = val ?? ''"
>
    <el-option label="类型A" value="1" />
</el-select>

<!-- ✅ 正确：单个日期 -->
<el-date-picker
    v-model="formData.date"
    type="date"
    clearable
    @change="val => formData.date = val ?? ''"
/>

<!-- ✅ 正确：日期范围 -->
<el-date-picker
    v-model="formData.dateRange"
    type="daterange"
    clearable
    @change="val => formData.dateRange = val ?? []"
/>
```

## 有联动逻辑时的写法

当 `@change` 需要同时处理 null 值和联动逻辑时：

```vue
<!-- 空值处理统一在模板层，联动函数只管业务逻辑 -->
<el-select
    v-model="formData.userName"
    clearable
    @change="val => (formData.userName = val ?? '', handleUserChange(val))"
>
    <el-option v-for="item in userList" :key="item.id" :label="item.name" :value="item.id" />
</el-select>

<script setup>
/**
 * 用户选择变更联动处理
 * @param {string} val 选中的用户名
 */
const handleUserChange = val => {
    // 只处理联动逻辑，无需关心 null（模板层已处理）
    const matched = userList.value.find(u => u.userName === val)
    formData.jobNum = matched ? matched.jobNum : ''
}
</script>
```
