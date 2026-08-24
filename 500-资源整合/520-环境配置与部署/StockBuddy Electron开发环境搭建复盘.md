---
tags:
  - Electron
  - pnpm
  - 环境配置
  - Windows
  - 桌面开发
  - better-sqlite3
date: 2026-08-24
---

# StockBuddy 开发环境搭建复盘

> 记录从 `pnpm install` 到 `pnpm dev` 成功弹出 Electron 窗口的全过程，踩过的坑、根因、最终解决方案都沉淀在这里。
> 适用版本：StockBuddy `0.8.0`，Electron `43.1.0`，Windows 11，pnpm + Node 22。

---

## 1. 概述

`README.md` 里快速开始只写了两行：

```bash
pnpm install
pnpm dev
```

但实际在 Windows 上按这两步走，**会连续踩到 5 个坑**才能让 Electron 窗口弹出来。本文档按踩坑顺序复盘，每个坑给出：现象 / 根因 / 修复。

---

## 2. 环境要求

| 工具 | 版本要求 | 用途 | 备注 |
|------|---------|------|------|
| Node.js | ≥ 22.12 | 跑 pnpm / vite / electron | Electron 43 内置 Node 22 |
| pnpm | ≥ 9 | 包管理 | workspace 隔离 native modules |
| Python | 3.12 (推荐) | 编译 better-sqlite3 的 C++ 源码 | 仅在 prebuild 命中失败时需要 |
| Visual Studio Build Tools | 2022, "使用 C++ 的桌面开发" | 提供 MSVC `cl.exe` 与 Windows SDK | 同上 |
| uv | 0.9+ (可选) | 管理本地 Python，比 pyenv / 手动安装更轻量 | 仅推荐，**非必须** |
| Git | 任意 | 拉代码 | - |

### 2.1 推荐安装顺序

```powershell
# 1) Node（用 nvm-windows 或直接装 LTS 22）
winget install -e --id OpenJS.NodeJS.LTS

# 2) pnpm
npm install -g pnpm

# 3) uv（可选，用于管理 Python）
winget install -e --id astral-sh.uv

# 4) 用 uv 装一份 Python
uv python install 3.12

# 5) 拿到 Python 完整路径（待会儿要塞到 .npmrc）
uv python find 3.12

# 6) Visual Studio Build Tools（仅在 prebuild 兜底编译时需要）
#    https://visualstudio.microsoft.com/visual-cpp-build-tools/
#    勾选「使用 C++ 的桌面开发」
```

---

## 3. 问题复盘

### 3.1 坑① `pnpm dev` 只开了浏览器，Electron 窗口没弹出

**现象**

```
pnpm install     # OK
pnpm dev         # 终端起 Vite，但 electron 没启动
```

**根因**

`package.json` 里 `dev:electron` 原本是：

```json
"dev:electron": "wait-on tcp:5173 && tsc -p tsconfig.node.json --watch --preserveWatchOutput & wait-on dist-electron/electron/main.js && electron ."
```

中间那个 `&` 在 **macOS / Linux bash** 是「把前面丢到后台」；但在 **Windows cmd.exe**（npm 默认调用的 shell）里，`&` 是**命令分隔符**，按顺序同步执行。于是实际执行效果是：

1. `wait-on tcp:5173 && tsc --watch` 一直跑（因为 `--watch` 永不结束）
2. 后面的 `wait-on ... && electron .` 永远等不到执行

所以 Vite 起得来，`electron .` 这一行压根没运行。

**修复**

把 `dev:electron` 拆成两个子任务，用 `concurrently` 并行调度，跨平台一致：

```json
{
  "dev:renderer": "vite --host 127.0.0.1",
  "dev:electron:tsc": "wait-on tcp:5173 && tsc -p tsconfig.node.json --watch --preserveWatchOutput",
  "dev:electron:wait": "wait-on dist-electron/electron/main.js && electron .",
  "dev:electron": "concurrently -k \"npm:dev:electron:tsc\" \"npm:dev:electron:wait\"",
  "dev": "concurrently -k \"npm:dev:renderer\" \"npm:dev:electron\""
}
```

`concurrently` 本身就是跨平台的，等于把「后台执行」的能力从 shell 抽到了 Node 工具。

---

### 3.2 坑② Electron 二进制下载失败 `TypeError: fetch failed`

**现象**

