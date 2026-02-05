---
title: 表格筛选区规范
impact: HIGH
impactDescription: 筛选区结构和 class 不规范会破坏 UI 一致性
type: best-practice
tags: [vue3, element-plus, table, filter, search]
appliesTo: [DataTable]
---

# 表格筛选区规范

**Impact: HIGH** - 筛选区必须使用标准 class 结构，禁止自创 class。

## Task Checklist

- [ ] 外层容器使用 `table_select_div_style`
- [ ] 左侧筛选区使用 `table_select_div_style_left`
- [ ] 每个筛选项使用 `table_select_div_style_item` 包裹
- [ ] 右侧按钮区使用 `table_select_div_style_screen`
- [ ] 下拉框必须处理 null 值（清空时赋空字符串）
- [ ] 下拉框 change 事件同时触发搜索

## 标准结构

```vue
<!-- 搜索栏 -->
<div class="table_select_div_style">
    <div class="table_select_div_style_left">
        <!-- 关键字输入框 -->
        <div class="table_select_div_style_item">
            <el-input
                v-model="tableSelectData.keyword"
                placeholder="关键字"
                :prefix-icon="Search"
                @change="handleSearch"
                clearable
            ></el-input>
        </div>
        <!-- 动态数据下拉框（需要 filterable） -->
        <div class="table_select_div_style_item">
            <el-select
                v-model="tableSelectData.typeID"
                placeholder="类型"
                @change="
                    val => {
                        tableSelectData.typeID = val ?? ''
                        handleSearch()
                    }
                "
                clearable
                filterable
            >
                <el-option
                    v-for="item in typeList"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value"
                ></el-option>
            </el-select>
        </div>
        <!-- 静态字典下拉框（不需要 filterable） -->
        <div class="table_select_div_style_item">
            <el-select
                v-model="tableSelectData.status"
                placeholder="状态"
                @change="
                    val => {
                        tableSelectData.status = val ?? ''
                        handleSearch()
                    }
                "
                clearable
            >
                <el-option label="全部" value="99"></el-option>
                <el-option label="启用" value="1"></el-option>
                <el-option label="禁用" value="0"></el-option>
            </el-select>
        </div>
    </div>
    <div class="table_select_div_style_screen">
        <el-button @click="handleReset()">重置</el-button>
    </div>
</div>
```

## Class 说明

| Class 名称 | 用途 |
|:----------|:----|
| `table_select_div_style` | 搜索栏外层容器，flex 布局 |
| `table_select_div_style_left` | 左侧筛选项容器 |
| `table_select_div_style_item` | 单个筛选项包装器 |
| `table_select_div_style_screen` | 右侧重置按钮区域 |

## 下拉框类型判断

| 数据类型 | filterable | 说明 |
|:--------|:----------|:----|
| 动态数据 | ✅ 必须 | 用户、部门、分类等后台接口数据 |
| 静态字典 | ❌ 不需要 | 状态、启用/禁用等固定选项 |

**Incorrect:**
```vue
<!-- ❌ 错误：自创 class -->
<div class="search-area">
    <div class="search-item">
        <el-input v-model="keyword" />
    </div>
</div>

<!-- ❌ 错误：没有处理 null 值 -->
<el-select v-model="status" clearable @change="handleSearch">
    <el-option label="启用" value="1" />
</el-select>

<!-- ❌ 错误：动态数据没加 filterable -->
<el-select v-model="typeID" clearable>
    <el-option v-for="item in typeList" :key="item.id" :label="item.name" :value="item.id" />
</el-select>
```

**Correct:**
```vue
<!-- ✅ 正确：使用标准 class + null 处理 + filterable -->
<div class="table_select_div_style">
    <div class="table_select_div_style_left">
        <div class="table_select_div_style_item">
            <el-select
                v-model="tableSelectData.typeID"
                placeholder="类型"
                @change="
                    val => {
                        tableSelectData.typeID = val ?? ''
                        handleSearch()
                    }
                "
                clearable
                filterable
            >
                <el-option v-for="item in typeList" :key="item.value" :label="item.label" :value="item.value" />
            </el-select>
        </div>
    </div>
    <div class="table_select_div_style_screen">
        <el-button @click="handleReset()">重置</el-button>
    </div>
</div>
```

## 导入说明

```javascript
import { Search } from '@element-plus/icons-vue'
```
