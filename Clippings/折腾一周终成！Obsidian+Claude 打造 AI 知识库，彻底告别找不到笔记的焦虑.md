---
title: "折腾一周终成！Obsidian+Claude 打造 AI 知识库，彻底告别找不到笔记的焦虑"
source: "https://zhuanlan.zhihu.com/p/2044197495492830282"
author:
  - "[[机器学习社区]]"
published:
created: 2026-08-24
description: "你是否经历过这样的场景：浏览网页时看到一篇好文章，随手收藏后就再也没打开过？或者读书时记了大量笔记，但需要用到时却怎么也找不到？ 信息时代，我们缺的不是信息，而是把信息转化为知识的能力。 今天，我要介…"
tags:
  - "clippings"
---
[收录于 · 大模型技术](https://www.zhihu.com/column/c_1735987714556661760)

10 人赞同了该文章

目录

收起

一、系统架构：LLM Wiki 是什么？

二、安装 Obsidian 并创建知识库

2.1 下载安装 Obsidian

2.2 创建 Vault（知识库）

2.3 初始化目录结构

三、安装 Obsidian Web Clipper（网页剪藏）

3.1 安装浏览器扩展

3.2 配置 Web Clipper

3.3 试试剪藏

四、安装 Claude Code

4.1 前置要求：安装 Node.js

4.2 安装 Claude Code

4.3 配置 API（使用 cc-switch）

4.4 首次启动 Claude Code

五、配置知识结构化提示词（CLAUDE.md）

5.1 创建 CLAUDE.md

5.2 编写结构化规则

5.3 初始化 index.md 和 log.md

六、使用 Claude Code 进行知识结构化

6.1 准备素材

6.2 启动 Claude Code 并执行摄入

6.3 查看结果

6.4 使用 Obsidian 图谱视图

6.5 后续增量更新

6.6 向 AI 提问

6.7 检查知识库健康

七、安装 Claudian 插件（在 Obsidian 内使用 AI）

7.1 前置要求

7.2 方法一：通过 GitHub Release 安装（推荐）

八、使用 Claudian 在 Obsidian 中进行 AI 问答

8.1 打开 Claudian 侧边栏

8.2 开始对话

8.3 高级功能

九、自定义你的知识结构化提示词

9.1 调整知识条目分类

9.2 添加领域特定规则

9.3 设置知识条目模板

9.4 让提示词持续进化

十、完整工作流总结

十一、进阶项目推荐

11.1 llm\_wiki — 桌面端知识图谱应用

11.2 claude-obsidian — Claude Code 知识引擎插件

11.3 llm-wiki-agent — 多平台编码智能体

写在最后

附录

A. 常用终端命令速查

B. 推荐的 Obsidian 插件

C. 参考资源

你是否经历过这样的场景：浏览网页时看到一篇好文章，随手收藏后就再也没打开过？或者读书时记了大量笔记，但需要用到时却怎么也找不到？

信息时代，我们缺的不是信息，而是把信息转化为知识的能力。

今天，我要介绍一套完整的知识管理方案，它能帮你：

- **快速采集** ：一键抓取网页内容到本地
- **自动结构化** ：让 AI 把零散笔记整理成互相关联的知识条目
- **随时问答** ：在笔记软件里直接向 AI 提问，基于你的知识库获得回答

这套方案的核心工具组合是：

![](https://pic4.zhimg.com/v2-d3376c31fb607b4b151a655b3d55c6cd_1440w.jpg)

而这套方案的 [知识组织](https://zhida.zhihu.com/search?content_id=275914070&content_type=Article&match_order=1&q=%E7%9F%A5%E8%AF%86%E7%BB%84%E7%BB%87&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODc3Mzc3MTksInEiOiLnn6Xor4bnu4Tnu4ciLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzU5MTQwNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.MRa8lhceXPpGH9AcfUIHj8NexhENB_IcFcRhoogYKJw&zhida_source=entity) 理念，来自 AI 领域大牛 [Andrej Karpathy](https://zhida.zhihu.com/search?content_id=275914070&content_type=Article&match_order=1&q=Andrej+Karpathy&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODc3Mzc3MTksInEiOiJBbmRyZWogS2FycGF0aHkiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzU5MTQwNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.3pvfYSiqne0svWz1TWkNhGqDNRL9TeSfe1AEpdeV6VA&zhida_source=entity) 提出的 **[LLM Wiki](https://zhida.zhihu.com/search?content_id=275914070&content_type=Article&match_order=1&q=LLM+Wiki&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODc3Mzc3MTksInEiOiJMTE0gV2lraSIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjI3NTkxNDA3MCwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0._8oy5WyRSWkqfhwe15jNX4A3VZ8Hti9n0aioCLD32uA&zhida_source=entity)** 模式。

下面，我们从零开始，一步步搭建这个系统。

## 一、系统架构：LLM Wiki 是什么？

在动手之前，先理解这个系统的核心理念。

Andrej Karpathy（OpenAI 联合创始人、前特斯拉 AI 总监）提出了一个优雅的知识管理思路： **让大语言模型像维基百科编辑一样，持续维护和更新你的个人知识库** 。

这个系统的核心架构分为三层：

```bash
📂 你的知识库（Obsidian Vault）
├── 📁 raw/              ← 第一层：原始素材（只读，不可修改）
│   ├── 文章1.md
│   ├── 文章2.md
│   └── ...
├── 📁 wiki/             ← 第二层：结构化知识（AI 生成和维护）
│   ├── index.md         ← 内容总目录
│   ├── log.md           ← 变更日志
│   ├── 主题A.md
│   ├── 主题B.md
│   └── ...
└── 📄 CLAUDE.md         ← 第三层：规则文件（告诉 AI 该怎么做）
```

**三层各自的职责：**

1. **Raw（原始素材层）** ：你从网页、书籍、文章中收集的原始内容，存放在这里后不再修改。这是知识的”源头”。
2. **Wiki（知识层）** ：AI 阅读你的原始素材后，生成结构化的、互相链接的知识条目。每当你添加新素材，AI 会增量更新相关的知识条目——就像维基百科编辑看到新资料会更新相关词条一样。
3. **Schema（规则层）** ：一个 `CLAUDE.md` 文件，定义了 AI 应该如何组织知识、使用什么格式、遵循什么规则。

**关键操作只有三个：**

![](https://pic1.zhimg.com/v2-d52282a40f33ddbe38984428b28eb416_1440w.jpg)

这就是全部。不需要 [向量数据库](https://zhida.zhihu.com/search?content_id=275914070&content_type=Article&match_order=1&q=%E5%90%91%E9%87%8F%E6%95%B0%E6%8D%AE%E5%BA%93&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODc3Mzc3MTksInEiOiLlkJHph4_mlbDmja7lupMiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzU5MTQwNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.0fJLzwo5Yo8LhwDtERnViX9_2Sr9DapBTM6_p6ZlKPs&zhida_source=entity) ，不需要复杂的 RAG 管道，只需要 Markdown 文件和一个够聪明的 AI。

理解了这个架构，下面我们开始搭建。

## 二、安装 Obsidian 并创建知识库

### 2.1 下载安装 Obsidian

Obsidian 是一款免费的个人 Markdown 笔记应用，所有数据都以 `.md` 文件存储在本地，你完全掌控自己的数据。

打开 Obsidian 官网下载页面：

```bash
https://obsidian.md/download
```

选择 macOS 版本下载，下载完成后将 Obsidian 拖入”应用程序”文件夹。

![](https://pic2.zhimg.com/v2-a9f3d4e5f111b9fc282f84af4056dc15_1440w.jpg)

### 2.2 创建 Vault（知识库）

首次打开 Obsidian，会看到欢迎界面。点击”创建新库”（Create new vault），选择一个存放位置；或者点击“Open folder as vault”（事先用 `mkdir ~/Documents/MyWiki` 创建好空目录）

建议将知识库放在一个容易找到的路径，比如：

```bash
~/Documents/MyWiki
```

创建完成后，你会进入一个空的 Obsidian 界面。这就是你的知识库了，现在它还是空的，我们接下来往里面添加内容。

![](https://picx.zhimg.com/v2-916aaa3aa02bc9df0b657b0872c8fb2b_1440w.jpg)

### 2.3 初始化目录结构

打开终端（Terminal），执行以下命令创建 LLM Wiki 所需的目录结构：

```bash
# 进入你的知识库目录（请替换为实际路径）
cd ~/Documents/MyWiki

# 创建目录结构
mkdir -p raw wiki
```

创建完成后，你的知识库结构如下：

```bash
MyWiki/
├── raw/      ← 原始素材存放处
└── wiki/     ← 结构化知识存放处
```
![](https://pic3.zhimg.com/v2-464724303ccfbfcf5b10f71cb3229cae_1440w.jpg)

## 三、安装 Obsidian Web Clipper（网页剪藏）

在知识管理的工作流中，第一步是 **采集信息** 。Obsidian Web Clipper 是 Obsidian 官方提供的浏览器扩展，可以一键将网页内容甚至是 [youtube视频](https://zhida.zhihu.com/search?content_id=275914070&content_type=Article&match_order=1&q=youtube%E8%A7%86%E9%A2%91&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODc3Mzc3MTksInEiOiJ5b3V0dWJl6KeG6aKRIiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6Mjc1OTE0MDcwLCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.Od6A2J_vfrNkTdpxC4Z2d5MYs_lSajXj7e36EHmIb6g&zhida_source=entity) 字幕！！！保存为 Markdown 文件到你的知识库中。

### 3.1 安装浏览器扩展

根据你使用的浏览器，选择对应的安装方式：

**Chrome / Edge 用户：**

打开 Chrome Web Store 搜索”Obsidian Web Clipper”，或直接访问：

```
https://chromewebstore.google.com/detail/obsidian-web-clipper/hoolglpddlnpcljhifenmeljncjmdkjn
```

点击”添加到 Chrome”。

![](https://pic4.zhimg.com/v2-6d02bb59f1f05eba7ee1af053dc22ec9_1440w.jpg)

![](https://pic1.zhimg.com/v2-8c51c8da9301d5fb14ad1a633250ab1e_1440w.jpg)

**Safari 用户：**

在 Mac App Store 搜索”Obsidian Web Clipper”安装。

### 3.2 配置 Web Clipper

安装完成后，点击浏览器工具栏中的 Obsidian 图标，进行初始配置：

1. **Vault 路径** ：指向你的 Obsidian 知识库路径，如 `~/Documents/MyWiki`
2. **保存位置** ：设置默认保存到 `raw/` 文件夹——这很重要！所有从 [网页抓取](https://zhida.zhihu.com/search?content_id=275914070&content_type=Article&match_order=1&q=%E7%BD%91%E9%A1%B5%E6%8A%93%E5%8F%96&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODc3Mzc3MTksInEiOiLnvZHpobXmipPlj5YiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzU5MTQwNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.qN5rHjPzVn2JG_C8KAHbDWWAD5cVKg0waN1zQfrmjw8&zhida_source=entity) 的内容都应该作为原始素材存放在 raw 目录中
3. **文件名格式** ：可以设置为 `{{title}}` ，即使用网页标题作为文件名

### 3.3 试试剪藏

现在打开一篇你想收藏的网页文章，点击浏览器工具栏的 Obsidian Web Clipper 图标，选择”Clip as Markdown”，内容就会被保存到你的 `raw/` 文件夹中。

回到 Obsidian，你应该能在左侧的 `raw` 文件夹中看到刚保存的文件。

> **提示** ：Karpathy 在他的 LLM Wiki 中建议，如果原始素材中有重要图片，最好将图片下载到本地保存（比如放在 `raw/images/` 目录），这样可以避免因图片链接失效导致内容丢失。

## 四、安装 Claude Code

Claude Code 是 Anthropic 官方推出的命令行 AI 工具，它是我们实现知识结构化的”引擎”。本节简要介绍安装步骤，更多细节请参考官方文档。

### 4.1 前置要求：安装 Node.js

Claude Code 基于 Node.js，推荐使用 Homebrew 安装：

```bash
# 安装 Node.js
brew install node
```

验证安装：

```bash
node --version   # 应输出 v20.x.x 或更高
npm --version    # 应输出 10.x.x 或更高
```

### 4.2 安装 Claude Code

由于国内网络原因，建议使用 npm 镜像源安装：

```bash
npm install -g @anthropic-ai/claude-code --registry=https://registry.npmmirror.com
```

> **常见问题** ：如果安装后运行 `claude` 提示 `Error: claude native binary not installed` ，说明平台二进制文件缺失，需要手动补装：

```bash
# 安装 macOS 平台二进制包
npm install -g @anthropic-ai/claude-code-darwin-x64 --registry=https://registry.npmjs.org

# 找到你的 Node.js 全局模块路径，将二进制文件链接到主程序预期位置
# 以下路径需替换为你实际的 node 版本号和用户目录
mkdir -p /Users/你的用户名/.nvm/versions/node/v24.x.x/lib/node_modules/@anthropic-ai/claude-code/bin
ln -f /Users/你的用户名/.nvm/versions/node/v24.x.x/lib/node_modules/@anthropic-ai/claude-code-darwin-x64/claude /Users/你的用户名/.nvm/versions/node/v24.x.x/lib/node_modules/@anthropic-ai/claude-code/bin/claude.exe
chmod +x /Users/你的用户名/.nvm/versions/node/v24.x.x/lib/node_modules/@anthropic-ai/claude-code/bin/claude.exe
```

安装完成后验证：

```bash
claude --version
```

### 4.3 配置 API（使用 cc-switch）

Claude Code 默认使用 Anthropic API，但国内用户可能更希望使用国内大模型的 API。推荐使用 **cc-switch** 工具来切换模型供应商。

**步骤 1：下载 cc-switch**

访问 GitHub Release 页面下载 macOS 版本：

```bash
https://github.com/farion1231/cc-switch/releases
```

**步骤 2：配置模型供应商**

以智谱 GLM 为例：

![](https://pic1.zhimg.com/v2-1e84479553007b84df2a436a494fe47e_1440w.jpg)

前往 [https://open.bigmodel.cn](https://link.zhihu.com/?target=https%3A//open.bigmodel.cn/) 注册并获取 API Key。

> **提示** ：你也可以使用其他兼容 Anthropic API 格式的模型供应商，如 OpenAI 等，在 cc-switch 中配置对应的 API 请求地址和密钥即可。

### 4.4 首次启动 Claude Code

进入你的知识库目录，启动 Claude Code：

```bash
cd ~/Documents/MyWiki
claude
```

首次启动时，Claude Code 会要求你确认工作目录并接受使用条款。完成后你将看到 Claude Code 的交互界面。

输入 `/exit` 可以退出 Claude Code。

### 五、配置知识结构化提示词（CLAUDE.md）

现在到了最关键的一步： **告诉 AI 如何组织你的知识** 。

在 LLM Wiki 模式中， `CLAUDE.md` 就是给 AI 的”操作手册”。当你在知识库目录下启动 Claude Code 时，它会自动读取这个文件作为工作指引。

### 5.1 创建 CLAUDE.md

在知识库根目录创建 `CLAUDE.md` 文件：

```bash
cd ~/Documents/MyWiki
touch CLAUDE.md
```

然后在 Obsidian 中打开这个文件进行编辑（也可以用任何文本编辑器）。

### 5.2 编写结构化规则

以下是一个参考模板，基于 Karpathy 的 LLM Wiki 理念，你可以根据自己的需求调整。

将以下内容 **完整复制** 到你的 `CLAUDE.md` 文件中：

> **提示** ：在 Obsidian 中点击右上角的编辑按钮进入编辑模式，粘贴后保存即可。

```bash
# 知识库规则

## 身份
你是一个个人知识库的管理助手。你的任务是阅读原始素材，并将其转化为结构化、互相链接的知识条目。

## 目录结构
- \`raw/\`：原始素材，只读，永远不要修改
- \`wiki/\`：结构化知识条目，由你负责创建和更新
- \`CLAUDE.md\`：本规则文件

## 核心操作

### Ingest（摄入）
当我说"摄入 [文件名]"时：
1. 阅读 \`raw/\` 中指定的原始素材
2. 提取其中的关键概念、事实和见解
3. 更新 \`wiki/\` 中已有的相关条目，或创建新条目
4. 在条目之间建立双向链接 \`[[]]\`
5. 更新 \`wiki/index.md\` 内容目录
6. 在 \`wiki/log.md\` 中追加变更记录

### Query（查询）
当我提出问题时：
1. 搜索 \`wiki/\` 中所有相关条目
2. 综合多个条目的信息，给出完整的回答
3. 如果发现知识缺口，建议需要补充的素材

### Lint（检查）
当我说"检查知识库"时：
1. 检查各条目之间是否存在矛盾
2. 找出孤立的（没有其他条目链接到的）条目
3. 标记可能过时的信息
4. 报告发现的问题和建议

## 知识条目格式
每个 wiki 条目应遵循以下格式：

    ---
    created: YYYY-MM-DD
    updated: YYYY-MM-DD
    sources: [来源文件列表]
    tags: [相关标签]
    ---

    # 条目标题

    一句话概述这个概念。

    ## 详细说明

    正文内容...

    ## 相关条目
    - [[相关条目A]]
    - [[相关条目B]]

## index.md 格式
内容目录应列出所有 wiki 条目及其一句话摘要：

    ## 知识条目索引

    - [[条目A]]：一句话描述
    - [[条目B]]：一句话描述

## log.md 格式
变更日志按时间倒序排列：

    ## [YYYY-MM-DD] ingest | 来源标题
    - 新增：[[条目X]]
    - 更新：[[条目Y]]（新增关于...的内容）

## 重要原则
1. 永远不要修改 \`raw/\` 中的原始素材
2. 知识条目要简洁，用你自己的话总结，不要照搬原文
3. 积极建立条目之间的链接，形成知识网络
4. 每次摄入新素材时，要增量更新已有条目，而不是重新创建
5. 保持客观，标注信息来源
```
![](https://picx.zhimg.com/v2-da86ed377f5aa2f0c278a24eac5d6823_1440w.jpg)

> **进阶推荐** ：上面是一个简化的入门版 CLAUDE.md。强烈建议使用大神的！！！如果你想参考 Karpathy 本人编写的完整版提示词，可以查看他的 Gist：llm-wiki CLAUDE.md。他的版本包含更丰富的知识条目格式规范、lint 规则和错误恢复策略，适合在熟悉基本流程后进阶使用。

### 5.3 初始化 index.md 和 log.md

在 `wiki/` 目录中创建这两个核心文件：

```bash
cd ~/Documents/MyWiki/wiki
touch index.md log.md
```

在 `wiki/index.md` 中写入初始内容：

```bash
# 知识条目索引

> 此目录由 AI 自动维护，记录所有知识条目及其摘要。

（暂无条目）
```

在 `wiki/log.md` 中写入初始内容：

```bash
# 变更日志

> 记录知识库的所有变更。

（暂无记录）
```

## 六、使用 Claude Code 进行知识结构化

现在系统已经搭建好了，让我们实际操作一次完整的知识结构化流程。

### 6.1 准备素材

假设你通过 Web Clipper 抓取了几篇关于” [大语言模型](https://zhida.zhihu.com/search?content_id=275914070&content_type=Article&match_order=2&q=%E5%A4%A7%E8%AF%AD%E8%A8%80%E6%A8%A1%E5%9E%8B&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODc3Mzc3MTksInEiOiLlpKfor63oqIDmqKHlnosiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzU5MTQwNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MiwiemRfdG9rZW4iOm51bGx9.XdWhZGMe9b6qMNvfBQiEOe9DWm0NS5qXj9PyE2O6qv0&zhida_source=entity) ”的文章，它们都保存在 `raw/` 文件夹中。

### 6.2 启动 Claude Code 并执行摄入

```
cd ~/Documents/MyWiki
claude
```

进入 Claude Code 交互界面后，输入指令：

```bash
请摄入 raw/ 文件夹中的所有素材，按照 CLAUDE.md 中的规则进行知识结构化。
```

Claude Code 会：

1. 读取 `CLAUDE.md` 了解工作规则
2. 扫描 `raw/` 中的所有文件
3. 阅读每篇素材，提取关键知识
4. 在 `wiki/` 中创建知识条目
5. 在条目之间建立双向链接
6. 更新 `index.md` 和 `log.md`
![](https://pic2.zhimg.com/v2-fdbbdfa12629ab4e78e312ad9ac582fd_1440w.jpg)

### 6.3 查看结果

回到 Obsidian，你会发现 `wiki/` 文件夹中多了好几个文件：

```bash
wiki/
├── index.md          ← 内容目录
├── log.md            ← 变更日志
├── 大语言模型概述.md   ← 知识条目
├── Transformer架构.md ← 知识条目
├── 注意力机制.md      ← 知识条目
└── ...
```

打开任意一个条目，你会看到 AI 整理好的结构化内容，包含概述、详细说明，以及指向其他相关条目的链接。

![](https://pic4.zhimg.com/v2-bf9ca05edfcf595500c046df851c0e13_1440w.jpg)

### 6.4 使用 Obsidian 图谱视图

Obsidian 有一个非常强大的功能—— **图谱视图（Graph View）** 。点击左侧边栏的图谱图标，你可以看到所有知识条目之间的链接关系，形成一个直观的知识网络。

Karpathy 特别推荐了这个功能——当你的知识库逐渐丰富时，图谱视图会呈现出美丽的 [知识结构图](https://zhida.zhihu.com/search?content_id=275914070&content_type=Article&match_order=1&q=%E7%9F%A5%E8%AF%86%E7%BB%93%E6%9E%84%E5%9B%BE&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODc3Mzc3MTksInEiOiLnn6Xor4bnu5PmnoTlm74iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzU5MTQwNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.Sh42Z0G9236Wjt2yI76R_PQCQaPR0pqRBHroUp7gcN4&zhida_source=entity) 。

### 6.5 后续增量更新

当你通过 Web Clipper 添加了新的素材后，再次在 Claude Code 中执行摄入：

```bash
请摄入 raw/ 中新增的素材，增量更新 wiki 知识库。
```

AI 会只处理新添加的素材，并增量更新已有的知识条目，而不是推倒重来。

### 6.6 向 AI 提问

你还可以直接在 Claude Code 中提问：

```bash
请根据知识库中的内容，解释 Transformer 中的自注意力机制是什么。
```

AI 会搜索 `wiki/` 中的相关条目，综合后给出回答。

### 6.7 检查知识库健康

定期运行检查：

```bash
请检查知识库，找出矛盾、过时或孤立的条目。
```

AI 会审视整个知识库并报告问题。

## 七、安装 Claudian 插件（在 Obsidian 内使用 AI）

到目前为止，我们已经可以在命令行中使用 Claude Code 进行知识结构化。但如果你想在 Obsidian 内直接和 AI 对话，不用切换到终端，那就需要 **Claudian** 插件。

Claudian 是一个第三方 Obsidian 插件，它将 Claude Code 的能力直接嵌入到 Obsidian 中。

### 7.1 前置要求

- ✅ 已安装 Claude Code（上一节已完成）
- ✅ 已配置 API Key（上一节已完成）
- ✅ Obsidian v1.4.5 或更高版本
- ⚠️ Claudian 仅支持桌面端（macOS / Linux / Windows）

### 7.2 方法一：通过 GitHub Release 安装（推荐）

**步骤 1：下载插件文件**

访问 Claudian 的 GitHub Release 页面：

```
https://github.com/YishenTu/claudian/releases
```

下载最新版本的以下三个文件：

- `main.js`
- `manifest.json`
- `styles.css`
![](https://picx.zhimg.com/v2-523b7e507e8f151e04f8ee740d9dfa51_1440w.jpg)

**步骤 2：创建插件目录并放入文件**

```bash
# 在你的知识库中创建 claudian 插件目录
mkdir -p ~/Documents/MyWiki/.obsidian/plugins/claudian

# 将下载的文件移动到该目录
# 假设文件下载到了 ~/Downloads/ 目录
mv ~/Downloads/main.js ~/Documents/MyWiki/.obsidian/plugins/claudian/
mv ~/Downloads/manifest.json ~/Documents/MyWiki/.obsidian/plugins/claudian/
mv ~/Downloads/styles.css ~/Documents/MyWiki/.obsidian/plugins/claudian/
```

**步骤 3：在 Obsidian 中启用插件**

1. 打开 Obsidian，进入”偏好”（Preference）
2. 在左侧找到”第三方插件”（Community plugins）
3. 如果是首次使用第三方插件，需要点击”关闭安全模式”（Turn off restricted mode）
4. 在已安装插件列表中找到”claudian”，打开开关启用它
5. 点击“Browse”，可以探索其他第三方插件
![](https://picx.zhimg.com/v2-a5238b71b80ace8f0f9e819250139b09_1440w.jpg)

注意：建议用claude的api，或者openai的api，目前claudian支持的是claude和gpt的api，可以用claude code协助安装codex cli，会默认加载。

## 八、使用 Claudian 在 Obsidian 中进行 AI 问答

### 8.1 打开 Claudian 侧边栏

插件启用后，Obsidian 界面会出现 Claudian 的图标（通常在左侧边栏或右侧面板）。点击它即可打开 AI 对话界面。

![](https://pica.zhimg.com/v2-3ba6f93183589b6079779c85400964a2_1440w.jpg)

### 8.2 开始对话

在 Claudian 的输入框中，你可以直接输入问题。因为 Claudian 嵌入的是 Claude Code，它能直接读取和操作你的知识库文件。

**使用场景示例：**

```bash
请帮我总结 raw/ 中关于机器学习的所有文章要点
阅读 wiki/ 中的所有条目，找出关于"神经网络"的知识缺口
根据知识库中的内容，解释强化学习和监督学习的区别
```

### 8.3 高级功能

Claudian 还提供了一些强大的功能：

**Slash Commands（ [斜杠命令](https://zhida.zhihu.com/search?content_id=275914070&content_type=Article&match_order=1&q=%E6%96%9C%E6%9D%A0%E5%91%BD%E4%BB%A4&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODc3Mzc3MTksInEiOiLmlpzmnaDlkb3ku6QiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzU5MTQwNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.SZl2mK6DVvxd4MY5ZG3hSwxwJbl6a8EC0bBvWRa3XnQ&zhida_source=entity) ）**

在对话中输入 `/` 可以呼出快捷命令菜单，快速执行常用操作。

![](https://pic3.zhimg.com/v2-d141c5e30977951fb83fff96d55ad48c_1440w.jpg)

**@mention（引用文件）**

在对话中使用 `@` 可以引用知识库中的特定文件，让 AI 专注于处理该文件。

![](https://pic3.zhimg.com/v2-e2e3241f6a6d5341362a2bf385d4b2de_1440w.jpg)

**Plan Mode（ [规划模式](https://zhida.zhihu.com/search?content_id=275914070&content_type=Article&match_order=1&q=%E8%A7%84%E5%88%92%E6%A8%A1%E5%BC%8F&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODc3Mzc3MTksInEiOiLop4TliJLmqKHlvI8iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzU5MTQwNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.l74tzXUQAmyUBrGzEowFlaSyseiZ8xVWVmhe97cwWiw&zhida_source=entity) ）**

按 `Shift+Tab` 切换到规划模式，AI 会先制定计划再执行操作，适合复杂的批量处理。

![](https://pic3.zhimg.com/v2-1f61e1ba846703547a9eebd125218c5c_1440w.jpg)

## 九、自定义你的知识结构化提示词

Karpathy 的 LLM Wiki 提示词是一个参考模板。每个人都有自己的知识管理需求，你应该根据自己的使用场景来定制 `CLAUDE.md` 。

### 9.1 调整知识条目分类

比如，如果你是一名产品经理，你的知识库可能需要这样的分类：

```bash
## 知识条目分类

- **概念定义**：解释一个核心概念
- **案例分析**：记录真实案例及其分析
- **方法论**：可复用的方法论或框架
- **经验教训**：从实践中总结的经验
```

### 9.2 添加领域特定规则

```bash
## 领域规则

- 技术文章要提取：技术栈、架构图、性能指标
- 产品文章要提取：用户场景、问题定义、解决方案、数据指标
- 商业文章要提取：商业模式、竞争格局、关键数据
```

### 9.3 设置知识条目模板

你可以为不同类型的知识定义不同的模板：

```bash
## 技术概念模板

    ---
    created: YYYY-MM-DD
    updated: YYYY-MM-DD
    type: 技术概念
    sources: [来源]
    tags: [标签]
    ---

    # 概念名称

    ## 一句话解释
    > 用最通俗的话解释这个概念。

    ## 核心原理
    技术原理的详细说明...

    ## 应用场景
    这个技术用在哪里...

    ## 优缺点
    - 优点：...
    - 缺点：...

    ## 相关条目
    - [[相关技术A]]
    - [[相关技术B]]
```

### 9.4 让提示词持续进化

你的 `CLAUDE.md` 不是一成不变的。随着你对 [知识管理](https://zhida.zhihu.com/search?content_id=275914070&content_type=Article&match_order=5&q=%E7%9F%A5%E8%AF%86%E7%AE%A1%E7%90%86&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODc3Mzc3MTksInEiOiLnn6Xor4bnrqHnkIYiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzU5MTQwNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6NSwiemRfdG9rZW4iOm51bGx9.mNuAzebu4oDJM5GPzNe8I5KL12L28jmkGVWT6vcGJac&zhida_source=entity) 需求的深入理解，持续优化你的提示词。你可以：

- 当发现 AI 生成的条目不够好时，在规则中添加更明确的要求
- 当发现新的知识分类需求时，更新分类体系
- 当发现某些操作流程可以优化时，调整操作指令

**记住：好的提示词是迭代出来的，不是一次写就的。**

## 十、完整工作流总结

让我们把整个流程串起来，你的日常工作流如下：

```bash
┌─────────────────────────────────────────────────────────┐
│                    日常工作流                             │
│                                                         │
│  1. 浏览网页 → Web Clipper 一键保存到 raw/               │
│                     ↓                                    │
│  2. Claude Code / Claudian 执行"摄入"                    │
│     → AI 阅读 raw/ 中的新素材                            │
│     → 更新 wiki/ 中的知识条目                             │
│     → 更新 index.md 和 log.md                            │
│                     ↓                                    │
│  3. 在 Obsidian 中浏览结构化知识                          │
│     → 阅读知识条目                                       │
│     → 通过图谱视图探索知识关联                            │
│     → 通过 [[]] 双向链接在条目间跳转                     │
│                     ↓                                    │
│  4. 需要查找信息时                                       │
│     → 在 Claudian 中提问                                 │
│     → AI 基于知识库给出综合回答                           │
│                     ↓                                    │
│  5. 定期运行"检查"维护知识库健康                          │
│     → AI 检查矛盾、过时、孤立条目                        │
│     → 根据报告修复或更新                                 │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

## 十一、进阶项目推荐

掌握了基本的 LLM Wiki 工作流后，如果你想尝试更强大的工具，社区中已经有不少基于这套理论开发的 [开源项目](https://zhida.zhihu.com/search?content_id=275914070&content_type=Article&match_order=1&q=%E5%BC%80%E6%BA%90%E9%A1%B9%E7%9B%AE&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODc3Mzc3MTksInEiOiLlvIDmupDpobnnm64iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzU5MTQwNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.TVfWX-r1xjprQ-Yfyw-rRKN2YjsVDpHPAc78DXENthY&zhida_source=entity) 。以下推荐三个高星项目：

### 11.1 llm\_wiki — 桌面端知识图谱应用

nashsu/llm\_wiki 是一个跨平台桌面应用，基于 Tauri v2 构建，在 LLM Wiki 理论基础上做了大量增强：

- **两步思维链摄入** ：先用 LLM 生成结构化摘要，再提炼知识图谱节点和关系
- **四信号知识图谱** ：节点大小、颜色、位置、连线粗细分别承载不同语义
- **Louvain 社区发现** ：自动识别知识集群，发现隐藏的主题关联
- **向量语义搜索** ：基于 LanceDB 实现 [语义检索](https://zhida.zhihu.com/search?content_id=275914070&content_type=Article&match_order=1&q=%E8%AF%AD%E4%B9%89%E6%A3%80%E7%B4%A2&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODc3Mzc3MTksInEiOiLor63kuYnmo4DntKIiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzU5MTQwNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.eZZbyXt_BAr1iO8viGg5BX6OJNkyEgaCvZugwrLbDtE&zhida_source=entity) ，不只是关键词匹配
- **Chrome 剪藏扩展** ：浏览器中一键保存网页到知识库

**安装方式** （二选一）：

1. **直接下载** ：前往 Releases 页面 下载对应平台的安装包（macOS `.dmg` / Windows `.msi` / Linux `.deb` ）
2. **从源码构建** ：
```bash
git clone https://github.com/nashsu/llm_wiki.git
cd llm_wiki
npm install
npm run tauri dev
```

### 11.2 claude-obsidian — Claude Code 知识引擎插件

AgriciDaniel/claude-obsidian 是一个 Claude Code 插件，将 LLM Wiki 能力深度集成到你的笔记工作流中：

- **11 个内置技能** ：摄入、查询、检查、深度研究等，通过斜杠命令调用
- **[多智能体](https://zhida.zhihu.com/search?content_id=275914070&content_type=Article&match_order=1&q=%E5%A4%9A%E6%99%BA%E8%83%BD%E4%BD%93&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODc3Mzc3MTksInEiOiLlpJrmmbrog73kvZMiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzU5MTQwNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.PbhYga5YD7g8xhC8mBKOT810DxoFfXAlcy1iJMQeySw&zhida_source=entity) 支持** ：不同任务由专门的 Agent 处理，效果更精准
- **DragonScale Memory** ：扩展 [记忆系统](https://zhida.zhihu.com/search?content_id=275914070&content_type=Article&match_order=1&q=%E8%AE%B0%E5%BF%86%E7%B3%BB%E7%BB%9F&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODc3Mzc3MTksInEiOiLorrDlv4bns7vnu58iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzU5MTQwNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.Wvp_WEpcM02eKGw30IyK6ILsCOJWrex0tqZY3tlr890&zhida_source=entity) ，跨会话保持上下文
- **Canvas 画布** ： [可视化](https://zhida.zhihu.com/search?content_id=275914070&content_type=Article&match_order=1&q=%E5%8F%AF%E8%A7%86%E5%8C%96&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODc3Mzc3MTksInEiOiLlj6_op4bljJYiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzU5MTQwNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.AaDAkv2tRX1KUb8fytpZSMxRfi4lttmhSkoyhx0ldUs&zhida_source=entity) 知识图谱与笔记关系

**安装方式** （二选一）：

1. **一键安装** （需要 Claude Code）：
```
claude plugin install https://github.com/AgriciDaniel/claude-obsidian
```
1. **手动安装** ：
```
git clone https://github.com/AgriciDaniel/claude-obsidian.git
cd claude-obsidian
bash bin/setup-vault.sh /path/to/your/vault
```

### 11.3 llm-wiki-agent — 多平台编码智能体

SamurAIGPT/llm-wiki-agent 将 LLM Wiki 封装为一个通用编码智能体技能，不局限于特定工具：

- **多平台支持** ：兼容 Claude Code、OpenAI Codex CLI、Gemini CLI、OpenCode 等
- **无需 API Key** ：直接使用宿主工具的已有配置
- **可视化知识图谱** ：基于 vis.js 的交互式图谱展示
- **丰富的斜杠命令** ： `/ingest` 、 `/query` 、 `/lint` 等开箱即用

**安装方式** ：

```
git clone https://github.com/SamurAIGPT/llm-wiki-agent.git
cd llm-wiki-agent
# 用你喜欢的编码工具打开即可，例如：
claude
# 或 codex / opencode / gemini
```

> **提示** ：以上三个项目各有侧重——llm\_wiki 适合想要独立桌面应用和知识图谱可视化的用户，claude-obsidian 适合深度使用 Claude Code 的用户，llm-wiki-agent 适合多工具切换的用户。建议先上手基本流程，再根据需求选择进阶工具。

## 写在最后

这套系统的核心理念来自 Karpathy 的一个深刻洞察： **知识管理的本质不是存储，而是连接** 。

传统的笔记方法，每条笔记都是一座孤岛。而 LLM Wiki 模式通过 AI 把零散的信息编织成一个互联的知识网络。随着你不断添加新素材，这个网络会越来越丰富，知识之间会产生你意想不到的关联。

更重要的是，这个过程是 **增量** 的。你不需要一次性花大量时间整理笔记——每次添加新素材时，AI 都会自动将新知识融入已有的 [知识体系](https://zhida.zhihu.com/search?content_id=275914070&content_type=Article&match_order=1&q=%E7%9F%A5%E8%AF%86%E4%BD%93%E7%B3%BB&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODc3Mzc3MTksInEiOiLnn6Xor4bkvZPns7siLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzU5MTQwNzAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.tqaWobY8gAbPrENaEJ8uLvvGWfooarlPDpup5Zs671o&zhida_source=entity) 中。你的知识库会像一棵树一样，持续生长。

开始动手吧。先用 Web Clipper 收集几篇你感兴趣的文章，然后让 AI 帮你把它们变成结构化的知识。你会发现，知识管理原来可以这么轻松。

### 附录

### A. 常用终端命令速查

```bash
# 进入知识库目录
cd ~/Documents/MyWiki

# 启动 Claude Code
claude

# 在 Claude Code 中的常用指令：
# 摄入新素材
# > 请摄入 raw/ 中的新素材

# 提问
# > 请解释 xxx

# 检查知识库
# > 请检查知识库健康状况
```

### B. 推荐的 Obsidian 插件

![](https://picx.zhimg.com/v2-7587c5253b20b70f2f4f95dc9a743c31_1440w.jpg)

### C. 参考资源

- Karpathy 的 LLM Wiki 原文： `https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f`
- Claudian 插件： `https://github.com/YishenTu/claudian`
- Obsidian Web Clipper：Obsidian 官网插件页面
- Anthropic API 控制台： `https://console.anthropic.com/`
- Claude Code 文档：Anthropic 官方文档
- llm\_wiki 桌面应用： `https://github.com/nashsu/llm_wiki`
- claude-obsidian 插件： `https://github.com/AgriciDaniel/claude-obsidian`
- llm-wiki-agent 智能体： `https://github.com/SamurAIGPT/llm-wiki-agent`

编辑于 2026-05-30 23:53・北美地区

赞同 10