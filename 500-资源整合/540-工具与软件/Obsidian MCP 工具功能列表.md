---
tags: [Obsidian, MCP, 工具]
date: 2026-07-26
---

# Obsidian MCP 工具功能列表

## 概述

Obsidian MCP（Model Context Protocol）是一个用于与 Obsidian 知识库交互的协议，可以通过代码/AI 助手直接操作 Obsidian 笔记。

## 可用功能

| 功能 | 方法名 | 说明 |
|------|--------|------|
| **列出笔记** | `list_notes` | 获取仓库中所有笔记列表 |
| **读取笔记** | `read_note` | 读取指定笔记的内容 |
| **创建笔记** | `create_note` | 创建新笔记 |
| **更新笔记** | `update_note` | 更新已有笔记内容 |
| **删除笔记** | `delete_note` | 删除笔记 |
| **搜索笔记** | `search` | 搜索笔记内容 |
| **搜索标签** | `search_tags` | 根据标签搜索笔记 |
| **搜索 frontmatter** | `search_frontmatter` | 根据 frontmatter 属性搜索笔记 |
| **创建日记** | `daily_note` | 创建当日日期格式的日记 |
| **每周笔记** | `weekly_note` | 创建本周格式的笔记 |
| **每月笔记** | `monthly_note` | 创建本月格式的笔记 |
| **追加到日记** | `append_to_daily` | 向当日日记追加内容 |
| **批量读取** | `batch_read` | 批量读取多个笔记 |
| **批量创建** | `batch_create` | 批量创建多个笔记 |
| **仓库统计** | `vault_stats` | 获取仓库统计信息 |
| **列出命令** | `list_commands` | 列出可用的 Obsidian 命令 |
| **执行命令** | `execute_command` | 执行 Obsidian 命令 |
| **获取活动笔记** | `get_active_note` | 获取当前打开的笔记 |

## 功能分类

### 笔记操作
- `list_notes` - 列出所有笔记
- `read_note` - 读取笔记
- `create_note` - 创建笔记
- `update_note` - 更新笔记
- `delete_note` - 删除笔记
- `batch_read` - 批量读取
- `batch_create` - 批量创建

### 搜索功能
- `search` - 全文搜索
- `search_tags` - 标签搜索
- `search_frontmatter` - frontmatter 搜索

### 周期性笔记
- `daily_note` - 日记
- `weekly_note` - 周笔记
- `monthly_note` - 月笔记
- `append_to_daily` - 追加到日记

### 其他功能
- `vault_stats` - 仓库统计
- `list_commands` - 列出命令
- `execute_command` - 执行命令
- `get_active_note` - 获取当前笔记

## 使用场景

1. **批量管理笔记**：通过 `batch_create`、`batch_read` 批量操作笔记
2. **自动化工作流**：使用 `execute_command` 执行 Obsidian 命令
3. **快速搜索**：通过 `search`、`search_tags` 快速定位笔记
4. **日记记录**：使用 `daily_note`、`append_to_daily` 自动创建和更新日记
5. **获取上下文**：使用 `get_active_note` 获取当前编辑的笔记

---

**关联笔记：**

- [[Obsidian 文件命名注意事项]]
- [[Obsidian 编辑与阅读模式]]
