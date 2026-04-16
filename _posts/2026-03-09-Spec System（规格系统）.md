---
layout: post
title: "Spec System（规格系统）"
date: 2026-03-09 21:13:08 +0800
categories: [SDD]
tags: []
---


**Spec System（规格系统）** 是在 AI 时代对传统“需求文档 + 技术设计文档”的升级。
核心思想是：

```text
软件不是先写代码
而是先建立一套可执行的规格体系
```

然后：

```text
Spec → AI → Code
```

代码只是 **规格的实现结果**。

---

# 一、为什么需要 Spec System

单个 Spec 可以驱动一个任务，但大型项目会出现三个问题：

1️⃣ Spec之间没有关系
2️⃣ AI无法理解整体系统
3️⃣ 团队容易重复开发

因此必须建立：

```text
分层规格体系
```

即：

```text
System Spec
↓
Module Spec
↓
Task Spec
```

这就是 **Spec System**。

---

# 二、Spec System 的三层结构

完整结构：

```text
System Spec
↓
Module Spec
↓
Task Spec
```

可以理解为：

| 层级        | 控制范围 |
| ----------- | -------- |
| System Spec | 系统架构 |
| Module Spec | 功能模块 |
| Task Spec   | 具体任务 |

---

# 三、System Spec（系统规格）

System Spec 定义 **整个系统的规则与结构**。

它解决的问题：

```text
系统是什么
系统边界在哪里
系统由哪些模块组成
```

---

## System Spec 内容

一般包含五部分：

```text
系统目标
系统架构
模块划分
数据模型
技术约束
```

示例：

```text
System Spec

系统目标：
  构建一个在线订单系统

核心模块：
  User
  Order
  Payment
  Inventory

架构：
  API Service
  Worker
  Database

技术栈：
  Node.js
  PostgreSQL
  Redis
```

---

## System Spec 的作用

它决定：

```text
AI可以在哪里写代码
```

AI如果没有系统规格，很容易：

```text
创建混乱结构
```

---

# 四、Module Spec（模块规格）

Module Spec 描述 **一个功能模块的能力**。

例如：

```text
User Module
```

Module Spec 需要定义：

```text
模块职责
模块接口
模块数据
模块依赖
```

---

## Module Spec 示例

```text
Module: User

Responsibility:
  用户注册、登录、认证

APIs:
  createUser
  loginUser
  getUser

Data Model:
  User
  id
  email
  password_hash

Dependencies:
  Database
  AuthService
```

---

## Module Spec 的作用

解决一个问题：

```text
AI在哪里实现功能
```

否则 AI 可能：

```text
在多个地方实现同一功能
```

导致：

```text
重复模块
```

---

# 五、Task Spec（任务规格）

Task Spec 是 **最底层规格**。

用于描述：

```text
一个具体开发任务
```

例如：

```text
实现 createUser API
```

---

## Task Spec 示例

```text
Goal:
  实现 createUser API

Inputs:
  email
  password

Outputs:
  user_id

Constraints:
  使用 UserService
  密码必须 bcrypt 加密

Non-Goals:
  不实现邮箱验证
```

---

# 六、三层 Spec 如何协作

执行流程：

```text
System Spec
↓
Module Spec
↓
Task Spec
↓
AI生成代码
```

每一层约束下一层。

例如：

```text
System Spec
规定系统架构
```

↓

```text
Module Spec
规定模块职责
```

↓

```text
Task Spec
规定具体实现
```

AI只能在这个框架内工作。

---

# 七、Spec System 的工程结构

推荐在代码库中建立：

```text
/specs
```

目录。

结构示例：

```text
specs/

  system/
    system-spec.md

  modules/
    user-spec.md
    order-spec.md

  tasks/
    task-create-user.md
    task-create-order.md
```

这样 AI 可以读取整个规格体系。

---

# 八、Spec System 的优势

## 1 AI理解系统

AI可以读取：

```text
System Spec
Module Spec
```

从而理解：

```text
系统结构
```

---

## 2 避免重复开发

AI知道：

```text
模块已经存在
```

不会再创建：

```text
user-service2
```

---

## 3 控制系统复杂度

Spec System 实际上是：

```text
系统蓝图
```

所有代码必须符合它。

---

# 九、Spec System 与传统文档区别

传统文档：

```text
PRD
技术设计
API文档
```

问题：

```text
AI无法理解
```

Spec System：

```text
结构化
任务驱动
AI可执行
```

---

# 十、Spec System 的真正意义

传统软件开发：

```text
Code → System
```

AI开发：

```text
Spec → System → Code
```

代码只是：

```text
规格的实现
```

---

# 十一、Spec System 带来的团队变化

最明显变化是：

工程师的核心能力从：

```text
写代码
```

变成：

```text
设计规格
```

好的工程师能够：

```text
写清晰Spec
设计模块
控制系统
```

AI负责实现。

---

# 十二、Spec System 的最终形态

未来的软件项目可能是：

```text
Specs
AI Agents
Code
```

其中：

```text
Specs = 系统蓝图
Agents = 执行者
Code = 结果
```

---

**一句总结：**

```text
Spec System 是 AI 时代的软件蓝图系统
```

软件开发从：

```text
写代码
```

升级为：

```text
设计规格 + 驱动AI
```

---
