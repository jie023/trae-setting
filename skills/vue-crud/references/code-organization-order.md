---
title: Vue 组件代码组织顺序
impact: HIGH
impactDescription: 代码顺序混乱会导致维护困难，降低代码可读性
type: best-practice
tags: [vue3, code-style, organization]
appliesTo: [DataTable, DataUpdate, DataShow, index]
---

# Vue 组件代码组织顺序

**Impact: HIGH** - 代码必须严格按照以下区块顺序组织，**禁止打乱**。

## Task Checklist

- [ ] 手动引入 `defineProps` 和 `defineEmits`
- [ ] 按照规定顺序组织代码区块
- [ ] 响应式数据按优先级排列：页面状态 → 弹窗/抽屉 → 列表数据 → 表单数据

## 代码区块顺序

1. **props 和 emits 定义**
2. **常量配置**
3. **响应式数据（ref、reactive）**
   - 页面状态 (loading等) → 弹窗/抽屉 → 列表数据 → 表单数据
4. **表单验证规则** (自定义验证函数必须在 `rules` 对象之前)
5. **计算属性 (computed)**
6. **方法函数定义**
7. **watch 监听器**
8. **生命周期钩子**

**Incorrect:**
```javascript
// ❌ 错误：顺序混乱
import { ref, onMounted } from 'vue'

onMounted(() => {}) // ❌ 生命周期放在了前面

const tableData = ref([])

const emits = defineEmits(['add']) // ❌ emits 应该在最前面

watch(() => {}) // ❌ watch 放错位置
```

**Correct:**
```javascript
// ✅ 正确：严格按照区块顺序
import { reactive, ref, onMounted, defineEmits } from 'vue'

// ==================== 1. props 和 emits 定义 ====================
const emits = defineEmits(['add', 'edit', 'show'])

// ==================== 2. 常量配置 ====================
const STATUS_MAP = { 0: '禁用', 1: '启用' }

// ==================== 3. 响应式数据 ====================
const loading = ref(false)
const tableData = ref([])
const formData = reactive({})

// ==================== 4. 表单验证规则 ====================
const rules = {}

// ==================== 5. 计算属性 ====================
const computedValue = computed(() => {})

// ==================== 6. 方法函数定义 ====================
const handleSubmit = () => {}

// ==================== 7. watch 监听器 ====================
watch(() => {}, () => {})

// ==================== 8. 生命周期钩子 ====================
onMounted(() => {})
```
