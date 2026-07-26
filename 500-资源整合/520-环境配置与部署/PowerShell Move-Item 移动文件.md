---
tags: [PowerShell, Windows, 命令行]
date: 2026-07-26
---

# PowerShell Move-Item 移动文件/目录

`Move-Item` 是 PowerShell 中用于**移动文件或目录**的命令，相当于其他系统中的 `mv` 命令。

## 基本语法

```powershell
Move-Item -Path <源路径> -Destination <目标路径>
```

### 参数说明

| 参数 | 说明 |
|------|------|
| `-Path` | 要移动的源文件或目录路径 |
| `-Destination` | 目标路径（可以是目录，也可以是新文件名） |
| `-Force` | 强制移动，即使目标文件已存在也覆盖 |
| `-WhatIf` | 模拟执行，显示会发生什么但不实际移动 |
| `-Confirm` | 执行前提示确认 |

## 常用示例

### 1. 移动单个文件

```powershell
# 将文件移动到指定目录
Move-Item -Path "C:\Users\file.md" -Destination "D:\Notes\"

# 简写（省略参数名）
Move-Item "C:\Users\file.md" "D:\Notes\"
```

### 2. 移动时重命名

```powershell
# 移动文件并重命名
Move-Item -Path "old-name.md" -Destination "new-folder\new-name.md"
```

### 3. 移动整个目录

```powershell
# 将整个目录及其内容移动到目标位置
Move-Item -Path "C:\OldFolder" -Destination "D:\NewFolder"
```

### 4. 批量移动文件

```powershell
# 移动所有 .md 文件到目标目录
Move-Item -Path "*.md" -Destination "D:\Notes\"

# 移动匹配通配符的文件
Move-Item -Path "*.txt" -Destination "D:\TextFiles\"
```

### 5. 强制覆盖

```powershell
# 如果目标文件已存在，强制覆盖
Move-Item -Path "file.md" -Destination "D:\Notes\" -Force
```

### 6. 模拟执行（安全预览）

```powershell
# 查看移动结果但不实际执行
Move-Item -Path "*.md" -Destination "D:\Notes\" -WhatIf
```

## 实际应用：整理 Obsidian 笔记

```powershell
# 将根目录下的 Git 笔记移动到学习笔记目录
Move-Item -Path "c:\UserFileData\Documents\Git：本地已有仓库与新建GitHub远程仓库合并.md" -Destination "c:\UserFileData\Documents\学习笔记\Git：本地已有仓库与新建GitHub远程仓库合并.md"

Move-Item -Path "c:\UserFileData\Documents\版本控制基础.md" -Destination "c:\UserFileData\Documents\学习笔记\版本控制基础.md"
```

## 注意事项

1. **目标路径不存在**：如果目标目录不存在，`Move-Item` 会尝试将源重命名为目标路径，而不是创建目录
2. **跨驱动器移动**：在不同盘之间移动文件是允许的（如 C: → D:）
3. **权限问题**：移动系统目录或受保护文件需要管理员权限
4. **文件被占用**：如果文件正在被其他程序使用，移动会失败

## 与 Copy-Item 的区别

| 命令 | 作用 | 源文件状态 |
|------|------|------------|
| `Move-Item` | 移动（剪切+粘贴）| 源文件被删除 |
| `Copy-Item` | 复制（复制+粘贴）| 源文件保留 |

---

**关联笔记：**

- [[版本控制基础]]
- [[Obsidian 文件命名注意事项]]
