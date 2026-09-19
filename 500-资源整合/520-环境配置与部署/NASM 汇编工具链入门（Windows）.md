---
tags:
  - NASM
  - 汇编
  - Windows
  - MinGW
  - GoLink
  - 开发工具
  - 环境配置
date: 2026-09-10
---

# NASM 汇编工具链入门（Windows）

> NASM 是 **Netwide Assembler** 的缩写，是一款开源、跨平台的 x86/x64 汇编编译器。本文聚焦 Windows 平台的获取与配置。

---

## 一、NASM 的获取方式

NASM 不是 Windows 自带工具，系统默认没有，必须自己获取。**好消息是不需要复杂安装**，有两种方式可选：

| 方式 | 操作 | PATH 配置 | 推荐场景 |
| --- | --- | --- | --- |
| **Installer 安装包（.exe）** | 双击安装 | 安装向导自动加入 PATH | 新手首选 |
| **绿色压缩包（.zip）** | 解压即用 | 需手动把解压目录加入 PATH | 想随时删除的轻量场景 |

> 💡 NASM 本质只需要 `nasm.exe` 这一个主程序，没有复杂的依赖链。

---

## 二、下载地址与文件选择

**官网**：<https://www.nasm.us/>

进入 `DOWNLOAD` → `win64` 目录，按需选择：

- `nasm-xxx-win64-installer.exe` —— 安装版
- `nasm-xxx-win64.zip` —— 绿色解压版

> 注意文件名中的 `xxx` 是版本号，请按需选择最新稳定版。

---

## 三、验证是否配置成功

打开 CMD / PowerShell，输入：

```powershell
nasm -v
```

| 输出 | 含义 |
| --- | --- |
| 输出版本号（如 `NASM version 2.16.03`） | ✅ PATH 配置成功 |
| `nasm : 不是内部或外部命令` | ❌ PATH 没配置好，需补加解压/安装目录到环境变量 |

---

## 四、⚠️ 重要提醒：仅 NASM 不够

如果目标是写汇编代码并生成 `.exe`，**仅装 NASM 还不够**，还需要一个**链接器（linker）**。本文档默认使用的是：

- **NASM**：汇编阶段，把 `.asm` 编译成 `.obj` 目标文件
- **MinGW 的 `ld`**：链接阶段，把 `.obj` 链接成 `.exe`

> 🔗 **两者缺一不可**：NASM + MinGW 的 `ld` 共同构成最小汇编工具链。

---

## 五、替代方案

### 5.1 不想装 MinGW：使用 GoLink

**GoLink** 是小巧的 Windows 链接器，体积远小于 MinGW 整套工具链。

| 工具 | 大小 | 适用场景 |
| --- | --- | --- |
| MinGW（含 gcc / ld） | ~数百 MB | 需要完整 C/C++ 工具链 |
| **GoLink** | ~数百 KB | 仅做 NASM 链接，最小化 |

### 5.2 想跑 Hello World 但不想折腾汇编：用 C + MinGW

如果只是想快速跑出 `.exe`、不打算深入汇编：

```bash
gcc hello.c -o hello.exe
```

C 工具链比汇编 + NASM + 链接器这套组合更省心，推荐作为入门首选。

---

## 六、工具链选择速查

| 你的目标 | 推荐方案 |
| --- | --- |
| 学习 x86/x64 汇编 | NASM + MinGW `ld`（或 GoLink） |
| 只想跑出最小可执行文件 | C + MinGW（`gcc` 一行命令） |
| 既要 NASM 又不想装 MinGW | NASM + GoLink |
| 想完全无侵入、可随时删除 | NASM 绿色版 + GoLink 绿色版 |

> **建议**：先用 C + MinGW 跑通 hello world 建立信心，再按需升级到 NASM + 链接器。