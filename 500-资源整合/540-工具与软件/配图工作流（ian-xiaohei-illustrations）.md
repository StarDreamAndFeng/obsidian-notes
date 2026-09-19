---
tags:
  - Obsidian
  - 配图工作流
  - xiaohei
  - Ian风格
  - 提示词工程
  - 即梦
  - AI绘画
date: 2026-08-29
skill: ian-xiaohei-illustrations
---

# 配图工作流（ian-xiaohei-illustrations）

> **使用场景**：每次想为中文笔记配图时，省去临时编 prompt 的成本。
> **默认平台**：即梦（jimeng）—— 中文 prompt 为主，控制 2000 字符内。
> **默认行为**：只生成 shot list 与可粘贴的 prompt，不调用付费生图 API。

---

## 一、适用与不适用

### ✅ 适合配图的笔记

- **观点/方法论类**：知乎剪藏、个人成长、财富认知
- **教程/流程类**：分步骤讲解、有明确结构（坑① 坑② 坑③…）
- **概念解释类**：技术概念、业务隐喻、底层原理
- **对比/决策类**：前后对比、左右取舍、A vs B
- **小漫画分镜**：失败到成功、使用前后变化

### ❌ 不适合配图的笔记

- **纯 API 参考**：参数列表、接口文档
- **代码片段合集**：零散代码块
- **数据/表格笔记**：纯统计、表格驱动
- **个人日志**：流水账、待办清单
- **超短笔记**：< 200 字，配图反而稀释文字

---

## 二、用户提供的 6 个标准输入

每次请求配图时，按下面 6 项明确说明：

| # | 要素 | 说明 | 默认值 |
| --- | --- | --- | --- |
| 1 | **哪个笔记** | 笔记路径或粘贴文本 | — |
| 2 | **要做什么** | shot list / 出图 prompt / 两者都要 | 两者都要 |
| 3 | **配多少张** | 数字 | 由我按文章长度推断（见第五节） |
| 4 | **使用平台** | 即梦 / Midjourney / SD / DALL·E / 豆包 | **即梦** |
| 5 | **prompt 内容** | shot list / 完整 prompt / 精简 prompt | **shot list + 完整 prompt** |
| 6 | **其他约束** | 配色、风格、字数限制、特殊要求 | 无 |

---

## 三、补充要素（容易被忽略）

下面这些项经常影响最终效果，请一并考虑：

### 3.1 字符限制

- 即梦：**≤ 2000 字符**（超出自动截断）
- Midjourney：≤ 6000 字符
- DALL·E / GPT-4o：≤ 4000 token
- SD（自然语言 prompt）：≤ 77 token / 512 字符效果最佳

### 3.2 图片比例

- **默认 16:9** 横版（landscape_16_9）
- 笔记封面图可考虑 3:2 或 4:3
- 文章插图首选 16:9（与正文阅读节奏一致）

### 3.3 是否需要中文标注

- **需要**：博客、知乎、公众号（文字是认知锚点）
- **不需要**：纯氛围图、海外发布、推文封面

### 3.4 留白密度偏好

- **高留白**（> 50% 空白）：克制、严肃、白板风
- **中等留白**（35-50%）：默认推荐
- **低留白**（< 35%）：信息密集，但容易变成 PPT

### 3.5 是否接受英文 prompt

- 即梦 / 豆包：中文 prompt 优先，但接受中英混合
- Midjourney / SD：英文 prompt 优先
- DALL·E / GPT-4o：英文 prompt 优先

---

## 四、Shot List 标准结构

每张图提供以下 8 项：

| 字段 | 说明 | 示例 |
| --- | --- | --- |
| 位置 | 放在哪个段落后 | 一、核心观点 末尾 |
| 主题 | 一句话概括 | RPG 起点对比 |
| 核心意思 | 这张图要表达什么 | 同一场游戏，不同装备起点 |
| 结构类型 | Workflow / 系统局部 / 前后对比 / 角色状态 / 概念隐喻 / 方法分层 / 地图路线 / 小漫画分镜 | 前后对比 |
| 小黑动作 | xiaohei 在做什么 | 赤手空拳站在空白起点 |
| 建议元素 | 1-2 个核心物件 | 一条橙色细线 + 散落小石子 |
| 中文标注 | 2-8 字的手写批注 | 「氪金玩家」「免费玩家」「同一场游戏」「没有托举」 |
| 配色方案 | 黑 / 红 / 橙 / 蓝的分配 | 黑主 + 橙线 + 红标注 |

