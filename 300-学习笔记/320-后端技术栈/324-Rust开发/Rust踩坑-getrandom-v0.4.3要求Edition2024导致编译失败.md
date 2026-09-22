---
tags: [踩坑记录, Rust, Cargo, getrandom, Edition2024, 版本兼容]
date: 2026-08-10
category: 后端技术
severity: 中
environment: Windows / Cargo 1.81.0 / Rust 1.81.0
---

# Rust 踩坑：getrandom v0.4.3 要求 Edition 2024 导致编译失败

## 错误日志

```
error: failed to download `getrandom v0.4.3`

Caused by:
  unable to get packages from source

Caused by:
  failed to download replaced source registry `crates-io`

Caused by:
  failed to parse manifest at `C:\Users\star-dream\.cargo\registry\src\rsproxy.cn-0dccff568467c15b\getrandom-0.4.3\Cargo.toml`

Caused by:
  feature `edition2024` is required

  The package requires the Cargo feature called `edition2024`, but that feature is not stabilized in this version of Cargo (1.81.0 (2dbb1af80 2024-08-20)).
  Consider trying a newer version of Cargo (this may require the nightly release).
  See https://doc.rust-lang.org/nightly/cargo/reference/unstable.html#edition-2024 for more information about the status of this feature.
ELIFECYCLE Command failed with exit code 101.
```

---

## 原因分析

核心错误信息：

```
feature `edition2024` is required
but that feature is not stabilized in this version of Cargo (1.81.0)
```

| 项目 | 内容 |
|:---|:---|
| 触发 crate | `getrandom v0.4.3` |
| 使用特性 | Rust Edition 2024 |
| Edition 2024 稳定版本 | Rust **1.85.0** |
| 当前 Rust 版本 | **1.81.0**（不支持 Edition 2024） |

`getrandom v0.4.3` 在 `Cargo.toml` 中声明了 `edition = "2024"`，而 Edition 2024 直到 Rust 1.85.0 才进入稳定通道。低于该版本的 Cargo 无法识别此声明，直接拒绝解析 manifest。

---

## 解决方案

### 方案一：升级 Rust 工具链（推荐）

```bash
rustup update stable
```

升级至 **1.85.0 或更高版本**即可恢复正常编译。

如需指定具体版本：

```bash
rustup install 1.85.0
rustup default 1.85.0
```

### 方案二：锁定 `getrandom` 为旧版本

当暂时无法升级 Rust 工具链时，可在 `Cargo.toml` 中强制使用兼容的旧版本：

```toml
[dependencies]
getrandom = "=0.2.15"   # 锁定为不需要 edition2024 的版本
```

或通过 `Cargo.lock` 约束，确保不拉取 `0.4.x` 系列。

---

## 总结

| 项目 | 说明 |
|:---|:---|
| 根本原因 | `getrandom v0.4.3` 使用 Edition 2024，要求 Rust ≥ 1.85 |
| 当前版本 | Rust 1.81.0，不支持 Edition 2024 |
| 最佳方案 | `rustup update stable` 升级至最新稳定版 |
| 临时方案 | 在 `Cargo.toml` 中锁定 `getrandom = "=0.2.15"` |

> 注意：随着生态中越来越多的 crate 迁移至 Edition 2024，保持 Rust 工具链定期更新是最稳妥的长期做法。

---

**关联笔记：**

- [[Rust 版本与 Edition 对照表]]
- [[Cargo 依赖版本管理]]
