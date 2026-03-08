# IoT Platform Knowledge Base

> 20 万设备物联网管理系统文档导航

---

## 概览

**状态说明：**

- `已完成`：当前版本可直接用于评审、实施或交付准备
- `待细化`：结构已具备，建议在项目推进中补充具体环境参数、责任人或脚本内容

这组文档围绕一个基于 `Go + Kubernetes + MQTT + Kafka` 的物联网管理系统展开，覆盖：

- 架构设计
- 实施排期
- 角色任务拆分
- 迭代执行看板
- Kubernetes 部署
- 性能压测
- 上线与故障处理

建议阅读顺序：

1. [总体架构方案](./01-architecture-iot-platform-2026-03-08.md)
2. [实施排期计划](./02-implementation-plan-iot-platform-2026-03-08.md)
3. [按角色拆分任务清单](./03-role-based-task-list-iot-platform-2026-03-08.md)
4. [每周迭代看板任务表](./04-iteration-board-iot-platform-2026-03-08.md)
5. [Kubernetes 部署清单与 Helm Values 模板](./05-kubernetes-deployment-checklist-and-helm-template-iot-platform-2026-03-08.md)
6. [压测方案与设备模拟器脚本清单](./06-performance-test-plan-and-device-simulator-checklist-iot-platform-2026-03-08.md)
7. [上线 Runbook 与故障处理手册](./07-runbook-and-incident-handbook-iot-platform-2026-03-08.md)

---

## 一、架构与规划

### 1. 总体架构

- [01-architecture-iot-platform-2026-03-08.md](./01-architecture-iot-platform-2026-03-08.md) `已完成`
- 内容包括：
  - 业务范围与容量假设
  - 关键架构决策过程
  - 总体架构与 Mermaid 图示
  - Go 技术栈建议
  - 资源清单与容量规划表

### 2. 实施计划

- [02-implementation-plan-iot-platform-2026-03-08.md](./02-implementation-plan-iot-platform-2026-03-08.md) `已完成`
- 内容包括：
  - 16 周排期
  - 每周目标
  - 每周任务清单
  - 每周交付物
  - 阶段验收标准

---

## 二、项目执行

### 3. 按角色任务拆分

- [03-role-based-task-list-iot-platform-2026-03-08.md](./03-role-based-task-list-iot-platform-2026-03-08.md) `已完成`
- 适合查看：
  - 后端任务
  - 前端任务
  - 测试任务
  - DevOps/SRE 任务

### 4. 每周迭代看板

- [04-iteration-board-iot-platform-2026-03-08.md](./04-iteration-board-iot-platform-2026-03-08.md) `已完成`
- 适合用于：
  - 周会跟踪
  - Jira/禅道录入前整理
  - 按周推进与状态管理

---

## 三、部署与交付

### 5. Kubernetes 部署与 Helm 模板

- [05-kubernetes-deployment-checklist-and-helm-template-iot-platform-2026-03-08.md](./05-kubernetes-deployment-checklist-and-helm-template-iot-platform-2026-03-08.md) `待细化`
- [05a-helm-chart-directory-example-iot-platform-2026-03-08.md](./05a-helm-chart-directory-example-iot-platform-2026-03-08.md) `已完成`
- [05b-helm-template-examples-iot-platform-2026-03-08.md](./05b-helm-template-examples-iot-platform-2026-03-08.md) `已完成`
- 内容包括：
  - 部署前检查清单
  - 命名空间与节点池建议
  - 服务部署清单
  - Helm `values.yaml` 模板
  - 发布与回滚建议

### 6. 压测方案与设备模拟器

- [06-performance-test-plan-and-device-simulator-checklist-iot-platform-2026-03-08.md](./06-performance-test-plan-and-device-simulator-checklist-iot-platform-2026-03-08.md) `待细化`
- 内容包括：
  - 压测目标
  - 场景设计
  - 指标定义
  - 执行步骤
  - 设备模拟器脚本清单

