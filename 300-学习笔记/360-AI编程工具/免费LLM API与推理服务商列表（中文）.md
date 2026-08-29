---
tags:
  - LLM
  - API
  - 免费资源
  - AI编程
  - 模型推理
  - OpenAI兼容
date: 2026-08-29
---

## 厂商 API

由训练或微调模型的公司自行运营的 API 服务。

### [Aion Labs](https://www.aionlabs.ai/app/api-keys/) 🇮🇱

永久免费档，无需信用卡。15 RPM，每天 20K tokens。专注于角色扮演与故事叙事。

Base URL：`https://api.aionlabs.ai/v1`

| 模型名称                          | 上下文 | 最大输出   | 模态             | 速率限制          |
| --------------------------------- | ------ | ---------- | ---------------- | ----------------- |
| `aion-labs/aion-2.0`              | 128K   | 32K        | 文本（推理型）   | 15 RPM，20K TPD   |
| `aion-labs/aion-rp-llama-3.1-8b`  | 32K    | 32K        | 文本             | 15 RPM，20K TPD   |
| `aion-labs/aion-3.0`              | 128K   | 32K        | 文本（推理型）   | 15 RPM，20K TPD   |
| `aion-labs/aion-3.0-mini`         | 128K   | 32K        | 文本（推理型）   | 15 RPM，20K TPD   |

### [Cohere](https://dashboard.cohere.com/api-keys) 🇨🇦

免费「Trial」API key，无需信用卡。每月 1,000 次 API 调用。仅限非商业用途。

Base URL：`https://api.cohere.com/v2`

| 模型名称             | 上下文 | 最大输出 | 模态             | 速率限制 |
| -------------------- | ------ | -------- | ---------------- | -------- |
| Command A+ (218B)    | 128K   | 64K      | 文本 + 图像      | 20 RPM   |
| Command A (111B)     | 256K   | 8K       | 文本             | 20 RPM   |
| Command R+           | 128K   | 4K       | 文本             | 20 RPM   |
| Command R            | 128K   | 4K       | 文本             | 20 RPM   |
| Command R7B          | 128K   | 4K       | 文本             | 20 RPM   |
| Command A Reasoning  | 256K   | 32K      | 文本（推理型）   | 20 RPM   |
| Command A Translate  | 8K     | 8K       | 文本             | 20 RPM   |
| Command A Vision     | 128K   | 8K       | 文本 + 图像      | 20 RPM   |
| Command R7B Arabic   | 128K   | ~4K      | 文本             | 20 RPM   |
| Aya Expanse 32B      | 128K   | 4K       | 文本             | 20 RPM   |
| Aya Vision 32B       | 16K    | 4K       | 文本 + 图像      | 20 RPM   |

### [Google Gemini](https://aistudio.google.com/app/apikey) 🇺🇸

免费档，无需信用卡。免费档的 Prompt 可能被 Google 用于改进产品。[^1]

Base URL：`https://generativelanguage.googleapis.com/v1beta`

| 模型名称              | 上下文 | 最大输出 | 模态                         | 速率限制          |
| --------------------- | ------ | -------- | ---------------------------- | ----------------- |
| Gemini 3.7 Flash      | 1M     | 65K      | 文本 + 图像 + 音频 + 视频    | —                 |
| Gemini 3.6 Flash      | 1M     | 65K      | 文本 + 图像 + 音频 + 视频    | 15 RPM，1,500 RPD |
| Gemini 3.5 Flash      | 1M     | 65K      | 文本 + 图像 + 音频 + 视频    | 15 RPM，1,500 RPD |
| Gemini 3.5 Flash-Lite | 1M     | 65K      | 文本 + 图像 + 音频 + 视频    | 30 RPM，1,500 RPD |
| Gemini 3.1 Flash-Lite | 1M     | 65K      | 文本 + 图像 + 音频 + 视频    | 30 RPM，1,500 RPD |
| Gemini 2.5 Flash      | 1M     | 65K      | 文本 + 图像 + 音频 + 视频    | 15 RPM，1,500 RPD |
| Gemini 2.5 Flash-Lite | 1M     | 65K      | 文本 + 图像 + 音频 + 视频    | 30 RPM，1,500 RPD |
| Gemini 2.5 Pro        | 1M     | 65K      | 文本 + 图像 + 音频 + 视频    | 5 RPM，50 RPD     |
| Gemma 4 31B           | 256K   | 32K      | 文本                         | —                 |
| Gemma 4 26B A4B       | 256K   | 32K      | 文本                         | —                 |

