# 物联网管理系统每周迭代看板任务表

**Document Version:** 1.0  
**Date:** 2026-03-08  
**Author:** System Architect  
**Status:** Draft

---

## 1. 文档目标

本看板用于把 16 周实施计划转化为可跟踪的迭代任务表，适合导入 Jira、禅道、TAPD 或作为周会跟踪清单。

建议每周按照 4 个泳道跟踪：

- `Todo`
- `In Progress`
- `Testing`
- `Done`

建议每条任务增加以下字段：

- 任务编号
- 模块
- 负责人
- 优先级
- 预计工时
- 状态
- 依赖项
- 风险备注

---

## 2. 看板字段模板

| 字段 | 说明 |
|---|---|
| Task ID | 唯一编号，如 `BE-W05-001` |
| Iteration | 周次，如 `W05` |
| Module | 模块名称，如 `command-service` |
| Title | 任务标题 |
| Owner | 负责人 |
| Priority | `P0/P1/P2` |
| Estimate | 预计工时 |
| Status | `Todo/In Progress/Testing/Done/Blocked` |
| Dependency | 依赖任务或依赖团队 |
| Risk | 风险备注 |

---

## 3. 周迭代看板模板

## W01 看板

| Task ID | Module | Title | Owner | Priority | Estimate | Status | Dependency | Risk |
|---|---|---|---|---|---|---|---|---|
| PM-W01-001 | Scope | 冻结 MVP 范围 | 产品/架构 | P0 | 2d | Todo | 无 | 范围膨胀 |
| ARC-W01-002 | Capacity | 冻结容量目标 | 架构 | P0 | 1d | Todo | 无 | 指标不清 |
| ARC-W01-003 | SLO | 输出 SLO/SLA 指标表 | 架构 | P0 | 1d | Todo | 容量目标 | 指标无法量化 |
| QA-W01-004 | Test Plan | 输出测试范围矩阵 | 测试 | P1 | 1d | Todo | MVP 范围 | 场景遗漏 |

## W02 看板

| Task ID | Module | Title | Owner | Priority | Estimate | Status | Dependency | Risk |
|---|---|---|---|---|---|---|---|---|
| BE-W02-001 | Protocol | 输出 MQTT Topic 规范 | 后端 | P0 | 2d | Todo | W01 范围冻结 | 设备联调返工 |
| BE-W02-002 | API | 输出 OpenAPI 文档 v1 | 后端 | P0 | 2d | Todo | W01 范围冻结 | 前后端并行受阻 |
| BE-W02-003 | Kafka | 输出 Topic/分区设计 | 后端 | P0 | 1d | Todo | 协议模型 | 分区键错误 |
| FE-W02-004 | UI Spec | 对齐后台页面范围 | 前端 | P1 | 1d | Todo | OpenAPI 草案 | 页面返工 |
| QA-W02-005 | Cases | 补协议测试用例 | 测试 | P1 | 1d | Todo | Topic 规范 | 用例不完整 |

## W03 看板

| Task ID | Module | Title | Owner | Priority | Estimate | Status | Dependency | Risk |
|---|---|---|---|---|---|---|---|---|
| OPS-W03-001 | K8s | 搭建 dev/test 集群 | DevOps | P0 | 3d | Todo | 资源申请 | 环境延期 |
| OPS-W03-002 | Middleware | 部署 Kafka/PG/Redis | DevOps | P0 | 2d | Todo | K8s 可用 | 配置不稳定 |
| OPS-W03-003 | Gateway | 部署 APISIX/EMQX | DevOps | P0 | 2d | Todo | 网络与证书 | 接入不可测 |
| OPS-W03-004 | Registry | 配置镜像仓库与域名证书 | DevOps | P1 | 1d | Todo | 外部资源 | 证书配置问题 |

## W04 看板

| Task ID | Module | Title | Owner | Priority | Estimate | Status | Dependency | Risk |
|---|---|---|---|---|---|---|---|---|
| BE-W04-001 | Scaffold | 初始化 Go 工程骨架 | 后端 | P0 | 2d | Todo | 无 | 目录不统一 |
| BE-W04-002 | Common | 封装日志/配置/Trace | 后端 | P0 | 2d | Todo | Scaffold | 公共库反复修改 |
| OPS-W04-003 | CI/CD | 建立构建发布流水线 | DevOps | P0 | 2d | Todo | 镜像仓库 | 发布效率低 |
| QA-W04-004 | Baseline | 建立接口测试框架 | 测试 | P1 | 1d | Todo | OpenAPI | 自动化测试延期 |

