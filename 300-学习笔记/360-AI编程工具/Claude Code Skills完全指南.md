---
tags:
  - Claude Code
  - Skills
  - AI编程
  - 插件
  - 提示词工程
date: 2026-08-24
source: https://zhuanlan.zhihu.com/p/2034659438850750016
---

你有没有遇到过这种情况：每次开新对话，都要重新给 Claude 解释一遍项目背景、技术栈、代码规范……感觉像在教一个每天都失忆的同事？

今天介绍的 Skills，就是彻底解决这个问题的方法。

## 一、什么是 Skills？

Skills（技能）是 Claude Code 的能力扩展包。

你可以把 Claude Code 想象成一个全能但有点泛的通才工程师。他什么都会一点，但在某些专业领域——比如 Spring Boot 安全最佳实践、Go 语言惯用法、TDD 测试驱动开发——可能不够深入。

而 Skills，就是给他额外报的「专业培训课」。

安装一个 `go-review` Skill，他就懂 Go 的最佳实践了；安装 `claude-mem`，他就拥有了跨会话的项目记忆，不再每次都是「初来乍到」。

### Skills 能做什么？

- 代码审查与测试驱动开发（TDD）
- 项目上下文记忆，跨会话保持一致
- 特定语言/框架的最佳实践（Spring Boot、Django、Go、Swift 等）
- 部署、数据库迁移、安全扫描等工程化能力
- 文档处理、内容创作、市场研究等通用能力

## 二、Skills vs 传统提示词：为什么不直接写 Prompt？

很多人会问：「我直接写个长提示词不就行了？」

这个问题很好。我们用一个类比来回答：

- **传统提示词就像便利贴。** 你把需要注意的事项写在便利贴上，每次开会前贴到额头上提醒自己。但新会议开始，便利贴就丢了，下次还得重新写。
- **Skills 就像工作手册。** 写一次，放在固定的地方，任何时候打开就有，还能让整个团队共享同一套规范。

| 对比项 | 传统提示词 | Skills |
| --- | --- | --- |
| 复用性 | 每次都要复制粘贴 | 安装一次，永久可用 |
| 跨会话 | 新会话需要重新提供 | 自动加载，始终一致 |
| 触发方式 | 手动输入完整提示词 | 自动感知上下文，智能触发 |
| 团队协作 | 难以分享给团队 | 可打包成插件，一键安装 |
| 版本管理 | 无法追踪变更 | 支持 Git 版本控制 |

**结论：** 临时用一次？写提示词就够了。长期用、团队用？就该用 Skills。

## 三、搞清楚三个核心概念

在安装之前，必须先分清这三个经常混用的词，否则你会装错、找不到、卸载不掉。

### Skill（技能）—— 最小能力单元

一个 Skill 就是一个具体的专业能力，比如「Go 代码审查」或「跨会话记忆」。

必须包含 `SKILL.md` 文件（全大写！），这是 Claude 识别并激活它的关键。

### Plugin（插件）—— 安装的最小单位

插件相当于一个「礼盒」，里面可以装一个或多个 Skills，还可以包含自定义命令（Commands）和钩子（Hooks）。

通过插件市场安装时，插件是最小单位——不能只装礼盒里的某一样东西。

### Marketplace（市场）—— 插件的集合仓库

就像应用商店一样，是存放和分发插件的地方。

三者关系如下：

```text
Marketplace（市场）
└── Plugin（插件）← 安装的最小单位
    ├── plugin.json（必需）
    ├── Skill A（技能）
    │   └── SKILL.md（必需）
    ├── Skill B（技能）
    ├── Command（命令）
    └── Hook（钩子）
```

## 四、Skills 的文件结构长什么样？

最简单的 Skill，只需要一个文件夹和一个文件：

```text
~/.claude/skills/
└── my-skill/
    └── SKILL.md
```

**核心规则：**

- 文件夹名 = Skill 的唯一标识
- `SKILL.md` 必须全大写（SKILL 大写，`.md` 小写）

`SKILL.md` 长这样：

```markdown
---
name: explain-code
description: Explains code with visual diagrams and analogies
---

When explaining code, always include:
1. **Start with an analogy**
2. **Draw a diagram** using ASCII art
3. **Walk through the code** step-by-step
4. **Highlight a gotcha** or common mistake
```

**关键点：** `description` 是灵魂——Claude 靠这段描述来判断「什么时候该激活这个 Skill」。描述写得清不清楚，直接决定 Skill 能不能被正确触发。

## 五、Skills 怎么触发？不用你管！

这是很多人没想到的：大部分 Skills 安装后会自动激活，你不需要手动输入命令。

Claude Code 会实时分析你的对话内容、打开的文件类型、项目结构，然后智能判断该调用哪个 Skill。

