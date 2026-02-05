---
title: 删除操作确认规范
impact: HIGH
impactDescription: 删除操作未正确确认会导致数据误删除
type: best-practice
tags: [vue3, element-plus, delete, confirm, messagebox]
appliesTo: [DataTable]
---

# 删除操作确认规范

**Impact: HIGH** - 所有删除操作必须使用 ElMessageBox.confirm 进行二次确认。

## Task Checklist

- [ ] **必须使用 `async/await` + `try-catch`**，禁止使用 `.then()`
- [ ] 确认弹窗使用 `ElMessageBox.confirm`
- [ ] 提示语固定为：`'删除后不可恢复，请谨慎操作'`
- [ ] 标题固定为：`'提示'`
- [ ] 必须设置 `type: 'warning'`

> ⚠️ **WARNING**: 禁止使用 `.then()` 链式调用，必须使用 `async/await` + `try-catch`

## 标准删除操作代码

```javascript
/**
 * 删除数据
 * @param {object} row 行数据
 */
const handleDelete = async row => {
    try {
        await ElMessageBox.confirm('删除后不可恢复，请谨慎操作', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
        })
        await requestToken({
            url: '/Api/xxx/xxx/delete',
            data: { xxxID: row.xxxID }
        })
        getAllDataTable()
    } catch (error) {
        // error
    }
}
```

**Incorrect:**
```javascript
// ❌ 错误：使用 .then() 链式调用
const handleDelete = row => {
    ElMessageBox.confirm('确定删除吗？', '提示').then(() => {
        requestToken({ url: '/Api/xxx/delete', data: { id: row.id } }).then(() => {
            getAllDataTable()
        })
    }).catch(() => {})
}

// ❌ 错误：提示语不规范
const handleDelete = async row => {
    try {
        await ElMessageBox.confirm('确定删除吗？', '警告', {
            type: 'error'
        })
        // ...
    } catch (error) {}
}

// ❌ 错误：缺少 type: 'warning'
const handleDelete = async row => {
    try {
        await ElMessageBox.confirm('删除后不可恢复，请谨慎操作', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消'
            // 缺少 type: 'warning'
        })
        // ...
    } catch (error) {}
}
```

**Correct:**
```javascript
// ✅ 正确：完整的删除操作
const handleDelete = async row => {
    try {
        await ElMessageBox.confirm('删除后不可恢复，请谨慎操作', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
        })
        await requestToken({
            url: '/Api/xxx/xxx/delete',
            data: { xxxID: row.xxxID }
        })
        getAllDataTable()
    } catch (error) {
        // error
    }
}
```

## 配置说明

| 配置项 | 固定值 | 说明 |
|:------|:------|:----|
| 提示语 | `'删除后不可恢复，请谨慎操作'` | 警示用户操作不可逆 |
| 标题 | `'提示'` | 统一标题 |
| confirmButtonText | `'确定'` | 确认按钮文字 |
| cancelButtonText | `'取消'` | 取消按钮文字 |
| type | `'warning'` | 警告类型图标 |

## 导入说明

```javascript
import { ElMessageBox } from 'element-plus'
```
