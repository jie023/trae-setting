---
title: 输入框规范
impact: HIGH
impactDescription: 输入框属性不完整会影响用户体验
type: best-practice
tags: [vue3, element-plus, form, input]
appliesTo: [DataTable, DataUpdate]
---

# 输入框规范

**Impact: HIGH** - 输入框必须包含必要的属性以提升用户体验。

## Task Checklist

- [ ] 普通输入框必须添加 `clearable` 属性
- [ ] 数字类型输入框**不添加** `clearable`
- [ ] 多行文本框添加 `resize="none"` 和 `show-word-limit`
- [ ] 所有输入框添加 `placeholder`

## 通用规范

| 组件类型 | 必填属性 |
|:--------|:--------|
| `el-input` (普通) | `clearable`, `placeholder` |
| `el-input` (数字) | `placeholder` (不加 clearable) |
| `el-input` (textarea) | `resize="none"`, `show-word-limit`, `maxlength`, `placeholder` |

**Incorrect:**
```vue
<!-- ❌ 错误：缺少 clearable -->
<el-input v-model="formData.name" placeholder="请输入" />

<!-- ❌ 错误：数字输入框加了 clearable -->
<el-input v-model="formData.amount" type="number" clearable />

<!-- ❌ 错误：多行文本框缺少必要属性 -->
<el-input v-model="formData.remark" type="textarea" />
```

**Correct:**
```vue
<!-- ✅ 正确：普通输入框 -->
<el-input v-model="formData.name" placeholder="请输入名称" clearable maxLength="20" show-word-limit />

<!-- ✅ 正确：数字输入框（不加 clearable） -->
<el-input v-model="formData.amount" placeholder="请输入金额" maxLength="10" show-word-limit />

<!-- ✅ 正确：多行文本框 -->
<el-input
    v-model="formData.remark"
    type="textarea"
    :rows="3"
    maxlength="120"
    show-word-limit
    resize="none"
    placeholder="请输入备注"
/>
```

## placeholder 规范

| 场景 | 格式 | 示例 |
|:----|:----|:----|
| 筛选区输入框 | 直接使用字段名 | `"关键字"`、`"编号"` |
| 表单页输入框 | `"请输入"` + 字段名 | `"请输入名称"`、`"请输入金额"` |

## 搜索栏输入框

搜索栏中的输入框需要添加搜索图标和 `@change` 事件：

```vue
<el-input
    v-model="tableSelectData.keyword"
    placeholder="关键字"
    :prefix-icon="Search"
    @change="handleSearch"
    clearable
/>
```

> **注意**：`:prefix-icon="Search"` 必须使用动态绑定（冒号），Search 是从 `@element-plus/icons-vue` 导入的组件。
