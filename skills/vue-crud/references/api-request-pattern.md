---
title: API 请求规范
impact: HIGH
impactDescription: 不规范的 API 请求会导致异常处理不当、代码难以维护
type: best-practice
tags: [vue3, api, async, request]
appliesTo: [DataTable, DataUpdate, DataShow]
---

# API 请求规范

**Impact: HIGH** - 所有 API 请求必须使用 `requestToken`，并遵循 async/await 规范。

> ⚠️ **CRITICAL WARNING - 最高优先级规则**:
>
> ## 🚫 绝对禁止使用 `.then()` 链式调用！
>
> **所有异步操作（包括 requestToken、ElMessageBox 等）必须使用 `async/await` + `try-catch` 模式！**
>
> 违反此规则的代码将被视为不合格代码。

## Task Checklist

- [ ] ⭐ **第一条（最重要）：禁止使用 `.then()` 链式调用，必须使用 `async/await` + `try-catch`**
- [ ] 使用 `import { requestToken } from '@/util/adminRequest'`
- [ ] 所有异步请求包裹在 `try-catch` 中
- [ ] 列表请求包含 `offset` 和 `rows` 参数
- [ ] 业务数据包裹在 `data` 对象中

**Incorrect:**
```javascript
// ❌❌❌ 严重错误：使用 .then() 链式调用 ❌❌❌
// 这是最常见的错误，绝对禁止！
requestToken({ url: '/Api/xxx', data: {} }).then(res => {
    // 处理结果
})

// ❌❌❌ 严重错误：ElMessageBox 使用 .then() ❌❌❌
ElMessageBox.confirm('确认？', '提示').then(() => {
    // 确认逻辑
}).catch(() => {})

// ❌❌❌ 严重错误：嵌套 .then() ❌❌❌
ElMessageBox.confirm('确定删除吗？', '提示').then(() => {
    requestToken({ url: '/Api/xxx/delete', data: {} }).then(() => {
        getAllDataTable()
    })
}).catch(() => {})

// ❌ 错误：缺少 try-catch
const fetchData = async () => {
    const res = await requestToken({ url: '/Api/xxx', data: {} })
    // 处理结果
}
```

**Correct:**
```javascript
/**
 * 获取列表数据
 * @description 所有异步请求必须 try-catch，并使用 requestToken
 */
const getAllDataTable = async () => {
    try {
        const res = await requestToken({
            url: '/Api/xxx/xxx/xxx',
            data: {
                offset: tableData.offset,
                rows: tableData.rows,
                data: {
                    ...tableSelectData
                }
            }
        })
        tableData.tableDataList = res.data.list
        tableData.tableDataSize = res.data.allSize
    } catch (error) {
        // error
    }
}

/**
 * 删除数据
 * @param {object} row 行数据
 * @description 确认弹窗必须使用 async/await + try-catch
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

## 请求参数结构

| 场景 | 参数结构 |
|:----|:--------|
| 列表请求 | `{ offset, rows, data: { 筛选条件 } }` |
| 详情请求 | `{ data: { xxxID: xxxID } }` |
| 新增/编辑 | `{ data: { xxxID: formData.xxxID } }` |
| 删除请求 | `{ data: { xxxID: xxxID } }` |
