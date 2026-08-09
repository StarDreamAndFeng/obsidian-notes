---
tags:
  - uni-app
  - APP
  - 回弹效果
  - iOS
date: 2026-08-04
---

# uni-app APP 端页面回弹效果彻底关闭

## 概述

在 uni-app 开发 APP（尤其是 iOS 端）时，页面顶部或底部常出现橡皮筋回弹（果冻拉伸）效果。仅关闭 `<scroll-view>` 组件的回弹通常不足以完全消除，因为存在**两层回弹机制**。

---

## 回弹层级说明

uni-app APP 端包含两层独立的回弹机制：

| 层级 | 位置 | 控制属性 |
|------|------|---------|
| **组件层** | `<scroll-view>` 组件 | `bounces` 属性 |
| **页面层** | pages.json 页面 style | `"bounce"` 字段（APP-PLUS 专属） |

仅关闭组件层回弹后，WebView 容器本身（整个页面的 UIScrollView）的回弹机制仍在工作，因此滚动到顶部或底部时仍可继续拖拽仍会产生果冻拉伸感。

---

## 三层关闭机制

### 第一层：pages.json 页面级配置

在 `pages.json` 中为目标页面添加以下配置：

```json
{
    "path": "pages/xxx/xxx",
    "style": {
        "navigationBarTitleText": "页面标题",
        "navigationStyle": "custom",
        "backgroundColor": "#F6F6F6",
        "disableScroll": true,
        // #ifdef APP-PLUS
        "bounce": "none",
        "popGesture": "close"
        // #endif
    }
}
```

**属性说明：**

| 属性 | 作用域 | 说明 |
|------|--------|------|
| `"bounce": "none"` | APP-PLUS | 彻底关闭 WebView/UIScrollView 容器级的橡皮筋回弹（核心开关） |
| `"disableScroll": true` | 全端 | 禁止页面级原生滚动，防止页面级 scroll 抢手势 |
| `"popGesture": "close"` | APP-PLUS | 保留 iOS 侧滑返回（不影响回弹，保持与其他页面一致） |

---

### 第二层：scroll-view 组件配置

将 `<scroll-view>` 的 `bounces` 属性改为**静态字符串**写法：

```html
<!-- ❌ 动态绑定写法（部分旧机型不识别 -->
<scroll-view scroll-y :bounces="false"></scroll-view>

<!-- ✅ 静态属性写法（所有端稳定识别） -->
<scroll-view scroll-y bounces="false"></scroll-view>
```

**原因：** 动态绑定 `:bounces="false"` 在部分旧版 uni-app APP 端存在已知缺陷：渲染层解析时将 `false` 识别为字符串 `"false"`（非空字符串被视为 truthy），等同于未关闭。改为静态属性可规避此问题。

**完整写法示例：

```html
<scroll-view
    class="list-scroll"
    scroll-y
    bounces="false"
    :enhanced="true"
    :show-scrollbar="false"
>
    <!-- 列表内容 -->
</scroll-view>
```

---

### 第三层：页面根容器样式

页面根容器设置 `overflow: hidden`，配合 `disableScroll: true` 形成双保险：

```scss
.page-container {
    width: 100%;
    height: 100%;
    overflow: hidden;
    display: flex;
    flex-direction: column;
}
```

---

### 第四层：视觉优化（可选）

列表最后一项的 `margin-bottom` 改为 `padding-bottom`，避免滚动到底部时出现视觉上的"真空地带"：

```scss
.list-item {
    margin-bottom: 24rpx;

    &:last-child {
        margin-bottom: 0;
        padding-bottom: 24rpx;
    }
}
```

**原理：** `margin-bottom` 会让滚动容器底部额外多出透明区域，滚动到底部时看到卡片下方存在空白地带，松手回弹时容易被误认为是果冻拉伸的一部分。改为 `padding-bottom` 将空白算入卡片自身内容区域，滚动到底部时视觉上为"贴边"。

---

## 关闭机制总览

```
┌─ 页面级 WebView 容器: bounce="none"  ←  最大层级
│  ┌─ 页面根容器: overflow:hidden + disableScroll:true
│  │  ┌─ scroll-view: bounces="false"    ←  组件层
│  │  │   [列表内容]
│  │  └────────────────────────────────
│  └────────────────────────────────────
└────────────────────────────────────────
```

---

## 方法对比

| 配置项 | 作用层级 | 推荐度 |
|--------|---------|--------|
| pages.json `"bounce": "none"` | 容器层 | ⭐⭐⭐ |
| pages.json `"disableScroll": true` | 页面层 | ⭐⭐⭐ |
| scroll-view `bounces="false"`（静态写法） | 组件层 | ⭐⭐⭐ |
| 根容器 `overflow: hidden` | 页面层 | ⭐⭐ |
| last-child margin → padding | 视觉层 | ⭐⭐ |

---

## 注意事项

- **残余阻尼感说明：三层关闭后，若滚动到顶/到底再用力拨动仍存在极轻微的阻尼感，属于手指触摸时的系统级手势反馈，并非果冻回弹。该阻尼感为 iOS 系统特性，所有应用均存在，无法完全去除。

- **条件编译**：`"bounce": "none"` 为 APP-PLUS 专属属性，必须使用 `// #ifdef APP-PLUS` 条件编译包裹，避免在小程序或 H5 端解析报错。

- **新文件未找到问题**：在 uvue 文件中若 pages.json 配置同样生效，无需额外配置。

---

**关联笔记：**

- [[351-Android开发]] - 移动端开发相关笔记
