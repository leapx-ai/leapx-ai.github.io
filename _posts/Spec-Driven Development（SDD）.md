---
layout: post
title: "Spec-Driven Development"
date: 2026-02-22 21:13:08 +0800
categories: [SDD]
tags: [SDD]
---

# Spec-Driven Development（SDD）

（AI时代的软件开发方法）

**Spec-Driven Development** 的核心思想是：

```text
系统由规格驱动，而不是由代码驱动
```

传统开发：

```text
需求 → 设计 → 写代码 → 系统
```

SDD：

```text
Spec → AI实现 → 系统
```

代码只是 **规格的自动实现结果**。

---

# 一、为什么 SDD 会出现

SDD 的出现是因为两个技术变化：

### 1 代码生成能力突破

大型模型已经能稳定生成：

- API
- CRUD
- 前后端逻辑
- 测试代码

典型代表：

- GPT
- Claude

因此：

```text
写代码不再是稀缺能力
```

---

### 2 Repo级理解能力

AI已经可以理解：

```text
整个代码库结构
跨文件依赖
模块关系
```

因此 AI 可以根据 **规格** 完整实现功能。

---

# 二、SDD 的核心思想

SDD 有三条核心原则。

---

## 原则1：Spec 是唯一事实来源（非常重要）

系统逻辑必须写在：

```text
Spec
```

而不是：

```text
代码
```

代码只是：

```text
Spec 的实现
```

---

## 原则2：Spec 必须结构化

Spec 不是自然语言需求。

它必须包含：

```text
目标
输入
输出
约束
边界
```

AI 才能执行。

---

## 原则3：开发过程围绕 Spec

开发流程变成：

```text
写 Spec
↓
AI生成代码
↓
运行测试
↓
调整 Spec
```

而不是：

```text
写代码 → 修 bug
```

---

# 三、SDD 的三层规格体系

SDD 的基础是 **Spec System**。

结构：

```text
System Spec
↓
Module Spec
↓
Task Spec
```

---

## System Spec

描述系统结构：

```text
模块划分
架构
数据模型
技术约束
```

---

## Module Spec

描述模块职责：

```text
模块接口
数据结构
依赖关系
```

---

## Task Spec

描述具体任务：

```text
实现 API
实现逻辑
实现组件
```

AI根据 Task Spec 生成代码。

---

# 四、SDD 开发流程

完整 SDD 流程：

```text
1 定义系统 Spec
2 定义模块 Spec
3 编写任务 Spec
4 AI生成代码
5 自动测试
6 修订 Spec
```

流程核心是：

```text
Spec → AI → Code
```

---

# 五、SDD 的实际开发示例

例如开发订单系统。

---

### Step 1 System Spec

```text
系统模块：

User
Order
Payment
Inventory
```

---

### Step 2 Module Spec

Order 模块：

```text
职责：
管理订单生命周期

接口：
createOrder
cancelOrder
getOrder
```

---

### Step 3 Task Spec

```text
Goal:
实现 createOrder API

Inputs:
user_id
product_id

Outputs:
order_id

Constraints:
必须校验库存
订单状态默认 pending
```

---

### Step 4 AI 实现

AI生成：

```text
controller
service
repository
tests
```

工程师验证即可。

---

# 六、SDD 与 Vibecoding 的关系

Vibecoding：

```text
自然语言驱动 AI 编码
```

但它往往是：

```text
个人开发习惯
```

SDD 是：

```text
工程化 Vibecoding
```

区别：

| Vibecoding | SDD        |
| ---------- | ---------- |
| 个人行为   | 团队方法   |
| 自然语言   | 结构化Spec |
| 临时Prompt | 标准Spec   |
| 即兴开发   | 规范流程   |

---

# 七、SDD 带来的效率变化

传统开发时间分布：

```text
需求理解 20%
编码 50%
调试 30%
```

SDD：

```text
Spec设计 40%
AI生成 20%
验证测试 40%
```

编码时间大幅下降。

---

# 八、SDD 对工程师能力要求

能力结构变化。

传统工程师：

```text
编码
算法
调试
```

SDD 工程师：

```text
系统设计
规格设计
AI协作
```

核心能力变成：

```text
Spec设计
```

---

# 九、SDD 的工程结构

典型项目结构：

```text
repo/

  specs/
    system/
    modules/
    tasks/

  src/
  tests/
```

AI根据：

```text
specs
```

生成：

```text
src
```

代码。

---

# 十、SDD 最大优势

SDD 解决了 AI 开发最严重问题：

```text
AI写代码很快
但系统容易混乱
```

Spec 作为：

```text
系统蓝图
```

可以稳定结构。

---

# 十一、SDD 的组织意义

SDD 本质上是一种：

```text
AI时代的软件生产体系
```

它把软件开发从：

```text
Code Driven
```

转变为：

```text
Spec Driven
```

---

# 十二、未来趋势

未来的软件项目可能是：

```text
Specs
↓
Agents
↓
Code
```

开发流程：

```text
写规格
AI实现
自动测试
持续迭代
```

代码会越来越自动化。

---

**一句话总结：**

```text
SDD = 用规格驱动AI完成软件开发
```

软件工程的核心将从：

```text
写代码
```

转变为：

```text
设计系统规格
```