---

## 五、数量策略

### 5.1 按文章长度

| 字数 | 推荐张数 | 备注 |
| --- | --- | --- |
| < 500 字 | 1-2 张 | 优先选最核心的认知锚点 |
| 500-2000 字 | 3-5 张 | 平均 2-3 张/章 |
| 2000-5000 字 | 5-8 张 | skill 推荐的甜点区 |
| > 5000 字 | ≤ 9 张 | **避免变成画册**，优先给关键转折点配图 |

### 5.2 不平均配图

优先选择「认知锚点」：

- ✅ 核心判断、两个断点、输入输出闭环
- ✅ 分流、前后对比、一鱼多吃
- ✅ 承接路径、常见坑、角色状态变化
- ❌ 过渡段、铺垫段、纯描述段

### 5.3 极端情况

- **只要 1 张**：选文章开头 200 字内的核心命题
- **只能 ≤ 3 张**：每个二级标题选 1 张
- **想要 ≤ 9 张**：按章节均匀分布，但保留重点

---

## 六、Prompt 模板（3 个版本）

### 6.1 完整版（约 1500 字符 · 默认）

适用于 Midjourney / DALL·E / GPT-4o / SD。

```text
Generate one standalone 16:9 horizontal Chinese article illustration.

Visual DNA:
Pure white background. Minimalist black hand-drawn line art. Slightly wobbly pen lines. Lots of empty white space. Sparse red/orange/blue handwritten Chinese annotations. Clean absurd product-sketch feeling. No gradients, no shadows, no paper texture, no complex background, no commercial vector style, no PPT infographic look, no cute mascot poster, no children's illustration, no realistic UI.

Recurring IP character required:
小黑, a small solid-black absurd creature with white dot eyes, tiny thin legs, blank serious expression, slightly uneven hand-drawn body shape. 小黑 must perform the core conceptual action, not decorate the scene. Make 小黑 serious, deadpan, and slightly bizarre, not cute.

Theme: {主题}
Structure type: {结构类型}
Core idea: {核心意思}
Composition: {构图细节}
Suggested elements: {元素1} / {元素2} / {元素3}
Chinese handwritten labels: {标注1} / {标注2} / {标注3} / {标注4}

Color use:
Black for main line art and 小黑. Orange for main flow/path/arrows. Red only for key warnings/problems/results. Blue only for secondary notes or feedback/system state.

Constraints:
One image explains only one core structure. Keep the main subject around 40-60% of canvas. Preserve at least 35% blank white space. Use at most 5-8 short handwritten Chinese labels. Do not write a title in the top-left corner. Do not write the structure type on the image. Do not make it a formal diagram, course slide, or dense explainer. Do not copy prior examples or reuse known case compositions unless explicitly requested; invent a fresh visual metaphor for this specific article.
```

### 6.2 精简版（约 700 字符 · 即梦推荐）

中文为主 + 关键英文术语。

```text
16:9 中文文章配图。纯白底，黑色细线手绘风，线条略带抖动，大面积留白。

角色：黑色实心小怪物 xiaohei（白点眼、细腿、呆愣表情、不规则手绘身体、不可爱），必须是画面核心动作主体，不能只是站着。

主题：{主题}
结构类型：{结构类型}

构图：{构图细节}

中文手写标注 4 处：
- {标注1} {颜色}
- {标注2} {颜色}
- {标注3} {颜色}
- {标注4} {颜色}

配色：黑色主线稿，{配色方案}。

要求：主体占画面 40-50%，至少 35% 纯白留白，不要左上角标题，不要 PPT 信息图风格，xiaohei 不要可爱化，一张图只讲一个对比。
```

### 6.3 极简版（约 300 字符 · 即梦兜底）

当完整版 / 精简版仍超 2000 字符时使用。

