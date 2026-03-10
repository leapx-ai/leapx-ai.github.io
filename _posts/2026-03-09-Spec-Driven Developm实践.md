---
layout: post
title: "Spec-Driven Development（SDD）在实际项目中的样子"
date: 2026-03-09 21:13:08 +0800
categories: [SDD]
tags: [SDD]
---


下面用一个**真实项目形态**展示 **Spec-Driven Development（SDD）在实际项目中的样子**。
重点不是概念，而是让你看到：

- 项目目录
- Spec 怎么写
- AI如何执行
- 代码如何生成

示例项目：

```text
Online Order System
（在线订单系统）
```

功能：

- 用户下单
- 查询订单
- 取消订单

技术栈：

- Node.js
- PostgreSQL
- REST API

---

# 一、SDD项目整体结构

在 SDD 项目中，**Spec 是一级结构**。

项目结构通常是：

```text
repo/

  specs/
    system/
    modules/
    tasks/

  src/
    controllers/
    services/
    repositories/

  tests/

  ai-context/
```

含义：

| 目录       | 作用       |
| ---------- | ---------- |
| specs      | 系统规格   |
| src        | AI生成代码 |
| tests      | 自动测试   |
| ai-context | AI理解项目 |

---

# 二、System Spec（系统规格）

位置：

```text
specs/system/system-spec.md
```

内容包括：

```
系统目标
系统架构
模块划分
技术栈
数据模型
```

AI读取后可以理解：

```
整个系统结构
```

示例：

```text
# Online Order System

## Goal

构建一个简单订单系统，支持用户创建订单、查询订单、取消订单。

## Architecture

REST API Service
Worker Service
PostgreSQL Database

## Modules

User
Order
Inventory

## Tech Stack

Backend: Node.js
Database: PostgreSQL
Cache: Redis
```

作用：

AI在开发时会理解：

```text
系统有哪些模块
系统结构是什么
```

---

# 三、Module Spec（模块规格）

例如：

```text
specs/modules/order-module.md
```

示例：

```text
# Order Module

## Responsibility

管理订单生命周期。

## APIs

createOrder
getOrder
cancelOrder

## Data Model

Order

fields:
  id
  user_id
  product_id
  status
  created_at

status values:
  pending
  paid
  cancelled

## Dependencies

User Module
Inventory Module
```

作用：

AI知道：

```text
订单模块的职责
订单数据结构
模块依赖
```

这样AI 不会重复创建模块

---

# 四、Task Spec（任务规格）

Task Spec 是 **AI执行单元**，定义具体开发任务。

例如：

```text
specs/tasks/create-order-api.md
```

任务粒度：

```
一个任务 ≈ 一个API或功能
```

示例：

```text
# Task Spec

Goal:
实现 createOrder API。

Context:
属于 Order Module。
系统为 Node.js REST API。

Inputs:
user_id: string
product_id: string

Outputs:
{
  order_id: string
  status: "pending"
}

Constraints:
必须检查库存
订单默认状态为 pending
必须使用 OrderService

Non-Goals:
不实现支付逻辑
不实现订单取消
```

---

# 五、AI执行任务

工程师在 AI IDE 中执行。

例如使用：

- Cursor
- Visual Studio Code

工程师输入：

```text
根据 specs/tasks/create-order-api.md
实现 API
```

AI读取：

```
System Spec
Module Spec
Task Spec
```

然后生成代码。

---

# 六、AI生成代码结构

AI生成：

```text
src/

  controllers/
    order-controller.js

  services/
    order-service.js

  repositories/
    order-repository.js
```

示例代码：

```javascript
// order-controller.js

async function createOrder(req, res) {
  const { user_id, product_id } = req.body;

  const order = await orderService.createOrder(user_id, product_id);

  res.json(order);
}
```

---

Service：

```javascript
async function createOrder(userId, productId) {
  const order = await orderRepository.create({
    user_id: userId,
    product_id: productId,
    status: "pending",
  });

  return order;
}
```

---

# 七、AI生成测试

根据 Spec：

AI还可以生成测试。

例如：

```text
tests/order/create-order.test.js
```

示例：

```javascript
test("create order success", async () => {
  const response = await request(app).post("/orders").send({
    user_id: "u1",
    product_id: "p1",
  });

  expect(response.status).toBe(200);
});
```

---

# 八、SDD完整开发流程

真实开发流程如下：

```text
1 写 System Spec
2 写 Module Spec
3 写 Task Spec
4 AI生成代码
5 运行测试
6 修正 Spec
```

工程师很少直接写代码。

---

# 九、任务迭代示例

下一任务：

```text
specs/tasks/cancel-order-api.md
```

Spec：

```text
Goal:
  实现 cancelOrder API

Inputs:
  order_id

Outputs:
  status = cancelled

Constraints:
  只能取消 pending 状态订单
```

AI再次生成代码。

---

# 十、团队协作方式

团队协作变成：

Product：

```
写系统Spec
```

Architect：

```
写模块Spec
```

Engineer：

```
写任务Spec
```

AI：

```
实现代码
```

---

# 十一、SDD项目的真实形态

一个成熟 SDD 项目通常有：

```text
50+ Task Specs
10+ Module Specs
1 System Spec
```

AI根据这些：

```text
持续生成代码
```

---

# 十二、SDD带来的最大变化

传统项目：

```text
代码是核心资产
```

SDD项目：

```text
Specs是核心资产
```

代码只是：

```text
Specs的实现
```

---

# 十三、现实中的一个趋势

很多 AI 原生团队正在形成这样的项目结构：

```text
repo/

  specs/
  agents/
  code/
  tests/
```

系统开发逐渐变成：

```text
设计规格
编排AI
```

而不是：

```text
手写代码
```

---
