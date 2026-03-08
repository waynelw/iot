# 物联网管理系统文档总览

**Date:** 2026-03-08  
**Scope:** 20 万设备物联网管理系统方案与落地文档索引

---

## 1. 阅读顺序建议

建议按以下顺序查看：

1. 总体架构方案
2. 实施排期计划
3. 角色任务拆分
4. 周迭代看板
5. Kubernetes 部署与 Helm 模板
6. 压测方案与设备模拟器清单
7. 上线 Runbook 与故障处理手册

---

## 2. 文档分类目录

### A. 架构与实施总览

1. `docs/01-architecture-iot-platform-2026-03-08.md`
   - 内容：总体架构、决策过程、技术栈、容量规划
   - 适合：架构师、技术负责人、项目负责人

2. `docs/02-implementation-plan-iot-platform-2026-03-08.md`
   - 内容：16 周实施排期、每周目标、交付物、阶段验收
   - 适合：项目经理、技术负责人、交付负责人

### B. 项目执行与协同

3. `docs/03-role-based-task-list-iot-platform-2026-03-08.md`
   - 内容：后端、前端、测试、DevOps 按角色拆分任务清单
   - 适合：各角色负责人、项目经理

4. `docs/04-iteration-board-iot-platform-2026-03-08.md`
   - 内容：W01~W16 周迭代看板任务表
   - 适合：周会跟踪、迭代管理、Jira/禅道录入前整理

### C. 平台部署与运维

5. `docs/05-kubernetes-deployment-checklist-and-helm-template-iot-platform-2026-03-08.md`
   - 内容：Kubernetes 部署清单、环境检查项、Helm values 模板
   - 适合：DevOps/SRE、后端负责人

6. `docs/06-performance-test-plan-and-device-simulator-checklist-iot-platform-2026-03-08.md`
   - 内容：压测目标、压测场景、设备模拟器脚本清单、执行步骤
   - 适合：测试、性能团队、DevOps、后端

7. `docs/07-runbook-and-incident-handbook-iot-platform-2026-03-08.md`
   - 内容：上线 Runbook、故障处理、回滚手册、事故复盘模板
   - 适合：SRE、值班工程师、技术负责人

---

## 3. 按角色阅读建议

### 架构/技术负责人

- `docs/01-architecture-iot-platform-2026-03-08.md`
- `docs/02-implementation-plan-iot-platform-2026-03-08.md`
- `docs/05-kubernetes-deployment-checklist-and-helm-template-iot-platform-2026-03-08.md`
- `docs/07-runbook-and-incident-handbook-iot-platform-2026-03-08.md`

### 项目经理 / 交付经理

- `docs/02-implementation-plan-iot-platform-2026-03-08.md`
- `docs/03-role-based-task-list-iot-platform-2026-03-08.md`
- `docs/04-iteration-board-iot-platform-2026-03-08.md`

### 后端开发

- `docs/01-architecture-iot-platform-2026-03-08.md`
- `docs/03-role-based-task-list-iot-platform-2026-03-08.md`
- `docs/05-kubernetes-deployment-checklist-and-helm-template-iot-platform-2026-03-08.md`
- `docs/06-performance-test-plan-and-device-simulator-checklist-iot-platform-2026-03-08.md`

### 前端开发

- `docs/02-implementation-plan-iot-platform-2026-03-08.md`
- `docs/03-role-based-task-list-iot-platform-2026-03-08.md`
- `docs/04-iteration-board-iot-platform-2026-03-08.md`

### 测试 / 性能测试

- `docs/03-role-based-task-list-iot-platform-2026-03-08.md`
- `docs/04-iteration-board-iot-platform-2026-03-08.md`
- `docs/06-performance-test-plan-and-device-simulator-checklist-iot-platform-2026-03-08.md`
- `docs/07-runbook-and-incident-handbook-iot-platform-2026-03-08.md`

### DevOps / SRE

- `docs/02-implementation-plan-iot-platform-2026-03-08.md`
- `docs/03-role-based-task-list-iot-platform-2026-03-08.md`
- `docs/05-kubernetes-deployment-checklist-and-helm-template-iot-platform-2026-03-08.md`
- `docs/06-performance-test-plan-and-device-simulator-checklist-iot-platform-2026-03-08.md`
- `docs/07-runbook-and-incident-handbook-iot-platform-2026-03-08.md`

---

## 4. 推荐使用方式

- 先读 `01` 和 `02`，建立全局理解
- 再按角色阅读 `03` 和 `04`
- 部署与上线前重点看 `05`、`06`、`07`
- 周会使用 `04`，上线前评审使用 `07`

---

## 5. 当前文档清单

```text
00-iot-platform-doc-index-2026-03-08.md
01-architecture-iot-platform-2026-03-08.md
02-implementation-plan-iot-platform-2026-03-08.md
03-role-based-task-list-iot-platform-2026-03-08.md
04-iteration-board-iot-platform-2026-03-08.md
05-kubernetes-deployment-checklist-and-helm-template-iot-platform-2026-03-08.md
06-performance-test-plan-and-device-simulator-checklist-iot-platform-2026-03-08.md
07-runbook-and-incident-handbook-iot-platform-2026-03-08.md
```
