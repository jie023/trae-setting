---
title: v-if 与 v-show 使用规范
impact: MEDIUM
impactDescription: 错误使用会导致性能问题或权限控制失效
type: best-practice
tags: [vue3, v-if, v-show, permission]
appliesTo: [DataTable, DataUpdate, DataShow, index]
---

# v-if 与 v-show 使用规范

**Impact: MEDIUM** - 正确选择 v-if 和 v-show，确保性能和安全性。

## Task Checklist

- [ ] 权限控制**必须使用 v-if**（v-show 仅隐藏元素，仍在 DOM 中）
- [ ] 频繁切换场景使用 v-show
- [ ] 初始渲染成本高的组件使用 v-if

## 对比说明

| 指令 | 特点 | 适用场景 |
|:----|:----|:--------|
| `v-show` | 仅切换 CSS display，组件保持挂载 | 频繁切换（如 Tab）、需保留组件状态 |
| `v-if` | 真正销毁/重建组件 | 初始渲染成本高、切换不频繁、需按需加载、**权限控制** |

## 权限控制必须用 v-if

**Incorrect:**
```vue
<!-- ❌ 错误：使用 v-show 控制权限，元素仍在 DOM 中 -->
<el-button v-show="hasPermission">删除</el-button>

<!-- ❌ 错误：敏感操作使用 v-show -->
<div v-show="isAdmin">
    <el-button @click="handleAdminAction">管理员操作</el-button>
</div>
```

**Correct:**
```vue
<!-- ✅ 正确：权限控制使用 v-if -->
<el-button v-if="judgeAuthority('xxx_delete')">删除</el-button>

<!-- ✅ 正确：敏感操作使用 v-if -->
<div v-if="judgeAuthority('xxx_admin')">
    <el-button @click="handleAdminAction">管理员操作</el-button>
</div>
```

## Tab 切换场景

```vue
<!-- 纯展示组件：使用 v-show（保留状态） -->
<div v-show="activeTab === 'info'">
    <InfoPanel :data="infoData" />
</div>

<!-- 有独立请求的组件：使用 v-if + 数据就绪判断 -->
<div v-if="activeTab === 'detail' && detailData">
    <DetailPanel :data="detailData" />
</div>
```

## 性能考虑

```vue
<!-- 初始渲染成本高、不频繁切换：使用 v-if -->
<HeavyChart v-if="showChart" :data="chartData" />

<!-- 频繁切换、渲染成本低：使用 v-show -->
<SimpleTooltip v-show="showTooltip" />
```
