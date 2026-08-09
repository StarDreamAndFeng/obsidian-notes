---
tags:
  - Maven
  - Java
  - 环境配置
  - 安装指南
date: 2026-08-05
---

# Apache Maven 安装指南

> 翻译自官方文档：[https://maven.apache.org/install.html](https://maven.apache.org/install.html)
> 文档发布日期：2026-07-27

---

## 概述

Apache Maven 支持通过包管理器自动安装，或手动下载压缩包并配置环境变量。

---

## 前置条件

必须已安装 Java 开发工具包（JDK）。满足以下任一条件即可：

- 设置 `JAVA_HOME` 环境变量，指向 JDK 安装目录
- `java` 可执行文件位于系统 `PATH` 环境变量中

当前稳定版本 **3.9.16** 要求 JDK 8 及以上，推荐使用更新的 JDK 版本。

---

## 方式一：二进制压缩包手动安装（通用）

该方式适用于所有操作系统。

### 详细步骤

**步骤一：下载压缩包**

从官方下载页面获取 Apache Maven 二进制压缩包：
[https://maven.apache.org/download.html](https://maven.apache.org/download.html)

提供两种格式：
- `.zip` 格式：适用于 Windows
- `.tar.gz` 格式：适用于 macOS/Linux

**步骤二：解压压缩包**

选择任意目录进行解压：

```bash
# ZIP 格式（Windows / Linux）
unzip apache-maven-3.9.16-bin.zip

# TAR.GZ 格式（macOS / Linux）
tar xzvf apache-maven-3.9.16-bin.tar.gz
```

解压后生成目录：`apache-maven-3.9.16`

**步骤三：配置 PATH 环境变量**

将解压目录下的 `bin` 子目录添加到系统 `PATH` 环境变量中。

| 操作系统 | 配置位置 |
|---------|---------|
| Windows | 系统属性 → 环境变量 → PATH |
| macOS (zsh) | `~/.zshrc` |
| macOS (bash) | `~/.bash_profile` |
| Linux (bash) | `~/.bashrc` |
| Linux (zsh) | `~/.zshrc` |

**macOS / Linux 示例：**

```bash
# 在 ~/.zshrc 或 ~/.bashrc 中添加
export MAVEN_HOME=/path/to/apache-maven-3.9.16
export PATH=$MAVEN_HOME/bin:$PATH
```

**Windows PowerShell 示例：**

```powershell
# 临时设置（当前窗口有效）
$env:PATH += ";C:\path\to\apache-maven-3.9.16\bin"

# 永久设置（管理员权限）
[Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\path\to\apache-maven-3.9.16\bin", "Machine")
```

**步骤四：验证安装**

打开新的终端窗口，执行以下命令：

```bash
mvn -v
```

输出类似以下内容表示安装成功：

```
Apache Maven 3.9.16 (2bdd9fddda4b155ebf8000e807eb73fd829a51d5)
Maven home: /opt/apache-maven-3.9.16
Java version: 1.8.0_45, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_45.jdk/Contents/Home/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "mac os x", version: "10.8.5", arch: "x86_64", family: "mac"
```

安装完成。

---

## 方式二：包管理器安装

### macOS

支持 Homebrew、SDKMAN! 和 MacPorts。

#### Homebrew

```bash
brew install maven
```

#### SDKMAN!

```bash
sdk install maven
```

#### MacPorts

```bash
sudo port install maven3
```

---

### Linux

根据发行版选择对应的包管理器命令。

#### APT（Debian / Ubuntu）

```bash
sudo apt install maven
```

#### DNF（Fedora / RHEL 8+）

```bash
sudo dnf install maven
```

#### YUM（CentOS / RHEL 7）

```bash
sudo yum install maven
```

---

### Windows

支持 Chocolatey 和 Scoop。

#### Chocolatey

```powershell
choco install maven
```

#### Scoop

```powershell
scoop install main/maven
```

---

## 安装方式对比

| 方式 | 适用系统 | 优点 | 缺点 | 推荐度 |
|------|---------|------|------|--------|
| 手动解压配置 | 全平台 | 版本自由控制，无额外依赖 | 需手动配置环境变量 | ⭐⭐⭐ |
| Homebrew / MacPorts | macOS | 一键安装，自动管理 | 受限于软件源版本 | ⭐⭐⭐⭐ |
| SDKMAN! | macOS / Linux | 支持多版本切换 | 需额外安装 SDKMAN! | ⭐⭐⭐⭐ |
| APT / DNF / YUM | Linux | 系统自带，最简单 | 版本通常滞后 | ⭐⭐⭐ |
| Chocolatey / Scoop | Windows | 一键安装 | 需先安装包管理器 | ⭐⭐⭐ |

---

## 注意事项

1. **验证 JDK 版本**：安装 Maven 前，请确保 `java -version` 输出 JDK 8 及以上版本
2. **终端窗口**：修改 `PATH` 后需打开**新的终端窗口**才能生效
3. **代理配置**：公司内网环境需配置 `settings.xml` 代理以正常下载依赖
4. **本地仓库**：Maven 默认本地仓库位于 `~/.m2/repository`，可通过 `settings.xml` 修改路径

---

**关联笔记：**

- [[320-后端技术栈]] - 后端技术笔记
- [[322-Spring生态]] - Spring 框架相关笔记
