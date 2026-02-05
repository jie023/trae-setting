---
title: BaseShowFileList 附件列表展示组件
impact: HIGH
impactDescription: 展示组件使用错误会导致文件无法正确显示
type: component
tags: [vue3, element-plus, display, component]
appliesTo: [DataShow]
---

# BaseShowFileList 附件列表展示组件

**Impact: HIGH** - 附件列表展示组件，自动区分图片与普通文件。全局注册，无需导入。

## Task Checklist

- [ ] 传入正确格式的文件列表数据
- [ ] 设置 `emptyText` 处理空数据情况

## Props

| 属性 | 类型 | 默认值 | 说明 |
|:----|:----|:------|:----|
| fileList | Array | `[]` | 文件列表数据 |
| emptyText | String | `''` | 无数据时的提示文本 |
| onPreview | Function | `null` | 预览回调，执行后仍会触发默认下载 |
| stopInsideOnPreview | Function | `null` | 预览回调，执行后**阻止**默认下载行为 |

## 数据结构

```javascript
[
    {
        systemFileID: '12345',        // 文件ID
        systemFileName: '文件名.pdf', // 文件名
        systemFileUrl: 'https://...', // 文件下载链接
        systemFileShowUrl: 'https://...', // 文件预览链接
        systemFileFormatType: 'pdf',  // 文件格式类型（jpg/png 识别为图片）
        systemFileFormatTypeImgUrl: '...' // 文件类型图标链接
    }
]
```

**Correct:**
```vue
<!-- ✅ 正确：基础用法 -->
<BaseShowFileList :file-list="detailData.attachmentList" empty-text="暂无附件" />

<!-- ✅ 正确：自定义预览行为 -->
<BaseShowFileList
    :file-list="detailData.attachmentList"
    :stopInsideOnPreview="handleCustomPreview"
/>

<script setup>
const handleCustomPreview = fileData => {
    console.log('自定义预览:', fileData)
    window.open(fileData.systemFileShowUrl, '_blank')
}
</script>
```

## KHD 客户端

KHD 客户端项目使用 `BaseShowFileListKHD`，Props 和数据结构一致。
