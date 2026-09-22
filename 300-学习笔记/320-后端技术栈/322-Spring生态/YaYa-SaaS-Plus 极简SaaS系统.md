---
tags: [Java, SpringBoot, SaaS, 开源项目, Gitee]
date: 2026-08-03
source: https://gitee.com/ukoko/yaya-saas-plus
---

# YaYa-SaaS-Plus 极简 SaaS 系统

<div align="center">
  <img src="/000-仓库管理/020-附件与资源/image-20260803202537.png" width="300">
</div>

<div align="center">

![Jdk >=25](https://img.shields.io/badge/Jdk-%3E=25-red) ![SpringBoot >=v4.0.6](https://img.shields.io/badge/SpringBoot->=v4.0.6-orange) ![YaYa LayUI Admin Plus v2.x](https://img.shields.io/badge/YaYa%20LayUI%20Admin%20Plus-v2.x-blue) ![MYSQL >=v8.0](https://img.shields.io/badge/MYSQL->=v8.0-gold)
![Redis >=v3.0.5](https://img.shields.io/badge/Redis->=v3.0.5-brown) ![LICENSE MIT](https://img.shields.io/badge/LICENSE-MIT-indigo)

</div>

---

## 一、项目介绍

YaYa-SaaS-Plus 是一款适合中小企业的开源 SaaS 系统，系统精简（保留 SaaS 最小单元），用户操作方便，利于企业维护和扩展。

- 后端采用 [SpringBoot 4.x](https://spring.io/) 版本构建
- 前端采用 [YaYa-LayUI-Admin-Plus](https://gitee.com/ukoko/yaya-layui-admin-plus) 模板构建

### 分支说明

| 分支     | 说明                |
| :------- | :------------------ |
| master   | SaaS 系统的基础分支 |
| yaya-rag | 知识库系统分支      |

---

## 二、项目优势

1. 适合新手开发和使用（前端新手 + 后端新手）
2. 持续的成本控制（开发成本极低 + 维护成本极低）
3. 基于 MIT 开源协议（协议宽泛 + 友好）
4. 功能强大（丰富的工具类 + 核心功能）

---

## 三、系统演示

| 项目       | 内容                  |
| :--------- | :-------------------- |
| 预览地址   | http://106.14.27.178/ |
| 超级管理员 | root / 123456         |
| 系统管理员 | admin / 123456        |
| 运营管理员 | operation / 123456    |

---

## 四、系统权限介绍

YaYa 平台默认权限体系：

1. 自带默认部门 1 个 —— 不能删除，用来存放默认用户
2. 自带默认角色 3 个 —— 不能删除，给默认用户分配角色
3. 自带默认用户 3 个 —— 不能删除，用于平台的基本管理

### 三个默认账户

| 账户      | 角色       | 用途                                                             |
| :-------- | :--------- | :--------------------------------------------------------------- |
| root      | 超级管理员 | 权限最高，不分配任何权限，一般用于菜单权限和数据权限混乱时的梳理 |
| admin     | 系统管理员 | 开发人员使用，权限由超级管理员分配                               |
| operation | 运营管理员 | 公司运营人员使用，主要负责租户管理（含租户管理账号、套餐管理等） |

![[image-20260803204144.png]]

---

## 五、技术架构介绍

### 1. 技术栈

#### 1.1 前端技术栈

| 框架                  | 版本   | 官网                                                            | 备注                               |
| :-------------------- | :----- | :-------------------------------------------------------------- | :--------------------------------- |
| Layui                 | 2.13.8 | https://layui.dev/                                              | 基于原生 HTML/CSS/JS (JQuery) 开发 |
| yaya-layui-admin-plus | 2.2.1  | https://gitee.com/ukoko/yaya-layui-admin-plus                   | 基于 Layui 实现                    |
| xm-select             | 1.2.4  | https://xm-select.com/file/xm-select/v1.2.4/#/component/install | 始于 Layui 实现，可独立使用        |

#### 1.2 后端技术栈

| 框架          | 版本   | 官网                                     | 备注            |
| :------------ | :----- | :--------------------------------------- | :-------------- |
| SpringBoot    | 4.0.6  | https://spring.io/                       | 项目底座        |
| mybatis-plus  | 3.5.15 | https://baomidou.com/                    | 持久层框架      |
| knife4j       | 4.5.0  | https://doc.xiaominfo.com/               | 在线文档增强    |
| jwt           | 4.4.0  | https://www.jwt.io/                      | Token 令牌工具  |
| jthinking     | 2.1.7  | https://gitee.com/jthinking/ip-info      | IP 地址解析工具 |
| thumbnailator | 0.4.21 | https://github.com/coobird/thumbnailator | 图片压缩工具    |
| bouncycastle  | 1.84   | https://www.bouncycastle.org/            | 算法库          |

#### 1.3 数据库

| 类型     | 版本要求   |
| :------- | :--------- |
| 关系型   | MySQL 8.4+ |
| 非关系型 | Redis 3.5+ |

### 2. 架构介绍

#### 2.1 前端技术架构

前端基于 [YaYa-Layui-Admin-Plus](https://gitee.com/ukoko/yaya-layui-admin-plus) 模板实现，该模板基于 [LayUI](https://layui.dev/) 框架实现，纯原生开发，只需掌握基础的 HTML/CSS/JS (jQuery) 即可完成开发，开发成本极低。

项目采用多页面独立开发模式，页面之间基本没有业务关联，在多用户协同开发时可以降低交叉影响，减少因程序员习惯差异对整个项目造成的影响，同时降低后期运维成本。

![[image-20260803204304.png]]

#### 2.2 后端技术架构

基于 SpringBoot 4.x 生态开发，使用简单，框架生态强大，社区兼容性强。Java 相关技术均能与之适配，与 AI 相关生态兼容性良好。

![[image-20260803204316.png]]

### 3. 架构优缺点

#### 前端

**优点：** 入门门槛极低，开发简单，运维简单，开发成本极低，运维成本极低。

**缺点：** 针对 PC 端基本没有缺点。唯一可能的不足是响应式需要自行实现，但 PC 端响应式需求不强。

#### 后端

**优点：**

1. 单 Module 设计，项目架构简单，方便新手程序员开发和运维
2. 前后端分离架构设计，后端可单点部署，也可集群部署，无需额外修改
3. 基于最新的 SpringBoot 4.x 框架，对快速发展的 AI 生态兼容性好，便于升级
4. 项目设计阶段已充分考虑开发和运维成本，提供了多项简单高效的功能和工具类：
   - 4.1 统一日志收集系统（当前仅收集增删改操作日志）；如需收集查询日志，在控制器方法上添加 `@LogCollect` 注解即可
   - 4.2 统一异常收集系统：异常通过统一日志收集系统存入数据库（需定期清理，避免数据膨胀）；同时以 `.log` 文件形式保存到本地
   - 4.3 前端重复提交 / 频繁点击控制：基于 `@RepeatSubmit` 注解，灵活可控
   - 4.4 常用工具类集合

**缺点：**

1. 项目规模扩大后，开发工具可能出现卡顿，启动速度变慢
2. 单个项目中文件过多，对开发和维护人员的可视性不佳
3. 单点项目通病：项目过大、开发文件过多时，协同开发易出现版本冲突

> **注意：** 以上缺点在成本（开发成本 + 运行成本 + 时间成本）面前，均不算严重问题。

---

## 六、基础模块介绍

![[image-20260803204329.png]]

---

## 七、代码审查

针对政企项目，需要进行代码安全审查。

![[image-20260803204342.png]]

---

## 八、开发环境搭建

### 1. 开发工具

| 角色 | 工具                         |
| :--- | :--------------------------- |
| 后端 | IntelliJ IDEA (IDEA)         |
| 前端 | Visual Studio Code (VS Code) |

### 2. 项目导入和运行

#### 2.1 项目地址

```
https://gitee.com/ukoko/yaya-saas-plus
```

> 前后端项目在同一个仓库中。

![[image-20260803204400.png]]

#### 2.2 项目导入

- Java 项目导入到 IDEA 中
- 前端项目导入到 VS Code 中

![[image-20260803204443.png]]

![[image-20260803204451.png]]

#### 2.3 数据库创建和导入

（略）

---

## 九、参与贡献

前后端由作者独立完成，并借助 AI 工具辅助开发。

| 项目     | 链接                         |
| :------- | :--------------------------- |
| 作者博客 | https://hs-an-yue.github.io/ |
| 作者邮箱 | hd1611756908@163.com         |

---

## 十、致谢

感谢 LayUI、Echarts、xm-select、jQuery 等前端框架支持；同时感谢 Gemini、Grok、ChatGPT、豆包、千问等大模型的支持。

---

**关联笔记：**

- [[SpringBoot 学习笔记]]
- [[Java 项目开源库索引]]
