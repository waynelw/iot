# 物联网管理系统 Kubernetes 部署清单与 Helm Values 模板

**Document Version:** 1.1  
**Date:** 2026-03-08  
**Author:** System Architect  
**Status:** Detailed Draft

---

## 1. 文档目标

本文档用于指导物联网管理系统在 Kubernetes 上落地部署，覆盖以下内容：

- 集群与中间件部署边界
- Namespace、节点池、资源配额规划
- 平台服务部署清单
- 服务级部署规范
- 配置、Secret、证书与发布管理
- Helm `values.yaml` 通用模板与服务覆写样例
- HPA、PDB、亲和性、探针等生产配置建议
- 灰度发布、回滚与上线检查

本文档是 `docs/01-architecture-iot-platform-2026-03-08.md` 的部署落地补充，适用对象：

- `DevOps/SRE`
- `后端开发`
- `技术负责人`

---

## 2. 部署总体原则

### 2.1 设计原则

- **无状态服务容器化**：所有 Go 业务服务部署在 Kubernetes
- **状态中间件独立治理**：Kafka、PostgreSQL、Redis、对象存储优先托管或专属集群
- **双可用区部署**：避免单 AZ 故障导致平台整体不可用
- **故障域隔离**：接入层、业务层、观测层分节点池部署
- **配置与镜像分离**：应用镜像不携带环境差异，通过 Helm values 注入
- **默认支持弹性**：所有核心服务具备 HPA 与 PDB
- **最小权限**：ServiceAccount、NetworkPolicy、Secret 最小化授权

### 2.2 部署边界

#### 建议部署在 Kubernetes 的组件

- `APISIX`
- Go 微服务
- 定时任务与批处理 Job
- 观测代理与 Exporter
- 部分接入辅助组件

#### 不建议优先自建在 Kubernetes 的组件

- `Kafka`
- `PostgreSQL`
- `Redis`
- 对象存储

**原因：** 这些组件均为状态系统，在生产场景中更建议采用托管版或专属稳定集群，降低运维复杂度与数据风险。

---

## 3. 总体部署拓扑

### 3.1 逻辑部署拓扑

```mermaid
flowchart TB
    Internet[Internet / Corporate Network] --> LB[Public Load Balancer]
    Device[IoT Devices] --> MQTTE[MQTT Entry]
    Admin[Admin UI / OpenAPI Client] --> LB

    subgraph K8s[Kubernetes Cluster]
        subgraph GW[iot-gateway Namespace]
            APISIX[APISIX Ingress / Gateway]
            Auth[mqtt-auth-service]
            Ingest[message-ingest-service]
        end

        subgraph APP[iot-platform Namespace]
            DeviceSvc[device-service]
            ShadowSvc[shadow-service]
            CommandSvc[command-service]
            Dispatcher[command-dispatcher]
            RuleSvc[rule-service]
            AlarmSvc[alarm-service]
            OTASvc[ota-service]
            QuerySvc[query-service]
            OpenAPI[openapi-service]
            Webhook[webhook-service]
        end

        subgraph OBS[iot-observe Namespace]
            Prom[Prometheus]
            Grafana[Grafana]
            Loki[Loki / ELK]
            Tempo[Tempo]
        end
    end

    LB --> APISIX
    APISIX --> OpenAPI
    APISIX --> QuerySvc
    APISIX --> DeviceSvc
    MQTTE --> Auth
    MQTTE --> Ingest

    Ingest --> Kafka[(Kafka)]
    Dispatcher --> Kafka
    RuleSvc --> Kafka
    AlarmSvc --> Kafka
    CommandSvc --> Kafka

    DeviceSvc --> PG[(PostgreSQL)]
    ShadowSvc --> Redis[(Redis)]
    ShadowSvc --> PG
    CommandSvc --> PG
    QuerySvc --> PG
    QuerySvc --> TS[(TimescaleDB / ClickHouse)]
    OTASvc --> S3[(Object Storage)]
```

### 3.2 节点池拓扑

```mermaid
flowchart LR
    subgraph Cluster[Kubernetes Cluster]
        subgraph A[AZ-A]
            GW1[pool-gateway]
            APP1[pool-service]
            JOB1[pool-job]
            OBS1[pool-observe]
        end
        subgraph B[AZ-B]
            GW2[pool-gateway]
            APP2[pool-service]
            JOB2[pool-job]
            OBS2[pool-observe]
        end
    end

    GW1 --- GW2
    APP1 --- APP2
    JOB1 --- JOB2
    OBS1 --- OBS2
```

