# 项目级配置模板

在项目根目录创建 `.claude/` 文件夹，放置项目特定配置。

## 目录结构

```
your-project/
├── .claude/
│   ├── rules.md          # 项目特定规则
│   └── context.md        # 项目上下文信息
└── ... (其他项目文件)
```

## rules.md 示例

```markdown
# 项目规则

## 项目信息
- 名称：用户服务
- 技术栈：Node.js + TypeScript + PostgreSQL

## 架构规范
- 使用三层架构：Controller → Service → Repository
- 禁止跨层调用

## 代码风格
- 文件名：kebab-case
- 类名：PascalCase
- 函数：camelCase

## 测试要求
- Services 覆盖率 ≥ 80%
- 新功能必须有测试
```

## context.md 示例

```markdown
# 项目上下文

## 项目背景
[项目的用途和目标]

## 目录结构
```
src/
├── controllers/    # 路由处理
├── services/       # 业务逻辑
├── repositories/   # 数据访问
└── models/         # 数据模型
```

## 常用命令
```bash
npm run dev         # 开发服务器
npm test            # 运行测试
npm run build       # 构建
```

## 注意事项
- 数据库迁移需要先在开发环境测试
- API 变更需要更新 OpenAPI 文档
```
