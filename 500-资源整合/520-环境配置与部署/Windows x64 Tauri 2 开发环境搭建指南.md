---
tags:
  - Tauri
  - Rust
  - 环境配置
  - Windows
  - 桌面开发
date: 2026-08-07
---

# Windows x64 Tauri 2 开发环境从零搭建

> **项目阶段**：金融桌面工具开发 · 第一周任务
> **关联方案**：[[个人金融桌面工具技术选型方案]]

---

## 概述

本指南面向 Windows x64 平台，从零开始搭建 Tauri 2 + Vue 3 桌面应用开发环境。每一步均包含检查点，确认通过后再进行下一步。

---

## 步骤总览

```
步骤一：安装 Visual Studio Build Tools（C/C++ 编译环境）
步骤二：安装 Rust（编译器 + 包管理器）
步骤三：配置 Rust 国内镜像源
步骤四：安装 Node.js LTS
步骤五：安装 pnpm 包管理器
步骤六：确认 WebView2 Runtime
步骤七：创建 Tauri 2 + Vue 3 项目并运行
步骤八（可选）：加速 Rust 编译
```

---

## 步骤一：安装 Visual Studio Build Tools

Rust 在 Windows 平台编译依赖 C/C++ 编译器。

### 1.1 下载安装器

浏览器访问：
```
https://visualstudio.microsoft.com/visual-cpp-build-tools/
```

点击「下载生成工具」，获取安装器 `vs_BuildTools.exe`。

### 1.2 安装组件

运行安装器，勾选 **「使用 C++ 的桌面开发」** 工作负载，右侧确保勾选以下组件：

```
☑ MSVC v143 - VS 2022 C++ x64/x86 生成工具
☑ Windows 11 SDK（或 Windows 10 SDK，匹配当前系统）
☑ C++ ATL（最新版）
```

点击「安装」。下载量约 2-4 GB，安装耗时约 5-10 分钟。

### ✅ 检查点

打开 PowerShell 执行：

```powershell
cl
```

输出类似以下内容表示安装成功（按 `Ctrl+C` 退出）：

```
用于 x86 的 Microsoft (R) C/C++ 优化编译器版本...
```

---

## 步骤二：安装 Rust

### 2.1 下载 rustup

浏览器访问：
```
https://www.rust-lang.org/tools/install
```

下载 `rustup-init.exe`。

### 2.2 执行安装

双击运行安装程序，输入 `1` 选择默认安装（Default installation）。

安装程序将自动完成：
- 下载 Rust 编译器（`rustc`）
- 下载 Cargo 包管理器
- 配置 PATH 环境变量

### 2.3 重启终端

安装完成后，关闭所有 PowerShell / CMD 窗口，重新打开新终端。

### ✅ 检查点

```powershell
rustc --version
cargo --version
```

预期输出示例（版本号无需完全一致）：
```
rustc 1.84.0 (9fc6b4312 2025-01-07)
cargo 1.84.0 (66221abde 2024-11-19)
```

---

## 步骤三：配置 Rust 国内镜像源

国内环境必须配置，否则编译会超时或下载失败。

### 3.1 创建配置文件

在 PowerShell 中执行：

```powershell
mkdir "$env:USERPROFILE\.cargo" -ErrorAction SilentlyContinue
notepad "$env:USERPROFILE\.cargo\config.toml"
```

### 3.2 写入镜像配置

粘贴以下内容后保存关闭：

```toml
[source.crates-io]
replace-with = 'ustc'

[source.ustc]
registry = "sparse+https://mirrors.ustc.edu.cn/crates.io-index/"

[net]
git-fetch-with-cli = true
```

### ✅ 检查点

```powershell
cargo search tauri
```

返回搜索结果列表表示镜像源工作正常。按 `Ctrl+C` 中断。

---

## 步骤四：安装 Node.js

### 4.1 下载 LTS 版本

浏览器访问：
```
https://nodejs.org/
```

下载左侧 **LTS** 版本（推荐 22.x 或更新稳定版）。

### 4.2 执行安装

双击 `.msi` 安装包，全程默认选项 Next 完成。

### 4.3 重启终端

关闭所有终端窗口，重新打开。

### ✅ 检查点

```powershell
node --version
npm --version
```

预期输出示例：
```
v22.13.1
10.9.2
```

### 4.4 配置 npm 国内镜像

国内环境必须配置：

```powershell
npm config set registry https://registry.npmmirror.com
```

验证：

```powershell
npm config get registry
```

