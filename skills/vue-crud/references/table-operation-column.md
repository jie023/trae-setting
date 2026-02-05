---
title: 表格操作列规范
impact: MEDIUM
impactDescription: 操作列配置不当会影响用户体验和界面一致性
type: best-practice
tags: [vue3, element-plus, table, operation]
appliesTo: [DataTable]
---

# 表格操作列规范

**Impact: MEDIUM** - 根据按钮数量选择对应的宽度和布局方式。

> ⚠️ **CRITICAL WARNING**:
>
> ## 超过 3 个按钮必须使用 el-dropdown！
>
> **当操作列按钮超过 3 个时，必须将第 3 个及之后的按钮放入 `el-dropdown` 中！**
>
> 直接平铺超过 3 个按钮是不合规的。

## 按钮计数规则

> **重要**：按钮数量按**模板中定义的按钮总数**计算，**不考虑 v-if 条件**！
>
> 即使按钮有条件显示（v-if），只要模板中定义了超过 3 个按钮，就必须使用 dropdown。

**Incorrect:**
```vue
<!-- ❌❌❌ 错误：模板中定义了 5 个按钮，即使有 v-if 条件也必须使用 dropdown ❌❌❌ -->
<el-table-column label="操作" fixed="right" width="140">
    <template #default="scope">
        <el-button v-if="judgeAuthority('xxx_show')" @click="handleShow(scope.row)" link type="primary" size="small">详情</el-button>
        <el-button v-if="judgeAuthority('xxx_edit') && scope.row.source === 2" @click="handleEdit(scope.row)" link type="primary" size="small">编辑</el-button>
        <el-button v-if="judgeAuthority('xxx_revoke') && scope.row.source === 1" @click="handleRevoke(scope.row)" link type="warning" size="small">撤销</el-button>
        <el-button v-if="judgeAuthority('xxx_restore') && scope.row.status === 3" @click="handleRestore(scope.row)" link type="warning" size="small">恢复</el-button>
        <el-button v-if="judgeAuthority('xxx_delete')" @click="handleDelete(scope.row)" link type="danger" size="small">删除</el-button>
    </template>
</el-table-column>
```

## Task Checklist

- [ ] **按模板中定义的按钮总数计算（不考虑 v-if）**
- [ ] **超过 3 个按钮必须使用 el-dropdown**
- [ ] 操作列使用 `fixed="right"` 固定在右侧
- [ ] 根据按钮数量设置宽度
- [ ] 危险操作使用红色样式

## 宽度与布局规范

| 按钮数量 | width | 布局方式 |
|:--------|:------|:--------|
| 1-2 个 | `100` | 全部使用 `el-button` |
| 3 个 | `140` | 全部使用 `el-button` |
| >3 个 | `140` | 前 2 个 `el-button` + 第 3 个 `el-dropdown` |

## 按钮类型规范

| 操作类型 | el-button | el-dropdown-item |
|:--------|:----------|:-----------------|
| **危险操作**（撤销、删除、下架、下线） | `type="danger"` | `class="table_operation_dropdown_danger"` |
| **普通操作**（详情、编辑、恢复、日志） | `type="primary"` | `class="table_operation_dropdown_primary"` |

## 示例：1-2 个按钮（width=100）

```vue
<el-table-column label="操作" fixed="right" width="100">
    <template #default="scope">
        <el-button @click="handleShow(scope.row)" link type="primary" size="small">详情</el-button>
        <el-button @click="handleEdit(scope.row)" link type="primary" size="small">编辑</el-button>
    </template>
</el-table-column>
```

## 示例：3 个按钮（width=140）

```vue
<el-table-column label="操作" fixed="right" width="140">
    <template #default="scope">
        <el-button @click="handleShow(scope.row)" link type="primary" size="small">详情</el-button>
        <el-button @click="handleEdit(scope.row)" link type="primary" size="small">编辑</el-button>
        <el-button @click="handleDelete(scope.row)" link type="danger" size="small">删除</el-button>
    </template>
</el-table-column>
```

## 示例：超过 3 个按钮（width=140 + dropdown）

```vue
<el-table-column label="操作" fixed="right" width="140">
    <template #default="scope">
        <el-button @click="handleShow(scope.row)" link type="primary" size="small">详情</el-button>
        <el-button @click="handleEdit(scope.row)" link type="primary" size="small">编辑</el-button>
        <el-dropdown size="small" class="table_operation_dropdown">
            <el-button class="table_operation_more" link type="primary" :icon="MoreFilled"></el-button>
            <template #dropdown>
                <el-dropdown-menu>
                    <el-dropdown-item @click="handleLog(scope.row)" class="table_operation_dropdown_primary">
                        日志
                    </el-dropdown-item>
                    <el-dropdown-item @click="handleDelete(scope.row)" class="table_operation_dropdown_danger">
                        删除
                    </el-dropdown-item>
                </el-dropdown-menu>
            </template>
        </el-dropdown>
    </template>
</el-table-column>
```

> **注意**：`MoreFilled` 图标需要从 `@element-plus/icons-vue` 导入。