## W05 看板

| Task ID | Module | Title | Owner | Priority | Estimate | Status | Dependency | Risk |
|---|---|---|---|---|---|---|---|---|
| BE-W05-001 | device-service | 实现设备注册接口 | 后端 | P0 | 2d | Todo | PG 表结构 | 主数据不稳定 |
| BE-W05-002 | mqtt-auth-service | 实现 MQTT 鉴权 Hook | 后端 | P0 | 2d | Todo | EMQX 环境 | 认证性能不足 |
| QA-W05-003 | Auth Test | 覆盖设备认证测试用例 | 测试 | P1 | 1d | Todo | W05 服务可测 | 接口变更频繁 |

## W06 看板

| Task ID | Module | Title | Owner | Priority | Estimate | Status | Dependency | Risk |
|---|---|---|---|---|---|---|---|---|
| BE-W06-001 | message-ingest | 实现上行消息接入 | 后端 | P0 | 3d | Todo | Kafka 环境 | 消息格式不稳 |
| BE-W06-002 | Kafka Topic | 初始化遥测/事件 Topic | 后端 | P0 | 1d | Todo | Kafka | 分区数不足 |
| QA-W06-003 | Ingest Test | 验证消息入站与非法报文处理 | 测试 | P1 | 1d | Todo | message-ingest | 场景覆盖不足 |

## W07 看板

| Task ID | Module | Title | Owner | Priority | Estimate | Status | Dependency | Risk |
|---|---|---|---|---|---|---|---|---|
| BE-W07-001 | shadow-service | 实现影子读写接口 | 后端 | P0 | 2d | Todo | Redis/PG | 热点冲突 |
| BE-W07-002 | Presence | 实现在线状态更新 | 后端 | P0 | 1d | Todo | 接入链路 | 心跳丢失 |
| QA-W07-003 | Shadow Test | 覆盖并发覆盖保护测试 | 测试 | P1 | 1d | Todo | shadow-service | 并发缺陷 |

## W08 看板

| Task ID | Module | Title | Owner | Priority | Estimate | Status | Dependency | Risk |
|---|---|---|---|---|---|---|---|---|
| BE-W08-001 | command-service | 实现命令创建接口 | 后端 | P0 | 2d | Todo | Device/Shadow | 状态机设计不完整 |
| BE-W08-002 | dispatcher | 实现命令投递器 | 后端 | P0 | 2d | Todo | Kafka/EMQX | 投递延迟高 |
| FE-W08-003 | Command UI | 开发命令下发页面 | 前端 | P1 | 2d | Todo | OpenAPI | 联调延期 |
| QA-W08-004 | Command Test | 覆盖回执与超时测试 | 测试 | P1 | 1d | Todo | W08 功能可用 | 回执模型变更 |

## W09 看板

| Task ID | Module | Title | Owner | Priority | Estimate | Status | Dependency | Risk |
|---|---|---|---|---|---|---|---|---|
| BE-W09-001 | query-service | 实现设备列表查询 | 后端 | P0 | 2d | Todo | PG/Redis | 查询慢 |
| FE-W09-002 | Device UI | 开发设备列表与详情页 | 前端 | P0 | 3d | Todo | query-service | 接口字段变更 |
| QA-W09-003 | Query Test | 覆盖分页/筛选/详情测试 | 测试 | P1 | 1d | Todo | W09 服务完成 | 数据样本不足 |

## W10 看板

| Task ID | Module | Title | Owner | Priority | Estimate | Status | Dependency | Risk |
|---|---|---|---|---|---|---|---|---|
| BE-W10-001 | rule-service | 实现阈值规则引擎 | 后端 | P0 | 3d | Todo | Kafka 遥测入站 | 规则误报 |
| BE-W10-002 | alarm-service | 实现告警生成与去重 | 后端 | P0 | 2d | Todo | rule-service | 告警风暴 |
| FE-W10-003 | Alarm UI | 开发告警页面 | 前端 | P1 | 2d | Todo | alarm API | 页面交互复杂 |

## W11 看板

| Task ID | Module | Title | Owner | Priority | Estimate | Status | Dependency | Risk |
|---|---|---|---|---|---|---|---|---|
| BE-W11-001 | ota-service | 实现固件与任务模型 | 后端 | P0 | 3d | Todo | 对象存储 | 元数据不稳定 |
| FE-W11-002 | OTA UI | 开发 OTA 页面 | 前端 | P1 | 2d | Todo | OTA API | 流程复杂 |
| QA-W11-003 | OTA Test | 覆盖暂停/取消/进度测试 | 测试 | P1 | 1d | Todo | OTA 功能可测 | 设备侧模拟不足 |

