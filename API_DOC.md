# Claude Code API 文档

关于 Claude Code 使用的 API 配置说明。

## 🌐 支持的 API 提供商

### 1. Anthropic 官方 API

**适用场景：** 国际用户，需要官方支持

**配置：**
```bash
export ANTHROPIC_AUTH_TOKEN="sk-ant-xxxxx"
# 不需要设置 BASE_URL，使用默认
```

**获取 Token：**
1. 访问 [Anthropic Console](https://console.anthropic.com/)
2. 创建 API Key
3. 复制 token（格式：`sk-ant-`开头）

**定价：**
- Claude Sonnet 4.5: $3/MTok (input), $15/MTok (output)
- 查看最新定价：https://www.anthropic.com/pricing

---

### 2. 火山引擎方舟

**适用场景：** 国内用户，需要稳定访问

**配置：**
```bash
export ANTHROPIC_BASE_URL="https://ark.cn-beijing.volces.com/api/v3"
export ANTHROPIC_AUTH_TOKEN="your-token"
export ANTHROPIC_MODEL="doubao-seed-code-preview-latest"
```

**获取 Token：**
1. 访问 [火山引擎控制台](https://console.volcengine.com/ark)
2. 创建模型接入点
3. 获取 API Key

**可用模型：**
- `doubao-seed-code-preview-latest` - 代码专用模型
- `doubao-pro-32k` - 通用模型
- 其他模型查看控制台

---

### 3. 其他兼容服务

Claude Code 兼容任何遵循 Anthropic API 格式的服务。

**配置：**
```bash
export ANTHROPIC_BASE_URL="https://your-api-endpoint.com/v1"
export ANTHROPIC_AUTH_TOKEN="your-token"
export ANTHROPIC_MODEL="your-model-name"
```

**常见兼容服务：**
- Azure OpenAI Service (通过适配器)
- 企业内部部署的 API 网关
- 第三方代理服务

---

## 🔧 环境变量说明

### 必需变量

#### `ANTHROPIC_AUTH_TOKEN`
- **说明：** API 认证 Token
- **格式：** 字符串
- **示例：** `sk-ant-xxxxx` 或自定义格式
- **安全：** 不要提交到 Git，不要分享给他人

#### `ANTHROPIC_BASE_URL`（使用非官方 API 时必需）
- **说明：** API 服务器地址
- **格式：** URL
- **示例：** `https://ark.cn-beijing.volces.com/api/v3`
- **注意：** 官方 API 不需要设置

### 可选变量

#### `ANTHROPIC_MODEL`
- **说明：** 指定使用的模型
- **默认值：** `claude-sonnet-4-5-20250929`（官方 API）
- **示例：** `doubao-seed-code-preview-latest`
- **用途：** 使用特定模型或代码专用模型

#### `ANTHROPIC_API_VERSION`
- **说明：** API 版本
- **默认值：** `2023-06-01`
- **用途：** 兼容性控制

---

## 📡 API 请求示例

### 基本请求结构

```bash
curl https://api.anthropic.com/v1/messages \
  -H "content-type: application/json" \
  -H "x-api-key: $ANTHROPIC_AUTH_TOKEN" \
  -H "anthropic-version: 2023-06-01" \
  -d '{
    "model": "claude-sonnet-4-5-20250929",
    "max_tokens": 1024,
    "messages": [
      {"role": "user", "content": "Hello, Claude"}
    ]
  }'
```

### 使用自定义 BASE_URL

```bash
curl $ANTHROPIC_BASE_URL/messages \
  -H "content-type: application/json" \
  -H "x-api-key: $ANTHROPIC_AUTH_TOKEN" \
  -d '{
    "model": "doubao-seed-code-preview-latest",
    "max_tokens": 1024,
    "messages": [
      {"role": "user", "content": "Hello, Claude"}
    ]
  }'
```

---

## 🔍 测试 API 连接

### 快速测试

```bash
# 测试官方 API
curl https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_AUTH_TOKEN" \
  -H "anthropic-version: 2023-06-01" \
  -H "content-type: application/json" \
  -d '{"model":"claude-sonnet-4-5-20250929","max_tokens":10,"messages":[{"role":"user","content":"Hi"}]}'

# 测试自定义 API
curl $ANTHROPIC_BASE_URL/messages \
  -H "x-api-key: $ANTHROPIC_AUTH_TOKEN" \
  -H "content-type: application/json" \
  -d '{"model":"'$ANTHROPIC_MODEL'","max_tokens":10,"messages":[{"role":"user","content":"Hi"}]}'
```

### 验证配置脚本

创建 `test-api.sh`：

```bash
#!/bin/bash

echo "测试 Claude API 连接..."
echo ""

# 检查环境变量
if [ -z "$ANTHROPIC_AUTH_TOKEN" ]; then
    echo "❌ ANTHROPIC_AUTH_TOKEN 未设置"
    exit 1
fi

echo "✅ ANTHROPIC_AUTH_TOKEN: ${ANTHROPIC_AUTH_TOKEN:0:10}..."

if [ -n "$ANTHROPIC_BASE_URL" ]; then
    echo "✅ ANTHROPIC_BASE_URL: $ANTHROPIC_BASE_URL"
    API_URL="$ANTHROPIC_BASE_URL/messages"
else
    echo "ℹ️  使用官方 API"
    API_URL="https://api.anthropic.com/v1/messages"
fi

if [ -n "$ANTHROPIC_MODEL" ]; then
    echo "✅ ANTHROPIC_MODEL: $ANTHROPIC_MODEL"
    MODEL="$ANTHROPIC_MODEL"
else
    MODEL="claude-sonnet-4-5-20250929"
fi

echo ""
echo "发送测试请求..."

response=$(curl -s -w "\n%{http_code}" $API_URL \
  -H "x-api-key: $ANTHROPIC_AUTH_TOKEN" \
  -H "anthropic-version: 2023-06-01" \
  -H "content-type: application/json" \
  -d "{\"model\":\"$MODEL\",\"max_tokens\":20,\"messages\":[{\"role\":\"user\",\"content\":\"Say hello\"}]}")

http_code=$(echo "$response" | tail -n 1)
body=$(echo "$response" | head -n -1)

if [ "$http_code" == "200" ]; then
    echo "✅ API 连接成功！"
    echo ""
    echo "响应："
    echo "$body" | jq '.'
else
    echo "❌ API 连接失败"
    echo "HTTP Code: $http_code"
    echo "Response: $body"
fi
```

运行测试：
```bash
chmod +x test-api.sh
./test-api.sh
```

---

## 🛡️ 安全最佳实践

### Token 管理

#### ✅ 推荐做法

1. **使用环境变量**
   ```bash
   # 在 ~/.claude-env 中配置
   export ANTHROPIC_AUTH_TOKEN="sk-ant-xxxxx"

   # 不提交到 Git
   echo ".claude-env" >> ~/.gitignore
   ```

2. **使用密钥管理工具**
   ```bash
   # macOS Keychain
   security add-generic-password -a $USER -s claude-api -w "sk-ant-xxxxx"

   # 读取
   export ANTHROPIC_AUTH_TOKEN=$(security find-generic-password -a $USER -s claude-api -w)
   ```

3. **定期轮换 Token**
   - 每 3-6 个月更新一次
   - 离职人员立即撤销

#### ❌ 危险做法

- 硬编码在代码中
- 提交到 Git 仓库
- 在公共论坛分享
- 使用不安全的通信传输

### 权限控制

1. **最小权限原则**
   - 只授予必要的 API 权限
   - 使用专门的 Token（不复用）

2. **环境隔离**
   ```bash
   # 开发环境
   export ANTHROPIC_AUTH_TOKEN="sk-ant-dev-xxxxx"

   # 生产环境
   export ANTHROPIC_AUTH_TOKEN="sk-ant-prod-xxxxx"
   ```

3. **监控使用**
   - 定期检查 API 使用量
   - 设置使用限额告警
   - 审计异常调用

---

## 📊 使用限制和配额

### 官方 API 限制

| 限制类型 | Claude Sonnet 4.5 | 说明 |
|---------|------------------|------|
| 请求频率 | 50 请求/分钟 | 标准账户 |
| Token 限制 | 200K tokens | 上下文窗口 |
| 并发请求 | 5 个 | 同时进行的请求数 |

### 火山引擎方舟限制

根据你的套餐不同，限制会有所不同。查看控制台了解详情。

### 优化建议

1. **批量处理**
   ```bash
   # 合并多个小请求为一个大请求
   ```

2. **缓存响应**
   ```bash
   # 对相同请求使用缓存
   ```

3. **异步处理**
   ```bash
   # 使用队列处理非紧急请求
   ```

---

## 🔧 故障排查

### 常见错误

#### 401 Unauthorized
```
错误：Authentication failed
原因：Token 无效或过期
解决：检查 ANTHROPIC_AUTH_TOKEN 是否正确
```

#### 429 Too Many Requests
```
错误：Rate limit exceeded
原因：超过请求频率限制
解决：减慢请求速度，实现重试逻辑
```

#### 500 Internal Server Error
```
错误：Server error
原因：API 服务器问题
解决：稍后重试，检查服务状态页面
```

#### Connection timeout
```
错误：Request timeout
原因：网络问题或服务不可达
解决：检查网络，确认 BASE_URL 正确
```

### 调试技巧

1. **启用详细日志**
   ```bash
   export CLAUDE_DEBUG=true
   claudex
   ```

2. **使用 curl 测试**
   ```bash
   curl -v $ANTHROPIC_BASE_URL/messages \
     -H "x-api-key: $ANTHROPIC_AUTH_TOKEN" \
     ...
   ```

3. **检查环境变量**
   ```bash
   env | grep ANTHROPIC
   ```

---

## 📚 参考资源

### 官方文档

- [Anthropic API 文档](https://docs.anthropic.com/claude/reference/getting-started-with-the-api)
- [Claude Code 官方文档](https://claude.com/claude-code)
- [火山引擎方舟文档](https://www.volcengine.com/docs/82379)

### 社区资源

- [GitHub: awesome-claude-code](https://github.com/anthropics/claude-code)
- [Claude API Cookbook](https://github.com/anthropics/anthropic-cookbook)

### 支持渠道

- Anthropic 官方支持：support@anthropic.com
- 火山引擎工单系统
- GitHub Issues

---

## 🔄 API 版本历史

### v2023-06-01（当前）
- 支持 Messages API
- 支持 vision（图像理解）
- 支持 streaming

### 升级指南

```bash
# 检查当前版本
echo $ANTHROPIC_API_VERSION

# 升级到最新版本
export ANTHROPIC_API_VERSION="2023-06-01"
```

---

**最后更新：** 2026-01-07
**文档版本：** 1.0.0
