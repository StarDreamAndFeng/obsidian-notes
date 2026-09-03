---
tags:
  - 自托管
  - 个人记账
  - Docker
  - Golang
  - PWA
  - 开源工具
  - 财务管理
  - 工具收藏
date: 2026-08-29
source: https://ezbookkeeping.mayswind.net/zh_Hans/
repo: https://github.com/mayswind/ezbookkeeping
---

# ezBookkeeping

> **开源、轻量、自托管（self-hosted）的个人记账应用**
>
> **官方网址**：<https://ezbookkeeping.mayswind.net/zh_Hans/>
> **开源仓库**：<https://github.com/mayswind/ezbookkeeping>

---

## 一、产品定位

ezBookkeeping 是一款轻量、自托管的个人记账应用，拥有用户友好的界面和强大的记账功能。它能帮助您：

- 记录日常交易、导入各种来源的数据
- 快速搜索和筛选账单
- 使用预设图表或自定义查询分析历史数据
- 了解消费情况和财务趋势

**部署成本**：只需一条 Docker 命令即可启动，资源占用低，在树莓派、NAS 和微型服务器等设备上也能流畅运行。

**多端体验**：为移动端和桌面端提供各自原生的界面设计。借助 PWA（渐进式网页应用）技术，可将其「添加到手机主屏幕」，像原生 App 一样使用。

### 界面预览

**桌面端**（登录、概览、图表、交易详情、添加交易）：

![ezBookkeeping 桌面端界面预览](/000-仓库管理/020-附件与资源/ezbookkeeping_desktop_screenshot_light.png)

## 二、核心特性

### 🛠️ 开源 & 自托管

专为隐私与数据自主而设计。

### ⚡ 轻量 & 快速

资源占用极少，即便在资源有限的设备上也运行流畅。

### 📦 安装简单

- 支持 Docker
- 支持 SQLite、MySQL、PostgreSQL 多种数据库
- 跨平台运行（Windows、macOS、Linux）
- 支持 x86、amd64、ARM 架构

### 🎨 友好的用户界面

- 针对手机与桌面优化的 UI
- 支持 PWA，带来接近原生 App 的使用体验
- 深色模式

### 🤖 AI 驱动的功能

- 收据图片识别
- 支持 MCP（Model Context Protocol）用于 AI 集成
- 支持 Agent Skill 和 API 命令行脚本工具用于 AI 集成

### 📘 强大的记账功能

- 两级账户与两级分类
- 支持交易图片附件
- 记录交易地理位置并在地图上展示
- 支持定时交易
- 高级筛选、搜索、数据可视化与分析功能

### 🌍 本地化与国际化支持

- 多语言与多币种支持
- 多汇率数据源及汇率自动更新
- 多时区支持
- 自定义日期、数字与货币格式

### 🔐 安全可靠

- 两步认证（2FA）
- OIDC 外部认证
- 登录频次限制
- 应用锁（PIN 码 / WebAuthn）

### 📑 数据导入 & 导出

支持 CSV、OFX、QFX、QIF、IIF、Camt.052、Camt.053、MT940、GnuCash、Firefly III、Beancount、随手记，以及支付宝、微信支付及京东金融的对账单等多种格式。

---

## 三、快速开始

### 3.1 使用 Docker 部署

访问 Docker Hub 查看所有镜像和标签。

最新发布版本：

```bash
docker run -p8080:8080 mayswind/ezbookkeeping
```

> **提示**：上述命令仅适用于测试和评估目的。对于生产环境，建议使用**特定版本的镜像**，并使用持久化数据卷来存储数据，以避免数据丢失。

### 3.2 从二进制包安装

下载最新发布版本：<https://github.com/mayswind/ezbookkeeping/releases>

**Linux / macOS：**

```bash
./ezbookkeeping server run
```

**Windows：**

```powershell
.\ezbookkeeping.exe server run
```

