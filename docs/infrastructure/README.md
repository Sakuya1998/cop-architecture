# Infrastructure Index

## 分类目的

记录 COP 的部署拓扑、集群与网络边界、存储职责，以及可观测基础设施的运行约束。

| ID | Document | Priority | Status | Purpose |
| --- | --- | --- | --- | --- |
| COP-INFRA-001 | [COP Infrastructure Overview](infrastructure-overview.md) | P1 | draft | 描述 COP 管理面和可观测基础设施的总体部署边界及阶段演进。 |
| COP-INFRA-002 | [COP Kubernetes Topology](kubernetes-topology.md) | P2 | draft | 描述 COP 工作负载的集群、命名空间、隔离和调度边界。 |
| COP-INFRA-003 | [COP Data Storage Architecture](data-storage.md) | P1 | draft | 定义关系数据、缓存、遥测数据、对象数据和事件数据的职责边界。 |
| COP-INFRA-004 | [COP Observability Stack](observability-stack.md) | P1 | draft | 定义 OTel Collector、VictoriaMetrics、日志、追踪和查询组件的职责与数据路径。 |
| COP-INFRA-005 | [COP Network and Ingress](network-and-ingress.md) | P2 | draft | 定义外部入口、Gateway、服务流量、管理连接和网络信任边界。 |

## 推荐阅读顺序

1. COP-INFRA-001 COP Infrastructure Overview
2. COP-INFRA-002 COP Kubernetes Topology
3. COP-INFRA-005 COP Network and Ingress
4. COP-INFRA-003 COP Data Storage Architecture
5. COP-INFRA-004 COP Observability Stack

只有 `status: accepted` 的文档具有实现权威；`draft` 和 `review` 文档仅用于讨论与设计收敛。
