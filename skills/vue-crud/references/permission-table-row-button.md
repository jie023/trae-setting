---
title: 权限控制规范（表格行内按钮）
impact: HIGH
impactDescription: 权限控制错误会导致越权操作
type: best-practice
tags: [vue3, permission, button, table]
appliesTo: [DataTable]
---

# 权限控制规范（表格行内按钮）

**Impact: HIGH** - 表格操作列中的按钮必须使用 `v-if` + `judgeAuthority()` 进行权限控制。

## Task Checklist

- [ ] 每个按钮使用 `<template v-if>` 包裹
- [ ] 使用 `judgeAuthority('模块_操作')` 函数
- [ ] dropdown 的外层和内部选项都需要权限判断

**Incorrect:**
```vue
<!-- ❌ 错误：直接在按钮上加 v-if，不使用 template 包裹 -->
<el-table-column label="操作" fixed="right" width="140">
    <template #default="scope">
        <el-button v-if="judgeAuthority('xxx_show')" @click="handleShow(scope.row)" link type="primary">详情</el-button>
        <el-button v-if="judgeAuthority('xxx_update')" @click="handleEdit(scope.row)" link type="primary">编辑</el-button>
    </template>
</el-table-column>

<!-- ❌ 错误：使用 v-show -->
<el-button v-show="hasEditPermission" @click="handleEdit(scope.row)">编辑</el-button>
```

**Correct:**
```vue
<el-table-column label="操作" fixed="right" width="140">
    <template #default="scope">
        <!-- ✅ 正确：使用 template v-if 包裹每个按钮 -->
        <template v-if="judgeAuthority('xxx_show')">
            <el-button @click="handleShow(scope.row)" link type="primary" size="small">详情</el-button>
        </template>
        <template v-if="judgeAuthority('xxx_update')">
            <el-button @click="handleEdit(scope.row)" link type="primary" size="small">编辑</el-button>
        </template>

        <!-- ✅ 正确：dropdown 外层和内部都有权限判断 -->
        <el-dropdown size="small" class="table_operation_dropdown" v-if="judgeAuthority('xxx_delete')">
            <el-button class="table_operation_more" link type="primary" :icon="MoreFilled"></el-button>
            <template #dropdown>
                <el-dropdown-menu>
                    <template v-if="judgeAuthority('xxx_delete')">
                        <el-dropdown-item
                            @click="handleDelete(scope.row)"
                            class="table_operation_dropdown_danger"
                        >
                            删除
                        </el-dropdown-item>
                    </template>
                </el-dropdown-menu>
            </template>
        </el-dropdown>
    </template>
</el-table-column>
```

## 多个 dropdown 选项的权限控制

```vue
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
```
