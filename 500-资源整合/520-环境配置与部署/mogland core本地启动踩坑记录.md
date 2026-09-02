---
tags:
  - Node.js
  - pnpm
  - 环境配置
  - Windows
  - MongoDB
  - Redis
  - bcrypt
date: 2026-08-29
source: https://github.com/mogland/core
---

# mogland/core 本地启动踩坑记录

环境：Windows + Node v22.14.0 + pnpm。

## 1. 安装与启动

```powershell
# 确认包管理器（项目 preinstall 会强制要求 pnpm）
pnpm -v

# 安装依赖
pnpm i
```

启动脚本一览（来自 `package.json`）：

| 命令 | 作用 |
| --- | --- |
| `pnpm start` | 并发拉起全部 9 个微服务（资源占用较高） |
| `pnpm dev:minimum` | 只启动 core / user-service / page-service / comments-service |

## 2. 数据库与 Redis 配置

配置入口有两个，优先级：**命令行参数 > `env.yaml`**。

### 方式 A：`env.yaml`（推荐）

`env.yaml` 的 key 必须用**下划线**命名（与 `shared/commander.ts` 中的 CLI 参数名一致）。

默认配置见 `apps/core/src/app.config.ts`：

- Mongo：`127.0.0.1:27017`，无 user/password，dbName 默认 `mog`
- Redis：`127.0.0.1:6379`，无密码

本地无密码的最小配置示例：

```yaml
console:
  enable: true

# mongodb
db_host: 127.0.0.1
db_port: 27017
db_user: ""
db_password: ""
collection_name: fly_mog

# redis
redis_host: 127.0.0.1
redis_port: 6379
redis_password: ""
redis_user: ""

# security
jwt_secret: "asjhczxiucipoiopiqm2376"
```

### 方式 B：命令行参数

```powershell
pnpm start --db_host=127.0.0.1 --db_port=27017 --redis_host=127.0.0.1 --redis_port=6379
```

参数名见 `shared/commander.ts`：

- 数据库：`-H/--db_host`、`-P/--db_port`、`-U/--db_user`、`-W/--db_password`、`-N/--collection_name`、`-A/--db_atlas`
- Redis：`-RH/--redis_host`、`-RP/--redis_port`、`-RW/--redis_password`、`-RU/--redis_user`
- 安全：`--jwt_secret`、`--jwt_expire`

### 注意事项

- YAML 里空字符串 `""` 会被当作"未设置"（代码里有 `|| ''` / `|| null` 兜底），不会把空密码拼到 URI。
- `jwt_secret` 默认值 `asjhczxiucipoiopiqm2376` 仅用于本地开发，生产前必须换成强随机串。

## 3. Node 版本与 bcrypt 原生模块踩坑

### 问题现象

```
ERROR  Error: Cannot find module 'E:\Project\self-project\core\node_modules\.pnpm\bcrypt@5.1.1\node_modules\bcrypt\lib\binding\napi-v3\bcrypt_lib.node'
```

### 根因

- 项目锁定的 `bcrypt@5.1.1` 是 Node 22 之前发布的，自带的预编译 `napi-v3` 二进制不包含 Node 22 对应的版本。
- pnpm 把 bcrypt 装到 `.pnpm/bcrypt@5.1.1/...` 虚拟存储里，找不到对应 ABI 的 `.node` 文件就报这个错。

### 解决方案：手动跑 `node-pre-gyp install`

无需装 MSVC / Build Tools（因为 Node 22 的预编译包其实在 GitHub releases 上是存在的）。

```powershell
cd "E:\Project\self-project\core\node_modules\.pnpm\bcrypt@5.1.1\node_modules\bcrypt"

npx node-pre-gyp install
```

成功输出示例：

```
node-pre-gyp info using node@22.14.0 | win32 | x64
node-pre-gyp info check checked for "...\lib\binding\napi-v3\bcrypt_lib.node" (not found)
node-pre-gyp http GET https://github.com/kelektiv/node.bcrypt.js/releases/download/v5.1.1/bcrypt_lib-v5.1.1-napi-v3-win32-x64-unknown.tar.gz
node-pre-gyp info install unpacking napi-v3/bcrypt_lib.node
node-pre-gyp info extracted file count: 1
[bcrypt] Success: "...\lib\binding\napi-v3\bcrypt_lib.node" is installed via remote
node-pre-gyp info ok
```

### 验证产物

```powershell
# 物理路径
Test-Path "E:\Project\self-project\core\node_modules\.pnpm\bcrypt@5.1.1\node_modules\bcrypt\lib\binding\napi-v3\bcrypt_lib.node"

# pnpm 顶层符号链接（应该指向同一个文件）
Test-Path "E:\Project\self-project\core\node_modules\bcrypt\lib\binding\napi-v3\bcrypt_lib.node"
```

两个都返回 `True` 即正常。

### 备选方案（如果远程下载也失败）

1. **从源码编译**：

   ```powershell
   cd "E:\Project\self-project\core\node_modules\.pnpm\bcrypt@5.1.1\node_modules\bcrypt"
   npx node-pre-gyp install --fallback-to-build
   # 或直接
   npx node-gyp rebuild
   ```

   需要装好：
   - Visual Studio Build Tools（工作负载："使用 C++ 的桌面开发"，含 MSVC + Windows SDK）
   - Python 3.x

2. **换成纯 JS 实现 `bcryptjs`**（最稳，不依赖编译器）：

   ```diff
   -    "bcrypt": "5.1.1",
   +    "bcryptjs": "^2.4.3",
   ```

   再把 `apps/`、`libs/`、`shared/` 下所有 `from 'bcrypt'` 替换成 `from 'bcryptjs'`。API 完全兼容，业务代码不用动。

### ⚠️ 重要提醒

**不要在 `.pnpm/.../bcrypt/` 目录下执行 `pnpm install` 或 `npm install`**。

`.pnpm` 是 pnpm 的内容寻址存储，里面跑包管理命令会破坏 pnpm 的目录结构，导致其他包符号链接断裂。只跑 `node-pre-gyp` 或 `node-gyp` 这类**针对单个原生模块构建**的命令即可。

## 4. 完整启动顺序

1. 启动 MongoDB（监听 27017，无 auth）
2. 启动 Redis（监听 6379，无密码）
3. 确认 `env.yaml` 配置无误（或用 CLI 参数）
4. `pnpm i`
5. 若 bcrypt 报错，按第 3 节处理
6. `pnpm start`（或 `pnpm dev:minimum`）
