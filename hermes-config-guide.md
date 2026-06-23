# Hermes Agent 配置笔记

## 核心原则

**什么时候必须改 `.env`？**
- 使用 **Anthropic (Claude)** 中转站 → 必须在 `.env` 里设 `ANTHROPIC_BASE_URL` + `ANTHROPIC_API_KEY`
- 使用 **OpenAI** 中转站 → 必须在 `.env` 里设 `OPENAI_BASE_URL` + `OPENAI_API_KEY`
- 原因：Hermes 对这两家有内置 provider，用环境变量才能正确识别

**什么时候可以只改 `config.yaml`？**
- 使用 **OpenAI 兼容** 的中转站（如 dashscope、nvidia、deepseek）→ 在 `providers` 里配置 `transport: chat_completions`
- 原因：这些走通用的 chat_completions 协议

---

## 一、添加 OpenAI 兼容供应商（config.yaml）

适用于：DashScope、NVIDIA、DeepSeek、通义千问、Kimi 等 OpenAI 兼容 API

### config.yaml 模板

```yaml
providers:
  # 供应商名称（自定义，小写英文）
  dashscope:
    api: https://ws-cklp60rmebtk02zc.cn-beijing.maas.aliyuncs.com/compatible-mode/v1
    api_key: sk-ws-xxxxx
    default_model: qwen3.7-plus
    models:
      qwen3.7-plus:
        name: Qwen 3.7 Plus
    name: dashscope
    transport: chat_completions

  nvidia:
    api: https://integrate.api.nvidia.com/v1
    api_key: nvapi-xxxxx
    default_model: moonshotai/kimi-k2.6
    models:
      moonshotai/kimi-k2.6:
        name: moonshotai/kimi-k2.6
    name: nvidia
    transport: chat_completions
```

### 字段说明

| 字段 | 必填 | 说明 |
|------|------|------|
| `api` | ✅ | API 端点地址（不含 `/v1/chat/completions`） |
| `api_key` | ✅ | API 密钥 |
| `default_model` | ✅ | 默认使用的模型名称 |
| `models` | ✅ | 模型列表，key 和 name 保持一致即可 |
| `name` | ✅ | 供应商名称，必须和顶级 key 一致 |
| `transport` | ✅ | 必须是 `chat_completions` |

### 切换到该供应商

```bash
hermes config set model.provider dashscope
hermes config set model.default qwen3.7-plus
```

### ⚠️ 注意
- 自定义 provider 最多 **3 个**，添加第 4 个会丢失配置
- 超过 3 个需要替换已有的，或者用环境变量方式

---

## 二、添加 Anthropic 中转站（.env + config.yaml）

适用于：Claude 系列模型的中转站

### `.env` 模板（路径：`D:\hermes\.env`）

```bash
# Anthropic 中转站
ANTHROPIC_BASE_URL=https://api.viptoken.top
ANTHROPIC_API_KEY=sk-c9b-xxxxx
```

### config.yaml 设置

```yaml
model:
  base_url: https://api.viptoken.top
  default: claude-opus-4-8
  provider: anthropic   # 必须是 anthropic，不能是自定义名称
```

### ⚠️ 注意
- 不能在 `providers` 里配置 `transport: anthropic`（v0.17.0 有 bug）
- `model.provider` 必须设为 `anthropic`，不能是自定义名称如 `kiro`
- 中转站必须支持 `/v1/messages` 端点（标准 Anthropic 格式）

---

## 三、添加 OpenAI 中转站（.env）

适用于：GPT-4、GPT-4o 等 OpenAI 模型的中转站

### `.env` 模板

```bash
# OpenAI 中转站
OPENAI_BASE_URL=https://your-proxy.com/v1
OPENAI_API_KEY=sk-xxxxx
```

### config.yaml 设置

```yaml
model:
  base_url: https://your-proxy.com/v1
  default: gpt-4o
  provider: openai
```

---

## 四、辅助模型配置（config.yaml）

辅助模型用于：图片识别、网页抓取、上下文压缩、技能生成等

### config.yaml 模板

```yaml
auxiliary:
  # 图片识别/视觉模型
  vision:
    provider: xiaomi-token-plan
    model: mimo-v2.5
    base_url: https://token-plan-cn.xiaomimimo.com/v1
    api_key: tp-xxxxx
    timeout: 120
    extra_body:
      enable_thinking: false
    download_timeout: 30

  # 网页抓取
  web_extract:
    provider: auto          # auto = 跟随主模型
    model: ''               # 空 = 使用默认模型
    base_url: ''
    api_key: ''
    timeout: 360
    extra_body: {}

  # 上下文压缩
  compression:
    provider: auto
    model: ''
    base_url: ''
    api_key: ''
    timeout: 120
    extra_body: {}

  # 技能生成
  skills_hub:
    provider: auto
    model: ''
    base_url: ''
    api_key: ''
    timeout: 30
    extra_body: {}

  # 审批确认
  approval:
    provider: auto
    model: ''
    base_url: ''
    api_key: ''
    timeout: 30
    extra_body: {}

  # MCP 工具调用
  mcp:
    provider: auto
    model: ''
    base_url: ''
    api_key: ''
    timeout: 30
    extra_body: {}

  # 会话标题生成
  title_generation:
    provider: auto
    model: ''
    base_url: ''
    api_key: ''
    timeout: 30
    extra_body: {}
    language: ''

  # 分类/标签
  triage_specifier:
    provider: auto
    model: ''
    base_url: ''
    api_key: ''
    timeout: 120
    extra_body: {}

  # Kanban 任务分解
  kanban_decomposer:
    provider: auto
    model: ''
    base_url: ''
    api_key: ''
    timeout: 180
    extra_body: {}

  # 用户画像生成
  profile_describer:
    provider: auto
    model: ''
    base_url: ''
    api_key: ''
    timeout: 60
    extra_body: {}

  # 技能库维护
  curator:
    provider: auto
    model: ''
    base_url: ''
    api_key: ''
    timeout: 600
    extra_body: {}

  # 监控
  monitor:
    provider: auto
    model: ''
    base_url: ''
    api_key: ''
    timeout: 60
    extra_body: {}
```

