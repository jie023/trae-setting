---
title: DataImport.vue 骨架模板
type: skeleton
output: DataImport.vue
description: Excel/数据导入页骨架，含下载模板、上传文件、错误预览
---

# DataImport.vue 骨架模板

> **生成前必读**：[代码生成规则汇总](all-rules-summary.md)

复制以下代码作为 DataImport.vue 的基础框架，根据 TODO 注释填充业务逻辑。

## 相关规则

生成代码时需参考以下规则：
- 上传组件 → [component-base-upload-attachment.md](../references/component-base-upload-attachment.md)
- CSS 规范 → [css-class-standard.md](../references/css-class-standard.md)
- API 请求 → [api-request-pattern.md](../references/api-request-pattern.md)

## 骨架代码

```vue
<template>
    <div class="removeTheBox page_container" v-loading="loading">
        <div class="page_scroll_middle">
            <el-form
                ref="formDataRef"
                :model="formData"
                :rules="rules"
                label-position="left"
                :label-width="$FORM_LABEL_WIDTH.FOUR"
            >
                <!-- 第一步：下载模板 -->
                <el-form-item label="第一步">
                    <div class="page_import_button">
                        <el-button size="small" type="primary" @click="handleDownloadTemplate" :loading="exportLoading">
                            下载模板
                        </el-button>
                    </div>
                </el-form-item>

                <!-- 第二步：上传文件 -->
                <el-form-item label="第二步" prop="systemFileID">
                    <BaseUploadAttachmentKHD
                        :limit="1"
                        :uploadUrl="indexSystem.baseUploadApiUrl"
                        :on-success="handleUpload"
                        tip="只能上传xls文件，且不超过50M，一次最多能导入1000条"
                        accept="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet, application/vnd.ms-excel"
                        v-model="formData.fileList"
                    ></BaseUploadAttachmentKHD>
                </el-form-item>

                <!-- 错误内容展示 -->
                <el-form-item label="错误内容" v-if="formData.errorData !== null && formData.errorData.length > 0">
                    <div
                        class="indexData_DataUpdate_class_detail_span page_import_detail_span"
                        v-for="(item, index) in formData.errorData"
                        :key="index"
                    >
                        {{ item }}
                    </div>
                </el-form-item>
            </el-form>
        </div>

        <!-- 底部按钮 -->
        <div class="page_suspension_bottom">
            <el-button type="primary" @click="handleSubmit">保存</el-button>
            <el-button @click="handleCancel">取消</el-button>
        </div>
    </div>
</template>

<script setup>
import { defineEmits, defineProps, reactive, ref } from 'vue'
import { requestToken } from '@/util/adminRequest.js'
import { downloadFile } from '@/util/windowMethodKHD.js'
import indexSystem from '@/configure/commons/index'

// ==================== 1. props 和 emits 定义 ====================
const props = defineProps({
    // TODO: 根据业务需要定义 props
    parentID: {
        type: String,
        default: ''
    }
})

const emits = defineEmits(['confirm', 'cancel'])

// ==================== 3. 响应式数据（ref、reactive） ====================
// 页面状态
const loading = ref(false)
const exportLoading = ref(false)
const formDataRef = ref(null)

// 表单数据
const formData = reactive({
    fileList: [],
    errorData: [],
    systemFileID: ''
})

// ==================== 4. 表单验证规则 ====================
const rules = reactive({
    systemFileID: [{ required: true, message: '请上传文件', trigger: 'blur' }]
})

// ==================== 6. 方法函数定义 ====================
/**
 * 下载导入模板
 */
const handleDownloadTemplate = async () => {
    try {
        exportLoading.value = true
        const res = await requestToken({
            url: '/Api/xxx/xxx/getTemplate', // TODO: 替换为实际接口
            data: {}
        })

        if (res.data) {
            downloadFile(res.data.systemFileUrl, res.data.systemFileName)
        }
    } catch (error) {
        // error
    } finally {
        exportLoading.value = false
    }
}

/**
 * 文件上传成功回调
 */
const handleUpload = () => {
    if (formData.fileList && formData.fileList.length > 0) {
        formData.systemFileID = formData.fileList[0].uid
    } else {
        formData.systemFileID = ''
    }
    formData.errorData = []
}

/**
 * 提交导入数据
 */
const handleSubmit = async () => {
    try {
        loading.value = true

        const valid = await formDataRef.value.validate()
        if (!valid) return

        const submitData = {
            systemFileID: formData.systemFileID
            // TODO: 添加其他业务参数
        }

        const res = await requestToken({
            url: '/Api/xxx/xxx/import', // TODO: 替换为实际接口
            data: submitData
        })

        if (res.data) {
            const errorList = res.data?.errorList

            if (errorList && errorList.length > 0) {
                formData.errorData = errorList
            } else {
                emits('confirm')
            }
        }
    } catch (error) {
        // error
    } finally {
        loading.value = false
    }
}

/**
 * 取消操作
 */
const handleCancel = () => {
    emits('cancel')
}
</script>
```

## Class 使用规范

| 元素 | Class | 说明 |
|:----|:-----|:----|
| 外层容器 | `removeTheBox page_container` | 页面主容器，配合 v-loading |
| 滚动区域 | `page_scroll_middle` | 内容可滚动区域 |
| 底部按钮 | `page_suspension_bottom` | 固定在底部的操作按钮区 |
| 导入按钮区 | `page_import_button` | 下载模板按钮包装 |
| 错误信息 | `indexData_DataUpdate_class_detail_span page_import_detail_span` | 错误内容展示 |

## 导入流程

1. **下载模板** - 用户获取标准导入模板
2. **上传文件** - 用户填写数据后上传 Excel 文件
3. **错误处理** - 展示导入失败的错误信息

## 文件类型限制

```javascript
accept="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet, application/vnd.ms-excel"
```

- `.xlsx` - `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`
- `.xls` - `application/vnd.ms-excel`