| 场景 | 自动激活的 Skill |
| --- | --- |
| 打开 Go 项目并问代码问题 | `go-review`、`golang-patterns` 自动提供最佳实践 |
| 粘贴一段报错堆栈 | 调试相关 Skills 自动分析根因 |
| 修改代码后说「帮我提交」 | Git 集成 Skills 自动生成 commit message |
| 在 Spring Boot 项目加新功能 | `springboot-patterns` 自动提供架构指导 |
| 上传 PDF 并问「提取关键信息」 | 文档处理 Skills 自动解析内容 |

只有少数需要手动触发，比如自定义命令型（`/build-java`）或明确指令型（`/plan`、`/tdd`）。

**总结：** 安装好就忘了它，自然语言描述需求就行，Claude 会自动调用。

## 六、两种安装方式

### 方式一：插件市场安装（推荐 90% 的场景）

通过 Claude Code 官方 CLI，从插件市场一键安装。依赖、路径、更新全部自动处理。

#### 1. 官方市场（优先选择）

由 Anthropic 官方维护，每个插件可单独安装，质量有保障。

```bash
# 添加官方市场（只需做一次）
/plugin marketplace add https://github.com/anthropics/claude-plugins-official

# 安装某个插件
/plugin install code-simplifier@claude-plugins-official
```

#### 2. 社区市场

社区开发者维护，通常整个仓库就是一个大插件，安装即获得全部 Skills。

```bash
# 添加社区市场
/plugin marketplace add https://github.com/affaan-m/everything-claude-code

# 安装插件（获得全部 50+ Skills）
/plugin install everything-claude-code@everything-claude-code
```

#### 3. 独立市场

有些社区项目用两个仓库管理：一个放源码，一个是专属的市场索引。

```bash
# 先添加独立市场仓库
/plugin marketplace add https://github.com/obra/superpowers-marketplace.git

# 再安装插件
/plugin install superpowers@superpowers-marketplace
```

**三种市场对比：**

| 对比项 | 官方市场 | 社区市场 | 独立市场 |
| --- | --- | --- | --- |
| 维护方 | Anthropic | 社区开发者 | 社区开发者 |
| 能否选装单个插件 | ✅ 可以 | ❌ 全装 | ❌ 全装 |
| 质量保障 | 官方审核 | 自行判断 | 自行判断 |
| 推荐程度 | ⭐⭐⭐ 优先 | ⭐⭐ 按需 | ⭐⭐ 按需 |

### 方式二：本地手动安装（适合特殊场景）

不经过包管理体系，直接把 Skill 文件夹复制到本地目录。

```powershell
# macOS/Linux：复制到用户范围（所有项目可用）
cp -r <skill-folder> ~/.claude/skills/

# macOS/Linux：复制到项目范围（仅当前项目）
cp -r <skill-folder> ./.claude/skills/

# Windows PowerShell（用户范围）
Copy-Item -Recurse ".\skill-folder" "$env:USERPROFILE\.claude\skills\"

# 安装完成后，重启 Claude Code
```

**适合手动安装的场景：**

- 未上架插件市场的 Skills
- 涉及敏感密钥、需要完全本地运行的 Skills
- 需要深度修改 Skill 源码的场景
- 完全离线的开发环境

## 七、热门 Skills 推荐（精选）

### 🥇 everything-claude-code（必装）

> GitHub: `affaan-m/everything-claude-code` | ⭐ 41k+

Anthropic 黑客松冠军项目，奖金 $15,000，打磨了 10 个月。

**一句话评价：**「把 Claude Code 从聊天机器人变成资深工程师」。

**核心能力：**

- 50+ Skills 覆盖全开发流程
- 热门命令：`/tdd`、`/plan`、`/go-review`、`/security-review`
- 框架全覆盖：Spring Boot、Django、Go、Python、Swift、React/Next.js
- 工程化：部署、数据库迁移、安全扫描、Docker、E2E 测试

```bash
/plugin marketplace add https://github.com/affaan-m/everything-claude-code
/plugin install everything-claude-code@everything-claude-code
```

### 🥈 claude-mem（必装）

> GitHub: `thedotmack/claude-mem` | ⭐ 16k+（2 个月从 5k 涨到 22k）

**一句话评价：**「解决了 Claude 的金鱼记忆问题」。

你有没有遇到过：跟 Claude 聊了很久，建立了完整的项目上下文，结果第二天开新对话，它又什么都不知道了？这个 Skill 就是解决这个问题的。

**核心能力：**

- 跨会话记忆项目上下文、业务规则、历史修改
- 智能渐进式披露，按需检索相关记忆
- 自动压缩记忆内容，节省 Token