### 3.3 发布流向图

```mermaid
flowchart LR
    Dev[Developer] --> Git[Git Repository]
    Git --> CI[CI Pipeline]
    CI --> Image[Container Registry]
    CI --> Helm[Helm Chart Package]
    Image --> CD[CD / GitOps]
    Helm --> CD
    CD --> Staging[Staging Cluster]
    Staging --> Prod[Production Cluster]
```

---

## 4. 部署前检查清单

## 4.1 集群准备

- Kubernetes 版本已统一并稳定
- 至少双可用区或双节点池部署
- 节点资源满足接入、业务、作业、监控四类负载
- Ingress/LoadBalancer 已准备完成
- StorageClass 已验证可用
- 集群时间同步已配置
- 节点监控已接入
- CNI 插件与 DNS 服务稳定
- 镜像拉取链路已验证

## 4.2 网络与安全

- 域名已解析
- TLS 证书已签发
- 外部入口与内网访问策略已明确
- Namespace 网络策略已启用
- Pod 到中间件访问白名单已配置
- Secret 管理方式已确定
- 镜像仓库访问凭证已配置
- 服务账号权限已最小化
- OIDC / IAM 访问路径已验证

## 4.3 中间件依赖

- EMQX 可用
- Kafka 集群可用
- PostgreSQL 主备可用
- Redis 高可用可用
- 对象存储 Bucket 已创建
- IAM/OIDC 已准备完成
- 时序库已可连接

## 4.4 发布链路

- CI/CD 已配置构建、推镜像、部署流程
- Helm Chart 已准备
- 回滚策略已验证
- 配置差异已按环境隔离
- 发布窗口与变更审批流程已明确
- `helm template` 渲染校验已纳入流水线
- 制品版本与 Git commit 可追踪

---

## 5. Namespace、节点池与资源规划

## 5.1 命名空间建议

| Namespace | 用途 | 是否生产必需 |
|---|---|---|
| `iot-gateway` | APISIX、接入辅助组件、鉴权/消息入口服务 | 是 |
| `iot-platform` | Go 核心业务服务 | 是 |
| `iot-observe` | Prometheus、Grafana、Loki、Tempo | 是 |
| `iot-batch` | 导出、批处理、定时作业 | 建议 |
| `iot-tools` | 调试工具、迁移工具、一次性 Job | 建议 |

## 5.2 节点池建议

| 节点池 | 建议规格 | 承载服务 | 污点/亲和建议 |
|---|---|---|---|
| `pool-gateway` | 8 vCPU / 16 GB | APISIX、接入适配组件 | 允许网关专属调度 |
| `pool-service` | 16 vCPU / 32 GB | Go 核心服务 | 核心服务优先 |
| `pool-job` | 8 vCPU / 16 GB | 批处理与异步任务 | 与在线服务隔离 |
| `pool-observe` | 8 vCPU / 32 GB | 可观测组件 | 与业务流量隔离 |

## 5.3 ResourceQuota 建议

| Namespace | CPU 配额 | 内存配额 | Pod 数量 |
|---|---:|---:|---:|
| `iot-gateway` | 24 | 48Gi | 30 |
| `iot-platform` | 120 | 240Gi | 120 |
| `iot-batch` | 24 | 48Gi | 30 |
| `iot-observe` | 32 | 96Gi | 40 |

## 5.4 LimitRange 建议

- 默认 Pod `request`：`cpu=200m`、`memory=256Mi`
- 默认 Pod `limit`：`cpu=1`、`memory=1Gi`
- 核心服务必须显式指定资源，不允许走默认值

---

## 6. 平台服务部署清单

## 6.1 核心服务清单

- `mqtt-auth-service`
- `message-ingest-service`
- `device-service`
- `shadow-service`
- `command-service`
- `command-dispatcher`
- `rule-service`
- `alarm-service`
- `ota-service`
- `query-service`
- `openapi-service`
- `webhook-service`

## 6.2 建议暴露方式

