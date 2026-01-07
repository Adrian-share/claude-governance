# Debugging Skill

帮助定位和解决 bug 的技能。

## 使用场景

- 遇到运行时错误
- 行为不符合预期
- 性能问题排查

## 提示词

```
遇到问题，需要调试：

问题描述：
[具体的错误信息或异常行为]

环境信息：
- 系统：
- 语言/框架版本：
- 相关依赖：

复现步骤：
1.
2.
3.

请帮我：
1. 分析可能的原因
2. 建议调试方法
3. 提供修复方案
```

## 调试技巧

### 1. 日志调试
```javascript
console.log('调试点 1:', variable);
console.log('调试点 2:', JSON.stringify(object, null, 2));
```

### 2. 断点调试
```javascript
debugger; // 浏览器会在此处暂停
```

### 3. 条件断点
```javascript
if (someCondition) {
  debugger;
}
```

### 4. 性能分析
```javascript
console.time('operation');
// ... 代码
console.timeEnd('operation');
```

## 常见问题排查

### TypeError / ReferenceError
- 检查变量是否已定义
- 检查类型是否正确
- 检查是否为 null/undefined

### Promise 相关
- 检查是否使用 await
- 检查错误处理（.catch）
- 检查 Promise 链

### 性能问题
- 检查循环复杂度
- 检查不必要的重复计算
- 检查数据库查询效率