```
[dev:electron:wait] Downloading Electron binary...
[dev:electron:wait] TypeError: fetch failed
[dev:electron:wait] Error: Electron failed to install correctly.
```

**根因**

`electron` 这个 npm 包在 `postinstall` 阶段会通过 `@electron/get` 去 `https://github.com/electron/electron/releases` 拉 Electron 二进制。这个 URL 在国内访问经常超时或被墙。

`electron-builder.config.cjs` 里虽然配了：

```js
electronDownload: {
  mirror: process.env.ELECTRON_MIRROR || 'https://npmmirror.com/mirrors/electron/',
},
```

但**这条配置只在 `electron-builder` 打安装包时生效**，对 `electron` 包的 `postinstall` 完全不起作用——两边是两套不同的下载器。

**修复**

在仓库根目录新建 `.npmrc`：

```ini
ELECTRON_MIRROR=https://npmmirror.com/mirrors/electron/
npm_config_dist_url=https://npmmirror.com/mirrors/electron/
```

`@electron/get`（v5+）会读这两个环境变量。`.npmrc` 在 `pnpm install` 触发 `postinstall` 时会自动注入到子进程 env。

---

### 3.3 坑③ `better-sqlite3` 报错 `Could not locate the bindings file`

**现象**

```
Error: Could not locate the bindings file. Tried:
  → .../better-sqlite3/build/Release/better_sqlite3.node
  → .../better-sqlite3/lib/binding/node-v148-win32-x64/better_sqlite3.node
  ...
```

**根因**

`better-sqlite3` 是 **N-API / 原生 C++ 模块**，必须编译出 `.node` 二进制。问题分两层：

1. **没有产物**：看 `node_modules/.pnpm/better-sqlite3@12.11.1/node_modules/better-sqlite3/` 下面**根本没有 `build/` 目录**。说明 `pnpm install` 阶段既没下载到预编译、也没编译成功。
2. **ABI 必须绑 Electron**：即使下载到 Node 22 的预编译，也得用 `prebuild-install --runtime=electron --target=<electron版本>` 重拉匹配 Electron ABI 的版本，或者用 `node-gyp` 针对 Electron 的 V8 头重新编译。

**修复思路（两个阶段）**

第一阶段：让 pnpm 知道要按 Electron ABI 来重编

在 `.npmrc` 继续追加：

```ini
npm_config_runtime=electron
npm_config_target=43.1.0
```

这两个变量会被 `prebuild-install` 和 `node-gyp` 读取。

第二阶段：用官方子命令强制重编

```powershell
pnpm exec electron-builder install-app-deps
```

`electron-builder install-app-deps` 是 Electron 生态里「重新编译 native modules」的标准入口，内部调 `@electron/rebuild`，会自动：

- 读 `package.json` 里 `electron: "43.1.0"` 选 ABI
- 读 `.npmrc` 里 `npm_config_python` 喂 Python
- 扫所有声明了 `binary` 字段的 native modules 全部重编

---

### 3.4 坑④ `Could not find any Python installation to use`

**现象**

```
• preparing       moduleName=better-sqlite3 arch=x64
⨯ Error: Could not find any Python installation to use
    at PythonFinder.fail (.../node-gyp/lib/find-python.js:300:11)
```

**根因**

Electron 43 在 `better-sqlite3@12.11.1` 的预编译列表里没命中（prebuild 没有 `electron-v43.1.0-win32-x64` 这个组合），于是回落到 `node-gyp` 从源码编译，需要：

1. Python（`PYTHON` 或 `npm_config_python` 环境变量指向）
2. Visual Studio Build Tools 提供的 `cl.exe` / Windows SDK

`better-sqlite3` 自带 `prebuild-install` 但有时候确实匹配不到 Electron 的 ABI。

**修复**

#### 方案一：用 uv 管理 Python（推荐）

```powershell
# 1) 装一份 Python
uv python install 3.12

# 2) 拿到绝对路径
$py = uv python find 3.12

# 3) 写入 .npmrc
"npm_config_python=$py" | Out-File -Append -Encoding utf8 .npmrc
```

执行后 `.npmrc` 末尾会多出一行：

```ini
npm_config_python=C:\Users\Administrator\AppData\Roaming\uv\python\cpython-3.12.12-windows-x86_64-none\python.exe
```

注意：**写完后必须重开 PowerShell**，让 pnpm / npm 重新读 `.npmrc`。

