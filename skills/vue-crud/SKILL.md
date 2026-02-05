---
name: vue-crud
description: 生成 Vue3 + Element Plus 标准 CRUD 模块代码。此技能应在用户主动要求使用 company-crud-maker，或需要规范化/重构现有 Vue 代码时触发。
---

# Company CRUD Maker

基于标准骨架模板和原子化规则，生成 Vue 3 + Element Plus CRUD 模块代码。

## 核心原则

> **需要什么功能，就按对应骨架模板生成对应文件。不将多个功能合并到一个文件中。**

- 每个文件严格按照对应骨架模板的结构、class、代码规范生成
- 使用模板定义的标准 class，不自创 class 名称
- 生成代码前阅读对应的规则文件

## 按需生成规则

> **重要**：根据用户指定的页面和按钮智能推断需要生成的内容。

### 页面推断规则

1. **显式指定优先**：用户在 `需要生成的页面` 中明确指定的页面必须生成
2. **按钮自动推断**：根据用户定义的按钮自动推断需要的页面

| 按钮类型 | 自动推断需要的页面 |
|---------|-----------------|
| 添加、新增 | DataUpdate |
| 编辑、修改 | DataUpdate |
| 详情、查看 | DataShow |
| 导入 | DataImport |

**示例**：

用户需求：
```
需要生成的页面: index, DataTable
顶部按钮: 添加（50%抽屉）
```

推断结果：
- ✅ 生成 index.vue、DataTable.vue、DataUpdate.vue
- ✅ index.vue 包含编辑抽屉和 handleAdd 函数
- ❌ 不生成 DataShow.vue（用户未指定，也没有"详情/查看"按钮）
- ❌ index.vue 不包含详情抽屉

### index.vue 动态裁剪规则

根据推断结果，index.vue 只包含需要的代码：

| 推断需要的页面 | index.vue 包含的代码 |
|--------------|-------------------|
| DataTable | 组件导入、ref、基本事件 |
| DataUpdate | 编辑抽屉、handleAdd、handleUpdate、updateVisible |
| DataShow | 详情抽屉、handleShow、showVisible |
| DataImport | 导入抽屉、handleImport、importVisible |

## 触发场景

- "帮我生成一个用户管理的 CRUD 模块"
- "按照公司规范重构这个 Vue 页面"
- "使用 company-crud-maker 创建订单列表页"

## 使用流程

1. **确定页面类型** → 选择对应的骨架模板
2. **阅读骨架模板** → 从 assets/ 获取基础框架
3. **阅读规范清单** → 确认代码符合所有规范
4. **生成代码** → 确保代码符合所有规则

## 骨架模板

| 页面类型 | 模板文件 | 输出文件 |
|---------|---------|---------|
| 模块入口 | [skeleton-index](assets/skeleton-index.md) | index.vue |
| 列表页 | [skeleton-data-table](assets/skeleton-data-table.md) | DataTable.vue |
| 表单页 | [skeleton-data-update](assets/skeleton-data-update.md) | DataUpdate.vue |
| 详情页 | [skeleton-data-show](assets/skeleton-data-show.md) | DataShow.vue |
| 导入页 | [skeleton-data-import](assets/skeleton-data-import.md) | DataImport.vue |
| 日志表格 | [skeleton-data-log-table](assets/skeleton-data-log-table.md) | DataLogTable.vue |

## 规范清单

> 生成代码前阅读相关规范。

### 必须遵守

| 规范 | Impact | 位置 |
|:----|:------|:-----|
| 不使用 .then() 链式调用 | HIGH | [api-request-pattern](references/api-request-pattern.md) |
| ElMessageBox 使用 async/await | HIGH | [dialog-message-box](references/dialog-message-box.md) |
| 不使用 style 属性 | HIGH | [css-class-standard](references/css-class-standard.md) |
| 列宽使用 $tableItemWidthCalculation(n) | HIGH | [table-column-width](references/table-column-width.md) |
| 不手动调用 ElMessage | HIGH | [action-success-no-message](references/action-success-no-message.md) |
| Vue 组件代码组织顺序 | HIGH | [code-organization-order](references/code-organization-order.md) |

### 代码规范

| 规范 | Impact | 位置 |
|:----|:------|:-----|
| 父子组件通信命名规范 | HIGH | [code-naming-convention](references/code-naming-convention.md) |
| 函数注释规范 | MEDIUM | [code-function-comment](references/code-function-comment.md) |

### 布局规范

| 规范 | Impact | 位置 |
|:----|:------|:-----|
| 详情页/编辑页小标题 | HIGH | [page-subfield-title](references/page-subfield-title.md) |

### API 相关

| 规范 | Impact | 位置 |
|:----|:------|:-----|
| 分页参数规范 (offset/rows) | HIGH | [api-pagination-params](references/api-pagination-params.md) |

### 表格相关

| 规范 | Impact | 位置 |
|:----|:------|:-----|
| 操作列按钮规范 (>3个用dropdown) | HIGH | [table-operation-column](references/table-operation-column.md) |
| 筛选区标准样式 | LOW | [table-filter-area](references/table-filter-area.md) |

### 表单相关

| 规范 | Impact | 位置 |
|:----|:------|:-----|
| el-select/el-date-picker 空值处理 | HIGH | [form-reset-null-prevention](references/form-reset-null-prevention.md) |
| 输入框属性规范 | MEDIUM | [form-input-text](references/form-input-text.md) |
| 选择器属性规范 | MEDIUM | [form-select](references/form-select.md) |

### 权限控制

| 规范 | Impact | 位置 |
|:----|:------|:-----|
| 表头按钮权限控制 | HIGH | [permission-table-header-button](references/permission-table-header-button.md) |
| 行内按钮权限控制 | HIGH | [permission-table-row-button](references/permission-table-row-button.md) |
| v-if 与 v-show 使用规范 | MEDIUM | [permission-v-if-not-v-show](references/permission-v-if-not-v-show.md) |

### 工具函数

| 规范 | Impact | 位置 |
|:----|:------|:-----|
| 日期格式化 (formatDateString) | HIGH | [util-format-date](references/util-format-date.md) |
| 敏感信息脱敏 | HIGH | [util-format-sensitive](references/util-format-sensitive.md) |
| 金额格式化 | LOW | [util-format-amount](references/util-format-amount.md) |

### 交互规范

| 规范 | Impact | 位置 |
|:----|:------|:-----|
| 删除操作确认弹窗 | HIGH | [action-delete-confirm](references/action-delete-confirm.md) |

### 组件规范

| 规范 | Impact | 位置 |
|:----|:------|:-----|
| BaseShowFileList 附件列表展示 | HIGH | [component-base-show-file-list](references/component-base-show-file-list.md) |
| BaseNoData 空状态占位 | HIGH | [component-base-no-data](references/component-base-no-data.md) |
| BaseUploadAttachment 附件上传 | HIGH | [component-base-upload-attachment](references/component-base-upload-attachment.md) |
| BaseShowImageList 图片列表展示 | HIGH | [component-base-show-image-list](references/component-base-show-image-list.md) |
| BaseUploadCropImage 图片裁剪上传 | HIGH | [component-base-upload-crop-image](references/component-base-upload-crop-image.md) |
| BaseTagSelect 标签选择器 | LOW | [component-base-tag-select](references/component-base-tag-select.md) |
| BaseLocationShow 位置信息展示 | LOW | [component-base-location-show](references/component-base-location-show.md) |
