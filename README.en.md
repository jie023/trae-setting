# Trae Related Configurations

#### Description
This repository contains agent configurations, skill templates, coding standards, and IDE settings for Trae IDE, designed to improve development efficiency and code quality.

#### Project Structure

```
track-related-configurations/
├── skills/                    # Skill templates library
│   ├── java-*/               # Java development skills (Controller, Service, DAO, DO, VO, etc.)
│   ├── p3c-*/                # Alibaba P3C coding standards (coding style, exception logging, OOP standards, security rules, etc.)
│   ├── test-*/               # Testing skills (API functionality, data quality, performance, security, UI, etc.)
│   ├── *-template/           # Document templates (API documentation, requirement docs, database design, etc.)
│   └── vue-crud/             # Vue3 CRUD generator
├── 智能体/                    # Agent role definitions
│   ├── 产品设计师.md         # Product Designer
│   ├── 前端架构师.md         # Frontend Architect
│   ├── 后端架构师.md         # Backend Architect
│   ├── 接口文档.md           # API Documentation
│   ├── 数据库设计师.md       # Database Designer
│   └── 测试工程师.md         # Test Engineer
├── 规则/                      # Personal rules configuration
│   └── 个人规则.md           # Personal Rules
└── 设置/                      # IDE and tool configurations
    ├── eclipse-formatter.xml # Eclipse code formatting configuration
    ├── keybindings.json      # VS Code keybindings configuration
    ├── settings.json         # VS Code settings
    └── mcp.json              # MCP configuration file
```

#### Installation

1. Clone this repository to your local machine
2. Copy skill files from the `skills/` directory to your Trae IDE skills directory
3. Import agent configurations from the `智能体/` directory into Trae IDE
4. Import configuration files from the `设置/` directory to your respective IDE (VS Code, Eclipse, etc.)

#### Usage

1. **Skill Usage**: Call corresponding skills in Trae IDE, such as `java-controller`, `p3c-coding-style`, etc.
2. **Agent Usage**: Select the appropriate agent role for development assistance
3. **Standards Checking**: Use P3C-related skills for code quality checking
4. **Test Generation**: Use test-* series skills to generate test cases

#### Contribution

1. Fork the repository
2. Create Feat_xxx branch
3. Commit your code
4. Create Pull Request

#### Features

- Complete Java development skill system
- Follows Alibaba P3C coding standards
- Comprehensive testing support (functionality, performance, security, UI)
- Multi-role agent collaboration
- Standardized code formatting configuration
