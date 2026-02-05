---
title: DataLogTable.vue 骨架模板
type: skeleton
output: DataLogTable.vue
description: 纯展示用日志/记录表格骨架，通常无操作列
---

# DataLogTable.vue 骨架模板

> **生成前必读**：[代码生成规则汇总](all-rules-summary.md)

复制以下代码作为 DataLogTable.vue 的基础框架，根据 TODO 注释填充业务逻辑。

## 相关规则

生成代码时需参考以下规则：
- 表格列宽 → [table-column-width.md](../references/table-column-width.md)
- 分页参数 → [api-pagination-params.md](../references/api-pagination-params.md)
- 日期格式化 → [util-format-date.md](../references/util-format-date.md)

## 骨架代码

```vue
<template>
    <div class="data_table_container">
        <div class="data_table_scroll_middle">
            <!-- 表格 -->
            <el-table :data="tableData.tableDataList" class="indexData_DataTable_style">
                <el-table-column
                    label="序号"
                    type="index"
                    :width="$tableItemWidthCalculation(2)"
                    :index="tableData.offset + 1"
                    show-overflow-tooltip
                    fixed
                ></el-table-column>
                <el-table-column
                    prop="logTime"
                    label="操作时间"
                    :min-width="$tableItemWidthCalculation(9)"
                    show-overflow-tooltip
                >
                    <template #default="scope">
                        {{ formatDateString('yyyy-MM-dd HH:mm', scope.row.logTime) }}
                    </template>
                </el-table-column>
                <el-table-column
                    prop="logState"
                    label="操作"
                    :width="$tableItemWidthCalculation(4)"
                    show-overflow-tooltip
                >
                    <template #default="scope">
                        {{ getLogStateLabel(scope.row.logState) }}
                    </template>
                </el-table-column>
                <el-table-column
                    prop="logUserName"
                    label="操作人"
                    :width="$tableItemWidthCalculation(4)"
                    show-overflow-tooltip
                ></el-table-column>
                <el-table-column
                    prop="logRemarks"
                    label="备注"
                    :width="$tableItemWidthCalculation(12)"
                    show-overflow-tooltip
                ></el-table-column>
            </el-table>

            <!-- 分页 -->
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
        </div>
    </div>
</template>

<script setup>
import { reactive, defineProps, onMounted } from 'vue'
import { formatDateString } from '@web-npm/baseutils'
import { requestToken } from '@/util/adminRequest'

// ==================== 1. props 和 emits 定义 ====================
const props = defineProps({
    xxxInfoID: { type: String, default: '' }  // TODO: 修改为业务 ID
})

// ==================== 3. 响应式数据（ref、reactive） ====================
const tableData = reactive({
    tableDataList: [],
    tableDataSize: 0,
    currentPage: 1,
    offset: 0,
    rows: 10
})

// ==================== 6. 方法函数定义 ====================
/**
 * 获取日志操作状态文案
 */
const getLogStateLabel = state => {
    // TODO: 根据业务定义状态映射
    const stateMap = { 0: '创建', 1: '修改', 2: '提交审核', 3: '审核通过', 4: '审核驳回', 5: '发布', 6: '下线' }
    return stateMap[state] || '未知操作'
}

/**
 * 获取日志列表数据
 */
const getAllDataTable = async () => {
    try {
        const res = await requestToken({
            url: '/Api/xxx/xxx/getLogList', // TODO: 修改 API 地址
            data: {
                offset: tableData.offset,
                rows: tableData.rows,
                data: { xxxInfoID: props.xxxInfoID }
            }
        })
        tableData.tableDataList = res.data.list
        tableData.tableDataSize = res.data.allSize
    } catch (error) {
        // error
    }
}

/**
 * 查询日志列表（重置分页到第一页）
 */
const handleSearch = () => {
    tableData.offset = 0
    tableData.currentPage = 1
    getAllDataTable()
}

/**
 * 切换每页条数
 */
const handleSizeChange = val => {
    tableData.currentPage = 1
    tableData.rows = val
    tableData.offset = 0
    getAllDataTable()
}

/**
 * 切换页码
 */
const handleCurrentChange = val => {
    tableData.currentPage = val
    tableData.offset = (parseInt(val) - 1) * tableData.rows
    getAllDataTable()
}

// ==================== 8. 生命周期钩子 ====================
onMounted(() => handleSearch())
</script>
```

## Class 使用规范

| 元素 | Class | 说明 |
|:----|:-----|:----|
| 外层容器 | `data_table_container` | 列表页主容器 |
| 滚动区域 | `data_table_scroll_middle` | 内容可滚动区域 |
| 表格 | `indexData_DataTable_style` | el-table 的 class |
| 分页 | `indexData_DataTable_class_pagination` | el-pagination 的 class |

## 与 DataTable 的区别

| 特性 | DataLogTable | DataTable |
|:----|:------------|:----------|
| 用途 | 日志记录展示 | 业务数据管理 |
| 操作按钮 | 无 | 有（增删改查） |
| 搜索筛选 | 无 | 有 |
| 数据来源 | props 传入 ID | 独立查询 |
| 权限控制 | 无 | 有 |

## 使用场景

在 index.vue 中通过抽屉展示日志列表：

```vue
<!-- index.vue 中 -->
<el-drawer
    class="custom_drawer"
    :title="dialogData.title"
    v-model="dialogData.logVisible"
    :closeOnClickModal="false"
    size="75%"
    :destroy-on-close="true"
    :closeOnPressEscape="false"
>
    <indexDataLogTable :xxxInfoID="currentxxxInfoID" />
</el-drawer>
```
