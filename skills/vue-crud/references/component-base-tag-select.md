---
title: BaseTagSelect 标签选择器组件
impact: LOW
impactDescription: 标签选择器使用错误会导致数据绑定或交互异常
type: component
tags: [vue3, element-plus, selector, component]
appliesTo: [DataUpdate]
---

# BaseTagSelect 标签选择器组件

**Impact: LOW** - 标签选择器组件，展示已选标签。全局注册，无需导入。

## Task Checklist

- [ ] 使用 `v-model` 绑定选中的标签数组
- [ ] 配置正确的 `keyField` 和 `label` 字段名
- [ ] 绑定 `@click` 事件打开选择弹窗

## Props

| 属性名 | 类型 | 默认值 | 说明 |
|:------|:----|:------|:----|
| modelValue | Array | `[]` | v-model 绑定值，选中的标签数组 |
| disabled | Boolean | `false` | 是否禁用组件 |
| clearable | Boolean | `true` | 是否显示清空按钮 |
| placeholder | String | `'请选择'` | 无选中项时的占位文本 |
| keyField | String | `'id'` | 标签数据的唯一标识字段名 |
| label | String | `'name'` | 标签数据的显示文本字段名 |
| controlClear | Boolean | `false` | 是否由外部控制清空/删除操作 |

## Events

| 事件名 | 参数 | 说明 |
|:------|:----|:----|
| update:modelValue | `(newValue)` | 更新绑定值 |
| click | `(event)` | 点击容器时触发 |
| clear | - | 点击清空按钮时触发 |
| remove | `(index, item)` | 删除单个标签时触发 |
| change | `(newValue)` | 标签值发生改变时触发 |

## 数据结构

```javascript
{
  id: 1,           // 唯一标识（对应 keyField）
  name: '标签名',   // 显示文本（对应 label）
  clearable: true  // 可选，是否允许删除该标签，默认 true
}
```

**Incorrect:**
```vue
<!-- ❌ 错误：字段名不匹配 -->
<BaseTagSelect v-model="formData.users" />
<!-- 但 formData.users 的结构是 { userId, userName }，缺少 keyField 和 label 配置 -->
```

**Correct:**
```vue
<!-- ✅ 正确：基本用法 -->
<template>
    <BaseTagSelect v-model="formData.tagList" @click="handleOpenTagDialog" />
</template>

<!-- ✅ 正确：自定义字段名 -->
<template>
    <BaseTagSelect
        v-model="formData.userList"
        key-field="userId"
        label="userName"
        placeholder="请选择用户"
        @click="handleOpenUserDialog"
    />
</template>

<!-- ✅ 正确：外部控制删除逻辑 -->
<template>
    <BaseTagSelect
        v-model="formData.tags"
        :control-clear="true"
        @clear="handleClearConfirm"
        @remove="handleRemoveConfirm"
    />
</template>
```

## 设置不可删除的标签

```javascript
const tags = ref([
    { id: 1, name: '固定标签', clearable: false }, // 不可删除
    { id: 2, name: '普通标签' } // 可删除
])
```
