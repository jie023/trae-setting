---
title: 金额格式化工具函数
impact: LOW
impactDescription: 金额格式化不统一会导致显示不一致
type: utility
tags: [vue3, utils, money, format]
appliesTo: [DataTable, DataUpdate, DataShow]
---

# 金额格式化工具函数

**Impact: LOW** - 使用标准金额格式化函数，确保金额显示统一。

## Task Checklist

- [ ] 从 `@web-npm/baseutils` 导入工具函数
- [ ] 根据场景选择合适的格式化函数

## 导入方式

```javascript
import { addThousandsSeparator, formatAmount, formatMoneyToStr, formatNumberToChinese } from '@web-npm/baseutils'
```

## 函数列表

| 函数名 | 用途 | 示例 |
|:------|:----|:----|
| `addThousandsSeparator(num)` | 添加千分位 | `addThousandsSeparator(1234567)` → `'1,234,567'` |
| `formatAmount(num)` | 金额格式化（固定2位小数） | `formatAmount(1234.5)` → `'1,234.50'` |
| `formatMoneyToStr(num)` | 金额带单位 | `formatMoneyToStr(12345678)` → `'1234.56w'` |
| `formatMoneyToStr(num)` | 金额带单位（亿） | `formatMoneyToStr(123456789)` → `'1.23亿'` |
| `formatNumberToChinese(num)` | 数字转中文大写 | `formatNumberToChinese(1234.56)` → `'壹仟贰佰叁拾肆元伍角陆分'` |

**Incorrect:**
```javascript
// ❌ 错误：手动格式化金额
const amount = 1234567
const formatted = amount.toFixed(2).replace(/\B(?=(\d{3})+(?!\d))/g, ',')

// ❌ 错误：不处理小数位
const price = row.price + '元'
```

**Correct:**
```javascript
import { formatAmount, addThousandsSeparator, formatMoneyToStr } from '@web-npm/baseutils'

// ✅ 正确：表格中显示金额
const displayAmount = formatAmount(row.amount) // '1,234.50'

// ✅ 正确：大数字显示
const displayBigMoney = formatMoneyToStr(row.totalAmount) // '1234.56w' 或 '1.23亿'

// ✅ 正确：整数千分位
const displayCount = addThousandsSeparator(row.count) // '1,234,567'
```