默认 ezBookkeeping 将会监听 `8080` 端口。访问 `http://{YOUR_HOST_ADDRESS}:8080/` 即可。

### 3.3 从源代码构建

需先安装 **Golang、GCC、Node.js、NPM**。开源地址：<https://github.com/mayswind/ezbookkeeping>

**Linux / macOS：**

```bash
./build.sh package -o ezbookkeeping.tar.gz
```

**Windows：**

```bat
.\build.bat package -o ezbookkeeping.zip
```

或：

```powershell
.\build.ps1 package -Output ezbookkeeping.zip
```

**构建 Docker 镜像：**

```bash
./build.sh docker
```

---

## 四、详细部署（Linux / macOS）

最新发布版本：**v1.6.1**（基于 2026 年 7 月 20 日）

### 4.1 基本运行

下载并解压缩压缩包，然后执行：

```bash
./ezbookkeeping server run
```

执行完后，ezBookkeeping 将会以默认配置启动，并监听端口 `8080`。

> **修改配置**：
> - 使用 `--conf-path` 参数指定自定义配置路径
> - 或直接修改 `conf/config.ini` 文件
> - 更多信息请访问 [配置](https://ezbookkeeping.mayswind.net/zh_Hans/configuration/)

### 4.2 systemd 管理

如果你有 `systemd` 并想用它管理 ezBookkeeping，可在 `/lib/systemd/system`（Debian/Ubuntu）或 `/usr/lib/systemd/system`（CentOS）下创建服务单元配置。

下载 [示例配置](https://github.com/mayswind/ezbookkeeping/blob/main/etc/systemd/ezbookkeeping.service) 到 `/lib/systemd/system/ezbookkeeping.service`，创建名为 `ezbookkeeping` 的用户和分组，并根据实际路径修改配置文件。

**启动服务：**

```bash
systemctl start ezbookkeeping
```

**设置开机自启：**

```bash
systemctl enable ezbookkeeping
```

### 4.3 Nginx 反向代理

更多 Nginx 信息请参考 [Nginx 官方文档](https://nginx.org/en/docs/)。

#### 场景 A：ezBookkeeping 在域名的根路径

```nginx
upstream ezbookkeeping-upstream {
    server 127.0.0.1:8080;
}

server {
    listen 80;
    listen [::]:80;
    server_name ezbookkeeping.yourdomain;

    return 301 https://$server_name$request_uri;
}

server {
    listen 443      ssl;
    listen [::]:443 ssl;
    server_name ezbookkeeping.yourdomain;

    location / {
        proxy_pass http://ezbookkeeping-upstream;

        proxy_redirect   off;
        proxy_set_header Host            $host;
        proxy_set_header X-Real-IP       $remote_addr;
        proxy_set_header X-Real-Port     $remote_port;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

#### 场景 B：ezBookkeeping 在域名的子路径（如 `/ezbookkeeping`）

```nginx
upstream ezbookkeeping-upstream {
    server 127.0.0.1:8080;
}

server {
    listen 80;
    listen [::]:80;
    server_name yourdomain;

    location /ezbookkeeping {
        return 301 https://$server_name$request_uri;
    }
}

server {
    listen 443      ssl;
    listen [::]:443 ssl;
    server_name yourdomain;

    location = /ezbookkeeping {
        return 301 /ezbookkeeping/;
    }

    location ~ ^/ezbookkeeping/(desktop|mobile)/$ {
        return 301 /ezbookkeeping/$1;
    }

    location /ezbookkeeping/ {
        rewrite ^/ezbookkeeping/(.*) /$1 break;
        proxy_pass http://ezbookkeeping-upstream;

        proxy_redirect   off;
        proxy_set_header Host            $host;
        proxy_set_header X-Real-IP       $remote_addr;
        proxy_set_header X-Real-Port     $remote_port;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

> **注意**：通过子路径访问时，还需要在 [配置](https://ezbookkeeping.mayswind.net/zh_Hans/configuration/) 中设置 `root_url` 选项，例如 `https://%(domain)s:{your_nginx_port}/ezbookkeeping/`。

---

## 五、基础使用

### 5.1 注册新用户

ezBookkeeping 第一次运行后，并不会自动创建任何用户。需要通过注册页面注册一个新用户，注册成功后即可登录使用。

注册时只需设置：

- 用户语言
- 默认货币
- 星期第一天

更多本地化设置（财年第一天、日期格式、时间格式、货币显示格式、支出/收入金额颜色等）可在登录后通过「用户信息」页面调整。

### 5.2 创建账户

第一次登录后，需要先创建一个账户才能开始记录财务数据。

在「账户」页面点击「添加」按钮，设置账户的分类、名称、初始余额等信息即可完成创建。**建议每一个账户对应现实生活中的一个账户**（如独立银行账户等）。

**账户类型与余额规则：**

| 账户分类 | 余额规则 |
| --- | --- |
| 现金 / 借记账户 | 正数表示拥有的资金（资产） |
| 信用卡 / 负债账户 | 正数表示欠款（负债） |

> 信用卡分类的账户支持设置**账单日**，设置后在交易列表选择该账户时，可直接筛选当前/上一账单周期的交易记录。

> **编辑账户**：桌面版「账户」页 → 鼠标悬停 → 编辑图标；移动版「账户」页 → 左滑 → 「编辑」按钮。

### 5.3 创建交易分类

注册用户时可选择是否自动创建默认的交易分类。如未创建，也可在「交易分类」页面手动创建。

ezBookkeeping 支持两级交易分类：先创建一级，再在其下创建二级。后续每笔支出/收入/转账交易都必须指定一个二级分类。

### 5.4 创建交易

在主界面或「交易详情」页面，点击「添加」按钮创建交易，必填项：

- 交易类型（支出 / 收入 / 转账）
- 金额
- 分类
- 账户
- 交易时间

可选扩展项：地理位置、图片、标签、备注。

**标签功能**：支持为每个交易添加多个标签，可用于记录商户、付款人、收款人、项目等信息；标签支持分组管理。

> **编辑交易**：
> - 桌面版：「交易详情」页 → 点击某笔交易 → 「编辑」按钮；或「洞察探索」→「数据表格」→ 三点菜单 → 「进入编辑模式」批量更新
> - 移动版：「交易详情」页 → 左滑 → 「编辑」

### 5.5 查看交易

在「交易详情」页面，可按**时间或日历**查看交易记录。支持按时间范围、分类、账户、金额、标签、备注等条件筛选。

### 5.6 对账单

在「账户」页面选择账户并选择「对账单」功能，可设置时间范围查看：

- 每笔交易产生后的账户余额
- 该时间段内账户余额变化趋势（图表形式）

> **调整账户余额**：出于语义清晰原因，ezBookkeeping 只允许创建新账户时设置期初余额。后续调整余额需创建支出/收入交易。「对账单」页提供「**更新期末余额**」简化功能，会自动计算预期期末余额与实际余额的差异，并填入创建新交易页面。

### 5.7 查看图表

| 功能 | 用途 |
| --- | --- |
| 统计分析 | 提供多种预设图表，快速查看常用财务数据 |
| 洞察探索 | 自定义图表维度、筛选条件，满足高级分析需求 |

> **货币转换**：统计分析和洞察探索会将所有金额转换为默认货币计算。如果账户货币不在当前汇率数据中，无法转换的交易将从结果中剔除。可在 [汇率](https://ezbookkeeping.mayswind.net/zh_Hans/exchange_rates/) 设置包含所有账户货币的汇率数据源。

---

## 六、常见问题

如果在使用过程中遇到问题，可参考 [常见问题](https://ezbookkeeping.mayswind.net/zh_Hans/faq/) 页面，里面收集了一些常见问题及其解决方案。