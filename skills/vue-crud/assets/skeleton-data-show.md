---
title: DataShow.vue 骨架模板
type: skeleton
output: DataShow.vue
description: 只读详情页骨架，用于展示数据信息，支持 Tab 切换
---

# DataShow.vue 骨架模板

> **生成前必读**：[代码生成规则汇总](all-rules-summary.md)

复制以下代码作为 DataShow.vue 的基础框架，根据 TODO 注释填充业务逻辑。

## 相关规则

生成代码时需参考以下规则：
- 附件展示 → [component-base-show-file-list.md](../references/component-base-show-file-list.md)
- 空状态 → [component-base-no-data.md](../references/component-base-no-data.md)
- 脱敏处理 → [util-format-sensitive.md](../references/util-format-sensitive.md)

## 骨架代码

```vue
<template>
    <div class="data_show_container page_container" v-loading="loading">

        <div class="page_scroll_middle">
            <el-form :model="formData" label-position="left" :label-width="$FORM_LABEL_WIDTH.FOUR">
                <!-- TODO: 添加详情展示项 -->
            </el-form>
        </div>
    </div>
</template>

<script setup>
import { reactive, ref, onMounted, defineProps } from 'vue'
import { requestToken } from '@/util/adminRequest'

// ==================== 1. props 和 emits 定义 ====================
const props = defineProps({
    xxxID: {  // TODO: 修改为业务 ID 名称
        type: String,
        default: ''
    }
})

// ==================== 3. 响应式数据（ref、reactive） ====================
// 页面状态
const loading = ref(false)

// TODO: 定义详情数据
const formData = reactive({
    xxxID: '', // TODO: 修改为业务 ID 名称
    name: '',
    status: '',
})

// ==================== 6. 方法函数定义 ====================

/**
 * 获取详情数据
 */
const getOneData = async xxxID => {
    formData.xxxID = xxxID // TODO: 修改为业务 ID 名称
    if (!xxxID) return
    try {
        loading.value = true
        const res = await requestToken({
            url: '/Api/xxx/xxx/getOne', // TODO: 修改 API 地址
            data: { xxxID: xxxID }
        })
        const data = res.data || {}
        // TODO: 回显详情数据
        formData.name = data.name
        formData.status = data.status
    } catch (error) {
        // error
    } finally {
        loading.value = false
    }
}

// ==================== 8. 生命周期钩子 ====================
onMounted(() => getOneData(props.xxxID)) // TODO: 修改为业务 ID 名称
</script>
```

## Class 使用规范

| 元素 | Class | 说明 |
|:----|:-----|:----|
| 外层容器 | `data_show_container page_container` | 详情页主容器，配合 v-loading |
| 滚动区域 | `page_scroll_middle` | 内容可滚动区域 |
| 字段值容器 | `indexData_DataUpdate_class_detail_span` | 统一的字段值展示样式 |
| 富文本容器 | `rich-html-detail` | 富文本内容展示专用 |

## 详情项模板

### 普通文本

```vue
<el-form-item label="名称">
    <div class="indexData_DataUpdate_class_detail_span">{{ formData.name }}</div>
</el-form-item>
```

### 状态转换

```vue
<el-form-item label="状态">
    <div class="indexData_DataUpdate_class_detail_span">
        {{ formData.status === 1 ? '启用' : '禁用' }}
    </div>
</el-form-item>
```

### 富文本

```vue
<el-form-item label="内容">
    <div class="indexData_DataUpdate_class_detail_span">
        <div class="rich-html-detail" v-if="formData.content" v-html="formData.content"></div>
        <span v-else>-</span>
    </div>
</el-form-item>
```

### 图片展示

```vue
<el-form-item label="图片">
    <div class="indexData_DataUpdate_class_detail_span">
        <BaseShowImageList :image-list="formData.imageList" empty-text="-" />
    </div>
</el-form-item>
```

### 附件展示

```vue
<el-form-item label="附件">
    <div class="indexData_DataUpdate_class_detail_span">
        <BaseShowFileList :file-list="formData.attachmentList" empty-text="暂无附件" />
    </div>
</el-form-item>
```

## 空值处理

> **重要**：`indexData_DataUpdate_class_detail_span` 样式已内置空值处理，直接输出文本时**不需要**添加 `|| '-'`

| 场景 | 处理方式 |
|:----|:--------|
| 普通文本 | 直接输出，无需处理 |
| 状态转换 | 三元表达式，无需兜底 |
| 富文本 | v-if 判断 + v-else 占位 |
| 图片/组件 | 使用组件的 `empty-text` 属性 |
