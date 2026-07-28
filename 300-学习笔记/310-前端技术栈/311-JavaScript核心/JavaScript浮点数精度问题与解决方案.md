---
tags:
  - JavaScript
  - 浮点数
  - 精度问题
date: 2026-07-28
---

# JavaScript 浮点数精度问题与解决方案

## 问题概述

JavaScript 采用 **IEEE 754 双精度浮点数标准**，导致某些十进制小数无法在二进制中精确表示，从而产生精度误差。

### 典型示例

```javascript
// 简单加法出现精度误差
0.1 + 0.2;    // 0.30000000000000004

// 累加运算累积误差
2 + 2 + 1.467865 + 2.467865 + 0.06427;  // 7.999999999999999

// 相等比较失败
0.1 + 0.2 === 0.3;  // false
```

### 影响场景

- **金额计算**：订单金额、税费计算
- **库存管理**：出库数量、库存余量比较
- **数据统计**：累加求和、百分比计算

---

## 解决方案

以下方法按推荐程度从高到低排列。

### 方法对比

| 方法 | 特点 | 推荐度 |
|------|------|--------|
| 转为整数计算 | 全程处理数字，无类型转换损耗，精度安全 | ⭐⭐⭐ |
| Math.round 指定精度 | 运算结束后统一处理，简单易用 | ⭐⭐⭐ |
| toFixed | 返回字符串，需转回数字，不推荐中间计算使用 | ⭐⭐ |
| decimal.js 等库 | 企业级精度保障，功能强大 | ⭐⭐⭐ |

### 方法一：转为整数计算（最推荐）

找出数据中最长的小数位数，计算时先乘以 `10^n`（n 为小数位数），运算完成后再除以 `10^n`。

```javascript
// 示例：计算 0.1 + 0.2 + 0.3，最长小数位为 1
const nums = [0.1, 0.2, 0.3];

// 转为整数计算
const result = nums.reduce((sum, num) => sum + Math.round(num * 10), 0) / 10;
// 结果：0.6

// 示例：6位小数精度
const nums6 = [1.467865, 2.467865, 0.06427];
const result6 = nums6.reduce((sum, num) => sum + Math.round(num * 1e6), 0) / 1e6;
// 结果：4
```

**优点**：全程处理数字，无类型转换损耗，精度安全。

---

### 方法二：使用 Math.round 指定精度

在运算结束后，使用 `Math.round` 按指定精度处理结果。

```javascript
const nums = [0.1, 0.2, 0.3];

const rawSum = nums.reduce((sum, num) => sum + num, 0);
// rawSum: 0.6000000000000001

// 指定保留 1 位小数
const result = Math.round(rawSum * 10) / 10;
// 结果：0.6

// 指定保留 6 位小数
const result6 = Math.round(rawSum * 1e6) / 1e6;
// 结果：0.6
```

---

### 方法三：使用 toFixed（注意返回字符串）

`toFixed(n)` 返回指定位数的字符串，需用 `parseFloat` 转回数字。

```javascript
const nums = [0.1, 0.2, 0.3];

const rawSum = nums.reduce((sum, num) => sum + num, 0);
// rawSum: 0.6000000000000001

// 必须用 parseFloat 转回数字
const result = parseFloat(rawSum.toFixed(6));
// 结果：0.6
```

**注意**：`toFixed` 返回字符串，直接用于数学比较会触发隐式类型转换，不推荐在中间计算步骤使用。

---

## 企业级方案：使用专用精度库

对于涉及金额、库存等核心业务的系统，建议引入专门处理精度的库。

### decimal.js（推荐）

```javascript
import Decimal from 'decimal.js';

const nums = [0.1, 0.2, 0.3];

const result = nums.reduce((sum, num) => {
  return new Decimal(sum).plus(new Decimal(num)).toNumber();
}, 0);
// 结果：0.6
```

### 其他常用库

| 库名 | 特点 |
|------|------|
| `decimal.js` | 轻量级，功能强大，支持大数运算 |
| `big.js` | 极简实现，专注精度计算 |
| `bignumber.js` | 高精度，支持科学计数法 |

---

## 核心思路总结

**放大成整数运算，再缩小回小数**。

1. **确定精度**：根据业务需求确定小数位数（如金额通常保留 2 位）
2. **整数化计算**：乘以 `10^n` 转为整数，避免浮点误差
3. **统一格式化**：计算完成后除以 `10^n`，或使用 `Math.round` 处理

---

**关联笔记：**

- [[310-前端技术栈]] - 前端技术栈相关笔记