### 辅助模型配置说明

| 字段 | 说明 |
|------|------|
| `provider` | `auto` = 跟随主模型，或指定供应商名如 `xiaomi-token-plan` |
| `model` | 空字符串 = 使用默认模型，或指定具体模型名 |
| `base_url` | 空 = 跟随主模型，或指定独立端点 |
| `api_key` | 空 = 跟随主模型，或指定独立密钥 |
| `timeout` | 超时时间（秒） |
| `extra_body` | 额外请求参数，如 `enable_thinking: false` |

### 自定义辅助模型示例

```yaml
auxiliary:
  # 用便宜模型做图片识别
  vision:
    provider: dashscope
    model: qwen-vl-max
    base_url: https://dashscope.aliyuncs.com/compatible-mode/v1
    api_key: sk-xxxxx
    timeout: 120

  # 用快速模型做上下文压缩
  compression:
    provider: nvidia
    model: moonshotai/kimi-k2.6
    base_url: https://integrate.api.nvidia.com/v1
    api_key: nvapi-xxxxx
    timeout: 60
```

---

## 五、完整配置示例

### 场景：主力用 Claude，辅助用通义千问

**`.env`：**
```bash
# Claude 中转站
ANTHROPIC_BASE_URL=https://api.viptoken.top
ANTHROPIC_API_KEY=sk-c9b-xxxxx
```

**`config.yaml`：**
```yaml
model:
  base_url: https://api.viptoken.top
  default: claude-opus-4-8
  provider: anthropic

providers:
  # 通义千问（用于辅助任务）
  dashscope:
    api: https://dashscope.aliyuncs.com/compatible-mode/v1
    api_key: sk-ws-xxxxx
    default_model: qwen3.7-plus
    models:
      qwen3.7-plus:
        name: Qwen 3.7 Plus
    name: dashscope
    transport: chat_completions

auxiliary:
  # 图片识别用通义千问
  vision:
    provider: dashscope
    model: qwen-vl-max
    base_url: https://dashscope.aliyuncs.com/compatible-mode/v1
    api_key: sk-ws-xxxxx
    timeout: 120

  # 其他辅助任务跟随主模型
  compression:
    provider: auto
    model: ''
    base_url: ''
    api_key: ''
    timeout: 120
```

### 场景：主力用 MIMO，Claude 做辅助

**`.env`：**
```bash
# Anthropic 中转站（用于辅助）
ANTHROPIC_BASE_URL=https://api.viptoken.top
ANTHROPIC_API_KEY=sk-c9b-xxxxx
```

**`config.yaml`：**
```yaml
model:
  base_url: https://token-plan-cn.xiaomimimo.com/v1
  default: mimo-v2.5-pro
  provider: xiaomi-token-plan

providers:
  xiaomi-token-plan:
    api: https://token-plan-cn.xiaomimimo.com/v1
    api_key: tp-xxxxx
    default_model: mimo-v2.5-pro
    models:
      mimo-v2.5-pro:
        name: mimo v2.5 Pro
    name: xiaomi-token-plan
    transport: chat_completions

auxiliary:
  # 技能生成用 Claude
  skills_hub:
    provider: anthropic
    model: claude-opus-4-8
    base_url: https://api.viptoken.top
    api_key: sk-c9b-xxxxx
    timeout: 60

  # 其他跟随主模型
  vision:
    provider: auto
    model: ''
    base_url: ''
    api_key: ''
    timeout: 120
```

---

## 六、常用命令

```bash
# 查看当前配置
hermes config show

# 切换主模型
hermes config set model.provider anthropic
hermes config set model.default claude-opus-4-8

# 更新 Hermes
hermes update

# 重启 CLI
hermes restart
```

---

## 七、配置文件路径汇总

| 文件 | 路径 | 用途 |
|------|------|------|
| 主配置 | `D:\hermes\config.yaml` | 模型、provider、UI 设置 |
| 环境变量 | `D:\hermes\.env` | API 密钥、敏感配置 |
| Skills | `D:\hermes\skills\` | 自定义技能 |
| Plugins | `D:\hermes\plugins\` | 插件 |

---

*最后更新：2026-06-23*