---
title: BaseUploadCropImage 图片裁剪上传组件
impact: HIGH
impactDescription: 图片上传组件使用错误会导致图片无法正确上传或裁剪
type: component
tags: [vue3, element-plus, upload, image, crop, component]
appliesTo: [DataUpdate]
---

# BaseUploadCropImage 图片裁剪上传组件

**Impact: HIGH** - 图片上传组件，支持裁剪、压缩和预览。全局注册，无需导入。

## Task Checklist

- [ ] 必须传入 `uploadUrl` 属性
- [ ] 使用 `v-model` 绑定图片列表
- [ ] 根据需求配置裁剪比例 `cropperOptions.aspectRatio`
- [ ] 直接绑定 formData 字段，禁止创建额外中间变量

## Props

| 属性名 | 类型 | 默认值 | 必填 | 说明 |
|:------|:----|:------|:----|:----|
| uploadUrl | String | - | ✅ | 图片上传接口地址 |
| modelValue | Array | `[]` | - | 双向绑定的图片列表 |
| limit | Number | `0` | - | 最大上传数量，0 表示不限制 |
| maxSize | Number | `2097152` | - | 触发压缩的文件大小阈值（字节），默认 2MB |
| maxWidth | Number | `3840` | - | 压缩后最大宽度 |
| maxHeight | Number | `2160` | - | 压缩后最大高度 |
| cropperDialogWidth | String | `'80%'` | - | 裁剪对话框宽度 |
| cropperOptions | Object | `{ viewMode: 1, dragMode: 'move' }` | - | 裁剪器配置 |
| cropperPresetMode | Object | `{ mode: 'fixedSize' }` | - | 裁剪器预设模式 |
| onSuccess | Function | - | - | 上传成功回调 |
| onPreview | Function | - | - | 点击预览回调（不阻止默认预览行为） |
| onRemove | Function | - | - | 删除文件回调 |
| stopInsideOnPreview | Function | - | - | 点击预览回调（**阻止**默认预览行为） |

## 数据结构

```javascript
{
  systemFileID: '',           // 文件ID
  systemFileName: '',         // 文件名
  systemFileUrl: '',          // 文件URL
  systemFileShowUrl: '',      // 展示URL
  systemFileFormatType: '',   // 文件格式类型
  systemFileFormatTypeImgUrl: '' // 格式类型图标URL
}
```

**Correct:**
```vue
<!-- ✅ 正确：基本用法 -->
<BaseUploadCropImage
    v-model="formData.imageList"
    :uploadUrl="uploadUrl"
    :limit="1"
/>

<!-- ✅ 正确：自定义裁剪比例（3:4） -->
<BaseUploadCropImage
    v-model="formData.coverImage"
    :uploadUrl="uploadUrl"
    :limit="1"
    :cropperOptions="{
        viewMode: 1,
        dragMode: 'move',
        aspectRatio: 3 / 4,
        autoCropArea: 0.8,
        cropBoxMovable: true,
        cropBoxResizable: true
    }"
/>
<div class="upload_image_tips">建议上传比例为3:4的图片</div>

<!-- ✅ 正确：自定义压缩参数 -->
<BaseUploadCropImage
    v-model="formData.imageList"
    :uploadUrl="uploadUrl"
    :max-size="1024 * 1024"
    :max-width="1920"
    :max-height="1080"
/>

<script setup>
import indexSystem from '@/configure/commons/index'

const uploadUrl = indexSystem.baseUploadApiUrl
</script>
```

## 常用裁剪比例配置

| 场景 | aspectRatio |
|:----|:-----------|
| 正方形头像 | `1` 或 `1 / 1` |
| 16:9 横幅 | `16 / 9` |
| 3:4 封面 | `3 / 4` |
| 4:3 照片 | `4 / 3` |

## KHD 客户端

KHD 客户端项目使用 `BaseUploadCropImageKHD`，Props 和数据结构一致。
