---
name: "test-ui"
description: "使用Playwright进行UI测试，检查页面布局、样式和交互体验。Invoke when user needs to test user interface, layout, or visual elements."
---

# UI测试技能

使用Playwright工具进行UI测试，检查页面布局、样式和交互体验。

## 适用场景

- 检查UI布局和样式
- 测试页面响应式设计
- 测试交互体验和动画效果
- 测试表单验证和错误提示
- 测试可访问性
- 测试多浏览器兼容性

## 测试内容

### 布局测试
- 验证页面布局是否符合设计稿
- 测试元素位置和间距
- 测试响应式布局（不同屏幕尺寸）
- 测试滚动和分页

### 样式测试
- 验证字体、颜色、大小
- 测试按钮、链接、表单样式
- 测试图标和图片显示
- 测试主题切换（如有）

### 交互测试
- 测试按钮点击和悬停效果
- 测试表单输入和验证
- 测试弹窗和模态框
- 测试下拉菜单和导航
- 测试动画和过渡效果

### 可访问性测试
- 测试键盘导航
- 测试屏幕阅读器支持
- 测试对比度和可读性
- 测试焦点管理和标签

## 使用时机

- UI开发完成后
- 设计稿实现后
- UI调整后
- 多浏览器兼容性测试

## 执行方式

直接使用 Playwright MCP 工具进行自动化测试，无需额外脚本：

### 基本测试流程
1. **导航到测试页面**：使用 `mcp_Playwright_playwright_navigate`
2. **检查页面元素和布局**：使用 `mcp_Playwright_playwright_get_visible_html`、`mcp_Playwright_playwright_screenshot`
3. **测试交互操作**：使用 `mcp_Playwright_playwright_click`、`mcp_Playwright_playwright_fill`、`mcp_Playwright_playwright_hover`
4. **截图记录UI状态**：使用 `mcp_Playwright_playwright_screenshot`
5. **测试不同屏幕尺寸**：使用 `mcp_Playwright_playwright_resize`
6. **生成测试报告**：根据测试结果手动生成 Markdown 格式报告

### 常用 Playwright MCP 工具

**导航工具**
- `mcp_Playwright_playwright_navigate` - 导航到指定 URL
- `mcp_Playwright_playwright_go_back` - 返回上一页
- `mcp_Playwright_playwright_go_forward` - 前进到下一页

**交互工具**
- `mcp_Playwright_playwright_click` - 点击元素
- `mcp_Playwright_playwright_fill` - 填充输入框
- `mcp_Playwright_playwright_select` - 选择下拉框
- `mcp_Playwright_playwright_hover` - 悬停元素（测试悬停效果）
- `mcp_Playwright_playwright_press_key` - 按键（测试键盘导航）

**验证工具**
- `mcp_Playwright_playwright_get_visible_text` - 获取页面可见文本
- `mcp_Playwright_playwright_get_visible_html` - 获取页面 HTML（检查样式和布局）
- `mcp_Playwright_playwright_screenshot` - 截图（对比UI效果）

**视口调整工具**
- `mcp_Playwright_playwright_resize` - 调整浏览器窗口大小（测试响应式布局）
- `mcp_Playwright_playwright_resize` - 使用设备预设（如 iPhone、iPad、Android 等）

**HTTP 请求工具**
- `mcp_Playwright_playwright_get` - GET 请求
- `mcp_Playwright_playwright_post` - POST 请求
- `mcp_Playwright_playwright_put` - PUT 请求
- `mcp_Playwright_playwright_delete` - DELETE 请求

**其他工具**
- `mcp_Playwright_playwright_evaluate` - 执行 JavaScript（检查样式、计算元素位置等）
- `mcp_Playwright_playwright_console_logs` - 获取控制台日志（检查UI错误）
- `mcp_Playwright_playwright_custom_user_agent` - 设置自定义 User Agent（测试不同浏览器）

### UI 测试示例

**布局测试**
```
1. 使用 mcp_Playwright_playwright_navigate 导航到页面
2. 使用 mcp_Playwright_playwright_screenshot 截取完整页面
3. 使用 mcp_Playwright_playwright_evaluate 检查元素位置和尺寸
4. 对比设计稿验证布局
```

**样式测试**
```
1. 使用 mcp_Playwright_playwright_navigate 导航到页面
2. 使用 mcp_Playwright_playwright_evaluate 检查字体、颜色等样式
3. 使用 mcp_Playwright_playwright_screenshot 截取关键元素
4. 对比设计规范验证样式
```

**交互测试**
```
1. 使用 mcp_Playwright_playwright_navigate 导航到页面
2. 使用 mcp_Playwright_playwright_hover 悬停元素，截图记录悬停效果
3. 使用 mcp_Playwright_playwright_click 点击元素，验证交互效果
4. 使用 mcp_Playwright_playwright_screenshot 截取交互后的状态
```

**响应式测试**
```
1. 使用 mcp_Playwright_playwright_navigate 导航到页面
2. 使用 mcp_Playwright_playwright_resize 设置不同屏幕尺寸（桌面端、笔记本、平板、手机）
3. 使用 mcp_Playwright_playwright_screenshot 截取每个尺寸的布局
4. 验证响应式布局是否正常
```

**可访问性测试**
```
1. 使用 mcp_Playwright_playwright_navigate 导航到页面
2. 使用 mcp_Playwright_playwright_press_key 模拟 Tab 键导航
3. 使用 mcp_Playwright_playwright_evaluate 检查焦点顺序和 ARIA 标签
4. 验证可访问性标准

## 测试报告路径

`doc/{业务名称}/测试报告/UI测试/{测试页面名称}_{时间戳}.md`

## 测试报告内容

### 测试概要
- 测试范围
- 测试环境
- 浏览器信息
- 测试时间
- 测试人员

### UI测试项
- 测试项编号
- 测试标题
- 测试类型（布局/样式/交互/可访问性）
- 测试步骤
- 预期结果
- 实际结果
- 测试状态（通过/失败）
- 截图对比（如有）

### 响应式测试
- 屏幕尺寸
- 布局状态
- 截图
- 测试结果

### 测试结果统计
- 测试项总数
- 通过数量
- 失败数量
- 通过率

### 问题列表
- 问题编号
- 问题描述
- 问题类型
- 严重程度
- 复现步骤
- 截图

## 注意事项

1. 确保设计稿和UI规范文档齐全
2. 测试不同浏览器和屏幕尺寸
3. 注意加载状态和动画效果
4. 测试表单验证和错误提示
5. 检查可访问性标准
6. 截图保存关键UI状态
7. 对比设计稿和实际效果