### [Mistral AI](https://console.mistral.ai/api-keys) 🇫🇷

默认开启免费模式，无需信用卡。每月提供 $10 API 额度；除非你主动退出，否则免费模式下的 Prompt 可能被用于训练 Mistral 模型。[^13]

Base URL：`https://api.mistral.ai/v1`

| 模型名称                  | 上下文 | 最大输出 | 模态                  | 速率限制            |
| ------------------------- | ------ | -------- | --------------------- | ------------------- |
| Mistral Medium 3.5 (128B) | 256K   | —        | 文本 + 图像 + 代码    | ~1 RPS，500K TPM    |
| Mistral Small 4           | 256K   | —        | 文本 + 图像 + 代码    | ~1 RPS，500K TPM    |
| Mistral Large 3           | 256K   | —        | 多模态                | ~1 RPS，500K TPM    |
| Ministral 3 8B            | 256K   | —        | 文本 + 视觉           | ~1 RPS，500K TPM    |
| Codestral                 | 128K   | —        | 代码                  | ~1 RPS，500K TPM    |
| Ministral 3 3B            | 256K   | —        | 文本 + 视觉           | ~1 RPS，500K TPM    |
| Ministral 3 14B           | 256K   | —        | 文本 + 视觉           | ~1 RPS，500K TPM    |

### [Z AI（智谱 AI）](https://open.bigmodel.cn/usercenter/apikeys) 🇨🇳

永久免费模型，无需信用卡。[^12]

Base URL：`https://open.bigmodel.cn/api/paas/v4`

| 模型名称                              | 上下文 | 最大输出 | 模态             | 速率限制         |
| ------------------------------------- | ------ | -------- | ---------------- | ---------------- |
| GLM-4.7-Flash                         | 200K   | 128K     | 文本（推理型）   | 1 并发请求       |
| GLM-4.5-Flash（已宣布下线）           | 128K   | 96K      | 文本（推理型）   | 1 并发请求       |
| GLM-4.6V-Flash                        | 128K   | 32K      | 多模态           | 1 并发请求       |

## 推理服务商

托管来自各方的开源权重模型的第三方平台。

### [Cloudflare Workers AI](https://dash.cloudflare.com/profile/api-tokens) 🇺🇸

每日免费 10,000 Neurons，无需信用卡。免费档提供 75+ 模型。[^11]

Base URL：`https://api.cloudflare.com/client/v4/accounts/{account_id}/ai/run`

| 模型名称                                         | 上下文 | 最大输出         | 模态                           | 速率限制                |
| ------------------------------------------------ | ------ | ----------------- | ------------------------------ | ----------------------- |
| `@cf/meta/llama-3.3-70b-instruct-fp8-fast`        | 24K    | 与上下文共享      | 文本                           | 10K neurons/day（共享） |
| `@cf/meta/llama-4-scout-17b-16e-instruct`        | 131K   | 与上下文共享      | 多模态                         | 10K neurons/day（共享） |
| `@cf/openai/gpt-oss-120b`                        | 128K   | 与上下文共享      | 文本                           | 10K neurons/day（共享） |
| `@cf/google/gemma-4-26b-a4b-it`                   | 256K   | 与上下文共享      | 文本 + 视觉                    | 10K neurons/day（共享） |
| `@cf/zai-org/glm-4.7-flash`                      | 131K   | 与上下文共享      | 文本                           | 10K neurons/day（共享） |
| `@cf/mistralai/mistral-small-3.1-24b-instruct`    | 128K   | 与上下文共享      | 文本                           | 10K neurons/day（共享） |
| `@cf/deepseek-ai/deepseek-r1-distill-qwen-32b`    | 80K    | 与上下文共享      | 文本（推理型）                 | 10K neurons/day（共享） |
| + 另外 72 个模型                                 | 不定   | 不定              | 文本、图像、音频、Embeddings   | 10K neurons/day（共享） |

### [Groq](https://console.groq.com/keys) 🇺🇸

免费档，无需信用卡。超快 LPU 推理。[^2]

Base URL：`https://api.groq.com/openai/v1`

| 模型名称              | 上下文 | 最大输出 | 模态 | 速率限制          |
| --------------------- | ------ | -------- | ---- | ----------------- |
| `openai/gpt-oss-120b` | 131K   | 65K      | 文本 | 30 RPM，1,000 RPD |
| `openai/gpt-oss-20b`  | 131K   | 65K      | 文本 | 30 RPM，1,000 RPD |
| `groq/compound`       | 131K    | 8K       | 文本 | 30 RPM，250 RPD   |
| `groq/compound-mini`  | 131K    | 8K       | 文本 | 30 RPM，250 RPD   |
| `qwen/qwen3.6-27b`    | 131K    | 16K      | 文本 | 30 RPM，1,000 RPD |

