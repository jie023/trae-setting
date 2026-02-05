---
title: 分页参数规范
impact: HIGH
impactDescription: 分页参数不规范会导致后端无法正确解析
type: best-practice
tags: [vue3, pagination, api]
appliesTo: [DataTable]
---

# 分页参数规范

**Impact: HIGH** - 分页参数必须使用 `offset` 和 `rows`，这是后端接口的强制要求。

## Task Checklist

- [ ] 使用 `offset` 表示偏移量（从 0 开始）
- [ ] 使用 `rows` 表示每页条数
- [ ] `currentPage` 仅用于前端分页组件显示
- [ ] 计算公式：`offset = (currentPage - 1) * rows`

**Incorrect:**
```javascript
// ❌ 错误：参数命名不规范
const tableData = reactive({
    page: 1,           // ❌ 应该用 offset
    pageSize: 10,      // ❌ 应该用 rows
    list: [],
    total: 0
})

// ❌ 错误：直接传 page 给后端
const params = {
    page: tableData.page,
    pageSize: tableData.pageSize
}
```

**Correct:**
```javascript
// ✅ 正确：标准分页数据结构
const tableData = reactive({
    offset: 0,           // 偏移量（后端需要）
    rows: 10,            // 每页条数（后端需要）
    currentPage: 1,      // 当前页码（前端分页组件用）
    tableDataList: [],   // 列表数据
    tableDataSize: 0     // 总条数
})

// ✅ 正确：请求参数
const params = {
    offset: tableData.offset,
    rows: tableData.rows,
    data: { ...tableSelectData }
}
```

## 分页切换处理

```javascript
/**
 * 切换每页条数
 * @fixed 固定实现，禁止修改
 * @param {number} val 每页条数
 */
const handleSizeChange = val => {
    tableData.currentPage = 1
    tableData.rows = val
    tableData.offset = 0
    getAllDataTable()
}

/**
 * 切换页码
 * @fixed 固定实现，禁止修改
 * @param {number} val 当前页码
 */
const handleCurrentChange = val => {
    tableData.currentPage = val
    tableData.offset = (parseInt(val) - 1) * tableData.rows
    getAllDataTable()
}
```

## 分页组件绑定

```vue
<el-pagination
    class="indexData_DataTable_class_pagination"
    @size-change="handleSizeChange"
    @current-change="handleCurrentChange"
    v-model:current-page="tableData.currentPage"
    :page-sizes="[10, 20, 30, 40]"
    v-model:page-size="tableData.rows"
    layout="total, sizes, prev, pager, next, jumper"
    :total="tableData.tableDataSize"
></el-pagination>
```