| 服务 | 暴露方式 | 说明 |
|---|---|---|
| `openapi-service` | APISIX Route | 对外 REST API |
| `query-service` | APISIX Route | 后台查询接口 |
| `device-service` | 内网 Service / APISIX | 后台管理接口 |
| `ota-service` | 内网 Service / APISIX | 后台任务接口 |
| `mqtt-auth-service` | ClusterIP | 仅供 EMQX Hook 调用 |
| `message-ingest-service` | ClusterIP / gRPC | 仅内网入口 |
| `command-dispatcher` | 不对外 | 纯消费投递组件 |

## 6.3 每个服务部署时必须具备

- `Deployment`
- `Service`
- `ConfigMap`
- `Secret`
- `HorizontalPodAutoscaler`
- `PodDisruptionBudget`
- `ServiceMonitor`
- `Ingress` 或网关路由配置
- `NetworkPolicy`
- `ServiceAccount`

---

## 7. 服务级部署规范

## 7.1 Deployment 规范

- 副本数至少 `2`，核心服务至少 `3`
- 必须开启滚动更新策略
- 必须配置 `readinessProbe` 和 `livenessProbe`
- 必须限制单节点过度聚集，开启 `podAntiAffinity`
- 必须使用只读根文件系统
- 必须设置优雅退出时间，建议 `30~60s`

### 推荐滚动更新参数

- `maxUnavailable: 0`
- `maxSurge: 25%`

## 7.2 HPA 规范

| 服务 | 最小副本 | 最大副本 | 指标建议 |
|---|---:|---:|---|
| `mqtt-auth-service` | 3 | 10 | CPU + RPS |
| `message-ingest-service` | 4 | 12 | CPU + Kafka lag |
| `shadow-service` | 4 | 12 | CPU + Redis RTT |
| `command-service` | 3 | 10 | CPU + API latency |
| `command-dispatcher` | 4 | 12 | Kafka lag |
| `query-service` | 4 | 12 | CPU + P95 latency |
| `rule-service` | 4 | 12 | CPU + Kafka lag |

## 7.3 PDB 规范

- 核心服务：`minAvailable: 1` 或 `2`
- 单副本非核心服务：避免设置严格 PDB 阻碍节点维护

## 7.4 Probe 规范

### `livenessProbe`

- 用于发现死锁、goroutine 卡死、关键依赖失效后不可恢复状态
- 不要在探针中执行重 DB 查询

### `readinessProbe`

- 用于控制流量摘入/摘出
- 需要检查：配置加载、HTTP server 就绪、Kafka/Redis/PG 基础可用性

---

## 8. 配置、Secret 与证书管理

## 8.1 配置分类

### 放入 `ConfigMap`

- 日志级别
- 服务端口
- OTel 地址
- Kafka brokers
- Redis host
- Feature Flag
- 业务阈值参数

### 放入 `Secret`

- DB 用户名/密码
- Redis 密码
- Kafka SASL 凭证
- OIDC Client Secret
- 第三方通知通道密钥
- Webhook 签名密钥

## 8.2 证书管理建议

- 使用统一证书签发体系
- 内部服务建议 mTLS 或 service mesh 身份
- 证书轮换流程需纳入 Runbook

## 8.3 配置变更原则

- 配置变更必须通过 Git 或流水线审计
- 同一服务配置项命名保持一致前缀
- 禁止在容器镜像中写死环境连接串
- 所有生产 Secret 必须来源受控，不在仓库明文保存

---

## 9. NetworkPolicy 建议

### 9.1 网络最小化原则

- 默认拒绝跨 Namespace 任意访问
- 只放行必须访问路径
- 接入层可访问 `iot-platform` 的指定服务
- `iot-platform` 仅可访问必要中间件
- `iot-observe` 可抓取各服务 metrics

### 9.2 逻辑访问图

```mermaid
flowchart LR
    GW[iot-gateway] --> APP[iot-platform]
    APP --> Kafka[(Kafka)]
    APP --> PG[(PostgreSQL)]
    APP --> Redis[(Redis)]
    APP --> OBS[iot-observe]
    OBS --> APP
    TOOLS[iot-tools] -. restricted .-> APP
```

---

## 10. Helm Chart 目录建议

```text
charts/
  iot-service/
    Chart.yaml
    values.yaml
    values-dev.yaml
    values-staging.yaml
    values-prod.yaml
    templates/
      deployment.yaml
      service.yaml
      configmap.yaml
      secret.yaml
      hpa.yaml
      pdb.yaml
      servicemonitor.yaml
      serviceaccount.yaml
      networkpolicy.yaml
```

