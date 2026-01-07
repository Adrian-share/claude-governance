# 快速开始指南

5 分钟快速配置 Claude Code。

## 步骤 1: 配置环境变量（2 分钟）

```bash
# 复制模板
cp settings/env.template ~/.claude-env

# 编辑文件，填入你的配置
vim ~/.claude-env

# 根据你的 API 选择配置方式：

# 如果使用火山引擎方舟：
export ANTHROPIC_BASE_URL="https://ark.cn-beijing.volces.com/api/v3"
export ANTHROPIC_AUTH_TOKEN="你的token"
export ANTHROPIC_MODEL="doubao-seed-code-preview-latest"

# 如果使用 Anthropic 官方 API：
export ANTHROPIC_AUTH_TOKEN="sk-ant-xxxxx"

# 保存退出后，加载配置
echo 'source ~/.claude-env' >> ~/.bashrc  # 或 ~/.zshrc
source ~/.bashrc
```

## 步骤 2: 创建快捷命令（1 分钟）

```bash
# 在 ~/.bashrc 或 ~/.zshrc 中添加别名
echo "alias claudex='claude --append-system-prompt \"\$(cat $PWD/rules/global-rules.md)\"'" >> ~/.bashrc
source ~/.bashrc

# 现在可以使用 claudex 启动 Claude Code
claudex
```

## 步骤 3: 验证配置（1 分钟）

```bash
# 启动 Claude Code
claudex

# 在对话中测试
> 你好，请确认你已经加载了全局规则

# Claude 应该会确认已加载规则并遵守规范
```

## 步骤 4: 在项目中使用（1 分钟）

```bash
# 进入你的项目
cd ~/projects/my-project

# 创建项目配置
mkdir .claude

# 创建项目规则（可选）
cat > .claude/rules.md <<'EOF'
# 项目规则

## 项目信息
- 名称：我的项目
- 技术栈：Node.js + TypeScript

## 特定规范
- 使用 ESLint
- 测试覆盖率 > 80%
EOF

# 使用 Claude Code
claudex
```

## 常用技能

### 代码审查
```
请审查我刚才修改的代码，检查：
1. 代码质量
2. 代码风格
3. 最佳实践
```

### 编写测试
```
请为 [功能名] 编写单元测试，要求：
- 覆盖正常和异常情况
- 使用现有测试框架
```

### 调试问题
```
遇到问题需要调试：
问题：[描述问题]
错误信息：[粘贴错误]
请帮我分析原因并提供修复方案。
```

## 高级配置（可选）

### 配置 MCP

参考 `mcp/README.md` 配置 MCP servers，让 Claude 可以：
- 访问文件系统
- 操作 Git
- 连接数据库
- 调用 GitHub API

### 自定义规则

编辑 `rules/global-rules.md` 添加你的团队规范。

### 添加技能

在 `skills/` 目录创建新的技能文件。

## 故障排查

### 问题：claudex 命令找不到

解决：检查别名是否正确设置
```bash
# 查看别名
alias | grep claudex

# 如果没有，重新添加
echo "alias claudex='claude --append-system-prompt \"\$(cat ~/claude-governance/rules/global-rules.md)\"'" >> ~/.bashrc
source ~/.bashrc
```

### 问题：认证失败

解决：检查环境变量
```bash
# 查看配置
echo $ANTHROPIC_AUTH_TOKEN
echo $ANTHROPIC_BASE_URL

# 如果为空，重新加载
source ~/.claude-env
```

### 问题：规则没有生效

解决：确认使用了 claudex 而不是 claude
```bash
# 错误
claude

# 正确
claudex
```

## 下一步

- 浏览 `skills/` 目录了解更多技能
- 查看 `mcp/README.md` 配置高级功能
- 阅读 `README.md` 了解完整功能

🎉 开始使用 Claude Code 吧！
