---
tags:
  - Git
  - 暂存操作
  - 分支管理
date: 2026-08-03
---

# Git Stash 临时暂存机制

## 概述

`git stash` 用于将工作区和暂存区的修改临时保存，以便切换分支处理其他任务，稍后再恢复这些修改。

---

## 核心特性

### 特性一：支持跨分支应用

Stash 是**仓库全局**的临时栈，不属于任何特定分支。暂存的改动可以在任意分支上恢复。

```bash
# 在 feature-A 分支保存修改
git stash

# 切换到 main 分支
git checkout main

# 在 main 分支应用暂存的改动
git stash pop
```

若两个分支差异较大，应用时可能产生冲突，需手动解决。

---

### 特性二：不进入常规提交历史

Stash 与 commit 属于两个层面的概念：

| 层面 | 说明 |
|------|------|
| **用户视角** | Stash 保存的内容不会出现在任何分支的提交历史中。执行 `git log` 无法查看，推送分支时也不会同步到远程仓库。 |
| **底层实现** | Git 底层使用特殊的 commit 对象保存索引和工作区状态，但这些 commit 不在任何分支历史线上，仅通过 `refs/stash` 特殊引用悬挂在仓库中。 |

---

## 常用命令

```bash
git stash                    # 保存当前所有修改（包括暂存区和未暂存）
git stash push -m "描述"      # 带备注保存，便于识别
git stash list               # 查看所有 stash 记录
git stash pop                # 应用最新的 stash，并从栈中移除
git stash apply stash@{0}    # 应用但不移除
git stash drop stash@{0}     # 删除指定 stash
git stash clear              # 清空所有 stash
```

---

## 命令对比

| 命令 | 功能 | 推荐度 |
|------|------|--------|
| `git stash push -m "说明"` | 带备注保存，清晰可辨 | ⭐⭐⭐ |
| `git stash pop` | 应用并移除，防止栈内残留 | ⭐⭐⭐ |
| `git stash apply` | 仅应用不移除，多分支复用场景 | ⭐⭐ |
| `git stash list` | 查看栈内记录 | ⭐⭐⭐ |
| `git stash clear` | 清空所有暂存 | ⭐（慎用） |

---

## 注意事项

- **未跟踪文件处理**：未执行 `git add` 的新文件默认不会被暂存，需添加 `-u` 参数：`git stash push -u`
- **存储范围**：Stash 是本地仓库的临时机制，不会随 `git push` 上传到远程
- **生命周期**：长时间未清理的 stash 可能会被 Git 垃圾回收机制清理（周期较长，通常无需担心）

---

## 典型场景

### 场景一：中断当前开发，处理紧急任务

```bash
# 1. 保存当前开发中的修改
git stash push -m "正在开发的登录功能"

# 2. 切换到主分支处理紧急修复
git checkout main
# ... 修复代码并提交 ...

# 3. 回到原分支，恢复修改
git checkout feature-A
git stash pop
```

### 场景二：把当前改动迁移到新分支

```bash
# 1. 保存修改
git stash

# 2. 从当前分支创建新分支
git checkout -b feature-new

# 3. 在新分支上恢复修改
git stash pop
```

---

## 总结

Stash 的核心价值在于**安全地临时搁置修改**：既不绑定特定分支，也不会污染提交历史。适用于需要临时切换上下文但不希望产生半成品 commit 的场景。

---

**关联笔记：**

- [[Git合并分支忽略历史提交]] - Git 分支合并操作
- [[Git分支追踪错误修复]] - Git 分支追踪关系修复