```text
16:9 中文手绘风。白色背景黑色线稿，留白多。黑色小怪物 xiaohei（白点眼细腿呆愣不可爱）{核心动作}。{其他元素描述}。{配色方案}。{中文标注清单}。
```

---

## 七、平台适配速查

| 平台 | 推荐版本 | 比例参数 | 关键提示 | 价格 / 积分 |
| --- | --- | --- | --- | --- |
| **即梦**（图片 5.0 Pro） | 精简版 | 16:9 / 3:4 / 1:1 | ≤ 2000 字符；中文为主 | **8 积分 / 张**（原价 11，约 67.3 折） |
| **Midjourney** | 完整版 | `--ar 16:9` | 末尾加 `--style raw --stylize 50` 减弱风格化 | — |
| **SD** | 完整版 | landscape_16_9 | 加负面提示词：`top-left title, infographic, cute mascot` | — |
| **DALL·E / GPT-4o** | 完整版 | 16:9 | 可加 "Hand-drawn Chinese article style" | — |
| **豆包** | 精简版 | 16:9 | 中文 prompt 即可，类似即梦 | — |

### 7.1 即梦积分速算

即梦「图片 5.0 Pro」按生成张数计费：

- **原价**：11 积分 / 张
- **优惠后**（约 67.3 折）：**8 积分 / 张**（节省 3 积分）
- 优惠活动可能随时调整，下单前以实际弹窗为准

| 配图数量 | 积分消耗（优惠后） | 适用场景 |
| --- | --- | --- |
| 1 张 | 8 积分 | 单图补充、超短笔记 |
| 3 张 | 24 积分 | 教程类、方法论 |
| 5 张 | 40 积分 | 2000-3000 字深度文 |
| 8 张（上限推荐） | 64 积分 | 长文全景配图 |

> **省钱策略**：
> - 精简版 prompt 字符少，模型理解成本低，**一次出片率高**，减少重生成次数
> - 失败信号出现时优先局部编辑（不重生成整张），能省一半积分
> - 用占位工作流（第十一节 11.2）先确定 shot list，再批量提交，避免试错成本
> - **避免一次性提交 5 张以上**（失败时全要重做，积分倍增；3 张是张均成本与失败风险的平衡点）

---

## 八、典型场景示例

### 8.1 单图场景

**文章**：「普通人的唯一主线任务」知乎剪藏
**选图**：第一章末尾的 RPG 起点对比（最具张力的认知锚点）
**取舍**：舍弃上下半场图（一张图难同时讲"时间轴"+"两种机器"）

### 8.2 三图场景

**文章**：方法论 / 教程类
**典型结构**：

1. 核心命题（概念隐喻）
2. 关键论证 ①（流程或系统）
3. 收束（角色状态或前后对比）

### 8.3 五图场景（默认推荐）

**文章**：2000-3000 字深度观点文
**典型结构**：

1. 核心观点 / RPG 类比（前后对比或概念隐喻）
2. 第一个论证（系统局部或方法分层）
3. 第二个论证（地图路线或前后对比）
4. 第三个论证（概念隐喻或角色状态）
5. 收束 / 行动呼吁（小漫画分镜或角色状态）

---

## 九、失败信号与重生成

### 9.1 必须重生成

| 失败信号 | 修复方向 |
| --- | --- |
| 左上角出现「常见坑 / Workflow / 系统架构图」标题 | 删除标题词 |
| 小黑像吉祥物 / 表情包 / 可爱卡通 | 强调 deadpan、blank serious |
| 像 PPT、课程课件、正式流程图 | 去掉边框、整齐网格、过多箭头 |
| 元素太多、箭头太多、节点太多 | 删到只保留一个动作 + 3-5 个标注 |
| 文字变成大段解释 | 标注改为短词（2-8 字） |
| 背景有纸纹、阴影、渐变、米色 | 强调纯白底 |
| 真实 UI 截图或科技感界面 | 强调手绘风、抽象化 |
| 中文错字严重 | 简化中文标注，避免生僻字 |
| 太像旧案例（传送带 / 三层信息源 / 盖章工具箱） | 换主物件 + 换小黑动作 |
| 小黑只是站在角落 | 让小黑成为动作主体 |

