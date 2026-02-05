---
title: DataTable.vue 骨架模板
type: skeleton
output: DataTable.vue
description: 标准 CRUD 列表页骨架，包含搜索栏、表格、分页
---

# DataTable.vue 骨架模板

> **生成前必读**：[代码生成规则汇总](all-rules-summary.md)

复制以下代码作为 DataTable.vue 的基础框架，根据 TODO 注释填充业务逻辑。

## 相关规则

生成代码时需参考以下规则：
- 筛选区 → [table-filter-area.md](../references/table-filter-area.md)
- 权限控制 → [permission-table-header-button.md](../references/permission-table-header-button.md)
- 权限控制 → [permission-table-row-button.md](../references/permission-table-row-button.md)
- 表格列宽 → [table-column-width.md](../references/table-column-width.md)
- 操作列 → [table-operation-column.md](../references/table-operation-column.md)
- 下拉框 → [form-select.md](../references/form-select.md)
- 防 null → [form-reset-null-prevention.md](../references/form-reset-null-prevention.md)
- 分页参数 → [api-pagination-params.md](../references/api-pagination-params.md)
- 删除确认 → [action-delete-confirm.md](../references/action-delete-confirm.md)
- API 请求 → [api-request-pattern.md](../references/api-request-pattern.md)（禁止 .then）
- MessageBox → [dialog-message-box.md](../references/dialog-message-box.md)

## 骨架代码

