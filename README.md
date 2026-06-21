# Structured Clarification for AI Agents

**人工介入的结构化确认工作流** — 强制 AI agent 在执行前分步确认，减少幻觉和返工。

## 解决什么问题

AI coding agent 的常见问题：
- 理解偏了直接改，改完了才发现不对
- 一次输出太多，注意力分散导致遗漏
- 做出来的不是用户想要的

## 怎么解决

把任务拆成 **两次确认、三个阶段**，每步等用户放行：

```
读 → 📋 理解确认 → 📋 方案选择 → 执行 → 验证
         ↑               ↑
       用户确认          用户选择
```

### 为什么有效

1. **坍缩生成空间** — 枚举 2-4 个互斥选项，把开放问题压进离散子空间
2. **锚点效应** — 用户选中的选项作为高显著度 token，强约束后续生成
3. **卸载歧义** — 模型不再扛着多分支的不确定性，输出更聚焦

## 文件说明

| 文件 | 用途 |
|------|------|
| `task-flow-propose-first.md` | 修改用（Hermes 专用版，有 `clarify` 工具，支持点击选择） |
| `brainstorming.md` | 讨论用（新功能/新项目从零设计，需求澄清、方案探索） |
| `structured-clarification.md` | 通用版（任何 AI 助手都能用，markdown A/B/C 格式） |

两个 skill 是对应的：
- **修改已有东西** → task-flow-propose-first（读→理解→方案→执行→验证）
- **从零设计新东西** → brainstorming（探索需求→提出方案→写设计文档）

## 使用方式

### Hermes Agent
把 `task-flow-propose-first.md` 放到 `skills/` 目录下即可自动生效。

### Claude Code / Cursor / 其他
把 `structured-clarification.md` 的内容复制到项目的 `.cursorrules`、`CLAUDE.md` 或 system prompt 中。

## 适用场景

| 场景 | 效果 |
|------|------|
| 修 bug（复杂） | ✅ 理解阶段就能纠正错误假设 |
| UI 设计 | ✅ 选完再做，方向不会偏 |
| 多文件改动 | ✅ 影响范围写清楚再动手 |
| 简单改配置 | ⚠️ 多一轮对话，有例外跳过机制 |

## License

MIT