---

## 11. Helm Values 通用模板

以下模板适合作为 Go 微服务通用模板，再按服务覆写。

```yaml
nameOverride: ""
fullnameOverride: "mqtt-auth-service"

replicaCount: 3

image:
  repository: registry.example.com/iot/mqtt-auth-service
  tag: "v1.0.0"
  pullPolicy: IfNotPresent

imagePullSecrets:
  - name: regcred

serviceAccount:
  create: true
  annotations: {}
  name: ""

podAnnotations:
  prometheus.io/scrape: "true"
  prometheus.io/port: "9090"

podLabels: {}

podSecurityContext:
  runAsNonRoot: true
  fsGroup: 2000

securityContext:
  allowPrivilegeEscalation: false
  readOnlyRootFilesystem: true
  runAsUser: 10001
  runAsGroup: 10001
  capabilities:
    drop:
      - ALL

service:
  type: ClusterIP
  port: 8080
  metricsPort: 9090

ingress:
  enabled: false
  className: "nginx"
  annotations: {}
  hosts:
    - host: api.example.com
      paths:
        - path: /
          pathType: Prefix
  tls: []

resources:
  requests:
    cpu: "500m"
    memory: "512Mi"
  limits:
    cpu: "2"
    memory: "1Gi"

autoscaling:
  enabled: true
  minReplicas: 3
  maxReplicas: 10
  targetCPUUtilizationPercentage: 65
  targetMemoryUtilizationPercentage: 70

nodeSelector: {}

tolerations: []

affinity:
  podAntiAffinity:
    preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 100
        podAffinityTerm:
          topologyKey: kubernetes.io/hostname
          labelSelector:
            matchLabels:
              app.kubernetes.io/name: mqtt-auth-service

podDisruptionBudget:
  enabled: true
  minAvailable: 1

terminationGracePeriodSeconds: 30

probes:
  liveness:
    path: /healthz
    initialDelaySeconds: 10
    periodSeconds: 10
    timeoutSeconds: 2
  readiness:
    path: /readyz
    initialDelaySeconds: 5
    periodSeconds: 5
    timeoutSeconds: 2

config:
  appEnv: prod
  logLevel: info
  httpPort: "8080"
  metricsPort: "9090"
  otelEndpoint: tempo.iot-observe.svc.cluster.local:4317
  kafkaBrokers: kafka-0:9092,kafka-1:9092,kafka-2:9092
  redisAddr: redis-master.iot-platform.svc.cluster.local:6379
  pgDsn: postgresql://iot_app:change_me@postgres-rw.iot-platform.svc.cluster.local:5432/iot
  oidcIssuer: https://iam.example.com/realms/iot

secret:
  appSecret: "change-me"
  dbPassword: "change-me"
  redisPassword: "change-me"
  kafkaPassword: "change-me"

serviceMonitor:
  enabled: true
  interval: 30s
  scrapeTimeout: 10s

networkPolicy:
  enabled: true
  allowNamespaces:
    - iot-gateway
    - iot-observe
```

---

## 12. 服务级 Values 覆写示例

## 12.1 `message-ingest-service` 示例

```yaml
fullnameOverride: "message-ingest-service"

replicaCount: 4

resources:
  requests:
    cpu: "1"
    memory: "1Gi"
  limits:
    cpu: "4"
    memory: "2Gi"

autoscaling:
  enabled: true
  minReplicas: 4
  maxReplicas: 12
  targetCPUUtilizationPercentage: 65

nodeSelector:
  workload: gateway

config:
  consumerConcurrency: "8"
  maxMessageBytes: "2097152"
  kafkaTopicTelemetry: telemetry.raw
  kafkaTopicEvent: event.raw
```

## 12.2 `query-service` 示例

```yaml
fullnameOverride: "query-service"

replicaCount: 4

resources:
  requests:
    cpu: "1"
    memory: "1Gi"
  limits:
    cpu: "2"
    memory: "2Gi"

autoscaling:
  enabled: true
  minReplicas: 4
  maxReplicas: 12
  targetCPUUtilizationPercentage: 60

config:
  readReplicaEnabled: "true"
  redisCacheTTL: "60"
  queryTimeoutMs: "3000"
```