### 7. 上线 Runbook 与故障处理

- [07-runbook-and-incident-handbook-iot-platform-2026-03-08.md](./07-runbook-and-incident-handbook-iot-platform-2026-03-08.md) `待细化`
- 内容包括：
  - 上线前检查
  - 发布步骤
  - 上线后观察项
  - 常见故障排查与处理
  - 回滚与事故复盘模板

---

## 四、按角色阅读路径

### 架构师 / 技术负责人

- [总体架构方案](./01-architecture-iot-platform-2026-03-08.md)
- [实施排期计划](./02-implementation-plan-iot-platform-2026-03-08.md)
- [Kubernetes 部署清单与 Helm Values 模板](./05-kubernetes-deployment-checklist-and-helm-template-iot-platform-2026-03-08.md)
- [上线 Runbook 与故障处理手册](./07-runbook-and-incident-handbook-iot-platform-2026-03-08.md)

### 项目经理 / 交付经理

- [实施排期计划](./02-implementation-plan-iot-platform-2026-03-08.md)
- [按角色拆分任务清单](./03-role-based-task-list-iot-platform-2026-03-08.md)
- [每周迭代看板任务表](./04-iteration-board-iot-platform-2026-03-08.md)

### 后端开发

- [总体架构方案](./01-architecture-iot-platform-2026-03-08.md)
- [按角色拆分任务清单](./03-role-based-task-list-iot-platform-2026-03-08.md)
- [Kubernetes 部署清单与 Helm Values 模板](./05-kubernetes-deployment-checklist-and-helm-template-iot-platform-2026-03-08.md)
- [压测方案与设备模拟器脚本清单](./06-performance-test-plan-and-device-simulator-checklist-iot-platform-2026-03-08.md)

### 前端开发

- [实施排期计划](./02-implementation-plan-iot-platform-2026-03-08.md)
- [按角色拆分任务清单](./03-role-based-task-list-iot-platform-2026-03-08.md)
- [每周迭代看板任务表](./04-iteration-board-iot-platform-2026-03-08.md)

### 测试 / 性能测试

- [按角色拆分任务清单](./03-role-based-task-list-iot-platform-2026-03-08.md)
- [每周迭代看板任务表](./04-iteration-board-iot-platform-2026-03-08.md)
- [压测方案与设备模拟器脚本清单](./06-performance-test-plan-and-device-simulator-checklist-iot-platform-2026-03-08.md)
- [上线 Runbook 与故障处理手册](./07-runbook-and-incident-handbook-iot-platform-2026-03-08.md)

### DevOps / SRE

- [实施排期计划](./02-implementation-plan-iot-platform-2026-03-08.md)
- [按角色拆分任务清单](./03-role-based-task-list-iot-platform-2026-03-08.md)
- [Kubernetes 部署清单与 Helm Values 模板](./05-kubernetes-deployment-checklist-and-helm-template-iot-platform-2026-03-08.md)
- [压测方案与设备模拟器脚本清单](./06-performance-test-plan-and-device-simulator-checklist-iot-platform-2026-03-08.md)
- [上线 Runbook 与故障处理手册](./07-runbook-and-incident-handbook-iot-platform-2026-03-08.md)

---

## 五、推荐使用方式

### 项目启动阶段

- 先读 [总体架构方案](./01-architecture-iot-platform-2026-03-08.md)
- 再读 [实施排期计划](./02-implementation-plan-iot-platform-2026-03-08.md)

### 研发执行阶段

- 使用 [按角色拆分任务清单](./03-role-based-task-list-iot-platform-2026-03-08.md) 做任务下钻
- 使用 [每周迭代看板任务表](./04-iteration-board-iot-platform-2026-03-08.md) 做周会推进

### 部署上线阶段

