# Trae 相关配置

#### 介绍
本仓库包含 Trae IDE 的智能体配置、技能模板、编码规范和IDE设置，旨在提升开发效率和代码质量。

#### 项目结构

```
track-related-configurations/
├── skills/                    # 技能模板库
│   ├── java-*/               # Java开发相关技能（Controller、Service、DAO、DO、VO等）
│   ├── p3c-*/                # 阿里巴巴P3C编码规范（代码风格、异常日志、OOP标准、安全规则等）
│   ├── test-*/               # 测试相关技能（API功能、数据质量、性能、安全、UI等）
│   ├── *-template/           # 文档模板（API文档、需求文档、数据库设计等）
│   └── vue-crud/             # Vue3 CRUD生成器
├── 智能体/                    # 智能体角色定义
│   ├── 产品设计师.md
│   ├── 前端架构师.md
│   ├── 后端架构师.md
│   ├── 接口文档.md
│   ├── 数据库设计师.md
│   └── 测试工程师.md
├── 规则/                      # 个人规则配置
│   └── 个人规则.md
└── 设置/                      # IDE和工具配置
    ├── eclipse-formatter.xml # Eclipse代码格式化配置
    ├── keybindings.json      # VS Code快捷键配置
    ├── settings.json         # VS Code设置
    └── mcp.json              # MCP配置文件
```

#### 安装教程

1. 克隆本仓库到本地
2. 将 `skills/` 目录下的技能文件复制到 Trae IDE 的技能目录
3. 将 `智能体/` 目录下的智能体配置导入到 Trae IDE
4. 将 `设置/` 目录下的配置文件导入到对应的IDE（VS Code、Eclipse等）

#### 使用说明

1. **技能使用**：在 Trae IDE 中调用相应技能，如 `java-controller`、`p3c-coding-style` 等
2. **智能体使用**：选择对应的智能体角色进行开发辅助
3. **规范检查**：使用 P3C 相关技能进行代码质量检查
4. **测试生成**：使用 test-* 系列技能生成测试用例

#### 参与贡献

1. Fork 本仓库
2. 新建 Feat_xxx 分支
3. 提交代码
4. 新建 Pull Request

#### 特性

- 完整的 Java 开发技能体系
- 遵循阿里巴巴 P3C 编码规范
- 全方位的测试支持（功能、性能、安全、UI）
- 多角色智能体协作
- 标准化的代码格式化配置