### [Hugging Face](https://huggingface.co/settings/tokens) 🇺🇸

免费用户每月 $0.10 推理服务商额度（可能调整）。路由到 Fireworks、Together、Hyperbolic、Nebius、Novita、DeepInfra 等平台，提供数千个模型。

Base URL：`https://router.huggingface.co/v1`

| 模型名称                        | 上下文 | 最大输出 | 模态                           | 速率限制        |
| ------------------------------- | ------ | -------- | ------------------------------ | --------------- |
| Meta-Llama-3.1-8B-Instruct      | 128K   | ~4K      | 文本                           | 按额度计费      |
| gemma-3-4b-it                   | 131K   | ~4K      | 文本                           | 按额度计费      |
| phi-4                           | 16K    | ~4K      | 文本                           | 按额度计费      |
| Qwen2.5-Coder-7B-Instruct       | 131K   | ~4K      | 文本                           | 按额度计费      |
| Qwen2.5-7B-Instruct             | 131K   | ~4K      | 文本                           | 按额度计费      |
| + 数千个社区模型                | 不定   | 不定     | 文本、图像、音频、Embeddings   | 按额度计费      |

### [Kilo Code](https://app.kilo.ai/profile) 🇺🇸

免费模型，无需信用卡、无需 API key。`kilo-auto/free` 自动路由器动态分配到免费池中的模型。[^5]

Base URL：`https://api.kilo.ai/api/gateway`

| 模型名称                                             | 上下文 | 最大输出 | 模态           | 速率限制    |
| ---------------------------------------------------- | ------ | -------- | -------------- | ----------- |
| `nvidia/nemotron-3-ultra-550b-a55b:free`             | 1M     | 65K      | 文本           | 200 req/hr  |
| `stepfun/step-3.7-flash:free`                        | 262K   | 262K     | 文本 + 视觉    | 200 req/hr  |
| `nvidia/nemotron-3-super-120b-a12b:free`             | 262K   | 262K     | 文本           | 200 req/hr  |
| `nvidia/nemotron-3-nano-omni-30b-a3b-reasoning:free` | 256K   | 65K      | 多模态         | 200 req/hr  |
| `poolside/laguna-s-2.1:free`                         | 262K   | 32K      | 文本（代码）   | 200 req/hr  |
| `poolside/laguna-xs-2.1:free`                        | 262K   | 32K      | 文本（代码）   | 200 req/hr  |
| `cohere/north-mini-code:free`                        | 256K   | 64K      | 文本（代码）   | 200 req/hr  |
| `openrouter/free`                                    | 不定   | 不定     | 文本           | 200 req/hr  |
| `tencent/hy3:free`                                   | 262K   | 128K     | 文本           | 200 req/hr  |
| `nvidia/nemotron-3.5-lightning:free`                 | 1M     | 65K      | 文本           | 200 req/hr  |
| `liquid/lfm-2.5-2.6b:free`                           | 64K    | 8K       | 文本           | 200 req/hr  |

### [LLM7.io](https://token.llm7.io) 🇬🇧

带免费档的 API 网关。匿名访问无需 key，可访问 `turbo` 模型；从 token.llm7.io 获取免费 token 可提升速率与 token 上限，但访问的模型相同。[^10]

Base URL：`https://api.llm7.io/v1`

| 模型名称                   | 上下文 | 最大输出 | 模态             | 速率限制                       |
| -------------------------- | ------ | -------- | ---------------- | ------------------------------ |
| `gpt-oss:20b`              | 128K   | —        | 文本             | 10 RPM，60 req/hr（匿名）     |
| mistral-Nemo-Instruct-2407 | 128K   | —        | 文本             | 10 RPM，60 req/hr（匿名）     |
| minimax-m2.7               | 180K   | —        | 文本（推理型）   | 10 RPM，60 req/hr（匿名）     |

### [ModelScope（魔搭）](https://modelscope.cn/my/myaccesstoken) 🇨🇳

面向注册用户的免费 API 推理服务。需绑定阿里云账号 + 实名认证。[^6]

Base URL：`https://api-inference.modelscope.cn/v1`

