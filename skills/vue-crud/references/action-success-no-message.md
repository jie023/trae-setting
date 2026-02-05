---
title: 禁止手动调用消息提示规范
impact: MEDIUM
impactDescription: 请求已统一封装消息提示，手动调用会导致重复提示
type: best-practice
tags: [vue3, element-plus, message, ux]
appliesTo: [DataTable, DataUpdate]
---

# 禁止手动调用消息提示规范

**Impact: MEDIUM** - 请求已统一封装成功/失败的消息提示，业务代码中禁止手动调用 ElMessage。

> ⚠️ **WARNING**:
>
> ## 禁止手动调用 ElMessage！
>
> **项目的 `requestToken` 函数已统一封装了成功/失败的消息提示，业务代码中无需也禁止手动调用 `ElMessage.success` 或 `ElMessage.error`。**

## Task Checklist

- [ ] **禁止使用 ElMessage.success**
- [ ] **禁止使用 ElMessage.error**
- [ ] **禁止使用 ElMessage.warning**（除非是表单校验等非请求场景）
- [ ] 所有请求的成功/失败提示由 requestToken 统一处理

## 适用场景

| 操作类型 | 是否需要手动调用 ElMessage |
|:--------|:-------------------------|
| 删除 | ❌ 不需要（requestToken 自动提示） |
| 撤销 | ❌ 不需要（requestToken 自动提示） |
| 保存（新增/编辑） | ❌ 不需要（requestToken 自动提示） |
| 恢复 | ❌ 不需要（requestToken 自动提示） |
| 提交审批 | ❌ 不需要（requestToken 自动提示） |
| 请求失败 | ❌ 不需要（requestToken 自动提示） |

**Incorrect:**
```javascript
// ❌ 错误：手动提示成功
const handleDelete = async row => {
    try {
        await ElMessageBox.confirm('删除后不可恢复，请谨慎操作', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
        })
        await requestToken({
            url: '/Api/xxx/delete',
            data: { xxxID: row.xxxID }
        })
        ElMessage.success('删除成功')  // ❌ 不需要，requestToken 已自动提示
        getAllDataTable()
    } catch (error) {
        ElMessage.error('删除失败')  // ❌ 不需要，requestToken 已自动提示
    }
}

// ❌ 错误：手动提示成功/失败
const handleSubmit = async () => {
    try {
        await formDataRef.value.validate()
        await requestToken({
            url: '/Api/xxx/save',
            data: { ...formData }
        })
        ElMessage.success('保存成功')  // ❌ 不需要
        emits('confirm')
    } catch (error) {
        ElMessage.error('保存失败')  // ❌ 不需要
    }
}
```

**Correct:**
```javascript
// ✅ 正确：不手动调用 ElMessage，由 requestToken 统一处理
const handleDelete = async row => {
    try {
        await ElMessageBox.confirm('删除后不可恢复，请谨慎操作', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
        })
        await requestToken({
            url: '/Api/xxx/delete',
            data: { xxxID: row.xxxID }
        })
        getAllDataTable()
    } catch (error) {
        // requestToken 已自动处理错误提示
    }
}

// ✅ 正确：不手动调用 ElMessage
const handleSubmit = async () => {
    try {
        await formDataRef.value.validate()
        await requestToken({
            url: '/Api/xxx/save',
            data: { ...formData }
        })
        emits('confirm')
    } catch (error) {
        // requestToken 已自动处理错误提示
    }
}
```

## 为什么不需要手动提示？

**请求已封装好会自动提示**：项目的 `requestToken` 函数已统一封装了成功/失败的消息提示，无需在业务代码中重复调用 `ElMessage`。
