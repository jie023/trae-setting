---
title: ElMessageBox 使用规范
impact: HIGH
impactDescription: MessageBox 使用不规范会导致代码风格不统一和错误处理问题
type: best-practice
tags: [vue3, element-plus, messagebox, dialog, async]
appliesTo: [DataTable, DataUpdate, DataShow]
---

# ElMessageBox 使用规范

**Impact: HIGH** - ElMessageBox 必须使用 async/await + try-catch，禁止使用 .then()。

## Task Checklist

- [ ] **禁止使用 `.then()` 链式调用**
- [ ] 必须使用 `async/await` + `try-catch`
- [ ] 必须设置 `type: 'warning'`
- [ ] confirmButtonText 固定为 `'确定'`
- [ ] cancelButtonText 固定为 `'取消'`

> ⚠️ **CRITICAL WARNING**:
>
> **绝对禁止**使用 `.then().catch()` 链式调用！
>
> 必须使用 `async/await` + `try-catch` 模式！

## 标准配置

```javascript
await ElMessageBox.confirm('提示内容', '提示', {
    confirmButtonText: '确定',
    cancelButtonText: '取消',
    type: 'warning'
})
```

## 固定配置项

| 配置项 | 固定值 | 说明 |
|:------|:------|:----|
| title | `'提示'` | 弹窗标题 |
| confirmButtonText | `'确定'` | 确认按钮 |
| cancelButtonText | `'取消'` | 取消按钮 |
| type | `'warning'` | 图标类型，统一使用警告 |

**Incorrect:**
```javascript
// ❌ 错误：使用 .then() 链式调用
ElMessageBox.confirm('确定操作吗？', '提示').then(() => {
    // 确认逻辑
}).catch(() => {})

// ❌ 错误：使用 .then() + async
ElMessageBox.confirm('确定吗？', '提示').then(async () => {
    await doSomething()
}).catch(() => {})

// ❌ 错误：缺少 type
await ElMessageBox.confirm('确定吗？', '提示', {
    confirmButtonText: '确定',
    cancelButtonText: '取消'
})

// ❌ 错误：使用错误的 type
await ElMessageBox.confirm('确定吗？', '提示', {
    confirmButtonText: '确定',
    cancelButtonText: '取消',
    type: 'error'  // 应该使用 'warning'
})
```

**Correct:**
```javascript
// ✅ 正确：async/await + try-catch + type: 'warning'
const handleConfirm = async () => {
    try {
        await ElMessageBox.confirm('确定执行此操作吗？', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
        })
        // 用户点击确定后的逻辑
        await doSomething()
    } catch (error) {
        // 用户点击取消或关闭弹窗
    }
}

// ✅ 正确：删除确认
const handleDelete = async row => {
    try {
        await ElMessageBox.confirm('删除后不可恢复，请谨慎操作', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
        })
        await requestToken({
            url: '/Api/xxx/delete',
            data: { id: row.id }
        })
        getAllDataTable()
    } catch (error) {
        // error
    }
}
```

## 为什么禁止 .then()？

1. **代码可读性差**：嵌套的 .then() 链难以阅读和维护
2. **错误处理不一致**：.catch() 容易被遗漏
3. **与项目规范冲突**：项目统一使用 async/await 风格
4. **调试困难**：链式调用的堆栈信息不清晰

## 导入说明

```javascript
import { ElMessageBox } from 'element-plus'
```
