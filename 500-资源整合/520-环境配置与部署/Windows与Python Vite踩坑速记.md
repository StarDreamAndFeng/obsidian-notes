---
tags:
  - Windows
  - Python
  - uv
  - PowerShell
  - Vite
  - pnpm
  - 踩坑
  - 环境配置
date: 2026-08-29
---

# Windows + Python 与 Vite 杂项踩坑速记

> 三条独立的小坑汇总：Python 静态服务器、PowerShell 文件 hash、Vite 跨平台脚本。

## 坑①：用 uv + Python 开启本地静态服务器（端口 9002）

### 操作步骤

1. 新建一个空文件夹
2. 进入文件夹，执行 `uv init`
3. 同步依赖：`uv sync`
4. 启动服务：

```powershell
uv run python -m http.server 9002
```

### 运行日志

```text
PS E:\Project\self-project\python-static-serve> uv run python -m http.server 9002
Serving HTTP on :: port 9002 (http://[::]:9002/) ...
::ffff:127.0.0.1 - - [31/Aug/2026 15:17:36] "GET /assets/latest.json HTTP/1.1" 404 -
::ffff:127.0.0.1 - - [31/Aug/2026 15:18:46] "GET /latest.json HTTP/1.1" 200 -
```

> **注意**：HTTP 路径大小写敏感。`/assets/latest.json` 返回 404，`/latest.json` 返回 200，说明请求路径必须与文件实际位置完全一致（包括目录层级）。

## 坑②：用 PowerShell 计算文件的真实 SHA256

```powershell
Get-FileHash -Algorithm SHA256 D:\path\to\xxxx
```

无需安装任何工具，PowerShell 5.1+ 自带。

## 坑③：Windows 上 `pnpm dev` 报 `'NODE_ENV' 不是内部或外部命令`

### 现象

```powershell
PS E:\Project\self-project\core> pnpm dev:admin
$ pnpm -C apps/admin run dev
$ NODE_ENV=development vite --mode development --open --host
'NODE_ENV' 不是内部或外部命令，也不是可运行的程序
或批处理文件。
```

### 根因

`package.json` 里的脚本是：

```json
"dev": "NODE_ENV=development vite --mode development --open --host"
```

Windows 的 `cmd.exe` / PowerShell **不支持** `NODE_ENV=development` 这种语法——这是 **bash / zsh** 的内联变量赋值语法。Windows 把 `NODE_ENV=development` 当成一个可执行命令去查找，自然找不到，于是报「不是内部或外部命令」。

### 修复方案

#### 方案 A：使用 `cross-env` 跨平台兼容（推荐）

```json
"dev": "cross-env NODE_ENV=development vite --mode development --open --host"
```

需先安装：

```bash
pnpm add -D cross-env
```

#### 方案 B：去掉 `NODE_ENV` 前缀（极简）

```json
"dev": "vite --mode development --open --host"
```

Vite 会自动根据 `--mode` 推断环境。

> ⚠️ **取舍**：如果项目里有第三方 npm 包（如 React 内部）直接读 `process.env.NODE_ENV` 来判断 dev/prod 行为，方案 B 会让 `NODE_ENV` 保持 `undefined`，此时需要回到方案 A。
