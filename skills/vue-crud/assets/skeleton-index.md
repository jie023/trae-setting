---
title: index.vue 骨架模板
type: skeleton
output: index.vue
description: 模块入口文件骨架，用于编排列表、增删改查组件的显示
---

# index.vue 骨架模板

> **生成前必读**：[代码生成规则汇总](all-rules-summary.md)

复制以下代码作为 index.vue 的基础框架，根据 TODO 注释填充业务逻辑。

## 相关规则

生成代码时需参考以下规则：
- v-if/v-show → [permission-v-if-not-v-show.md](../references/permission-v-if-not-v-show.md)
- CSS 规范 → [css-class-standard.md](../references/css-class-standard.md)

## 抽屉 title 规范

> **重要**：所有抽屉的 title 统一使用 **模块名称**，禁止添加后缀！

| 场景 | 正确示例 | 错误示例 |
|:----|:--------|:--------|
| 新增 | `证书管理` | `新增证书`、`添加证书` |
| 编辑 | `证书管理` | `编辑证书`、`修改证书` |
| 详情 | `证书管理` | `证书详情`、`查看证书` |

**原因**：抽屉 title 用于标识当前操作的模块，而非操作类型。操作类型已通过按钮文字体现。

## 条件生成说明

骨架代码中使用 `[需要:PageName]` 标记条件部分：

- `[需要:DataUpdate]` - 仅当推断需要 DataUpdate 时生成
- `[需要:DataShow]` - 仅当推断需要 DataShow 时生成
- `[需要:DataImport]` - 仅当推断需要 DataImport 时生成

**生成时删除条件标记本身，只保留符合条件的代码。**

## 骨架代码

```vue
<template>
    <el-scrollbar>
        <el-card class="card_height">
            <template #header>
                <div class="card-header"><span>模块名称</span></div>  <!-- TODO: 修改模块名称 -->
            </template>
            <div class="card_height_content">
                <indexDataTable
                    @add="handleAdd"      <!-- [需要:DataUpdate] -->
                    @edit="handleUpdate"  <!-- [需要:DataUpdate] -->
                    @show="handleShow"    <!-- [需要:DataShow] -->
                    ref="indexDataTableRef"
                ></indexDataTable>
            </div>
        </el-card>

        <!-- [需要:DataUpdate] 编辑/新增抽屉 - 开始 -->
        <el-drawer
            class="custom_drawer"
            :title="dialogData.title"
            v-model="dialogData.updateVisible"
            :closeOnClickModal="false"
            size="50%"
            :destroy-on-close="true"
            :closeOnPressEscape="false"
        >
            <indexDataUpdate :xxxID="currentxxxID" @confirm="handleUpdateConfirm" @cancel="handleUpdateCancel"></indexDataUpdate>
        </el-drawer>
        <!-- [需要:DataUpdate] 编辑/新增抽屉 - 结束 -->

        <!-- [需要:DataShow] 详情抽屉 - 开始 -->
        <el-drawer
            class="custom_drawer"
            :title="dialogData.title"
            v-model="dialogData.showVisible"
            :closeOnClickModal="false"
            size="50%"
            :destroy-on-close="true"
            :closeOnPressEscape="false"
        >
            <indexDataShow :xxxID="currentxxxID"></indexDataShow>
        </el-drawer>
        <!-- [需要:DataShow] 详情抽屉 - 结束 -->
    </el-scrollbar>
</template>

<script setup>
import { reactive, ref, nextTick } from 'vue'
import indexDataTable from './Data/DataTable.vue'
import indexDataUpdate from './Data/DataUpdate.vue'  // [需要:DataUpdate]
import indexDataShow from './Data/DataShow.vue'      // [需要:DataShow]

// ==================== 3. 响应式数据（ref、reactive） ====================
// 页面状态
const indexDataTableRef = ref(null)

// 弹窗/抽屉相关
const currentxxxID = ref('')  // TODO: 修改 ID 变量名
const dialogData = reactive({
    title: '',
    updateVisible: false,  // [需要:DataUpdate]
    showVisible: false     // [需要:DataShow]
})

// ==================== 6. 方法函数定义 ====================

// [需要:DataUpdate] handleAdd - 开始
/**
 * 显示新增弹窗
 */
const handleAdd = () => {
    dialogData.title = '模块名称'  // TODO: 修改标题
    dialogData.updateVisible = true
    currentxxxID.value = ''
}
// [需要:DataUpdate] handleAdd - 结束

// [需要:DataUpdate] handleUpdate - 开始
/**
 * 显示编辑弹窗
 */
const handleUpdate = xxxID => {
    dialogData.title = '模块名称'  // TODO: 修改标题
    dialogData.updateVisible = true
    currentxxxID.value = xxxID
}
// [需要:DataUpdate] handleUpdate - 结束

// [需要:DataShow] handleShow - 开始
/**
 * 显示详情弹窗
 */
const handleShow = xxxID => {
    dialogData.title = '模块名称'  // TODO: 修改标题
    dialogData.showVisible = true
    currentxxxID.value = xxxID
}
// [需要:DataShow] handleShow - 结束

// [需要:DataUpdate] handleUpdateConfirm/Cancel - 开始
/**
 * 处理编辑/新增确认
 */
const handleUpdateConfirm = () => {
    dialogData.updateVisible = false
    nextTick(() => {
        indexDataTableRef.value.restartShowTable()
    })
}

/**
 * 处理编辑/新增取消
 */
const handleUpdateCancel = () => {
    dialogData.updateVisible = false
}
// [需要:DataUpdate] handleUpdateConfirm/Cancel - 结束
</script>
```

## Class 使用规范

| 元素 | Class | 说明 |
|:----|:-----|:----|
| `el-scrollbar` | 无 | 不添加任何 class |
| `el-card` | `card_height` | 卡片容器 |
| header 容器 | `card-header` | 必须用 `<template #header>` 包裹 |
| body 容器 | `card_height_content` | 卡片内容区 |

## el-drawer 属性规范

| 属性 | 值 | 说明 |
|:----|:--|:----|
| `class` | `custom_drawer` | 必须使用 |
| `:closeOnClickModal` | `false` | 禁止点击遮罩关闭 |
| `size` | `50%`（默认） | 默认 50%，复杂页面可用 75% |
| `:destroy-on-close` | `true` | 关闭时销毁子组件 |
| `:closeOnPressEscape` | `false` | 禁止 ESC 关闭 |
| `:append-to-body` | `true` | 嵌套抽屉时必须添加 |

## 目录结构

```
模块目录/
├── index.vue          # 主入口（本模板）
└── Data/
    ├── DataTable.vue  # 列表组件
    ├── DataUpdate.vue # 编辑组件
    └── DataShow.vue   # 详情组件
```
