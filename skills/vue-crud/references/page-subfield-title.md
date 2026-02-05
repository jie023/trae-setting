---
title: 详情页/编辑页小标题规范
impact: HIGH
impactDescription: 小标题样式不统一会破坏页面视觉一致性
type: best-practice
tags: [vue3, css, class, layout]
appliesTo: [DataUpdate, DataShow]
---

# 详情页/编辑页小标题规范

**Impact: HIGH** - 详情页和编辑页的分区小标题必须使用标准结构，样式已在 CDN 中定义。

> ⚠️ **CRITICAL WARNING**:
>
> ## 🚫 禁止自创小标题样式！
>
> **详情页（DataShow）和编辑页（DataUpdate）的分区小标题必须使用标准结构！**
>
> 样式已在 CDN 中预定义，直接使用即可。

## Task Checklist

- [ ] **详情页/编辑页分区小标题使用标准结构**
- [ ] 使用 `page_oa_subfield_container` 作为容器
- [ ] 使用 `page_oa_subfield_line` 作为装饰线
- [ ] 使用 `page_oa_subfield_name` 作为标题文字
- [ ] 禁止自创小标题 class 或 style

## 标准 Class 说明

| Class | 用途 |
|:-----|:----|
| `page_oa_subfield_container` | 小标题容器 |
| `page_oa_subfield_line` | 左侧装饰线 |
| `page_oa_subfield_name` | 标题文字 |

## 标准代码结构

**Correct:**
```vue
<!-- ✅ 正确：使用标准小标题结构 -->
<div class="page_oa_subfield_container">
    <div class="page_oa_subfield_line"></div>
    <div class="page_oa_subfield_name">基本信息</div>
</div>

<!-- 表单内容 -->
<el-form-item label="名称">
    <el-input v-model="formData.name" />
</el-form-item>

<!-- ✅ 正确：另一个分区 -->
<div class="page_oa_subfield_container">
    <div class="page_oa_subfield_line"></div>
    <div class="page_oa_subfield_name">联系信息</div>
</div>
```

**Incorrect:**
```vue
<!-- ❌ 错误：自创小标题样式 -->
<div class="section-title">基本信息</div>

<!-- ❌ 错误：使用 style 属性 -->
<div style="font-weight: bold; margin-bottom: 10px;">基本信息</div>

<!-- ❌ 错误：使用 h3 或其他标签 -->
<h3 class="form-section-title">基本信息</h3>
```

## 使用场景

1. **详情页（DataShow）**：用于分隔不同信息区块
2. **编辑页（DataUpdate）**：用于分隔不同表单区域

## 完整示例

```vue
<template>
    <div class="page_container">
        <!-- 第一分区：基本信息 -->
        <div class="page_oa_subfield_container">
            <div class="page_oa_subfield_line"></div>
            <div class="page_oa_subfield_name">基本信息</div>
        </div>

        <el-form :model="formData">
            <el-form-item label="名称">
                <el-input v-model="formData.name" placeholder="请输入名称" clearable maxLength="20" show-word-limit />
            </el-form-item>
        </el-form>

        <!-- 第二分区：详细信息 -->
        <div class="page_oa_subfield_container">
            <div class="page_oa_subfield_line"></div>
            <div class="page_oa_subfield_name">详细信息</div>
        </div>

        <el-form :model="formData">
            <el-form-item label="描述">
                <el-input v-model="formData.description" type="textarea" :rows="3" maxlength="120" show-word-limit resize="none" placeholder="请输入描述" />
            </el-form-item>
        </el-form>
    </div>
</template>
```