#### 方案二：装 Python（传统方式）

```powershell
winget install -e --id Python.Python.3.12 --scope user
# 重开 PowerShell
python --version
```

#### 同时确认 Visual Studio Build Tools 已就位

```powershell
where.exe cl.exe
# 期望返回 ...\VC\Tools\MSVC\<version>\bin\Hostx64\x64\cl.exe
```

如果返回空，去装 [VS Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/)，勾「使用 C++ 的桌面开发」。

---

### 3.5 坑⑤ `pnpm rebuild --force` 不支持，`electron-rebuild` 找不到

**现象 A**

```
pnpm rebuild better-sqlite3 --runtime=electron --target=43.1.0 --dist-url=... --force
ERROR  Unknown options: 'runtime', 'target', 'dist-url', 'force'
```

**现象 B**

```
pnpm exec electron-builder -f -w better-sqlite3 --version 43.1.0
26.15.3
# （只打印版本号，啥也没干）
```

**现象 C**

```
pnpm exec electron-rebuild -f -w better-sqlite3 --version 43.1.0
'electron-rebuild' 不是内部或外部命令
```

**根因**

| 错误 | 真实原因 |
|------|---------|
| `pnpm rebuild --force` | `pnpm rebuild` 子命令本身就**没有** `--force`、也没有 `--runtime` 这些参数；这些参数是给 `prebuild-install` / `node-gyp` 读的，得通过 `npm_config_*` 环境变量传入 |
| `electron-builder` 不响应 `-f -w` | `electron-builder` 是有**子命令**的 CLI（`build` / `pack` / `install-app-deps` 等），不是 flag 驱动；乱传 flag 它会拒绝并回退到只打印版本 |
| `electron-rebuild` 命令找不到 | `@electron/rebuild` 已经被 `electron-builder` 收编，没有单独的 `electron-rebuild` 可执行文件 |

**最终修复**：直接用 `electron-builder` 的官方子命令

```powershell
pnpm exec electron-builder install-app-deps
```

这条命令会：

1. 调 `@electron/rebuild` 内部 API
2. 自动读 `package.json` 的 `electron: "43.1.0"`
3. 自动读 `.npmrc` 的 `npm_config_python`、`npm_config_runtime` 等
4. 把所有 native modules 都按 Electron 43 的 ABI 重编

如果之前 `better-sqlite3` 装得"半成品"导致 pnpm 跳过重编，可以**先彻底删干净再装**：

```powershell
Remove-Item -Recurse -Force node_modules\.pnpm\better-sqlite3@12.11.1
pnpm install
pnpm exec electron-builder install-app-deps
```

---

## 4. 最终正确的开发启动流程

### 4.1 一次性配置（每个新机器 / 新 clone 项目都要做）

```powershell
# 1) 装 uv（如果你已经有 Python 可跳过 2-3 步）
winget install -e --id astral-sh.uv

# 2) 用 uv 装 Python
uv python install 3.12

# 3) 写好 .npmrc（见下方第 5 节）
#    关键是四个变量：ELECTRON_MIRROR / npm_config_python / npm_config_runtime / npm_config_target

# 4) 装 Visual Studio Build Tools（如还没装）

# 5) 首次拉依赖 + 触发 native modules 编译
pnpm install
pnpm exec electron-builder install-app-deps
```

### 4.2 日常开发

```powershell
pnpm dev
```

预期：

1. Vite 启 Renderer（端口 5173）
2. `tsc --watch` 编译 `electron/` 目录
3. `wait-on` 检测到 `dist-electron/electron/main.js` 后调起 `electron .`
4. **Electron 窗口弹出**，对话投研主页正常渲染
5. 终端不再有 bindings / Python / Unknown option 这三类错误

---

## 5. 关键配置文件

### 5.1 `.npmrc`（仓库根目录）

```ini
# Electron 二进制下载走国内镜像（@electron/get 与 electron-builder 共用）
ELECTRON_MIRROR=https://npmmirror.com/mirrors/electron/

# 喂给 node-gyp / prebuild-install 的 Python 路径（uv 管理的版本）
npm_config_python=C:\Users\Administrator\AppData\Roaming\uv\python\cpython-3.12.12-windows-x86_64-none\python.exe

# 让 native modules 预编译 / 编译针对 Electron ABI
npm_config_runtime=electron
npm_config_target=43.1.0
npm_config_dist_url=https://npmmirror.com/mirrors/electron/
```

