---
title: BaseShowImageList 图片列表展示组件
impact: HIGH
impactDescription: 图片展示组件使用错误会导致图片无法正确显示或预览
type: component
tags: [vue3, element-plus, display, image, component]
appliesTo: [DataShow]
---

# BaseShowImageList 图片列表展示组件

**Impact: HIGH** - 图片列表展示组件，支持预览功能。全局注册，无需导入。

## Task Checklist

- [ ] 传入正确格式的图片列表数据
- [ ] 设置 `emptyText` 处理空数据情况

## Props

| 属性 | 类型 | 默认值 | 说明 |
|:----|:----|:------|:----|
| imageList | Array | `[]` | 图片列表，每项需包含 `systemFileUrl` 字段 |
| emptyText | String | `'-'` | 列表为空时显示的文字 |
| onPreview | Function | `null` | 图片点击时的回调（不阻止默认预览） |
| stopInsideOnPreview | Function | `null` | 图片点击时的回调（阻止默认预览，完全自定义行为） |

## 数据结构

```javascript
[
    {
        systemFileID: '',           // 文件ID
        systemFileName: '',         // 文件名
        systemFileUrl: '',          // 文件下载链接
        systemFileShowUrl: '',      // 文件预览链接
        systemFileFormatType: '',   // 文件格式类型
        systemFileFormatTypeImgUrl: '' // 文件类型图标链接
    }
]
```

**Correct:**
```vue
<!-- ✅ 正确：基本用法 -->
<BaseShowImageList :image-list="detailData.imageList" empty-text="-" />

<!-- ✅ 正确：保留默认预览，同时执行自定义逻辑 -->
<BaseShowImageList
    :image-list="detailData.imageList"
    :onPreview="handlePreview"
/>

<!-- ✅ 正确：完全自定义预览行为 -->
<BaseShowImageList
    :image-list="detailData.imageList"
    :stopInsideOnPreview="handleCustomPreview"
/>

<script setup>
const handlePreview = (imageInfo, index) => {
    console.log('预览图片:', imageInfo, '索引:', index)
}

const handleCustomPreview = (imageInfo, index) => {
    window.open(imageInfo.systemFileUrl)
}
</script>
```

## KHD 客户端

KHD 客户端项目使用 `BaseShowImageListKHD`，Props 和数据结构一致。
