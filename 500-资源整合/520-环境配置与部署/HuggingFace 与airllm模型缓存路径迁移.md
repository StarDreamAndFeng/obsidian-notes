---
tags:
  - HuggingFace
  - airllm
  - 模型缓存
  - 环境变量
  - Python
  - Windows
  - C盘清理
date: 2026-09-10
repo: https://github.com/lyogavin/airllm
---

# HuggingFace 模型缓存路径迁移指南

> **airllm**：通过分层加载（layer-by-layer）大幅降低推理显存占用的 Python 库，**无需量化、蒸馏或剪枝**，仅需 4GB 显存即可运行 70B 大模型。支持 Llama 3.x、Qwen3、DeepSeek 等主流模型，提供预取加速、8bit/4bit 量化和 macOS 支持。
>
> **开源仓库**：<https://github.com/lyogavin/airllm>
>
> ---
>
> **问题**：HuggingFace 工具（包括 `airllm` 底层使用的 `transformers`）默认会将模型下载到用户目录下的 `.cache` 文件夹。Windows 下路径通常是 `C:\Users\你的用户名\.cache\huggingface`，大模型很容易占满 C 盘。
>
> **推荐方案**：方法 1（设置系统环境变量），一劳永逸，全局生效。

---

## 一、方法 1：设置系统环境变量（最推荐 · 全局生效）

这是最彻底的方法，所有使用 HuggingFace 的 Python 项目都会自动使用新路径。

### Windows 用户

1. 右键点击「此电脑」→「属性」→「高级系统设置」→「环境变量」
2. 在「用户变量」或「系统变量」区域，点击「新建」
3. **变量名**：`HF_HOME`
4. **变量值**：你想存的路径，例如：`D:\AI_Models\HuggingFace`
5. 确定保存，**重启终端或 IDE**（VSCode / PyCharm）即可生效

### Linux / macOS 用户

在终端运行（可以加到 `~/.bashrc` 或 `~/.zshrc` 里持久化）：

```bash
export HF_HOME=/path/to/your/disk/AI_Models
```

---

## 二、方法 2：在代码中指定 `cache_dir`（仅当前代码生效）

如果不改系统变量，可在调用 `from_pretrained` 时传入 `cache_dir` 参数。

```python
from airllm import AutoModel

# 指定缓存路径到 D 盘或其他盘
cache_path = "D:/AI_Models/huggingface_cache"

model = AutoModel.from_pretrained(
    "Qwen/Qwen1.5-32B-Chat",
    cache_dir=cache_path  # <--- 加上这一行
)
```

---

## 三、方法 3：指定 AirLLM 分片保存路径（`layer_shards_saving_path`）

**特别重要**：AirLLM 除了下载原始模型，还会生成一份**处理后的分层分片文件**，这份文件通常比原始模型还要大。

如果只改了下载地址而没改这个参数，AirLLM 默认可能会把处理后的分片文件继续丢到默认缓存（C 盘）或当前项目目录下。

**建议同时设置两个路径：**

```python
from airllm import AutoModel

# 1. 原始模型下载路径
raw_cache_dir = "D:/AI_Models/download_cache"

# 2. AirLLM 处理后的分片文件保存路径（这才是大头）
shards_dir = "D:/AI_Models/airllm_shards"

model = AutoModel.from_pretrained(
    "Qwen/Qwen1.5-32B-Chat",
    cache_dir=raw_cache_dir,                 # 原始模型放这里
    layer_shards_saving_path=shards_dir     # 分片文件放这里
)
```

---

## 四、清理 C 盘旧文件

设置好新路径并确认能正常运行后，可以手动删除旧文件释放空间：

| 平台 | 删除路径 |
| --- | --- |
| **Windows** | `C:\Users\你的用户名\.cache\huggingface` |
| **Linux / macOS** | `~/.cache/huggingface` |

> ⚠️ 删除前请确保没有其他正在运行的程序需要这些模型。

---

## 五、方案对比

| 维度 | 方法 1：环境变量 | 方法 2：`cache_dir` | 方法 3：`layer_shards_saving_path` |
| --- | --- | --- | --- |
| 生效范围 | 全局（所有项目） | 仅当前代码 | 仅当前代码 |
| 配置成本 | 一次配置，长期生效 | 每次调用都要写 | 每次调用都要写 |
| 覆盖原始模型 | ✅ | ✅ | ✅ |
| 覆盖 AirLLM 分片 | ✅（环境变量会同时影响） | ❌ 仍走默认 | ✅ |
| 推荐度 | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐（与方法 2 配合使用） |

> **最佳实践**：方法 1（环境变量）打底，处理 AirLLM 时同时使用方法 2 + 3 显式指定双路径，避免分片文件继续落 C 盘。
