---
title: 表格列宽规范
impact: HIGH
impactDescription: 列宽不统一会影响表格美观和可读性
type: best-practice
tags: [vue3, element-plus, table, width]
appliesTo: [DataTable]
---

# 表格列宽规范

**Impact: HIGH** - 使用 `$tableItemWidthCalculation(n)` 计算宽度，确保列宽统一。

> ⚠️ **CRITICAL WARNING**:
>
> ## 🚫 禁止硬编码宽度数值！
>
> **所有列宽（操作列除外）必须使用 `:width="$tableItemWidthCalculation(n)"` 或 `:min-width="$tableItemWidthCalculation(n)"`！**
>
> ❌ 错误：`min-width="120"` 或 `width="180"`
>
> ✅ 正确：`:min-width="$tableItemWidthCalculation(4)"` 或 `:width="$tableItemWidthCalculation(6)"`

## Task Checklist

- [ ] **禁止硬编码宽度数值（如 120、180），必须使用 $tableItemWidthCalculation(n)**
- [ ] 使用 `$tableItemWidthCalculation(n)` 计算宽度（全局注入，无需 import）
- [ ] 第一列（序号）使用 `width`
- [ ] 第二列（名称）使用 `min-width`（自适应）
- [ ] 其余列使用 `width`
- [ ] 操作列使用 `width="xxx"`，使用正常的数值

## 位数参考表

| 类型 | 位数 (n) | 示例 |
|:----|:--------|:----|
| 序号/单位 | 2 | `$tableItemWidthCalculation(2)` |
| 姓名/状态 | 4 | `$tableItemWidthCalculation(4)` |
| 日期 (YYYY-MM-DD) | 6 | `$tableItemWidthCalculation(6)` |
| 编号/手机 | 8 | `$tableItemWidthCalculation(8)` |
| 时间（YYYY-MM-DD HH:mm） | 9 | `$tableItemWidthCalculation(9)` |
| 时间 (YYYY-MM-DD HH:mm:ss) | 11 | `$tableItemWidthCalculation(11)` |
| 长文本/名称 | 16 | `$tableItemWidthCalculation(16)` |

## 列规范

| 列位置 | 属性 | 说明 |
|:------|:----|:----|
| 第一列（序号） | `width`, `fixed` | 固定宽度 |
| 第二列（名称） | `min-width`, `fixed` | **唯一使用 min-width 的列**，自适应宽度 |
| 其余列 | `width` | 固定宽度 |
| 操作列 | `width`, `fixed="right"` | 固定在右侧 |

**Incorrect:**
```vue
<!-- ❌ 错误：硬编码宽度 -->
<el-table-column label="序号" type="index" width="60" />

<!-- ❌ 错误：第二列使用 width 而非 min-width -->
<el-table-column prop="name" label="名称" :width="$tableItemWidthCalculation(4)" />

<!-- ❌ 错误：其余列使用 min-width -->
<el-table-column prop="phone" label="手机" :min-width="$tableItemWidthCalculation(8)" />
```

**Correct:**
```vue
<!-- ✅ 正确：第一列（序号） -->
<el-table-column
    label="序号"
    type="index"
    :width="$tableItemWidthCalculation(2)"
    :index="tableData.offset + 1"
    show-overflow-tooltip
    fixed
/>

<!-- ✅ 正确：第二列（名称）- 唯一使用 min-width -->
<el-table-column
    prop="name"
    label="名称"
    :min-width="$tableItemWidthCalculation(4)"
    show-overflow-tooltip
    fixed
/>

<!-- ✅ 正确：其余列 -->
<el-table-column
    prop="phone"
    label="手机"
    :width="$tableItemWidthCalculation(8)"
    show-overflow-tooltip
/>

<el-table-column
    prop="createTime"
    label="创建时间"
    :width="$tableItemWidthCalculation(11)"
    show-overflow-tooltip
/>

<!-- ✅ 正确：操作列 -->
<el-table-column label="操作" fixed="right" width="140">
    <template #default="scope">
        <!-- 操作按钮 -->
    </template>
</el-table-column>
```
