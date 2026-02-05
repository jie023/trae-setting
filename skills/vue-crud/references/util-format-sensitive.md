---
title: 敏感信息脱敏工具函数
impact: HIGH
impactDescription: 敏感信息未脱敏会导致隐私泄露
type: utility
tags: [vue3, utils, sensitive, privacy]
appliesTo: [DataTable, DataShow]
---

# 敏感信息脱敏工具函数

**Impact: HIGH** - 敏感信息必须脱敏显示，保护用户隐私。

## Task Checklist

- [ ] 从 `@web-npm/baseutils` 导入 `formatSensitive`
- [ ] 身份证、手机号、地址等敏感信息必须脱敏
- [ ] 根据数据类型选择对应的脱敏函数

## 导入方式

```javascript
import { formatSensitive } from '@web-npm/baseutils'
```

## 函数列表

| 函数名 | 脱敏规则 | 示例 |
|:------|:--------|:----|
| `formatSensitive.idCard(str)` | 保留首尾，中间星号 | `'330102199001011234'` → `'3****************4'` |
| `formatSensitive.phone(str)` | 保留前3后4 | `'13812345678'` → `'138****5678'` |
| `formatSensitive.address(str)` | 保留省市，后续星号 | `'浙江省杭州市西湖区...'` → `'浙江省杭州市******'` |

**Incorrect:**
```vue
<!-- ❌ 错误：直接显示敏感信息 -->
<span>{{ row.idCard }}</span>
<span>{{ row.phone }}</span>
<span>{{ row.address }}</span>

<!-- ❌ 错误：手动脱敏 -->
<span>{{ row.phone.substring(0, 3) + '****' + row.phone.substring(7) }}</span>
```

**Correct:**
```vue
<script setup>
import { formatSensitive } from '@web-npm/baseutils'
</script>

<template>
    <!-- ✅ 正确：使用脱敏函数 -->
    <span>{{ formatSensitive.idCard(row.idCard) }}</span>
    <span>{{ formatSensitive.phone(row.phone) }}</span>
    <span>{{ formatSensitive.address(row.address) }}</span>
</template>
```

## 表格列中使用

```vue
<el-table-column prop="phone" label="手机号">
    <template #default="scope">
        {{ formatSensitive.phone(scope.row.phone) }}
    </template>
</el-table-column>

<el-table-column prop="idCard" label="身份证">
    <template #default="scope">
        {{ formatSensitive.idCard(scope.row.idCard) }}
    </template>
</el-table-column>
```
