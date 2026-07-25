# Git：本地已有仓库与新建GitHub远程仓库合并（Obsidian笔记仓库场景）

场景
本地 obsidian-notes 文件夹已初始化 git，并且完成首次提交；
GitHub网页新建仓库时勾选生成 README，远程仓库自动产生一条提交。
直接push会报错：两者提交历史独立、无共同祖先。

## 操作步骤
1. 进入本地笔记目录，打开终端
2. 绑定远程仓库
```bash
# 若已存在origin先删除
git remote remove origin
# 添加远程地址
git remote add origin git@github.com:用户名/obsidian-notes.git
```

3. 拉取远程代码，允许合并无关历史（核心）
分支为 main：
```bash
git pull origin main --allow-unrelated-histories
```
分支为 master：
```bash
git pull origin master --allow-unrelated-histories
```
> 弹出合并编辑器，直接保存退出完成合并。

4. 将本地内容推送到远程
```bash
# main分支
git push origin main
# master分支
git push origin master
```

## ⚠️ 备选方案（慎用！以本地覆盖远程）
远程README等文件会丢失，不推荐
```bash
git push origin main --force-with-lease
```

## Obsidian仓库 .gitignore 推荐配置
```
.obsidian/cache
.obsidian/workspace.json
.obsidian/plugins/*
.obsidian/temp
.DS_Store
```

## 日常同步流程
修改笔记 → git add . → git commit -m "更新说明" → git push

## 常见报错
fatal: remote origin already exists.
解决：执行 `git remote remove origin` 再重新绑定远程。