### 9.2 迭代策略

- **太普通**：让小黑成为动作主体，加入一个奇怪但成立的隐喻
- **太复杂**：删节点，只保留一个动作和 3-5 个短标注
- **太可爱**：强调 deadpan、blank serious、not cute、not mascot
- **太 PPT**：去掉标题、边框、整齐网格和过多箭头
- **太像旧案例**：保留核心意思，换掉主物件和小黑动作
- **文字错**：优先局部编辑；错得多就重生成并减少标注数量

---

## 十、风格 DNA 速记

### 10.1 xiaohei IP

- 黑色实心，白点眼，细腿
- 表情空、呆、冷静、认真
- 像在白板草图里真的负责某个工作
- **不能只是装饰，必须参与核心动作**

### 10.2 颜色规则

- **黑色**：主体线稿、角色、框线
- **红色**：重点批注、问题、提醒、结果
- **橙色**：主流程、路径、箭头、自动化流向
- **蓝色**：补充说明、AI/assistant、系统状态
- **蓝色不是每张都必须用**

### 10.3 构图约束

- 主体占画面 40-60%
- 至少 35% 空白，最好有一整块安静区域
- 中文标注 ≤ 5-8 处，每处 ≤ 8 字
- 一张图只讲一个核心动作 / 结构 / 隐喻
- **不要在图上写结构类型名称**

---

## 十一、与其他工具的搭配

### 11.1 完整工作流

1. **Obsidian 笔记** → 复制到对话
2. **本 skill（ian-xiaohei-illustrations）** → 生成 shot list + prompt
3. **即梦 / MJ** → 出图（手动）
4. **保存到** `000-仓库管理/020-附件与资源/<slug>-illustrations/`
5. **命名**：`01-topic-name.png` / `02-topic-name.png` ...
6. **笔记引用**：`![alt](../../../000-仓库管理/020-附件与资源/...)`

### 11.2 简化工作流（不出图）

如果不想出图、只想把图的位置和内容先标注好：

1. 在笔记中插入占位段落：
   ```markdown
   ![图片描述：{核心意思}](/000-仓库管理/020-附件与资源/placeholder.png)
   ```
2. 在占位下方附 shot list 注释
3. 后续拿到图后直接替换路径

### 11.3 跳过生图（最简方案）

如果连 prompt 都不想要，只要"哪些位置适合配图"的建议：

- 列出每张图的位置 + 核心意思（一行）
- 不出 shot list，不出 prompt
- 由用户自行决定怎么画

---

## 十二、检查清单（生成前自检）

每次生成前，按以下顺序检查：

- [ ] 笔记路径已明确
- [ ] 数量策略已与文章长度匹配
- [ ] 平台已确认（默认即梦）
- [ ] prompt 内容已确认（默认 shot list + 完整 prompt）
- [ ] prompt 字符数 ≤ 平台限制（即梦 2000）
- [ ] 8 项 shot list 字段已全部覆盖
- [ ] 小黑不是装饰，而是核心动作主体
- [ ] 颜色分配符合 黑主 + 红 / 橙 / 蓝批注
- [ ] 中文标注 ≤ 8 处，每处 ≤ 8 字
- [ ] 已避开 10 个失败信号

---

## 十三、参考链接

基于项目根目录的路径：

- **Skill 路径**：`.trae\skills\ian-xiaohei-illustrations\SKILL.md`
- **风格 DNA**：`.trae\skills\ian-xiaohei-illustrations\references\style-dna.md`
- **xiaohei IP 定义**：`.trae\skills\ian-xiaohei-illustrations\references\xiaohei-ip.md`
- **构图模式**：`.trae\skills\ian-xiaohei-illustrations\references\composition-patterns.md`
- **Prompt 模板**：`.trae\skills\ian-xiaohei-illustrations\references\prompt-template.md`
- **QA 检查清单**：`.trae\skills\ian-xiaohei-illustrations\references\qa-checklist.md`
- **示例图集**：`.trae\skills\ian-xiaohei-illustrations\assets\examples\`

> 路径前缀依本地仓库根目录而定，Obsidian 用户可在笔记库根目录的 `.trae/skills/...` 下找到。