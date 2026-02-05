---
title: BaseUploadAttachment 附件上传组件
impact: HIGH
impactDescription: 上传组件使用错误会导致文件无法正确上传或数据结构不匹配
type: component
tags: [vue3, element-plus, upload, component]
appliesTo: [DataUpdate]
---

# BaseUploadAttachment 附件上传组件

**Impact: HIGH** - 附件上传组件，支持文件上传、预览、删除。全局注册，无需导入。

## Task Checklist

- [ ] 必须传入 `uploadUrl` 属性
- [ ] 使用 `v-model` 绑定文件列表
- [ ] 文件数据结构必须符合标准格式
- [ ] 直接绑定 formData 字段，禁止创建额外中间变量

## Props

| 属性名 | 类型 | 默认值 | 必填 | 说明 |
|:------|:----|:------|:----|:----|
| uploadUrl | String | - | ✅ | 文件上传接口地址 |
| modelValue | Array | [] | - | 文件列表，支持 v-model |
| limit | Number | 0 | - | 最大上传数量，0 表示不限制 |
| accept | String | '*' | - | 接受的文件类型 |
| tip | String | '' | - | 自定义提示文字 |
| onSuccess | Function | null | - | 上传成功回调 |
| onPreview | Function | null | - | 预览回调（不阻止默认下载行为） |
| onRemove | Function | null | - | 删除回调 |
| onExceed | Function | null | - | 超出限制回调 |
| stopInsideOnPreview | Function | null | - | 预览回调（阻止默认下载行为） |

## 数据结构

```javascript
{
  systemFileID: '',           // 文件唯一标识
  systemFileName: '',         // 文件名称
  systemFileUrl: '',          // 文件下载地址
  systemFileShowUrl: '',      // 文件预览地址
  systemFileFormatType: '',   // 文件格式类型
  systemFileFormatTypeImgUrl: '' // 文件类型图标地址
}
```

**Incorrect:**
```vue
<!-- ❌ 错误：使用中间变量 -->
<script setup>
const tempFileList = ref([])
// 然后再同步到 formData...
</script>
<template>
    <BaseUploadAttachment v-model="tempFileList" uploadUrl="/api/upload" />
</template>
```

**Correct:**
```vue
<!-- ✅ 正确：直接绑定 formData 字段 -->
<template>
    <BaseUploadAttachment
        v-model="formData.attachmentList"
        uploadUrl="/api/upload"
        :limit="5"
        accept=".pdf,.doc,.docx"
        tip="仅支持上传 PDF 和 Word 文档"
    />
</template>

<script setup>
import { reactive } from 'vue'

const formData = reactive({
    attachmentList: []
})
</script>
```

## KHD 客户端

KHD 客户端项目使用 `BaseUploadAttachmentKHD`，Props 和数据结构一致。