## W12 看板

| Task ID | Module | Title | Owner | Priority | Estimate | Status | Dependency | Risk |
|---|---|---|---|---|---|---|---|---|
| BE-W12-001 | IAM | 对接 OIDC 与 RBAC | 后端 | P0 | 2d | Todo | Keycloak | 权限模型复杂 |
| FE-W12-002 | RBAC UI | 开发用户/角色/审计页面 | 前端 | P1 | 2d | Todo | IAM API | 页面规则多 |
| QA-W12-003 | Security Test | 覆盖越权与租户隔离测试 | 测试 | P0 | 1d | Todo | IAM 完成 | 漏洞遗漏 |

## W13 看板

| Task ID | Module | Title | Owner | Priority | Estimate | Status | Dependency | Risk |
|---|---|---|---|---|---|---|---|---|
| OPS-W13-001 | Observe | 部署监控日志追踪栈 | DevOps | P0 | 3d | Todo | K8s 可用 | 可观测缺失 |
| BE-W13-002 | Metrics | 接入服务指标与 Trace | 后端 | P1 | 2d | Todo | Observe 栈 | 指标不统一 |
| QA-W13-003 | Monitor Verify | 校验告警规则触发 | 测试 | P1 | 1d | Todo | 监控完成 | 规则误报漏报 |

## W14 看板

| Task ID | Module | Title | Owner | Priority | Estimate | Status | Dependency | Risk |
|---|---|---|---|---|---|---|---|---|
| QA-W14-001 | E2E | 执行全链路联调回归 | 测试 | P0 | 3d | Todo | 核心模块完工 | 联调阻塞 |
| BE-W14-002 | Bugfix | 修复 P0/P1 缺陷 | 后端 | P0 | 2d | Todo | 联调报告 | 缺陷集中爆发 |
| FE-W14-003 | Bugfix | 修复前端联调问题 | 前端 | P1 | 2d | Todo | 联调报告 | 页面不稳定 |

## W15 看板

| Task ID | Module | Title | Owner | Priority | Estimate | Status | Dependency | Risk |
|---|---|---|---|---|---|---|---|---|
| QA-W15-001 | Perf | 执行 6 万连接压测 | 测试 | P0 | 2d | Todo | 模拟器就绪 | 机器资源不足 |
| QA-W15-002 | Perf | 执行 10k msg/s 压测 | 测试 | P0 | 2d | Todo | Kafka 稳定 | 积压严重 |
| OPS-W15-003 | Tuning | 调整 HPA/分区/实例规格 | DevOps | P0 | 2d | Todo | 压测结果 | 调优时间不足 |
| BE-W15-004 | SQL Tune | 调优慢查询与索引 | 后端 | P1 | 1d | Todo | 压测报告 | 查询瓶颈 |

## W16 看板

| Task ID | Module | Title | Owner | Priority | Estimate | Status | Dependency | Risk |
|---|---|---|---|---|---|---|---|---|
| OPS-W16-001 | Drill | 执行中间件故障演练 | DevOps | P0 | 2d | Todo | 生产候选环境 | 演练风险 |
| OPS-W16-002 | Release | 完成上线窗口与回滚准备 | DevOps | P0 | 1d | Todo | 演练通过 | 发布条件不足 |
| QA-W16-003 | Signoff | 输出上线验收结论 | 测试 | P0 | 1d | Todo | 缺陷清零 | 遗留问题 |
| ARC-W16-004 | Review | 完成最终上线评审 | 架构/管理 | P0 | 1d | Todo | 全部报告 | 风险未闭环 |

---

## 4. 看板使用建议

- 每周一更新本周 `Todo`
- 每天站会更新 `In Progress/Blocked`
- 每周四冻结新增需求，避免本周迭代失控
- 每周五输出 `Done` 与遗留问题清单
- `P0` 任务未完成时，不建议开启下周新范围

---

## 5. 状态定义建议

| 状态 | 定义 |
|---|---|
| Todo | 已确认待开始 |
| In Progress | 已开始开发/实施 |
| Testing | 已提测，等待验证 |
| Done | 已完成并验收 |
| Blocked | 被外部依赖阻塞 |

---

## 6. 结论

本看板可直接作为项目周会底稿，也适合继续转换成：

- Jira 任务导入表
- 禅道迭代任务表
- Excel 跟踪表
