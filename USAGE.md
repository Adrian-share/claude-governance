# 日常使用指南

快速参考：如何启动 Claude Code 和更新规则。

---

## 🚀 启动 Claude Code（自动加载规则）

### 方法 1：使用 claudex 脚本（推荐）

```bash
# 直接运行脚本
~/Documents/dev/cc\ rules/claude-governance/bin/claudex

# 或创建软链接（一次性设置）
sudo ln -s ~/Documents/dev/cc\ rules/claude-governance/bin/claudex /usr/local/bin/claudex

# 之后就可以在任意位置使用
claudex
```

### 方法 2：创建 Shell 别名

在 `~/.bashrc` 或 `~/.zshrc` 中添加：

```bash
# Claude Code 别名
alias claudex='~/Documents/dev/cc\ rules/claude-governance/bin/claudex'
```

然后重新加载配置：

```bash
source ~/.bashrc  # 或 source ~/.zshrc
```

之后在任意位置使用：

```bash
claudex  # 自动加载规则启动 Claude Code
```

### 方法 3：手动加载（临时使用）

```bash
claude --append-system-prompt "$(cat ~/Documents/dev/cc\ rules/claude-governance/rules/global-rules.md)"
```

---

## 📝 更新规则并提交到 Git

### 快速流程（使用脚本）

```bash
# 1. 编辑规则文件
vim ~/Documents/dev/cc\ rules/claude-governance/rules/global-rules.md

# 或编辑其他文件
vim ~/Documents/dev/cc\ rules/claude-governance/skills/code-review.md

# 2. 运行更新脚本
~/Documents/dev/cc\ rules/claude-governance/bin/update-rules

# 脚本会引导你：
# - 选择提交类型（feat/fix/docs等）
# - 输入变更范围（rules/skills等）
# - 输入变更描述
# - 确认并推送到 GitHub
```

### 手动流程

如果你更喜欢手动操作：

```bash
# 1. 进入仓库目录
cd ~/Documents/dev/cc\ rules/claude-governance

# 2. 查看变更
git status

# 3. 添加文件
git add rules/global-rules.md
# 或添加所有变更
git add .

# 4. 提交（使用规范的 commit message）
git commit -m "feat(rules): add new security rule"

# Commit message 格式：
# <type>(<scope>): <description>
#
# type: feat, fix, docs, refactor, chore
# scope: rules, skills, mcp, settings
# description: 简短描述

# 5. 推送到 GitHub
git push origin main
```

---

## 📂 常见编辑场景

### 场景 1：添加新的全局规则

```bash
# 1. 编辑全局规则
vim ~/Documents/dev/cc\ rules/claude-governance/rules/global-rules.md

# 2. 添加你的规则，例如：
## 新规则类别
- 规则内容 1
- 规则内容 2

# 3. 保存退出，运行更新脚本
~/Documents/dev/cc\ rules/claude-governance/bin/update-rules

# 选择：
# 类型: 1 (feat)
# 范围: rules
# 描述: add new coding standards
```

### 场景 2：添加新技能

```bash
# 1. 创建新技能文件
vim ~/Documents/dev/cc\ rules/claude-governance/skills/performance.md

# 2. 编写技能内容
# Performance Optimization Skill
## 使用场景
[...]

## 提示词
[...]

# 3. 保存后运行更新脚本
~/Documents/dev/cc\ rules/claude-governance/bin/update-rules

# 选择：
# 类型: 1 (feat)
# 范围: skills
# 描述: add performance optimization skill
```

### 场景 3：修复规则中的错误

```bash
# 1. 编辑文件修复错误
vim ~/Documents/dev/cc\ rules/claude-governance/rules/global-rules.md

# 2. 运行更新脚本
~/Documents/dev/cc\ rules/claude-governance/bin/update-rules

# 选择：
# 类型: 2 (fix)
# 范围: rules
# 描述: fix typo in security section
```

### 场景 4：更新文档

```bash
# 1. 编辑文档
vim ~/Documents/dev/cc\ rules/claude-governance/README.md

# 2. 运行更新脚本
~/Documents/dev/cc\ rules/claude-governance/bin/update-rules

# 选择：
# 类型: 3 (docs)
# 范围: （留空）
# 描述: update installation instructions
```

---

## 🔄 同步其他机器的配置

如果你在多台机器上使用：

```bash
# 在新机器上克隆
git clone https://github.com/Adrian-share/claude-governance.git ~/Documents/dev/cc\ rules/claude-governance

# 配置别名或软链接
echo 'alias claudex="~/Documents/dev/cc\ rules/claude-governance/bin/claudex"' >> ~/.bashrc
source ~/.bashrc

# 在已有机器上拉取最新配置
cd ~/Documents/dev/cc\ rules/claude-governance
git pull origin main
```

