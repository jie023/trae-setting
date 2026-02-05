---
name: "api-doc-template"
description: "Provides API documentation template and standards for RESTful API, gRPC, and other interfaces. Includes request/response format, parameter definitions, error codes, and Markdown documentation structure. Invoke when generating API documentation or interface specifications."
---

# API文档模版规范

本技能提供标准化的API接口文档模版，适用于RESTful API、gRPC等主流接口规范。确保接口文档结构清晰、内容完整、术语准确。

## 接口文档规范

### 标准模版格式

```markdown
##### 简要描述

- 获取全部摄像头报警

##### 请求URL
- `/Api/V1_0/AppAdmin/CameraReportPolice/GetAllCameraReportPoliceTable`

##### 请求方式
- POST

##### 参数

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
|token |是  |string |企业用户token   |

##### 请求示例

```json
{
    "iStart": 0,
    "iLength": 10,
    "data": {}
}
```

##### 返回示例

```json
{
    "code": 0,
    "message": "",
    "data": {
        "allSize": 1,
        "pageSize": 1,
        "list": [
            {
                "cameraReportPoliceEventType": ""
            }
        ]
    }
}
```

##### 返回参数说明

|参数名|类型|说明|
|:-----  |:-----|-----                           |
|code |int   |返回码  |
|message |String   |返回码描述  |
|data |   |返回数据  |
|allSize |int   |全部数据数量  |
```

## 文档规范要求

### 1. 简要描述
- 简洁明了地说明接口功能
- 使用动词开头，如"获取"、"保存"、"删除"等
- 避免使用过于技术化的术语

### 2. 请求URL
- 完整的接口路径
- 使用代码块格式包裹
- 包含版本号（如V1_0）

### 3. 请求方式
- 明确HTTP方法：GET、POST、PUT、DELETE
- 使用列表格式
- 与实际接口实现保持一致

### 4. 参数说明表格
|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |

- **参数名**：参数的名称，使用小驼峰命名
- **必选**：是/否，标识参数是否必须
- **类型**：string、int、boolean、object、array等
- **说明**：参数的业务含义和取值范围

### 5. 请求示例
- 使用JSON代码块格式
- 提供完整的请求示例
- 包含所有必选参数
- 可选参数可以省略或提供默认值

### 6. 返回示例
- 使用JSON代码块格式
- 提供完整的返回示例
- 包含成功和失败的示例（如有必要）
- 数据结构清晰，便于理解

### 7. 返回参数说明表格
|参数名|类型|说明|
|:-----  |:-----|-----                           |

- **参数名**：返回参数的名称
- **类型**：参数的数据类型
- **说明**：参数的业务含义

## 常用返回码规范

|返回码|说明|
|:-----  |:-----                           |
|0 |成功  |
|-1 |系统异常  |
|-2 |参数错误  |
|-3 |未登录  |
|-4 |无权限  |
|-5 |数据不存在  |

## 特殊场景处理

### 分页接口
- 请求参数包含：iStart（起始位置）、iLength（每页数量）
- 返回数据包含：allSize（总数量）、pageSize（当前页数量）、list（数据列表）

### 列表查询接口
- 请求参数包含：data对象，包含查询条件
- 返回数据包含：allSize（总数量）、list（数据列表）

### 文件上传接口
- 请求方式：POST
- Content-Type：multipart/form-data
- 请求示例需要说明文件字段名

### 文件下载接口
- 请求方式：GET
- 返回类型：文件流
- 需要说明文件名和格式

## 文档命名规范

- 接口文档文件名使用中文
- 格式：功能名称.md
- 示例：获取全部摄像头报警.md

## 文件存储规范

- 接口文档存储在：`doc/接口文档/` 目录下
- 按业务模块分类存放
- 文件名与接口功能对应

## 注意事项

1. **参数验证**：所有参数必须说明是否必选、数据类型、取值范围
2. **返回码**：必须说明所有可能的返回码及其含义
3. **示例完整**：请求和返回示例必须完整、真实可用
4. **术语统一**：文档中使用的术语必须与代码保持一致
5. **版本管理**：接口变更时需要更新文档版本号
6. **错误处理**：必须说明异常情况下的返回格式和错误信息
