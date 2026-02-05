---
title: 父子组件通信命名规范
impact: HIGH
impactDescription: 命名不规范会导致代码难以理解和维护
type: best-practice
tags: [vue3, props, emits, naming]
appliesTo: [DataTable, DataUpdate, DataShow, index]
---

# 父子组件通信命名规范

**Impact: HIGH** - Prop 和 Emit 的命名必须语义化，便于理解和维护。

## Task Checklist

- [ ] Prop 使用业务ID命名（如 `courseID`），禁止使用通用的 `dataID`
- [ ] Emit 使用语义化动词命名
- [ ] 事件处理函数以 `handle` 开头

## Prop 命名规范

**Incorrect:**
```javascript
// ❌ 错误：使用通用命名
const props = defineProps({
    dataID: String,      // ❌ 太通用，不知道是什么 ID
    id: String,          // ❌ 太通用
    data: Object         // ❌ 太通用
})
```

**Correct:**
```javascript
// ✅ 正确：使用业务语义命名
const props = defineProps({
    courseID: String,    // ✅ 课程 ID
    userID: String,      // ✅ 用户 ID
    orderInfo: Object    // ✅ 订单信息
})
```

## Emit 命名规范

**Incorrect:**
```javascript
// ❌ 错误：不规范的 emit 命名
const emits = defineEmits(['childMethod', 'emitFunc', 'callback'])
```

**Correct:**
```javascript
// ✅ 正确：使用语义化动词
const emits = defineEmits(['add', 'edit', 'show', 'confirm', 'cancel', 'refresh'])
```

## 事件处理函数命名

`@click` 绑定的函数名必须以 `handle` 开头：

**Incorrect:**
```vue
<!-- ❌ 错误：函数名不规范 -->
<el-button @click="search">查询</el-button>
<el-button @click="delete">删除</el-button>
<el-button @click="clickSave">保存</el-button>
```

**Correct:**
```vue
<!-- ✅ 正确：以 handle 开头 -->
<el-button @click="handleSearch">查询</el-button>
<el-button @click="handleDelete">删除</el-button>
<el-button @click="handleSave">保存</el-button>
```