```bash
/plugin marketplace add https://github.com/thedotmack/claude-mem
/plugin install claude-mem@claude-mem
```

### 🥉 superpowers（必装）

> GitHub: `obra/superpowers` | ⭐ 22k+

由 Claude Code 核心贡献者 Jesse Vincent 开发。

**一句话评价：**「让 AI 像资深工程师一样工作，而不是只会跑起来的实习生」。

**核心能力：**

- TDD 测试驱动开发
- 结构化调试方法论
- 自动代码审查（问题按 Minor/Normal/Critical 分类）
- 编写实施计划、完成前验证

```bash
/plugin marketplace add https://github.com/obra/superpowers-marketplace.git
/plugin install superpowers@superpowers-marketplace
```

### 官方插件（按需选择）

首次使用官方市场，先添加一次：

```bash
/plugin marketplace add https://github.com/anthropics/claude-plugins-official
```

| 插件名称 | 说明 | 安装命令 |
| --- | --- | --- |
| code-simplifier | 自动识别冗余代码、简化复杂逻辑 | `/plugin install code-simplifier@claude-plugins-official` |
| skill-creator | 根据需求自动生成自定义 Skill | `/plugin install skill-creator@claude-plugins-official` |
| code-review | 代码审查，提供规范检查和改进建议 | `/plugin install code-review@claude-plugins-official` |
| security-guidance | 安全编码最佳实践 | `/plugin install security-guidance@claude-plugins-official` |
| frontend-design | 前端设计辅助，生成 UI 组件 | `/plugin install frontend-design@claude-plugins-official` |

### 国内用户推荐：baoyu-skills

> GitHub: `JimLiu/baoyu-skills` | 用户数 50k+，宝玉老师出品

如果你是内容创作者或国内用户，这个更适合你：

**核心能力：** 封面图生成、文章配图、幻灯片生成、推文文案、SEO 优化、Excel 数据分析

```bash
/plugin marketplace add https://github.com/JimLiu/baoyu-skills
```

### ⚠️ 冲突警告：planning-with-files

如果你已经安装了 `everything-claude-code`，不要再装 `planning-with-files`。

两者的 `/plan` 命令会直接冲突。`everything-claude-code` 里已经包含了更完善的 `/plan` 功能。

## 八、常见问题解答

### Q：Skills 安装后不生效？

1. 重启 Claude Code——新增 Skills 必须重启才能生效
2. 检查文件路径——确保路径正确，如 `~/.claude/skills/<skill-name>/SKILL.md`
3. 检查文件名——必须是 `SKILL.md`（全大写 + `.md` 小写）
4. 检查插件状态——输入 `/plugin` 查看是否已启用

### Q：如何查看已安装的 Skills？

```bash
# 查看插件市场安装的插件
/plugin

# 查看手动安装的 Skills（macOS/Linux）
ls ~/.claude/skills/

# 查看手动安装的 Skills（Windows）
dir "$env:USERPROFILE\.claude\skills\"
```

### Q：能只安装大插件里的某一个 Skill 吗？

- 通过插件市场安装：**不能**，插件是安装的最小单位
- 通过本地手动安装：**可以**，从仓库中单独复制该 Skill 文件夹到 `~/.claude/skills/`

### Q：Windows 添加插件市场失败？

- 必须使用 HTTPS 地址，不要用 SSH 格式（`git@github.com:xxx`）
- 配置 Git 代理：`git config --global http.proxy http://127.0.0.1:端口`
- 或者选择本地手动安装方式

## 九、总结：新手应该怎么装？

### 按优先级安装

| 优先级 | 插件 | 理由 |
| --- | --- | --- |
| 🥇 第一 | everything-claude-code | 50+ Skills 全覆盖，必装 |
| 🥈 第二 | claude-mem | 解决记忆问题，长期项目必备 |
| 🥉 第三 | superpowers | TDD + 代码审查，提升代码质量 |
| 4 | code-simplifier（官方） | 代码简化重构 |
| 5 | skill-creator（官方） | 创建自定义技能 |

### 按场景推荐

| 场景 | 推荐插件 |
| --- | --- |
| 大型项目开发 | everything-claude-code + claude-mem + superpowers |
| 追求代码质量 | superpowers + code-simplifier |
| 国内内容创作 | baoyu-skills |
| 离线环境 | CCPlugins（手动安装） |

---

安装完成后，你只需要用自然语言描述你的需求就好了。Claude Code 会自动判断调用哪些 Skills。不需要记命令，不需要手动触发，就像给一个老同事发消息——他懂你在说什么。

> 来源：[Claude Code Skills 完全指南（2026 最新版）](https://zhuanlan.zhihu.com/p/2034659438850750016)
