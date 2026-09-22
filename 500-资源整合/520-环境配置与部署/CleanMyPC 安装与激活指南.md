---
tags:
  - CleanMyPC
  - Windows
  - 软件安装
  - 系统清理
date: 2026-09-22
---

# CleanMyPC 安装与激活指南

## 概述

记录 Windows 下 CleanMyPC 的下载、安装与激活完整流程。CleanMyPC 是 MacPaw 出品的 Windows 系统清理工具。

---

## 准备环境

- 官方历史版本下载页：<https://macpaw.com/download/old-versions>
- 参考教程：[CleanMyPC 安装与激活教程 - CSDN 博客](https://blog.csdn.net/weixin_53863236/article/details/123771621)

需要准备的文件：

| 文件 | 说明 |
|------|------|
| `CleanMyPC.exe` | 官方安装程序 |
| `Patch.exe` | 激活补丁文件 |

---

## 安装步骤

1. 双击 `CleanMyPC.exe` 启动安装程序。
2. 安装过程中直接单击"确定"或"下一步"即可。
3. 进入自定义安装步骤时，选择安装路径（示例路径：`D:\Program Files\CleanMyPC`）。

![自定义安装路径](/000-仓库管理/020-附件与资源/clearmypc-custom-install-path.png)

---

## 激活步骤

### 步骤一：放置补丁文件

将 `Patch.exe` 放到 CleanMyPC 安装目录下，例如 `D:\Program Files\CleanMyPC`。

### 步骤二：停止后台服务

1. 按下 `Win + R` 打开运行对话框，输入 `services.msc` 打开服务管理器。
2. 确保 CleanMyPC 的后台服务处于**停止**状态，并将启动类型改为**手动**。

![停止后台服务](/000-仓库管理/020-附件与资源/clearmypc-service-stop.png)

### 步骤三：运行补丁

双击 `Patch.exe` 完成激活。若激活失败，可尝试以管理员身份运行。

---

## 注意事项

激活过程中可能提示需要安装 **.NET Framework 3.5（包括 .NET 2.0 和 3.0）**：

1. 直接单击"下载并安装此功能"。
2. 下载耗时取决于网络速度，一般需要 3-5 分钟，请耐心等待。
3. 安装完成后，可能需要重新启动依赖该功能的应用。

![下载并安装 .NET Framework 3.5](/000-仓库管理/020-附件与资源/clearmypc-net-download.png)
