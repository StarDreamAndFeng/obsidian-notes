---
tags:
  - JavaScript
  - 浮点数
  - 精度计算
  - npm库
  - 格式化
date: 2026-08-08
---

# a-calc 高精度计算与格式化库

> **库定位**：解决 JavaScript 浮点数精度问题，同时提供强大的数字格式化能力
> **引入方式**：`const { calc, fmt } = require("a-calc/cjs")` 或 `import { calc, fmt } from "a-calc"`
> **完整文档**：[https://a-calc.vercel.app/](https://a-calc.vercel.app/)

---

## 概述

`a-calc` 是一个轻量且高性能的 JavaScript 精度计算与格式化库，具备以下特性：

| 特性 | 说明 |
|------|------|
| 精确计算 | 解决 IEEE 754 浮点数精度问题（`0.1 + 0.2 = 0.3`） |
| 丰富格式化 | 千分位、百分比、分数、科学计数法、紧凑格式、整数补零 |
| 单位计算 | 支持带单位（`%` 等）的数字运算 |
| 链式 API | 流畅的链式运算，支持多步操作 |
| 聚合函数 | `calc_sum`、`calc_avg`、`calc_max`、`calc_min`、`calc_count` |
| 多计算模式 | Decimal（默认）、BigInt、WASM 三种模式切换 |
| 高性能 | 同类库中处理速度最快 |
| TypeScript | 完整类型支持与智能推导 |

---

## 安装

```bash
npm install a-calc
```

---

## 引入方式

```javascript
// ES Module
import { calc, fmt, cadd } from "a-calc";

// CommonJS（Node.js / Webpack 旧项目）
const { calc, fmt } = require("a-calc/cjs");
```

---

## 核心 API

### 1. 表达式计算：`calc(expr, options?)`

将数学表达式以字符串形式传入，自动保证计算精度。

**基础运算**

```javascript
// 解决 0.1 + 0.2 精度问题
calc("0.1 + 0.2");                                    // "0.3"

// 复杂表达式，支持括号和优先级
calc("0.1 + 0.2 * 0.3 / 0.4 * (0.5 + 0.6)");          // "0.265"
```

**变量代入**

```javascript
// 简单变量
calc("a + b", { a: 1, b: 2 });                        // "3"

// 金额计算示例：单价 × 数量，保留两位小数
calc("price * qty | =2", { price: 9.9, qty: 3 });     // "29.70"

// 深层属性读取
calc("1 + o.a / arr[0].d", {
  o: { a: 2 },
  arr: [{ d: 8 }]
});                                                    // "1.25"
```

**三元运算**

```javascript
// 条件判断：大于 10 打 9 折
calc("a > 10 ? a * 0.9 : a", { a: 15 });              // "13.5"
```

**错误处理**

传入 `_error` 参数，在表达式非法或变量缺失时返回指定值而非抛出异常：

```javascript
// 变量缺失时返回 0
calc("a + b", { a: 1, _error: 0 });                   // 0

// 非法表达式返回占位符
calc("1 + invalid", { _error: "-" });                  // "-"
```

---

### 2. 数字格式化：`fmt(value, format?)`

v3.0 新增独立 API，用于纯格式化场景（无需进行计算）。

```javascript
// 千分位
fmt(1234567, ",");                   // "1,234,567"

// 固定两位小数
fmt(0.1234, "=2");                   // "0.12"

// 千分位 + 两位小数
fmt(1234567, "=2,");                 // "1,234,567.00"

// 紧凑格式（国际单位）
fmt(1234567, "!c");                  // "1.23M"

// 紧凑格式（中文万/亿）
fmt(1234567, "!c:wan");              // "123.45万"
```

格式化参数也可直接写在 `calc` 表达式中，用 `|` 与表达式主体分隔：

```javascript
calc("1234567 | ,");                 // "1,234,567"
calc("1234567 | =2,");               // "1,234,567.00"
calc("0.025 + 0.2 | %%");            // "22.5%"
```

---

## 格式化参数速查

### 小数位控制

| 参数 | 说明 | 示例 |
|------|------|------|
| `=N` | 固定 N 位小数 | `=2` → `"1.00"` |
| `<=N` | 最多 N 位小数（不补零） | `<=2` → `"1.1"` |
| `>=N` | 最少 N 位小数（不足补零） | `>=2` → `"1.00"` |

### 数字格式

| 参数 | 说明 | 示例 |
|------|------|------|
| `,` | 千分位分隔符 | `"1,000,000"` |
| `+` | 正号显示 | `"+100"` |
| `%%` | 百分比格式 | `"50%"` |
| `//` | 分数格式 | `"1/2"` |
| `!e` | 科学计数法 | `"1e+3"` |
| `!n` | 输出 number 类型（默认 string） | 返回 `1` |

### 紧凑与补零

| 参数 | 说明 | 示例 |
|------|------|------|
| `!c` | 国际紧凑格式 | `"1.23M"` / `"1.23K"` |
| `!c:wan` | 中文紧凑：万/亿 | `"123.45万"` |
| `!i:N` | 整数部分补零到 N 位 | `!i:3` → `"005"` |
| `!t:eu` | 欧洲千分位格式（点分号逗） | `"1.234,50"` |

### 舍入模式

| 参数 | 说明 |
|------|------|
| `~-` | 截断（默认） |
| `~5` | 四舍五入 |
| `~6` | 银行家舍入（四舍六入五取偶） |
| `~+` | 向上取整 |

---

## 链式计算 API

适合多次连续运算场景，以 `c` 前缀函数作为入口：

```javascript
import { cadd, csub, cmul, cdiv } from "a-calc";

// 多个参数相加
cadd(1, 2, 3)();                                       // "6"

// 链式：加完再乘，最后格式化
cadd(10).sub(3).mul(2)();                              // "14"

// 链式 + 格式化
cadd(1000, 2000)("=2,");                               // "3,000.00"

// 分步示例：(100 + 200) × 2 - 50 = 550
cadd(100, 200).mul(2).sub(50)();                       // "550"
```

---

## 独立运算函数

不需要格式化、不需要表达式解析时，使用独立函数性能更高。

### 返回 number 类型

```javascript
import { add, sub, mul, div } from "a-calc";

add(0.1, 0.2);    // 0.3   （number）
mul(3, 4, 5);     // 60    （number，支持多参数连乘）
div(10, 3);       // 3.3333333333333335
```

### 返回 string 类型（精度无损）

使用 `r` 前缀函数：

```javascript
import { radd, rsub, rmul, rdiv } from "a-calc";

radd("0.1", "0.2");  // "0.3"
rmul("9.9", "3");    // "29.7"
rdiv("10", "3");     // "3.33333333333333333333"
```

---

## 聚合函数

按字段名对对象数组执行统计运算：

```javascript
import { calc_sum, calc_avg, calc_max, calc_min, calc_count } from "a-calc";

const orders = [
  { price: 10, qty: 2 },
  { price: 20, qty: 3 },
  { price: 15, qty: 1 }
];

// 求和
calc_sum("price", orders);                             // "45"

// 求平均值
calc_avg("price", orders);                             // "15"

// 求最大
calc_max("price", orders);                             // "20"

// 求最小
calc_min("price", orders);                             // "10"

// 支持表达式作为字段（单价 × 数量的合计）
calc_sum("price * qty", orders);                       // "20 + 60 + 15" = "95"
```

---

## 全局配置

使用 `set_config` 设置全局默认行为，`reset_config` 还原。

```javascript
import { set_config, reset_config } from "a-calc";

// 错误时统一返回 0 而非 "-"
set_config({ _error: 0 });

// 全局默认格式化：两位小数 + 千分位
set_config({ _fmt: "=2," });

// 默认紧凑预设使用中文万/亿
set_config({ _compact_default: "wan" });

// 重置所有配置
reset_config();
```

---

## 常见业务场景示例

### 场景一：金额计算与格式化

```javascript
const { calc } = require("a-calc/cjs");

// 商品：单价 9.9，数量 3，服务费 5.5，折扣 8 折
const total = calc("(price * qty + serviceFee) * discount | =2,", {
  price: 9.9,
  qty: 3,
  serviceFee: 5.5,
  discount: 0.8
});
// 结果："28.16"（千分位 + 两位小数）
```

### 场景二：数组累加（库存出库数量）

```javascript
const { calc_sum } = require("a-calc/cjs");

const picks = [
  { num: 2 },
  { num: 2 },
  { num: 1.467865 },
  { num: 2.467865 },
  { num: 0.06427 }
];

const pickingNum = calc_sum("num", picks);
// 结果："8"（原生 JS 为 7.999999999999999）
```

### 场景三：统计报表格式化

```javascript
const { fmt } = require("a-calc/cjs");

const monthlyRevenue = 1234567.89;
const monthlyGrowth = 0.125;

// 报表展示：金额（千分位两位） + 增长率（百分比一位）
const displayAmount = fmt(monthlyRevenue, "=2,");       // "1,234,567.89"
const displayGrowth = fmt(monthlyGrowth, "=1%%");       // "12.5%"
```

---

## 方案对比

| 方案 | 精度 | 易用性 | 格式化能力 | 性能 | 推荐度 |
|------|------|--------|-----------|------|--------|
| 原生整数缩放（×10^n） | 高 | 中 | 无 | 最高 | ⭐⭐⭐ |
| `toFixed` 转数字 | 低 | 高 | 有限 | 高 | ⭐⭐ |
| `decimal.js` 类库 | 极高 | 低 | 弱 | 中 | ⭐⭐⭐ |
| **a-calc** | 极高 | 极高 | 极强 | 同类最快 | ⭐⭐⭐⭐⭐ |

**a-calc 核心优势**：
- 表达式形式最接近自然数学语法，学习成本极低
- 计算 + 格式化一步完成，省去额外处理步骤
- 性能优于 mathjs 等同类库

---

## 注意事项

1. **返回类型**：`calc` 默认返回 `string` 类型，如需参与比较运算，可加 `!n` 参数或手动 `Number()` 转换
2. **变量缺失**：表达式中用到的变量必须在 options 中完整传入，否则触发错误；建议配合 `_error` 使用
3. **格式化管道符**：`|` 是计算与格式化的分隔符，表达式中的逻辑或运算需使用 `||`
4. **性能选择**：单次简单加减优先使用 `add/sub/mul/div` 独立函数，避免解析表达式的开销

---

**关联笔记：**

- [[JavaScript浮点数精度问题与解决方案]] - 精度问题根源与其他方案