| 模型名称                     | 上下文 | 最大输出 | 模态       | 速率限制                                |
| ---------------------------- | ------ | -------- | ---------- | --------------------------------------- |
| `Qwen/Qwen3.5-35B-A3B`       | 256K   | —        | 文本       | 共计 2,000 RPD；≤500 RPD/模型（动态）   |
| `Qwen/Qwen3.5-27B`           | 256K   | —        | 文本       | 共计 2,000 RPD；≤500 RPD/模型（动态）   |
| + 启用 API 推理的模型        | 不定   | 不定     | LLM、MLLM  | 动态配额 + 动态并发                     |

### [NVIDIA NIM](https://build.nvidia.com/explore/discover) 🇺🇸

加入 NVIDIA Developer Program 即可免费使用。100+ 模型，按模型分别限速。

Base URL：`https://integrate.api.nvidia.com/v1`

| 模型名称                                | 上下文 | 最大输出 | 模态                                 | 速率限制          |
| --------------------------------------- | ------ | -------- | ------------------------------------ | ----------------- |
| `nvidia/nemotron-3-super-120b-a12b`     | 1M     | 262K     | 文本                                 | 40 RPM，10,000 RPD |
| `nvidia/nemotron-3-nano-30b-a3b`        | 262K   | 32K      | 文本                                 | 40 RPM，10,000 RPD |
| `nvidia/llama-3.1-nemotron-ultra-253b`  | 128K   | 4K       | 文本                                 | 40 RPM，10,000 RPD |
| `meta/llama-3.3-70b-instruct`           | 128K   | 4K       | 文本                                 | 40 RPM，10,000 RPD |
| `mistralai/mistral-nemotron`            | 128K   | 8K       | 文本                                 | 40 RPM，10,000 RPD |
| `google/gemma-4-31b-it`                 | 262K   | 8K       | 文本                                 | 40 RPM，10,000 RPD |
| `mistralai/mistral-large-2-instruct`    | 128K   | 4K       | 文本                                 | 40 RPM，10,000 RPD |
| `minimaxai/minimax-m3`                  | 1M     | ~64K     | 文本                                 | 40 RPM，10,000 RPD |
| `nvidia/nemotron-3-ultra-550b-a55b`      | 1M     | 262K     | 文本                                 | 40 RPM，10,000 RPD |
| `openai/gpt-oss-120b`                   | 131K   | 131K     | 文本                                 | 40 RPM，10,000 RPD |
| `openai/gpt-oss-20b`                    | 131K   | 131K     | 文本                                 | 40 RPM，10,000 RPD |
| + 另外 92 个模型                        | 不定   | 不定     | 文本、图像、视频、语音、Embeddings   | 40 RPM，10,000 RPD |

### [Ollama Cloud](https://ollama.com/settings/keys) 🇺🇸

带用量限制的免费档。来自 Ollama 库的 16 个云端模型族。通过 https://ollama.com/v1 兼容 OpenAI SDK。[^3]

Base URL：`https://ollama.com/api`

| 模型名称               | 上下文 | 最大输出        | 模态 | 速率限制                          |
| ---------------------- | ------ | --------------- | ---- | --------------------------------- |
| deepseek-v4-pro        | 1M     | 依模型而定      | 文本 | 会话/每周限额（未公开）           |
| deepseek-v4-flash      | 1M     | 依模型而定      | 文本 | 会话/每周限额（未公开）           |
| minimax-m3             | 512K   | 依模型而定      | 文本 | 会话/每周限额（未公开）           |
| kimi-k3                | 1M     | 依模型而定      | 文本 | 会话/每周限额（未公开）           |
| `gpt-oss:120b`         | 128K   | 依模型而定      | 文本 | 会话/每周限额（未公开）           |
| `gpt-oss:20b`          | 131K   | 依模型而定      | 文本 | 会话/每周限额（未公开）           |
| nemotron-3-ultra       | 262K   | 依模型而定      | 文本 | 会话/每周限额（未公开）           |
| `mistral-large-3:675b` | 256K   | 依模型而定      | 文本 | 会话/每周限额（未公开）           |
| `qwen3.5:397b`         | 256K   | 依模型而定      | 文本 | 会话/每周限额（未公开）           |
| + 另外 7 个云端模型    | 不定   | 不定            | 文本 | 会话/每周限额（未公开）           |

### [OpenRouter](https://openrouter.ai/keys) 🇺🇸

17 个免费模型（以 `:free` 后缀标识）。兼容 OpenAI SDK。[^4]

Base URL：`https://openrouter.ai/api/v1`

