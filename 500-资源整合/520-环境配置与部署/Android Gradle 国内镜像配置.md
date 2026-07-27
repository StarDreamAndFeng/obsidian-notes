---
tags:
  - Android
  - Gradle
  - 镜像
date: 2026-07-27
---

# Android Gradle 国内镜像配置

## 修改文件

文件位置：`gradle/wrapper/gradle-wrapper.properties`（基于项目根目录）

## 配置内容

```properties
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
distributionSha256Sum=2ab2958f2a1e51120c326cad6f385153bb11ee93b3c216c5fccebfdfbb7ec6cb
#distributionUrl=https\://services.gradle.org/distributions/gradle-9.4.1-bin.zip
networkTimeout=10000
validateDistributionUrl=true
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists

# 腾讯云镜像（推荐）
distributionUrl=https\://mirrors.cloud.tencent.com/gradle/gradle-9.4.1-bin.zip

# 阿里云镜像（注意：路径格式需根据版本调整）
#distributionUrl=https\://mirrors.aliyun.com/gradle/distributions/v9.4.1/gradle-9.4.1-bin.zip
```

## 使用说明

1. **注释原有地址**：在原始的 `distributionUrl` 前加上 `#` 注释掉
2. **启用国内镜像**：取消注释下方的国内镜像地址
3. **版本一致**：确保镜像地址中的 Gradle 版本号与原配置一致（如 `9.4.1`）

## 如何查看项目的 Gradle 版本号

### 方法一：查看 gradle-wrapper.properties 文件

打开 `gradle/wrapper/gradle-wrapper.properties`，找到 `distributionUrl` 配置项：

```properties
distributionUrl=https\://services.gradle.org/distributions/gradle-9.4.1-bin.zip
```

版本号即为 URL 中的数字部分（如 `9.4.1`）。

### 方法二：命令行查看

在项目根目录下执行：

```bash
# Windows (PowerShell)
.\gradlew --version

# macOS/Linux
./gradlew --version
```

输出示例：
```
------------------------------------------------------------
Gradle 9.4.1
------------------------------------------------------------

Build time:    2026-03-19 08:46:28 UTC
Revision:      2d6327017519d23b96af35865dc997fcb544fb40

Kotlin:        2.3.0
Groovy:        4.0.29
Ant:           Apache Ant(TM) version 1.10.15 compiled on August 25 2024
Launcher JVM:  17.0.1 (Oracle Corporation 17.0.1+12-39)
Daemon JVM:    Compatible with Java 21, any vendor, nativeImageCapable=false (from gradle/gradle-daemon-jvm.properties)
OS:            Windows 10 10.0 amd64
```

### 方法三：查看 build.gradle 文件

在根目录的 `build.gradle` 或 `build.gradle.kts` 中查找：

```groovy
// build.gradle
classpath 'com.android.tools.build:gradle:8.6.0'
```

> **注意**：这是 **Android Gradle Plugin (AGP)** 版本，不是 Gradle 本身的版本。但可以通过 AGP 版本反推所需的 Gradle 版本。

### 方法四：Android Studio 查看

1. 打开项目
2. 点击 `File` → `Project Structure`
3. 在 `Project` 选项卡中查看 `Gradle Version`

![Gradle Version](/000-仓库管理/020-附件与资源/android-project-structure-gradle-version.png)

## 镜像源对比

| 镜像源 | 地址格式 | 推荐度 |
|--------|----------|--------|
| 腾讯云 | `https://mirrors.cloud.tencent.com/gradle/gradle-{version}-bin.zip` | ⭐⭐⭐ |
| 阿里云 | `https://mirrors.aliyun.com/gradle/distributions/v{version}/gradle-{version}-bin.zip` | ⭐⭐ |

> **注意**：阿里云镜像的路径格式可能随版本变化，使用前需确认路径有效性。

## 注意事项

- 仅修改 `distributionUrl` 一项，其余配置保持不动
- 若使用阿里云镜像，需确认版本路径格式是否正确
- 修改后执行 `gradlew` 命令会自动从新地址下载 Gradle

---

**关联笔记：**

- [[351-Android开发]] - Android 开发相关笔记