---

## 🛠️ 工具脚本说明

### `bin/claudex`

**功能：** 启动 Claude Code 并自动加载全局规则

**参数：** 接受所有 `claude` 命令的参数

**示例：**
```bash
claudex                           # 启动 Claude Code
claudex --help                    # 查看帮助
claudex --model claude-opus-4     # 使用特定模型
```

### `bin/update-rules`

**功能：** 交互式提交规则变更到 Git

**特点：**
- 自动检测变更文件
- 引导式 commit message 生成
- 支持规范的提交格式
- 可选择是否推送到 GitHub

**使用：**
```bash
# 进入任意目录运行
~/Documents/dev/cc\ rules/claude-governance/bin/update-rules

# 或配置别名
alias update-rules='~/Documents/dev/cc\ rules/claude-governance/bin/update-rules'
```

---

## 📋 Commit Message 规范

遵循 Conventional Commits 规范：

```
<type>(<scope>): <subject>

<body>

<footer>
```

### 常用 Type

| Type | 说明 | 示例 |
|------|------|------|
| `feat` | 新增功能/规则 | `feat(rules): add API security rules` |
| `fix` | 修复问题 | `fix(skills): correct testing example` |
| `docs` | 文档更新 | `docs: update quick start guide` |
| `refactor` | 重构 | `refactor(rules): reorganize security section` |
| `chore` | 其他变更 | `chore: update gitignore` |

### 常用 Scope

- `rules` - 规则相关
- `skills` - 技能相关
- `mcp` - MCP 配置
- `settings` - 配置文件
- `docs` - 文档

---

## 🎯 最佳实践

### 启动 Claude Code

✅ **推荐：**
```bash
claudex  # 使用别名或脚本，自动加载规则
```

❌ **不推荐：**
```bash
claude  # 没有加载规则，每次需要手动配置
```

### 更新规则

✅ **推荐：**
```bash
# 修改后立即提交
vim ~/Documents/dev/cc\ rules/claude-governance/rules/global-rules.md
update-rules
```

❌ **不推荐：**
```bash
# 修改后忘记提交，导致配置不一致
vim rules/global-rules.md
# ... 忘记提交
```

### Commit Message

✅ **推荐：**
```bash
feat(rules): add TypeScript strict mode requirement
fix(skills): correct debugging example syntax
docs: update API configuration guide
```

❌ **不推荐：**
```bash
update
fix bug
修改规则
```

---

## 🆘 常见问题

### Q: claudex 命令找不到？

**A:** 检查以下几点：

```bash
# 1. 确认脚本存在
ls -la ~/Documents/dev/cc\ rules/claude-governance/bin/claudex

# 2. 确认有执行权限
chmod +x ~/Documents/dev/cc\ rules/claude-governance/bin/claudex

# 3. 检查别名是否设置
alias | grep claudex

# 4. 重新加载 shell 配置
source ~/.bashrc  # 或 source ~/.zshrc
```

### Q: 规则没有生效？

**A:** 确认使用了 `claudex` 而不是 `claude`

```bash
# 错误：直接使用 claude 不会加载规则
claude

# 正确：使用 claudex 自动加载规则
claudex
```

### Q: Git 推送失败？

**A:** 检查网络和认证：

```bash
# 1. 测试网络连接
ping github.com

# 2. 检查 remote 配置
cd ~/Documents/dev/cc\ rules/claude-governance
git remote -v

# 3. 使用 SSH（推荐）
git remote set-url origin git@github.com:Adrian-share/claude-governance.git

# 4. 测试推送
git push origin main
```

### Q: 如何撤销还未推送的提交？

**A:** 使用 git reset

```bash
# 撤销最后一次提交，保留修改
git reset --soft HEAD~1

# 撤销最后一次提交，丢弃修改
git reset --hard HEAD~1
```

### Q: 多台机器配置不一致？

**A:** 定期同步

```bash
# 在每台机器上
cd ~/Documents/dev/cc\ rules/claude-governance
git pull origin main
```

---

## 📚 相关文档

- [README.md](README.md) - 完整文档
- [QUICK_START.md](QUICK_START.md) - 快速开始
- [API_DOC.md](API_DOC.md) - API 配置
- [GitHub 仓库](https://github.com/Adrian-share/claude-governance)

---

## 🎉 快速命令参考卡

```bash
# 启动 Claude Code
claudex

# 更新规则
vim ~/Documents/dev/cc\ rules/claude-governance/rules/global-rules.md
update-rules

# 同步最新配置
cd ~/Documents/dev/cc\ rules/claude-governance
git pull origin main

# 查看提交历史
cd ~/Documents/dev/cc\ rules/claude-governance
git log --oneline
```

保存这个文档，随时查阅！