| 模型名称                                 | 上下文 | 最大输出 | 模态           | 速率限制      |
| ---------------------------------------- | ------ | -------- | -------------- | ------------- |
| `nvidia/nemotron-3-super-120b-a12b:free` | 262K   | 262K     | 文本           | 20 RPM，50 RPD |
| `openai/gpt-oss-20b:free`                | 131K   | 32K      | 文本           | 20 RPM，50 RPD |
| `cohere/north-mini-code:free`            | 256K   | 64K      | 文本（代码）   | 20 RPM，50 RPD |
| `google/gemma-4-26b-a4b-it:free`         | 262K   | 32K      | 文本 + 图像    | 20 RPM，50 RPD |
| `google/gemma-4-31b-it:free`             | 262K   | 32K      | 文本 + 图像    | 20 RPM，50 RPD |
| `inclusionai/ling-3.0-flash:free`        | 262K   | 32K      | 文本           | 20 RPM，50 RPD |
| `nvidia/nemotron-3-nano-30b-a3b:free`    | 256K   | —        | 文本           | 20 RPM，50 RPD |
| `nvidia/nemotron-nano-9b-v2:free`        | 128K   | —        | 文本           | 20 RPM，50 RPD |
| `nvidia/nemotron-nano-12b-v2-vl:free`    | 128K   | 128K     | 文本 + 图像    | 20 RPM，50 RPD |
| `poolside/laguna-s-2.1:free`             | 262K   | 32K      | 文本（代码）   | 20 RPM，50 RPD |
| `poolside/laguna-xs-2.1:free`            | 262K   | 32K      | 文本（代码）   | 20 RPM，50 RPD |
| + 另外 6 个免费模型                      | 不定   | 不定     | 文本 / 图像    | 20 RPM，50 RPD |

### [OVHcloud AI Endpoints](https://www.ovhcloud.com/en/public-cloud/ai-endpoints/catalog/) 🇫🇷

免费匿名档（无需 API key、无需注册）：每个 IP 每个模型 2 RPM。20+ 开源权重模型，部署在欧盟。兼容 OpenAI SDK。[^7]

Base URL：`https://oai.endpoints.kepler.ai.cloud.ovh.net/v1`

| 模型名称                     | 上下文 | 最大输出 | 模态           | 速率限制          |
| ---------------------------- | ------ | -------- | -------------- | ----------------- |
| Qwen3.5-397B-A17B            | 131K   | ~32K     | 文本           | 2 RPM（匿名）     |
| gpt-oss-120b                 | 128K   | ~32K     | 文本           | 2 RPM（匿名）     |
| gpt-oss-20b                  | 128K   | ~8K      | 文本           | 2 RPM（匿名）     |
| Meta-Llama-3_3-70B-Instruct  | 131K   | ~4K      | 文本           | 2 RPM（匿名）     |
| Qwen3.6-27B                  | 131K   | ~32K     | 文本           | 2 RPM（匿名）     |
| Qwen3.5-9B                   | 131K   | ~8K      | 文本           | 2 RPM（匿名）     |
| Qwen3-32B                    | 131K   | ~32K     | 文本           | 2 RPM（匿名）     |
| Qwen3-Coder-30B-A3B-Instruct | 262K   | ~32K     | 文本（代码）   | 2 RPM（匿名）     |
| Qwen2.5-VL-72B-Instruct      | 128K   | ~8K      | 文本 + 视觉    | 2 RPM（匿名）     |
| Mistral-Small-3.2-24B-Instruct | 128K | ~4K      | 文本           | 2 RPM（匿名）     |
| Mistral-Nemo-Instruct-2407   | 128K   | ~4K      | 文本           | 2 RPM（匿名）     |
| Mistral-7B-Instruct-v0.3     | 32K    | ~4K      | 文本           | 2 RPM（匿名）     |

### [SiliconFlow（硅基流动）](https://cloud.siliconflow.cn/account/ak) 🇨🇳

永久免费模型，无需信用卡。需实名认证。目录中 100+ 模型，多数为付费。[^9]

Base URL：`https://api.siliconflow.cn/v1`

| 模型名称        | 上下文 | 最大输出 | 模态 | 速率限制             |
| --------------- | ------ | -------- | ---- | -------------------- |
| `Qwen/Qwen3-8B` | 128K   | —        | 文本 | 1,000 RPM，50,000 TPM |

## 术语表

| 缩写   | 含义            |
| ------ | --------------- |
| **RPM** | 每分钟请求数    |
| **RPD** | 每天请求数      |
| **TPM** | 每分钟 token 数 |
| **TPD** | 每天 token 数   |
| **RPS** | 每秒请求数      |

## 贡献

知道还有哪些缺失的免费档？。请包含服务商、endpoint、速率限制（附文档链接）以及几个有代表性的模型。试用额度和限时优惠不计入。
