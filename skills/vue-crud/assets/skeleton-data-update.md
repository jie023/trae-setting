---
title: DataUpdate.vue 骨架模板
type: skeleton
output: DataUpdate.vue
description: 新增/编辑表单页骨架，包含 Tab、表单验证、富文本、上传组件
---

# DataUpdate.vue 骨架模板

> **生成前必读**：[代码生成规则汇总](all-rules-summary.md)

复制以下代码作为 DataUpdate.vue 的基础框架，根据 TODO 注释填充业务逻辑。

## 相关规则

生成代码时需参考以下规则：
- 输入框 → [form-input-text.md](../references/form-input-text.md)
- 下拉框 → [form-select.md](../references/form-select.md)
- 防 null → [form-reset-null-prevention.md](../references/form-reset-null-prevention.md)
- 上传组件 → [component-base-upload-attachment.md](../references/component-base-upload-attachment.md)
- 标签选择 → [component-base-tag-select.md](../references/component-base-tag-select.md)
- CSS 规范 → [css-class-standard.md](../references/css-class-standard.md)

> ⚠️ **禁止自定义 style**：表单组件禁止添加 `style` 属性，宽度由最顶层 class 统一控制！

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
                <!-- 示例：文本输入框 -->
                <el-form-item label="名称" prop="name">
                    <el-input v-model="formData.name" placeholder="请输入名称" clearable maxLength="20"></el-input>
                </el-form-item>

                <!-- 示例：动态数据下拉框（必须包含 filterable + @change 空值处理） -->
                <el-form-item label="类型" prop="typeID">
                    <el-select
                        v-model="formData.typeID"
                        placeholder="请选择类型"
                        clearable
                        filterable
                        @change="val => formData.typeID = val ?? ''"
                    >
                        <el-option
                            v-for="item in typeList"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value"
                        />
                    </el-select>
                </el-form-item>

                <!-- 示例：静态字典下拉框（无 filterable + @change 空值处理） -->
                <el-form-item label="状态" prop="status">
                    <el-select
                        v-model="formData.status"
                        placeholder="请选择状态"
                        clearable
                        @change="val => formData.status = val ?? ''"
                    >
                        <el-option label="启用" value="1" />
                        <el-option label="禁用" value="0" />
                    </el-select>
                </el-form-item>

                <!-- 示例：带联动逻辑的下拉框（空值处理必须在模板层，联动函数只管业务逻辑） -->
                <el-form-item label="用户" prop="userName">
                    <el-select
                        v-model="formData.userName"
                        placeholder="请选择用户"
                        clearable
                        filterable
                        @change="val => (formData.userName = val ?? '', handleUserChange(val))"
                    >
                        <el-option
                            v-for="item in userList"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value"
                        />
                    </el-select>
                </el-form-item>

                <!-- 示例：日期选择器（必须包含 @change 空值处理） -->
                <el-form-item label="签发日期" prop="issueDate">
                    <el-date-picker
                        v-model="formData.issueDate"
                        type="date"
                        placeholder="请选择日期"
                        value-format="YYYY-MM-DD"
                        clearable
                        @change="val => formData.issueDate = val ?? ''"
                    />
                </el-form-item>

                <!-- 示例：日期范围选择器（必须包含 @change 空值处理，重置为空数组） -->
                <el-form-item label="有效期" prop="dateRange">
                    <el-date-picker
                        v-model="formData.dateRange"
                        type="daterange"
                        start-placeholder="开始日期"
                        end-placeholder="结束日期"
                        value-format="YYYY-MM-DD"
                        clearable
                        @change="val => formData.dateRange = val ?? []"
                    />
                </el-form-item>

                <!-- TODO: 添加更多编辑项 -->
            </el-form>
        </div>

        <div class="page_suspension_bottom">
            <el-button type="primary" @click="handleSubmit">保存</el-button>
            <el-button @click="handleCancel">取消</el-button>
        </div>
    </div>
</template>

<script setup>
import { reactive, ref, onMounted, defineProps, defineEmits } from 'vue'
import { requestToken } from '@/util/adminRequest'

// ==================== 1. props 和 emits 定义 ====================
const props = defineProps({
    xxxID: { // TODO: 修改为业务 ID 名称
        type: String,
        default: ''
    }
})
const emits = defineEmits(['confirm', 'cancel'])

// ==================== 3. 响应式数据（ref、reactive） ====================
// 页面状态
const loading = ref(false)
const formDataRef = ref(null)

// TODO: 定义详情数据
const formData = reactive({
    xxxID: '', // TODO: 修改为业务 ID 名称
})

// ==================== 4. 表单验证规则 ====================
// 定义验证规则
const rules = reactive({
})

// ==================== 6. 方法函数定义 ====================

/**
 * 用户选择变更联动处理
 * @param {string} val 选中的用户
 * @description 空值处理已在模板层完成，此函数只处理联动逻辑
 */
const handleUserChange = val => {
    // 只处理联动逻辑，无需关心 null（模板层已处理）
    const matched = userList.value.find(u => u.value === val)
    formData.jobNum = matched ? matched.jobNum : ''
}

/**
 * 提交表单数据
 */
const handleSubmit = async () => {
    try {
        loading.value = true
        await formDataRef.value.validate()
        await requestToken({
            url: '/Api/xxx/xxx/save', // TODO: 修改 API 地址
            data: {
                // TODO: 构造提交数据
                xxxID: formData.xxxID,
                name: formData.name,
                typeID: formData.typeID,
                description: formData.description,
                attachmentFileIDs: formData.attachmentList.map(f => f.systemFileID).join(',')
            }
        })
        emits('confirm')
    } catch (error) {
        // error
    } finally {
        loading.value = false
    }
}

/**
 * 获取详情数据并回显表单
 */
const getOneData = async xxxID => {
    formData.xxxID = xxxID // TODO: 修改为业务 ID 名称

    if (!xxxID) return
    try {
        loading.value = true
        const res = await requestToken({
            url: '/Api/xxx/xxx/xxx', // TODO: 修改 API 地址
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

/**
 * 关闭弹窗
 */
const handleCancel = () => emits('cancel')

// ==================== 8. 生命周期钩子 ====================
onMounted(() => getOneData(props.xxxID)) // TODO: 修改为业务 ID 名称
</script>
```

## Class 使用规范

| 元素 | Class | 说明 |
|:----|:-----|:----|
| 外层容器 | `removeTheBox page_container` | 页面主容器，配合 v-loading |
| 滚动区域 | `page_scroll_middle` | 表单内容可滚动区域 |
| 底部按钮 | `page_suspension_bottom` | 固定在底部的操作按钮区 |

## 表单标签宽度

根据 label 最大字数选择对应常量：

| label 字数 | 使用常量 |
|:----------|:--------|
| ≤0 | `$FORM_LABEL_WIDTH.ZERO` |
| 1 | `$FORM_LABEL_WIDTH.ONE` |
| 2 | `$FORM_LABEL_WIDTH.TWO` |
| 3 | `$FORM_LABEL_WIDTH.THREE` |
| ≥4 | `$FORM_LABEL_WIDTH.FOUR` |
