# MCP (Model Context Protocol) 配置

MCP 允许 Claude 访问外部工具和数据源。

## 什么是 MCP？

MCP 是一个开放协议，让 Claude 可以：
- 访问本地文件系统
- 连接数据库
- 调用 API
- 使用外部工具

## 常用 MCP Servers

### 1. 文件系统访问
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/allowed/directory"]
    }
  }
}
```

### 2. Git 操作
```json
{
  "mcpServers": {
    "git": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-git"]
    }
  }
}
```

### 3. SQLite 数据库
```json
{
  "mcpServers": {
    "sqlite": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-sqlite", "/path/to/database.db"]
    }
  }
}
```

### 4. GitHub
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "your-github-token"
      }
    }
  }
}
```

## 配置位置

MCP 配置文件位置：

**macOS:**
```
~/Library/Application Support/Claude/claude_desktop_config.json
```

**Windows:**
```
%APPDATA%\Claude\claude_desktop_config.json
```

**Linux:**
```
~/.config/Claude/claude_desktop_config.json
```

## 示例完整配置

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/Users/yourname/projects"
      ]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "ghp_xxxxxxxxxxxx"
      }
    }
  }
}
```

## 安全注意事项

- 只授权必要的目录访问
- 不要在配置文件中硬编码敏感 token
- 使用环境变量管理凭证
- 定期审查 MCP 权限

## 更多资源

- [MCP 官方文档](https://modelcontextprotocol.io/)
- [MCP Servers 列表](https://github.com/modelcontextprotocol/servers)
