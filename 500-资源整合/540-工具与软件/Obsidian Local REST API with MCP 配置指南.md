---
tags:
  - Obsidian
  - MCP
  - 配置
date: 2026-07-26
---

# Obsidian Local REST API with MCP 配置指南

## 概述

通过 Obsidian 的 **Local REST API** 插件，可以让外部应用（如 Trae AI 助手）直接操作 Obsidian 笔记。配合 **MCP (Model Context Protocol)** 协议，AI 助手可以读取、创建、搜索笔记，实现智能知识管理。

## 安装步骤

### 1. 在 Obsidian 中安装插件

1. 打开 Obsidian → 设置 → 第三方插件 → 浏览
2. 搜索 **"Local REST API"**
3. 安装并启用插件

### 2. 配置 Local REST API

1. 进入插件设置
2. 设置一个 **API 密钥**（记住这个密钥，后面需要用到）
3. 默认会开启两个端口：
   - **HTTP**: `http://127.0.0.1:27123`（无证书，适合本地使用）
   - **HTTPS**: `https://127.0.0.1:27124`（自签名证书）

### 3. 在 Trae 中配置 MCP

在 Trae 的 MCP 配置文件中添加：

```json
{
  "mcpServers": {
    "obsidian": {
      "url": "http://127.0.0.1:27123/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY"
      }
    }
  }
}
```

### 4. 测试连接

在 Trae 中调用 MCP 工具测试：

```powershell
# 列出文件
vault_list

# 获取标签统计
tag_list

# 获取当前打开的笔记
active_file_get_path
```

## 常见问题与解决方案

### 🚨 问题：自签名证书错误

**错误日志：**
```
SSE error: TypeError: fetch failed: self signed certificate
```

**原因：**
Obsidian 的 Local REST API 默认使用 HTTPS（端口 27124），但 SSL 证书是本地自签名的，Trae 底层的 Node.js 环境出于安全考虑会拒绝这种证书。

**解决方案：改用 HTTP 协议**

将配置中的 URL 改为 HTTP 和 27123 端口：

```json
{
  "mcpServers": {
    "obsidian": {
      "url": "http://127.0.0.1:27123/mcp",  // http + 27123
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY"
      }
    }
  }
}
```

**为什么 HTTP 安全？**
- 数据传输在本地（127.0.0.1），不会流出电脑
- 完全没有必要使用 HTTPS
- 这是最稳定、最规范的本地用法

### 💡 补充：npx 方式的区别

之前使用 `npx obsidian-notes-mcp` 方式时，配置中包含：

```json
"env": {
  "OBSIDIAN_REJECT_UNAUTHORIZED": "false"
}
```

这个环境变量会让第三方包执行 `process.env.NODE_TLS_REJECT_UNAUTHORIZED = "0"`，强行忽略证书错误。

而 Trae 原生的 SSE 连接方式没有提供"忽略 SSL 证书"的配置项，所以必须使用 HTTP 端口。

## 可用 MCP 工具

配置成功后，Trae 可以调用以下工具：

| 工具 | 说明 |
|------|------|
| `vault_list` | 列出文件 |
| `vault_read` | 读取笔记 |
| `vault_write` | 创建/覆盖笔记 |
| `vault_append` | 追加内容 |
| `vault_patch` | 部分编辑 |
| `vault_delete` | 删除文件 |
| `vault_move` | 移动文件 |
| `vault_copy` | 复制文件 |
| `vault_get_document_map` | 获取文档映射 |
| `active_file_get_path` | 获取当前打开的笔记 |
| `search_query` | 搜索 |
| `search_simple` | 简单搜索 |
| `tag_list` | 获取标签列表 |
| `command_list` | 列出命令 |
| `command_execute` | 执行命令 |
| `open_file` | 打开文件 |

## 测试结果示例

### 文件列表 (`vault_list`)
```json
{
  "files": [
    "2026-07-25.md",
    "2026-07-26.md",
    "README.md",
    "学习笔记/",
    "资料整理/",
    "images/"
  ]
}
```

### 标签统计 (`tag_list`)
```json
{
  "tags": [
    {"name": "Obsidian", "count": 4},
    {"name": "使用技巧", "count": 2},
    {"name": "Git", "count": 1}
  ]
}
```

### 当前活动笔记 (`active_file_get_path`)
```json
{
  "path": "资料整理/Obsidian MCP 工具功能列表.md"
}
```

---

**关联笔记：**

- [[Obsidian MCP 工具功能列表]]
- [[Obsidian 编辑与阅读模式]]
