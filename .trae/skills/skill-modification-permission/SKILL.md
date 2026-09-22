---
name: skill-modification-permission
description: 技能变更权限控制。本仓库 .trae/skills/** 下任何 SKILL 文档（SKILL.md / references/*.md）的增删改、新建子目录、移动文件，必须先经用户明确授权；未授权时一律不得改动。在以下场景前调用本 skill：1) 用户明确说"修改/添加/新建/删除 SKILL"；2) 用户说"写入 SKILL"或"更新规范文档"；3) AI 准备 Write/Edit/Delete 任何 .trae/skills 下文件前自检。Use when the user asks to modify a SKILL doc, create/update a project skill, change rules under .trae/skills/, or when the assistant considers proactive edits to SKILL.md. Not for ordinary code changes under src/.
---

# 技能变更权限控制

## 核心规则（必须遵守）

**未经用户明确授权，不得对 `.trae/skills/**` 下任何文件做 Create / Edit / Delete / Move 操作。**

- "明确授权"是指用户当前消息或上一条消息中**显式出现**以下动词之一：
  - "修改 SKILL"、"更新 SKILL"、"写入 SKILL"、"写入规范"、"添加 SKILL"、"新建 SKILL"、"删除 SKILL"、"扩展 SKILL"、"完善 SKILL"、"在 SKILL 里加 XX 规则"
- 仅当用户引用 `.trae/skills/**` 下的文件路径、或使用上述动词时，才视为已授权。
- 用户只说"按规范重构"、"接入通用查询组件"等**业务指令**，**不构成**对 SKILL 文档的修改授权。
- 用户说"以后别动 SKILL"、"不要写 SKILL"等**禁止**表述时，必须立即停下，并提示用户：如确需修改请重新授权。

## 触发自检时机

凡 AI 准备执行以下任一动作前，必须先在脑内走"授权检查"流程：

1. `Write` / `Edit` / `DeleteFile` 工具作用于 `.trae/skills/` 路径
2. `RunCommand` 执行 `mkdir` / `mv` / `rm` / `cp` 操作涉及 `.trae/skills/`
3. 在回复中以"我先把这条规则写入 SKILL"等口吻，准备把对话中的新规则沉淀到 SKILL

## 授权检查 3 步

1. **定位证据**：用户最近 3 条消息内是否包含上述授权动词？
2. **匹配路径**：用户提到的是哪一个 SKILL（或全新）？是否给定了文件路径？
3. **判定**：
   - ✅ 有授权 → 直接执行，并在最终回复里再次复述改了什么，让用户复核
   - ❌ 无授权 → **不要执行写入**，改用以下方式之一：
     - 在回复中**给出建议改法**，让用户决定是否落地（推荐）
     - 用 `AskUserQuestion` 询问"是否要将 XX 写入 SKILL？"

## 反例（禁止行为）

- 用户："按 6 点规范重构这个列表页面"
  → AI 在改完业务代码后**主动**追加一段 SKILL 变更 → ❌ 违规。6 点规范本身是已有 SKILL，本次只是按其执行，不需要改动 SKILL。
- 用户："这里改成 enable-filter-config=false"
  → AI 在改完业务代码后**顺手**把这条规则追加到 SKILL → ❌ 违规。需询问"是否要把这条规则沉淀到 SKILL？"。
- 用户："接入通用查询组件"
  → AI 创建了一个新 SKILL 文件 → ❌ 违规。新建 SKILL 必须显式授权。

## 正例（允许行为）

- 用户："在 SKILL 里加一条 enable-filter-config 的规则"
  → ✅ 直接 Edit 当前 SKILL，按用户要求落地，并在回复里说明改动位置。
- 用户："新建一个关于权限控制的 SKILL"
  → ✅ 直接 Write 新文件，路径经用户确认或沿用本 SKILL 同名。
- 用户："改一下 references/vxe-table-snippet.md"
  → ✅ 直接 Edit 指定文件。

## 与其他 SKILL 的协作

- 本 SKILL 是**元规则**，覆盖所有业务 SKILL（包括 `erp-list-page-refactor`、`small-diff`、`collapsible-tree-table` 等）。
- 业务 SKILL 描述中可引用本规则以提醒 AI 自检，例如："本规范文档（SKILL.md）按 `skill-modification-permission` 规则，需用户授权后才能修改。"

## 例外

- 用户授权"修改 SKILL"后，AI 在落地过程中为保持最小改动做的**必要编辑**（如新增一行规则、修正拼写），无需再次授权。
- 但若用户授权后**顺手**做超出授权范围的改动（如授权改 SKILL.md 却被同时修改了 references/ 下的多个文件），需在回复中明确列出，由用户决定是否回滚。
