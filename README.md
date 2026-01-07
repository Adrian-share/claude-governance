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

### 一键配置（推荐）

```bash
# 1. 运行配置脚本
./bin/setup

# 2. 重新加载 shell 配置
source ~/.bashrc  # 或 source ~/.zshrc

# 3. 开始使用
claudex  # 自动加载规则启动 Claude Code
```

### 手动配置

如果喜欢手动配置：

```bash
# 1. 配置环境变量
cp settings/env.template ~/.claude-env
vim ~/.claude-env  # 填入你的 API token

# 2. 加载环境变量
echo 'source ~/.claude-env' >> ~/.bashrc
source ~/.bashrc

# 3. 创建别名
echo 'alias claudex="~/Documents/dev/cc\ rules/claude-governance/bin/claudex"' >> ~/.bashrc
echo 'alias update-rules="~/Documents/dev/cc\ rules/claude-governance/bin/update-rules"' >> ~/.bashrc
source ~/.bashrc
```

## 📖 日常使用

### 启动 Claude Code

```bash
claudex  # 自动加载全局规则
```

### 更新规则

```bash
# 1. 编辑规则文件
vim rules/global-rules.md

# 2. 提交变更（交互式）
update-rules
```

详细使用说明请查看 [USAGE.md](USAGE.md)

## 🛠️ 工具脚本

- **`bin/claudex`** - 启动 Claude Code（自动加载规则）
- **`bin/update-rules`** - 交互式提交规则变更
- **`bin/setup`** - 一键配置别名

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

- **[USAGE.md](USAGE.md)** - 日常使用指南（启动 Claude Code、更新规则）
- **[QUICK_START.md](QUICK_START.md)** - 5 分钟快速配置
- **[API_DOC.md](API_DOC.md)** - API 配置详解
- `rules/` - 查看所有规则
- `settings/` - 查看配置模板
- `skills/` - 查看可用技能
- `mcp/` - MCP 配置说明

## 🤝 贡献

欢迎提交 PR 改进规则和技能库。

## 📝 许可

MIT
