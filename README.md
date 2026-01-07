# Claude Code 治理配置

Claude Code 使用规范和配置管理。

## 📁 目录结构

```
claude-governance/
├── rules/              # 规则库
│   └── global-rules.md
├── settings/           # 配置文件
│   ├── env.template
│   └── project-template.md
├── skills/             # 技能库
│   ├── code-review.md
│   ├── testing.md
│   └── debugging.md
└── mcp/                # MCP 配置
    ├── README.md
    └── config-example.json
```

## 🚀 快速开始

### 1. 配置环境变量

```bash
# 复制模板
cp settings/env.template ~/.claude-env

# 编辑并填入你的 token
vim ~/.claude-env

# 在 ~/.bashrc 或 ~/.zshrc 中添加
echo 'source ~/.claude-env' >> ~/.bashrc
source ~/.bashrc
```

### 2. 在项目中使用

在你的项目根目录创建 `.claude/` 文件夹：

```bash
cd your-project
mkdir .claude
```

参考 `settings/project-template.md` 创建项目配置。

### 3. 配置 MCP（可选）

参考 `mcp/README.md` 配置 MCP servers。

## 📖 使用指南

### 加载全局规则

启动 Claude Code 时加载全局规则：

```bash
claude --append-system-prompt "$(cat /path/to/claude-governance/rules/global-rules.md)"
```

或创建别名：

```bash
# 在 ~/.bashrc 或 ~/.zshrc 中添加
alias claudex='claude --append-system-prompt "$(cat ~/claude-governance/rules/global-rules.md)"'
```

### 使用技能

查看 `skills/` 目录下的技能文件，复制提示词在对话中使用。

例如代码审查：
```
请审查我刚才修改的代码，检查代码质量、风格和最佳实践。
```

## 🔧 自定义

### 添加自己的规则

编辑 `rules/global-rules.md` 或创建新的规则文件。

### 创建项目配置

在项目目录：

```bash
mkdir .claude
vim .claude/rules.md        # 项目特定规则
vim .claude/context.md      # 项目上下文
```

### 添加新技能

在 `skills/` 目录创建新的 markdown 文件。

## 📚 文档

- `rules/` - 查看所有规则
- `settings/` - 查看配置模板
- `skills/` - 查看可用技能
- `mcp/` - MCP 配置说明

## 🤝 贡献

欢迎提交 PR 改进规则和技能库。

## 📝 许可

MIT
