---
title: BaseLocationShow 位置信息展示组件
impact: LOW
impactDescription: 位置展示组件使用错误会导致位置信息显示异常
type: component
tags: [vue3, element-plus, display, location, component]
appliesTo: [DataShow]
---

# BaseLocationShow 位置信息展示组件

**Impact: LOW** - 位置信息展示组件，支持脱敏显示。全局注册，无需导入。

## Task Checklist

- [ ] 传入正确的位置参数（title、content、latitude、longitude）
- [ ] 敏感场景启用 `sensitiveShow` 属性

## Props

| 参数 | 类型 | 默认值 | 说明 |
|:----|:----|:------|:----|
| title | String | `''` | 位置标题 |
| content | String | `''` | 位置地址（详细地址） |
| latitude | String / Number | `''` | 纬度 |
| longitude | String / Number | `''` | 经度 |
| sensitiveShow | Boolean | `false` | 是否启用敏感信息模式（启用后显示脱敏数据，并提供显示/隐藏切换按钮） |

## 显示逻辑

| 条件 | 显示结果 |
|:----|:--------|
| `title` 或 `content` 有值 | 正常展示位置信息 |
| `content` 有值但 `title` 为空 | 只展示地址 |
| `title` 和 `content` 都为空 | 显示 `-` |

**Correct:**
```vue
<!-- ✅ 正确：基础用法 -->
<BaseLocationShow
    title="公司地址"
    content="浙江省杭州市西湖区东湖路109号"
    latitude="30.2741"
    longitude="120.1551"
/>

<!-- ✅ 正确：敏感信息模式（地址自动脱敏） -->
<BaseLocationShow
    title="家庭住址"
    :content="detailData.homeAddress"
    :latitude="detailData.latitude"
    :longitude="detailData.longitude"
    :sensitiveShow="true"
/>

<!-- ✅ 正确：在表单项中使用 -->
<el-form-item label="办公地点">
    <div class="indexData_DataUpdate_class_detail_span">
        <BaseLocationShow
            :title="detailData.workLocationTitle"
            :content="detailData.workLocationAddress"
            :latitude="detailData.workLatitude"
            :longitude="detailData.workLongitude"
        />
    </div>
</el-form-item>
```
