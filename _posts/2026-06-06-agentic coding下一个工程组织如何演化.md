---
layout: post
title: "agentic coding下一个工程组织如何演化"
date: 2026-06-06 06:13:08 +0800
---

> 当 agentic coding（AI 主导编码）成为默认工作方式后，一个工程组织如何演化。



## 工程系统的“约束迁移”


过去的软件工程：
```
需求 → 设计 → 编码 → 测试 → 发布
```

其中最贵的是：**编码成本**

代码太贵，所以必须提前规划。为此诞生了：
```
长周期 roadmap
复杂 planning
Agile/Scrum
Jira 流程
多层审批
大量设计文档
```

但 AI 出现后：
```
代码生成成本 → 接近 0
```

于是系统约束发生迁移：

| 旧瓶颈              | 新瓶颈                 |
| ---------------- | ------------------- |
| 写代码              | 验证代码                |
| 人力不足             | 注意力不足               |
| 开发速度慢            | Review 跟不上          |
| 实现困难             | 判断困难                |
| Coding bandwidth | Cognitive bandwidth |

系统约束点迁移，这是非常典型的系统演化。


## 组织重构
Anthropic 的做法：

#### 规划：从长期规划 → JIT（Just In Time）

过去：
```
数月 roadmap
```

现在：
```
快速 prototype
→ 内部用户使用
→ 快速反馈
→ 持续调整
```

因为AI 提高了实现速度，因此：
```
未来不确定性 > 提前规划价值
```

于是：
```
规划周期必须缩短
```

这是经典的：
```
高变化率系统 → 需要短反馈环
```
#### 上下文获取：从“找人” → “问 AI”
过去：
```
谁写的代码？
```

现在：
```
Claude 知道上下文
```

当 AI 参与所有 commit 后：
```
代码 ownership 开始模糊
```


系统从：
```
human-memory system
```

变成：

```
AI-mediated memory system
```
组织知识不再存储在人脑里。而是：
```
AI + context layer
```
#### 代码审查：从“人 review 一切” → “AI review 普通问题，人 review 高阶问题”

AI 负责：
```
style
lint
test
obvious bug
PR suggestion
```

人类负责：
```
security
trust boundary
legal risk
architecture
product taste
```

核心：
```
低阶确定性问题 → AI
高阶模糊判断问题 → Human
```

#### 团队构成：岗位边界开始模糊

AI 降低了“技能执行门槛”，能力不再等于技能。
```
能力 = 判断 + 上下文 + taste
```

高阶变量：
```
system thinking
product sense
architecture judgment
constraint modeling
user understanding
```

过去：
```
软件工程的稀缺资源是“写代码的人力”。
```

现在：写代码已经不是瓶颈了，真正稀缺的是：
```
验证（verification）
审查（review）
安全（security）
判断（judgment）
产品感（taste）
```

## 组织从“执行机器”变成“认知系统”

传统软件组织：
```
组织 = 人力执行系统
```

AI-native 组织：
```
组织 = 认知协调系统
```

AI 已经承担大量执行。所以组织真正的问题变成：
```
什么值得做？
如何验证？
风险在哪里？
哪些约束不能突破？
如何协调多个 agent？
如何管理 context？
```


## 组织能做到的几个层级

大部分公司其实做的是，L1 优化（执行层）：

```
上 Copilot / Claude
鼓励写 prompt
提高 coding speed
```

但没有做，L2（流程）：

```
缩短 planning
减少文档
更快迭代
```

L3（结构）：
```

模糊角色边界
重构 ownership
重构 review 机制
AI 参与主流程
```


L4（目标函数）

```
从：
最大化工程效率

变成：
最大化认知质量 + 决策质量
```

end



