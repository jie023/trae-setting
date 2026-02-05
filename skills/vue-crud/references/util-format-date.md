---
title: 日期格式化工具函数
impact: MEDIUM
impactDescription: 日期格式化不统一会导致显示不一致
type: utility
tags: [vue3, utils, date, format]
appliesTo: [DataTable, DataUpdate, DataShow]
---

# 日期格式化工具函数

**Impact: MEDIUM** - 使用标准日期格式化函数，确保日期显示统一。

## Task Checklist

- [ ] 从 `@web-npm/baseutils` 导入工具函数
- [ ] **必须使用标准日期格式（使用 `-` 分隔）**
- [ ] 根据场景选择合适的格式化函数

## 标准日期格式规范（默认格式）

> **重要**：项目中涉及时间显示的字段，统一使用 `-` 分隔的标准格式。

| 场景 | 格式 | 示例 |
|:----|:----|:----|
| 仅日期 | `yyyy-MM-dd` | `2024-01-19` |
| 日期时间（默认） | `yyyy-MM-dd HH:mm` | `2024-01-19 12:33` |
| 日期时间带秒 | `yyyy-MM-dd HH:mm:ss` | `2024-01-19 12:33:22` |

### 默认规则

- **未明确要求带秒**：使用 `yyyy-MM-dd HH:mm`
- **明确要求带秒**：使用 `yyyy-MM-dd HH:mm:ss`
- **仅显示日期**：使用 `yyyy-MM-dd`

## 导入方式

```javascript
import { formatDate, formatDateString, formatTime } from '@web-npm/baseutils'
```

## 函数列表

| 函数名 | 用途 | 示例 |
|:------|:----|:----|
| `formatDate(format, date)` | 格式化 Date 对象 | `formatDate('yyyy-MM-dd HH:mm:ss', new Date())` → `'2024-01-19 12:33:22'` |
| `formatDateString(format, str)` | 格式化日期字符串 | `formatDateString('yyyy-MM-dd', '2024-01-19 12:33:22')` → `'2024-01-19'` |
| `formatTime(str)` | 友好时间显示 | `formatTime('2024-01-19 12:30:00')` → `'刚刚'` / `'5分钟前'` / `'昨天 12:30'` |

**Incorrect:**
```javascript
// ❌ 错误：手动拼接日期
const date = new Date()
const dateStr = date.getFullYear() + '-' + (date.getMonth() + 1) + '-' + date.getDate()

// ❌ 错误：使用 moment 或其他库
import moment from 'moment'
moment().format('YYYY-MM-DD')

// ❌ 错误：直接显示后台返回的原始值，没有格式化
const dateStr = row.createTime
```

**Correct:**
```javascript
import { formatDate, formatDateString } from '@web-npm/baseutils'

// ✅ 正确：格式化当前时间
const now = formatDate('yyyy-MM-dd HH:mm', new Date())

// ✅ 正确：格式化后台时间为日期
const dateStr = formatDateString('yyyy-MM-dd', '2024-01-19 12:33:22')

// ✅ 正确：格式化后台时间为日期时间（默认不带秒）
const dateTimeStr = formatDateString('yyyy-MM-dd HH:mm', '2024-01-19 12:33:22')

// ✅ 正确：格式化后台时间为日期时间带秒
const dateTimeWithSec = formatDateString('yyyy-MM-dd HH:mm:ss', '2024-01-19 12:33:22')

// ✅ 正确：显示友好时间
const friendlyTime = formatTime(row.updateTime)
```

## 表格列中使用日期格式化

> **重要**：表格中显示日期/时间字段时，必须使用插槽 + formatDateString 进行格式转换。

**Incorrect:**
```vue
<!-- ❌ 错误：直接绑定 prop，没有格式化 -->
<el-table-column
    label="签发日期"
    prop="issueDate"
    :width="$tableItemWidthCalculation(6)"
    show-overflow-tooltip
></el-table-column>
```

**Correct:**
```vue
<!-- ✅ 正确：日期列（使用插槽 + 标准格式） -->
<el-table-column
    label="签发日期"
    :width="$tableItemWidthCalculation(6)"
    show-overflow-tooltip
>
    <template #default="scope">
        {{ formatDateString('yyyy-MM-dd', scope.row.issueDate) }}
    </template>
</el-table-column>

<!-- ✅ 正确：日期时间列（默认不带秒） -->
<el-table-column
    label="创建时间"
    :width="$tableItemWidthCalculation(9)"
    show-overflow-tooltip
>
    <template #default="scope">
        {{ formatDateString('yyyy-MM-dd HH:mm', scope.row.createTime) }}
    </template>
</el-table-column>

<!-- ✅ 正确：日期时间带秒 -->
<el-table-column
    label="更新时间"
    :width="$tableItemWidthCalculation(11)"
    show-overflow-tooltip
>
    <template #default="scope">
        {{ formatDateString('yyyy-MM-dd HH:mm:ss', scope.row.updateTime) }}
    </template>
</el-table-column>
```

> **注意**：需要在 script 中导入 `import { formatDateString } from '@web-npm/baseutils'`
