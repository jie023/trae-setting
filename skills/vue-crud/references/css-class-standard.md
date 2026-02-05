---
title: CSS Class 规范
impact: HIGH
impactDescription: 自创 class 会导致样式不一致，破坏 UI 统一性
type: best-practice
tags: [vue3, css, class, style]
appliesTo: [DataTable, DataUpdate, DataShow, index]
---

# CSS Class 规范

**Impact: HIGH** - 禁止硬编码颜色和尺寸，禁止自创 class，必须使用项目标准 Class。

> ⚠️ **CRITICAL WARNING**:
>
> ## 🚫 禁止使用 style 属性！
>
> **表单组件（el-input、el-select、el-date-picker 等）禁止添加 `style` 属性！**
>
> 组件宽度由最顶层 class（如 `page_container`）统一控制，无需手动设置。

## Task Checklist

- [ ] **禁止使用 style 属性设置宽度、边距等样式**
- [ ] 使用项目定义的标准 Class
- [ ] 禁止自创 class 名称
- [ ] 禁止硬编码颜色值
- [ ] 禁止硬编码尺寸值

## 标准 Class 列表

| Class | 用途 | 适用页面 |
|:-----|:----|:--------|
| `card_height` | 卡片容器高度 | 通用 |
| `card-header` | 卡片头部 | 通用 |
| `data_table_container` | 列表页主容器 | DataTable |
| `data_table_scroll_middle` | 列表页滚动区域 | DataTable |
| `table_select_div_style` | 搜索栏外层 | DataTable |
| `table_select_div_style_left` | 搜索栏左侧筛选区 | DataTable |
| `table_select_div_style_item` | 单个筛选项包装 | DataTable |
| `table_select_div_style_screen` | 搜索栏右侧按钮区 | DataTable |
| `indexData_DataTable_style` | 表格 class | DataTable |
| `indexData_DataTable_class_pagination` | 分页 class | DataTable |
| `page_container` | 表单页容器 | DataUpdate/DataShow |
| `page_suspension_bottom` | 底部固定按钮栏 | DataUpdate |
| `common_no_data` | 暂无数据占位 | 通用 |
| `table_column_tip` | 表格警告状态文字 | DataTable |
| `table_operation_dropdown` | 操作列 dropdown | DataTable |
| `table_operation_more` | 操作列更多按钮 | DataTable |
| `table_operation_dropdown_danger` | dropdown 危险选项 | DataTable |
| `table_operation_dropdown_primary` | dropdown 普通选项 | DataTable |

**Incorrect:**
```vue
<!-- ❌❌❌ 严重错误：表单组件添加 style 属性 ❌❌❌ -->
<el-select v-model="formData.type" style="width: 200px;">
    <el-option label="类型A" value="1" />
</el-select>

<el-input v-model="formData.name" style="width: 300px;"></el-input>

<!-- ❌ 错误：自创 class -->
<div class="my-table-container">
    <div class="search-area">
        <el-input class="keyword-input" />
    </div>
</div>

<!-- ❌ 错误：硬编码样式 -->
<span style="color: red;">已驳回</span>
<div style="padding: 20px; margin-top: 10px;">
```

**Correct:**
```vue
<!-- ✅ 正确：表单组件不添加 style，宽度由容器 class 控制 -->
<el-select v-model="formData.type" clearable>
    <el-option label="类型A" value="1" />
</el-select>

<el-input v-model="formData.name" placeholder="请输入" clearable></el-input>

<!-- ✅ 正确：使用标准 class -->
<div class="data_table_container">
    <div class="data_table_scroll_middle">
        <div class="table_select_div_style">
            <div class="table_select_div_style_left">
                <div class="table_select_div_style_item">
                    <el-input />
                </div>
            </div>
            <div class="table_select_div_style_screen">
                <el-button>重置</el-button>
            </div>
        </div>
    </div>
</div>

<!-- ✅ 正确：使用标准警告 class -->
<span :class="scope.row.status == 2 ? 'table_column_tip' : ''">
    {{ getStatusLabel(scope.row.status) }}
</span>
```