> **注意事项**
> - `npm_config_python` 路径在每台机器上不同，`uv python find 3.12` 重新生成。
> - `npm_config_target` 跟着 `package.json` 里 `electron` 字段版本走，升级 Electron 时同步改。

### 5.2 `package.json` 的 `dev` 相关脚本

```json
{
  "dev": "concurrently -k \"npm:dev:renderer\" \"npm:dev:electron\"",
  "dev:renderer": "vite --host 127.0.0.1",
  "dev:electron:tsc": "wait-on tcp:5173 && tsc -p tsconfig.node.json --watch --preserveWatchOutput",
  "dev:electron:wait": "wait-on dist-electron/electron/main.js && electron .",
  "dev:electron": "concurrently -k \"npm:dev:electron:tsc\" \"npm:dev:electron:wait\""
}
```

---

## 6. 经验教训 / 后续优化建议

### 6.1 把 native modules 重编封进 `postinstall`

把"装完依赖自动重编 native modules"封成 npm 钩子，**新 clone 项目的队友只需要 `pnpm install` 一条命令**就能搞定：

```json
{
  "scripts": {
    "postinstall": "pnpm rebuild && pnpm exec electron-builder install-app-deps",
    ...
  }
}
```

或者更克制（不每次都强制 rebuild）：

```json
{
  "scripts": {
    "postinstall": "pnpm exec electron-builder install-app-deps",
    "native rebuild": "pnpm rebuild && pnpm exec electron-builder install-app-deps"
  }
}
```

### 6.2 `electron-builder.config.cjs` 已经预设了 mirror，可以再补一句

```js
electronDownload: {
  mirror: process.env.ELECTRON_MIRROR || 'https://npmmirror.com/mirrors/electron/',
},
```

这部分打包阶段是没问题的，无需修改。**关键还是 `.npmrc`**。

### 6.3 `pnpm` 不会自动重跑已成功的 `postinstall`

如果改了 `.npmrc` 想立刻生效，要么：

- `Remove-Item -Recurse -Force node_modules\.pnpm\<pkg>@<version>` 后再 `pnpm install`
- 或者用 `pnpm rebuild` 触发重新编译钩子

### 6.4 常见错误速查表

| 错误信息 | 对应坑 | 一句话修复 |
|---------|-------|----------|
| `pnpm dev` 只开浏览器 | 坑① | 拆 `dev:electron` 为 tsc + wait，concurrently 并行 |
| `TypeError: fetch failed`（electron 下载） | 坑② | `.npmrc` 加 `ELECTRON_MIRROR` |
| `Could not locate the bindings file` | 坑③ | `pnpm exec electron-builder install-app-deps` |
| `Could not find any Python installation` | 坑④ | `.npmrc` 加 `npm_config_python=<uv python find>` |
| `pnpm rebuild: Unknown options` | 坑⑤ | 别用 `pnpm rebuild --force`，改用 `electron-builder install-app-deps` |
| `electron-rebuild` 命令不存在 | 坑⑤ | 同上，用 `electron-builder install-app-deps` |

---

## 7. 附录：调试常用命令

```powershell
# 查看 Electron 当前拉取的 ABI 版本
pnpm exec electron --version

# 确认 .npmrc 被读到
pnpm config get npm_config_python

# 确认 better-sqlite3 产物存在
Get-Item .\node_modules\.pnpm\better-sqlite3@12.11.1\node_modules\better-sqlite3\build\Release\better_sqlite3.node

# 强制重新触发 postinstall
Remove-Item -Recurse -Force node_modules\.pnpm\better-sqlite3@12.11.1
pnpm install

# 完全干净重来（保留 lock 文件，只清 .npmpm 缓存 + dist）
Remove-Item -Recurse -Force node_modules
pnpm install
pnpm exec electron-builder install-app-deps

# 单独触发 better-sqlite3 自带的 install（终极 fallback）
cd node_modules\.pnpm\better-sqlite3@12.11.1\node_modules\better-sqlite3
node .\install.js
```

---

## 8. 变更记录

| 日期 | 版本 | 改动 |
|------|------|------|
| 2026-08-24 | 0.8.0 | 拆分 `dev:electron` 为 tsc + wait，concurrently 并行调度 |
| 2026-08-24 | 0.8.0 | 新增 `.npmrc`，统一 Electron 镜像与 Python 路径 |