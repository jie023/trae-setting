---
title: 权限控制规范（表格顶部按钮）
impact: HIGH
impactDescription: 权限控制错误会导致越权操作
type: best-practice
tags: [vue3, permission, button, table]
appliesTo: [DataTable]
---

# 权限控制规范（表格顶部按钮）

**Impact: HIGH** - 表格顶部的操作按钮必须使用 `v-if` + `judgeAuthority()` 进行权限控制。

## Task Checklist

- [ ] 必须使用 `v-if` 而非 `v-show`
- [ ] 必须使用 `judgeAuthority('模块_操作')` 函数
- [ ] 外层容器和按钮都需要权限判断（双重判断）
- [ ] 权限码格式：`模块名_操作名`

**Incorrect:**
```vue
<!-- ❌ 错误：使用 v-show，元素仍在 DOM 中可被检查到 -->
<el-button v-show="hasPermission">新增</el-button>

<!-- ❌ 错误：自定义权限判断函数 -->
<el-button v-if="checkAuth('add')">新增</el-button>

<!-- ❌ 错误：硬编码权限值 -->
<el-button v-if="isAdmin">新增</el-button>

<!-- ❌ 错误：缺少外层权限判断 -->
<div>
    <el-button v-if="judgeAuthority('xxx_add')">新增</el-button>
</div>
```

**Correct:**
```vue
<!-- ✅ 正确：外层 + 按钮双重权限判断 -->
<div v-if="judgeAuthority('xxx_add')">
    <el-button type="primary" @click="handleAdd" v-if="judgeAuthority('xxx_add')">
        <el-icon><Plus/></el-icon>
        <span>添加</span>
    </el-button>
</div>

<!-- ✅ 正确：多个按钮的权限控制 -->
<div v-if="judgeAuthority('xxx_add') || judgeAuthority('xxx_export') || judgeAuthority('xxx_import')">
    <el-button type="primary" @click="handleAdd" v-if="judgeAuthority('xxx_add')">
        <el-icon><Plus/></el-icon>
        <span>添加</span>
    </el-button>
    <el-button @click="handleExport" v-if="judgeAuthority('xxx_export')">导出</el-button>
    <el-button @click="handleImport" v-if="judgeAuthority('xxx_import')">导入</el-button>
</div>
```

## 常见权限码命名

| 操作 | 权限码格式 | 示例 |
|-----|-----------|------|
| 新增 | `模块_add` | `course_add` |
| 编辑 | `模块_update` | `course_update` |
| 删除 | `模块_delete` | `course_delete` |
| 导出 | `模块_export` | `course_export` |
| 导入 | `模块_import` | `course_import` |
| 详情 | `模块_show` | `course_show` |

## 导入说明

```javascript
import { judgeAuthority } from '@/util/authority'
```
