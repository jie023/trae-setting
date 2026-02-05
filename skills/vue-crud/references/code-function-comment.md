---
title: 函数注释规范
impact: MEDIUM
impactDescription: 缺少注释会降低代码可维护性
type: best-practice
tags: [vue3, code-style, comment]
appliesTo: [DataTable, DataUpdate, DataShow, index]
---

# 函数注释规范

**Impact: MEDIUM** - 所有函数（含事件处理、API请求、校验）必须包含中文注释块。

## Task Checklist

- [ ] 每个函数必须有 JSDoc 格式的注释
- [ ] 注释使用中文描述
- [ ] 参数类型和说明清晰

## 注释格式

```javascript
/**
 * 函数描述
 * @description 详细说明（可选）
 * @param {Type} name 参数说明（可选）
 * @returns {Type} 返回值说明（可选）
 */
```

**Incorrect:**
```javascript
// ❌ 错误：缺少注释或注释不规范
const handleSearch = () => {
    tableData.offset = 0
    getAllDataTable()
}

// 搜索  ← ❌ 单行注释不够规范
const handleSearch = () => {}
```

**Correct:**
```javascript
/**
 * 查询列表（重置分页到第一页）
 * @description 重置分页参数后调用列表查询接口
 */
const handleSearch = () => {
    tableData.offset = 0
    tableData.currentPage = 1
    getAllDataTable()
}

/**
 * 删除数据
 * @param {object} row 行数据
 * @description 确认弹窗后执行删除操作
 */
const handleDelete = async row => {
    // ...
}

/**
 * 获取状态文案
 * @param {number|string} state 状态值
 * @returns {string} 状态显示文案
 */
const getStatusLabel = state => ({ 0: '新建', 1: '待审核', 2: '已驳回' })[state] || '-'
```