```vue
<template>
    <div class="data_table_container">
        <div class="data_table_scroll_middle">

            <!-- 顶部操作区（如有新增按钮），参考 permission-table-header-button.md -->
            <!-- TODO: 根据权限码修改 judgeAuthority('xxx_add') -->
            <div v-if="judgeAuthority('xxx_add')">
                <el-button type="primary" @click="handleAdd" v-if="judgeAuthority('xxx_add')">
                    <el-icon><Plus/></el-icon>
                    <span>添加</span>
                </el-button>
            </div>

            <!-- 搜索栏，参考 table-filter-area.md -->
            <div class="table_select_div_style">
                <div class="table_select_div_style_left">
                    <div class="table_select_div_style_item">
                        <el-input
                            v-model="tableSelectData.keyword"
                            placeholder="关键字"
                            :prefix-icon="Search"
                            @change="handleSearch"
                            clearable
                        ></el-input>
                    </div>
                    <!-- TODO: 添加更多筛选项 -->
                </div>
                <div class="table_select_div_style_screen">
                    <el-button @click="handleReset()">重置</el-button>
                </div>
            </div>

            <!-- 表格 -->
            <!-- TODO: 添加表格列，参考 table-column-width.md -->
            <el-table :data="tableData.tableDataList" class="indexData_DataTable_style">
                <!-- 第一列：序号（使用 width） -->
                <el-table-column
                    label="序号"
                    type="index"
                    :width="$tableItemWidthCalculation(2)"
                    :index="tableData.offset + 1"
                    show-overflow-tooltip
                    fixed
                ></el-table-column>

                <!-- 第二列：名称（唯一使用 min-width 的列，禁止硬编码数值） -->
                <el-table-column
                    label="名称"
                    prop="name"
                    :min-width="$tableItemWidthCalculation(4)"
                    show-overflow-tooltip
                    fixed
                ></el-table-column>

                <!-- 普通列示例（使用 width） -->
                <el-table-column
                    label="状态"
                    prop="status"
                    :width="$tableItemWidthCalculation(4)"
                    show-overflow-tooltip
                ></el-table-column>

                <!-- 日期列示例（必须使用插槽 + formatDateString 格式化） -->
                <el-table-column
                    label="创建日期"
                    :width="$tableItemWidthCalculation(6)"
                    show-overflow-tooltip
                >
                    <template #default="scope">
                        {{ formatDateString('yyyy-MM-dd', scope.row.createDate) }}
                    </template>
                </el-table-column>

                <!-- 日期时间列示例（默认不带秒） -->
                <el-table-column
                    label="创建时间"
                    :width="$tableItemWidthCalculation(9)"
                    show-overflow-tooltip
                >
                    <template #default="scope">
                        {{ formatDateString('yyyy-MM-dd HH:mm', scope.row.createTime) }}
                    </template>
                </el-table-column>

                <!-- TODO: 添加更多业务列 -->

                <!-- 操作列示例（1-2 个按钮，width=100） -->
                <!-- 参考 table-operation-column.md、permission-table-row-button.md -->
                <!--
                <el-table-column label="操作" fixed="right" width="100">
                    <template #default="scope">
                        <template v-if="judgeAuthority('xxx_show')">
                            <el-button @click="handleShow(scope.row)" link type="primary" size="small">详情</el-button>
                        </template>
                        <template v-if="judgeAuthority('xxx_update')">
                            <el-button @click="handleEdit(scope.row)" link type="primary" size="small">编辑</el-button>
                        </template>
                    </template>
                </el-table-column>
                -->

                <!-- 操作列示例（3 个按钮，width=140） -->
                <el-table-column label="操作" fixed="right" width="140">
                    <template #default="scope">
                        <template v-if="judgeAuthority('xxx_show')">
                            <el-button @click="handleShow(scope.row)" link type="primary" size="small">详情</el-button>
                        </template>
                        <template v-if="judgeAuthority('xxx_update')">
                            <el-button @click="handleEdit(scope.row)" link type="primary" size="small">编辑</el-button>
                        </template>
                        <template v-if="judgeAuthority('xxx_delete')">
                            <el-button @click="handleDelete(scope.row)" link type="danger" size="small">删除</el-button>
                        </template>
                    </template>
                </el-table-column>

                <!-- 操作列示例（>3 个按钮，使用 dropdown） -->
                <!-- 超过 3 个按钮必须使用此结构 -->
                <!--
                <el-table-column label="操作" fixed="right" width="140">
                    <template #default="scope">
                        <template v-if="judgeAuthority('xxx_show')">
                            <el-button @click="handleShow(scope.row)" link type="primary" size="small">详情</el-button>
                        </template>
                        <template v-if="judgeAuthority('xxx_update')">
                            <el-button @click="handleEdit(scope.row)" link type="primary" size="small">编辑</el-button>
                        </template>
                        <el-dropdown size="small" class="table_operation_dropdown"
                            v-if="judgeAuthority('xxx_log') || judgeAuthority('xxx_delete')">
                            <el-button class="table_operation_more" link type="primary" :icon="MoreFilled"></el-button>
                            <template #dropdown>
                                <el-dropdown-menu>
                                    <template v-if="judgeAuthority('xxx_log')">
                                        <el-dropdown-item @click="handleLog(scope.row)" class="table_operation_dropdown_primary">
                                            日志
                                        </el-dropdown-item>
                                    </template>
                                    <template v-if="judgeAuthority('xxx_delete')">
                                        <el-dropdown-item @click="handleDelete(scope.row)" class="table_operation_dropdown_danger">
                                            删除
                                        </el-dropdown-item>
                                    </template>
                                </el-dropdown-menu>
                            </template>
                        </el-dropdown>
                    </template>
                </el-table-column>
                -->
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
import { reactive, ref, onMounted, defineEmits, defineExpose } from 'vue'
import { requestToken } from '@/util/adminRequest'
import { judgeAuthority } from '@/util/authority'
import { formatDateString } from '@web-npm/baseutils'
import { Search, Plus, MoreFilled } from '@element-plus/icons-vue'
import { ElMessageBox } from 'element-plus'

// ==================== 1. props 和 emits 定义 ====================
/**
 * 新增操作
 */
const emit = defineEmits(['add', 'edit', 'show'])

// ==================== 3. 响应式数据（ref、reactive） ====================

// 表格数据（分页参数必须为 offset 和 rows）
const tableData = reactive({
    offset: 0,
    rows: 10,
    currentPage: 1,
    tableDataList: [],
    tableDataSize: 0
})

// TODO: 添加筛选字段
const tableSelectData = reactive({
    keyword: ''
})

// ==================== 6. 方法函数定义 ====================

/**
 * 新增操作
 */
const emit = defineEmits(['add', 'edit', 'show'])

const handleAdd = () => {
    emit('add')
}

/**
 * 删除操作（必须使用 async/await + try-catch，禁止 .then）
 * 参考 action-delete-confirm.md
 */
const handleDelete = async row => {
    try {
        await ElMessageBox.confirm('删除后不可恢复，请谨慎操作', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
        })
        await requestToken({
            url: '/Api/xxx/xxx/delete', // TODO: 修改 API 地址
            data: { xxxID: row.xxxID }
        })
        getAllDataTable()
    } catch (error) {
        // error
    }
}

/**
 * 重置筛选条件并触发查询
 */
const handleReset = () => {
    tableSelectData.keyword = ''
    // TODO: 重置其他筛选字段为空字符串（禁止赋值 null）
    handleSearch()
}

/**
 * 查询列表（重置分页到第一页）
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

/**
 * 获取列表数据
 */
const getAllDataTable = async () => {
    try {
        const res = await requestToken({
            url: '/Api/xxx/xxx/xxx', // TODO: 修改 API 地址
            data: {
                offset: tableData.offset,
                rows: tableData.rows,
                data: {}
            }
        })
        tableData.tableDataList = res.data.list
        tableData.tableDataSize = res.data.allSize
    } catch (error) {
        // error
    }
}

/**
 * 刷新列表（提供给父组件调用）
 */
const restartShowTable = () => getAllDataTable()

defineExpose({ restartShowTable })

// ==================== 8. 生命周期钩子 ====================
onMounted(() => handleSearch())
</script>
```