- 使用 [Kubernetes 部署清单与 Helm Values 模板](./05-kubernetes-deployment-checklist-and-helm-template-iot-platform-2026-03-08.md) 检查环境
- 使用 [压测方案与设备模拟器脚本清单](./06-performance-test-plan-and-device-simulator-checklist-iot-platform-2026-03-08.md) 做性能验证
- 使用 [上线 Runbook 与故障处理手册](./07-runbook-and-incident-handbook-iot-platform-2026-03-08.md) 做上线与值班准备

---

## 六、文档状态总表

| 文档 | 状态 | 说明 |
|---|---|---|
| [00-iot-platform-doc-index-2026-03-08.md](./00-iot-platform-doc-index-2026-03-08.md) | 已完成 | 索引与阅读路径 |
| [01-architecture-iot-platform-2026-03-08.md](./01-architecture-iot-platform-2026-03-08.md) | 已完成 | 架构、决策、容量规划 |
| [02-implementation-plan-iot-platform-2026-03-08.md](./02-implementation-plan-iot-platform-2026-03-08.md) | 已完成 | 16 周实施计划 |
| [03-role-based-task-list-iot-platform-2026-03-08.md](./03-role-based-task-list-iot-platform-2026-03-08.md) | 已完成 | 按角色拆分任务 |
| [04-iteration-board-iot-platform-2026-03-08.md](./04-iteration-board-iot-platform-2026-03-08.md) | 已完成 | 周迭代任务看板 |
| [05-kubernetes-deployment-checklist-and-helm-template-iot-platform-2026-03-08.md](./05-kubernetes-deployment-checklist-and-helm-template-iot-platform-2026-03-08.md) | 待细化 | 建议继续细化为服务级 Helm values |
| [05a-helm-chart-directory-example-iot-platform-2026-03-08.md](./05a-helm-chart-directory-example-iot-platform-2026-03-08.md) | 已完成 | Helm Chart 目录组织与 values 合并样例 |
| [05b-helm-template-examples-iot-platform-2026-03-08.md](./05b-helm-template-examples-iot-platform-2026-03-08.md) | 已完成 | deployment/service/hpa 模板样例 |
| [06-performance-test-plan-and-device-simulator-checklist-iot-platform-2026-03-08.md](./06-performance-test-plan-and-device-simulator-checklist-iot-platform-2026-03-08.md) | 待细化 | 建议继续补充模拟器脚本与压测机规格 |
| [07-runbook-and-incident-handbook-iot-platform-2026-03-08.md](./07-runbook-and-incident-handbook-iot-platform-2026-03-08.md) | 待细化 | 建议继续补充按服务 SOP 与值班分级表 |

---

## 七、文档清单

- [00-iot-platform-doc-index-2026-03-08.md](./00-iot-platform-doc-index-2026-03-08.md)
- [01-architecture-iot-platform-2026-03-08.md](./01-architecture-iot-platform-2026-03-08.md)
- [02-implementation-plan-iot-platform-2026-03-08.md](./02-implementation-plan-iot-platform-2026-03-08.md)
- [03-role-based-task-list-iot-platform-2026-03-08.md](./03-role-based-task-list-iot-platform-2026-03-08.md)
- [04-iteration-board-iot-platform-2026-03-08.md](./04-iteration-board-iot-platform-2026-03-08.md)
- [05-kubernetes-deployment-checklist-and-helm-template-iot-platform-2026-03-08.md](./05-kubernetes-deployment-checklist-and-helm-template-iot-platform-2026-03-08.md)
- [05a-helm-chart-directory-example-iot-platform-2026-03-08.md](./05a-helm-chart-directory-example-iot-platform-2026-03-08.md)
- [05b-helm-template-examples-iot-platform-2026-03-08.md](./05b-helm-template-examples-iot-platform-2026-03-08.md)
- [06-performance-test-plan-and-device-simulator-checklist-iot-platform-2026-03-08.md](./06-performance-test-plan-and-device-simulator-checklist-iot-platform-2026-03-08.md)
- [07-runbook-and-incident-handbook-iot-platform-2026-03-08.md](./07-runbook-and-incident-handbook-iot-platform-2026-03-08.md)
