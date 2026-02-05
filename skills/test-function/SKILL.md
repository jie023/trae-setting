---
name: "test-function"
description: "使用Playwright进行功能测试，验证功能是否符合需求规格。Invoke when user needs to test functionality or business logic."
---

# 功能测试技能

使用Playwright工具进行功能测试，验证功能是否符合需求规格。

## 适用场景

- 验证功能是否符合需求规格
- 测试业务逻辑的正确性
- 测试表单提交、数据验证等功能
- 测试页面跳转、流程控制等功能
- 测试正常场景和异常场景

## 测试内容

### 正向测试
- 验证正常条件下的功能行为
- 测试核心业务流程
- 测试数据输入和输出

### 负向测试
- 验证无效输入的处理
- 测试异常情况的处理
- 测试边界条件的处理

### 边界测试
- 测试最小和最大输入值
- 测试空值和null值
- 测试特殊字符和格式

## 使用时机

- 功能开发完成后
- 需求变更后
- Bug修复后
- 回归测试

## 执行方式

直接使用 Playwright MCP 工具进行自动化测试，无需额外脚本：

### 基本测试流程
1. **导航到测试页面**：使用 `mcp_Playwright_playwright_navigate`
2. **执行测试操作**：使用 `mcp_Playwright_playwright_click`、`mcp_Playwright_playwright_fill` 等操作工具
3. **验证页面元素和响应**：使用 `mcp_Playwright_playwright_get_visible_text`、`mcp_Playwright_playwright_get_visible_html`
4. **截图记录测试结果**：使用 `mcp_Playwright_playwright_screenshot`
5. **生成测试报告**：根据测试结果手动生成 Markdown 格式报告

### 常用 Playwright MCP 工具

**导航工具**
- `mcp_Playwright_playwright_navigate` - 导航到指定 URL
- `mcp_Playwright_playwright_go_back` - 返回上一页
- `mcp_Playwright_playwright_go_forward` - 前进到下一页

**交互工具**
- `mcp_Playwright_playwright_click` - 点击元素
- `mcp_Playwright_playwright_fill` - 填充输入框
- `mcp_Playwright_playwright_select` - 选择下拉框
- `mcp_Playwright_playwright_hover` - 悬停元素
- `mcp_Playwright_playwright_press_key` - 按键

**验证工具**
- `mcp_Playwright_playwright_get_visible_text` - 获取页面可见文本
- `mcp_Playwright_playwright_get_visible_html` - 获取页面 HTML
- `mcp_Playwright_playwright_screenshot` - 截图

**HTTP 请求工具**
- `mcp_Playwright_playwright_get` - GET 请求
- `mcp_Playwright_playwright_post` - POST 请求
- `mcp_Playwright_playwright_put` - PUT 请求
- `mcp_Playwright_playwright_delete` - DELETE 请求

**其他工具**
- `mcp_Playwright_playwright_evaluate` - 执行 JavaScript
- `mcp_Playwright_playwright_console_logs` - 获取控制台日志
- `mcp_Playwright_playwright_resize` - 调整浏览器窗口大小

## 测试报告路径

`doc/{业务名称}/测试报告/功能测试/{测试功能名称}_{时间戳}.md`

## 测试报告内容

### 测试概要
- 测试范围
- 测试环境
- 测试时间
- 测试人员

### 测试用例
- 用例编号
- 测试标题
- 测试步骤
- 预期结果
- 实际结果
- 测试状态（通过/失败）
- 截图（如有）

### 测试结果统计
- 用例总数
- 通过数量
- 失败数量
- 通过率

### 问题列表
- 问题编号
- 问题描述
- 严重程度
- 复现步骤
- 截图

## 注意事项

1. 确保测试环境配置正确
2. 准备测试数据
3. 测试前清理缓存和Cookie
4. 测试后恢复初始状态
5. 记录详细的测试日志
6. 截图保存关键步骤