## 12.3 `command-dispatcher` 示例

```yaml
fullnameOverride: "command-dispatcher"

replicaCount: 4

resources:
  requests:
    cpu: "1"
    memory: "1Gi"
  limits:
    cpu: "4"
    memory: "2Gi"

autoscaling:
  enabled: true
  minReplicas: 4
  maxReplicas: 12

config:
  workerPoolSize: "64"
  kafkaTopicCommandRequested: command.requested
  kafkaTopicCommandAcked: command.acked
  dispatchTimeoutMs: "1500"
```

---

## 13. 环境差异模板建议

## 13.1 `values-dev.yaml`

- 副本数可降为 `1~2`
- 降低资源 request/limit
- 开启调试日志
- 可关闭 HPA
- 可关闭 PDB

## 13.2 `values-staging.yaml`

- 副本数接近生产
- 打开完整监控与 Trace
- 使用独立测试中间件
- 开启灰度发布验证
- 保留生产级 probes 和 network policy

## 13.3 `values-prod.yaml`

- 必须开启 HPA
- 必须开启 PDB
- 必须开启反亲和
- 使用正式域名与正式 Secret
- 健康检查与监控规则必须完整
- 日志级别默认 `info`
- 所有资源 request/limit 必须明确

---

## 14. 发布清单与检查项

## 14.1 服务部署检查项

| 检查项 | 必须 | 说明 |
|---|---|---|
| Deployment 副本数正确 | 是 | 与环境容量一致 |
| 镜像版本已确认 | 是 | 与发布单一致 |
| ConfigMap/Secret 已更新 | 是 | 且差异已审阅 |
| 健康检查可用 | 是 | `readiness/liveness` 可通过 |
| ServiceMonitor 已生效 | 是 | 指标可抓取 |
| HPA 已生效 | 是 | prod 必须开启 |
| PDB 已生效 | 是 | 避免节点维护全量中断 |
| 日志字段符合规范 | 是 | 含 traceId/requestId |
| Trace 已接入 | 是 | 便于联调与故障定位 |
| NetworkPolicy 已启用 | 是 | 最小化访问路径 |

## 14.2 发布顺序建议

1. 先发布基础公共服务
2. 再发布低风险后台服务
3. 再发布查询与命令服务
4. 最后发布接入相关服务

---

## 15. 灰度发布与回滚建议

### 15.1 灰度流程

```mermaid
flowchart LR
    A[部署到 Staging] --> B[基础验证]
    B --> C[生产灰度 10%]
    C --> D[观察关键指标]
    D --> E{指标正常?}
    E -->|是| F[扩到 50%]
    F --> G[继续观察]
    G --> H{指标正常?}
    H -->|是| I[全量发布]
    E -->|否| R[回滚]
    H -->|否| R
```

### 15.2 回滚条件

- P95 响应时间异常升高
- Kafka lag 明显积压
- 连接鉴权失败率异常
- 命令超时率异常
- 错误率超过阈值
- readiness 持续失败

### 15.3 回滚方式

- 使用 `helm rollback`
- 回滚配置与镜像版本
- 恢复上一个稳定 Release
- 记录变更与复盘结果

---

## 16. 上线前最终核对表

- 生产 `values-prod.yaml` 已评审
- 镜像 tag 与 Git commit 可对应
- 数据库变更脚本已审阅并回滚可行
- 核心大盘已准备
- 关键告警已打开
- 值班与升级路径已通知
- 回滚命令已预演
- 发布窗口已确认

---

## 17. 建议后续落地物

建议在本文档基础上继续补充以下具体交付物：

- `每个服务独立 Helm values 文件`
- `Deployment/HPA/PDB/NetworkPolicy 模板文件`
- `GitOps 目录结构`
- `staging/prod 发布流水线脚本`
- `APISIX Route 与 Upstream 样例`

---

## 18. 结论

本文档可作为 Kubernetes 落地部署基线，已经覆盖了从集群规划、服务部署、网络隔离、Helm 模板到发布回滚的关键内容。

若要继续细化，优先建议补两类内容：

1. **按服务拆分的 values 文件**，例如 `query-service`、`command-service`、`message-ingest-service`  
2. **可直接应用的 Helm Chart 模板与 GitOps 目录**，便于团队直接接入流水线  