应输出 `https://registry.npmmirror.com/`。

---

## 步骤五：安装 pnpm

Tauri 官方推荐使用 pnpm 替代 npm：

```powershell
npm install -g pnpm
```

### ✅ 检查点

```powershell
pnpm --version
```

---

## 步骤六：确认 WebView2 Runtime

Windows 10/11 默认已预装。执行以下命令验证：

```powershell
Get-ItemProperty -Path "HKLM:\SOFTWARE\WOW6432Node\Microsoft\EdgeUpdate\Clients\{F3017226-FE2A-4295-8BEB-227D99F9F383}" -Name "pv" -ErrorAction SilentlyContinue
```

- **输出版本号**（如 `124.0.2478.80`）：已安装，跳过此步
- **无任何输出**：前往以下地址下载安装 WebView2 Runtime：
  ```
  https://developer.microsoft.com/microsoft-edge/webview2/
  ```

---

## 步骤七：创建 Tauri 2 + Vue 3 项目

### 7.1 初始化项目

```powershell
cd $env:USERPROFILE\Desktop
pnpm create tauri-app finance-manager
```

按提示依次选择：

| 选项 | 选择值 |
|------|--------|
| Project name | `finance-manager`（直接回车） |
| Identifier | `com.finance.manager`（直接回车） |
| Frontend language | TypeScript / JavaScript |
| Package manager | pnpm |
| UI template | Vue |
| UI flavor | TypeScript |

### 7.2 安装依赖

```powershell
cd finance-manager
pnpm install
```

首次安装会同步拉取前端依赖与 Rust 依赖（通过 `@tauri-apps/cli` 触发 `cargo fetch`），耗时约 3-8 分钟。

### 7.3 首次编译运行

```powershell
pnpm tauri dev
```

首次执行将触发完整 Rust 编译，耗时约 5-15 分钟，终端会输出大量编译日志，此为正常现象。

### ✅ 检查点

编译完成后自动弹出桌面窗口，显示 Tauri + Vue 默认欢迎页：

```
┌──────────────────────────────┐
│  ○ ○ ○        finance-manager │
│──────────────────────────────│
│                              │
│     Tauri + Vue              │
│                              │
│     You can edit this...     │
│                              │
└──────────────────────────────┘
```

出现此窗口 = **环境搭建 100% 成功**。关闭窗口即停止开发服务。

---

## 步骤八（可选）：加速 Rust 编译

后续开发中每次 `pnpm tauri dev` 均需编译 Rust，可通过以下方式优化。

### 8.1 使用 sccache 缓存编译产物

```powershell
cargo install sccache
```

设置环境变量（永久生效）：

```powershell
[System.Environment]::SetEnvironmentVariable("RUSTC_WRAPPER", "sccache", "User")
```

重启终端后生效。首次编译速度不变，从第二次开始未改动的 crate 直接读取缓存，编译速度显著提升。

### 8.2 关闭调试信息（开发阶段可选）

编辑 `src-tauri/Cargo.toml`，在末尾添加：

```toml
[profile.dev]
debug = 0
```

可使开发编译速度提升 30-50%，但调试时无法看到精确行号。项目稳定后改回 `debug = 1`。

---

## 环境总览

搭建完成后，开发环境应包含以下组件：

| 组件 | 说明 | 检查命令 |
|------|------|---------|
| Visual Studio Build Tools 2022 | C++ 编译能力 | `cl` |
| Rust 1.8x + Cargo | 后端编译器 + 包管理 | `rustc --version` / `cargo --version` |
| Node.js 22 LTS | 前端运行时 | `node --version` |
| pnpm | 前端包管理器 | `pnpm --version` |
| WebView2 Runtime | 桌面窗口渲染引擎 | 注册表查询 |
| Tauri CLI | 前后端桥接工具 | `pnpm tauri --version` |

---

## 日常开发流程

环境搭建完成后，后续开发仅需两步：

```powershell
cd $env:USERPROFILE\Desktop\finance-manager
pnpm tauri dev
```

### 依赖管理

| 类型 | 操作方式 |
|------|---------|
| Rust 依赖（crate） | 编辑 `src-tauri/Cargo.toml` 保存后，下次 `pnpm tauri dev` 自动下载编译；也可手动 `cargo add <包名>` |
| 前端依赖 | `pnpm add <包名>` |

---

**关联笔记：**

- [[个人金融桌面工具技术选型方案]] - 项目技术选型与架构设计
- [[Apache Maven安装指南]] - JVM 系环境配置参考
