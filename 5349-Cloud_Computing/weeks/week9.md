# COMP5349 Week 9 — CodePipeline & Decoupled Architecture

---

# 🧠 总体主线

DevOps → CI/CD → CodePipeline → Decoupled Architecture → SQS → SNS

一句话总结：
> 自动化软件发布 + 构建低耦合、可扩展系统

---

# 1. DevOps & CI/CD

## 1.1 DevOps

### 定义
DevOps = Development + Operations  
目的是提高软件交付速度与可靠性。

### 核心思想
- Automation（自动化）
- Collaboration（协作）
- Fast feedback（快速反馈）
- Shared responsibility（共同负责）

---

## 1.2 传统问题（为什么需要DevOps）

- 手动部署 → 慢 + 易出错
- 环境不一致 → "it works on my machine"
- Bug发现晚
- 基础设施不可复现

---

## 1.3 CI/CD

### CI（Continuous Integration）
- 频繁提交代码
- 自动 build + test

### CD（Continuous Delivery / Deployment）

- Continuous Delivery：自动发布到仓库
- Continuous Deployment：自动部署到生产环境

---

## 1.4 标准流程
Code → Build → Test → Deploy → Monitor

---

# 2. AWS CodePipeline

## 2.1 定义

CodePipeline 是一个 CI/CD 自动化工具，用于协调软件发布流程。

👉 本质：**Pipeline Orchestrator（调度器）**

---

## 2.2 Pipeline结构

### Pipeline = 多个 Stage
### Stage = 多个 Action

---

## 2.3 常见Stage

- Source（获取代码）
- Build（构建）
- Test（测试）
- Deploy（部署）
- Approval（人工审核）

⚠️ 实际可以简化（Lab）：
Source → Deploy

---

## 2.4 Pipeline执行

- 每次运行 = 一次 execution
- 按顺序执行 stage
- 任一 stage 失败 → pipeline停止

---

## 2.5 Debug方法（重要）

如果失败：

1. 查看 CodePipeline 执行详情
2. 查看 CloudFormation Stack Events
3. 查看 CloudWatch Logs

---

## 2.6 常见失败原因

- CloudFormation 模板错误
- IAM 权限问题
- 资源冲突
- 依赖顺序错误

---

## 2.7 安全Pipeline设计（高分）
Source
→ Validate (cfn-lint)
→ Create Change Set
→ Manual Approval
→ Deploy

---

# 3. Decoupled Architecture（解耦架构）

## 3.1 Tight Coupling（强耦合）

特点：
- 同步通信（必须等待响应）
- 服务直接依赖

问题：
- 单点故障
- 扩展困难
- 系统复杂

---

## 3.2 Loose Coupling（弱耦合）

特点：
- 异步通信
- 服务独立
- 易扩展

---

## 3.3 三层架构问题

强耦合：
- Web → App 直接调用
- 扩展需要修改代码
- 一个服务挂 → 全部挂

---

## 3.4 解耦方案

- Load Balancer
- Auto Scaling
- 异步通信

---

## 3.5 Microservices（微服务）

将系统拆分为多个独立服务，例如：

- Finance Service
- Image Processing
- Calculation Service

优点：
- 独立部署
- 故障隔离
- 易扩展

---

# 4. Amazon SQS

## 4.1 定义

SQS = 消息队列服务（Queue）

👉 模型：**1对1（Producer → Consumer）**

---

## 4.2 基本结构
Producer → Queue → Consumer

---

## 4.3 作用

- 解耦系统
- 异步处理
- 提高可扩展性

---

## 4.4 Queue类型

### Standard Queue
- 至少一次（可能重复）
- 顺序不保证
- 高吞吐

### FIFO Queue
- 顺序保证
- Exactly-once

---

## 4.5 关键概念

### Visibility Timeout（重要）
- 消息被消费后暂时隐藏
- 防止重复处理

---

### Dead Letter Queue（DLQ）
- 存储处理失败的消息

---

## 4.6 消息生命周期
- Send message
- Receive message（进入visibility timeout）
- Process
- Delete message


---

## 4.7 Polling方式

- Short Polling（快速但不稳定）
- Long Polling（推荐）

---

# 5. Amazon SNS

## 5.1 定义

SNS = 发布/订阅系统（Pub/Sub）

👉 模型：**1对多（广播）**

---

## 5.2 基本结构
Publisher → Topic → Subscribers

---

## 5.3 特点

- Push机制（自动推送）
- 支持多个订阅者
- 不存储消息

---

## 5.4 Subscriber类型

- Email
- HTTP/HTTPS
- Lambda
- SQS

---

## 5.5 使用场景

- 系统通知
- 告警系统
- 广播消息

---

# 6. SNS vs SQS（重点对比）

| 特性 | SQS | SNS |
|------|-----|-----|
| 模型 | Producer-Consumer | Pub-Sub |
| 通信 | 1对1 | 1对多 |
| 机制 | Pull | Push |
| 存储 | 有 | 无 |
| 用途 | 任务处理 | 消息广播 |

---

# 7. SNS + SQS 组合（高分）

## 架构
S3 → SNS → 多个 SQS → 多个服务

---

## 流程

1. 用户上传文件到 S3
2. S3 触发 SNS
3. SNS 广播消息
4. 多个 SQS 接收
5. 不同服务分别处理任务

---

## 优势

- 高扩展性
- 松耦合
- 并行处理

---

# 8. 权限控制（重点）

## 关键链路

### 1. S3 → SNS
- 权限：sns:Publish
- 配置：SNS Topic Policy

---

### 2. SNS → SQS
- 权限：sqs:SendMessage
- 配置：SQS Queue Policy

---

### 3. EC2 → SQS
- 权限：
  - ReceiveMessage
  - DeleteMessage
- 配置：IAM Role

---

### 4. EC2 → S3
- 权限：
  - GetObject
  - PutObject
- 配置：IAM Role

---

# 🎯 考试总结

## 三大核心
- CI/CD 自动化发布
- CodePipeline 流程控制
- 解耦架构提高扩展性

---

## 两种通信模式
- SQS：1对1（队列）
- SNS：1对多（广播）

---

## 系统设计目标
- High Availability
- Scalability
- Fault Isolation

---

# 🧩 一句话总结

Week 9 = 自动化部署 + 解耦架构 + 消息驱动系统
