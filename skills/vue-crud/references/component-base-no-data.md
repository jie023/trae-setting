---
title: BaseNoData 空状态占位组件
impact: HIGH
impactDescription: 空状态处理不当会影响用户体验
type: component
tags: [vue3, element-plus, display, component]
appliesTo: [DataTable, DataShow]
---

# BaseNoData 空状态占位组件

**Impact: HIGH** - 空状态占位组件，用于列表或内容为空时的提示。全局注册，无需导入。

## Task Checklist

- [ ] 在数据为空时显示该组件
- [ ] 可自定义提示文本

## Props

| 属性名 | 类型 | 默认值 | 说明 |
|:------|:----|:------|:----|
| text | String | `暂无数据` | 空状态提示文本 |

## 样式说明

| 样式属性 | 值 | 说明 |
|:--------|:---|:----|
| 布局 | Flex | 垂直水平居中显示 |
| 容器高度 | `100%` | 需确保父容器有明确高度 |
| 文字样式 | 12px #A4ACB6 | 浅灰色提示文字 |

**Correct:**
```vue
<!-- ✅ 正确：基本用法 -->
<BaseNoData v-if="!dataList || dataList.length === 0" />

<!-- ✅ 正确：自定义提示文本 -->
<BaseNoData v-if="!detailData.logList?.length" text="暂无操作日志" />

<!-- ✅ 正确：与列表配合使用 -->
<template>
    <div class="list-container">
        <template v-if="list.length > 0">
            <div v-for="item in list" :key="item.id">{{ item.name }}</div>
        </template>
        <BaseNoData v-else text="暂无相关数据" />
    </div>
</template>
```

## 传统写法（备选）

如果不使用 BaseNoData 组件，也可使用标准 class 结构：

```vue
<div class="common_no_data" v-if="!dataList || dataList.length === 0">
    <img class="common_no_data_icon" src="@/assets/common_empty.png" alt="" />
    <div class="common_no_data_text">暂无数据</div>
</div>
```
