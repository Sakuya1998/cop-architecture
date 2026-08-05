---
id: COP-ARCH-002
title: COP Logical Architecture
status: draft
version: 0.2.0
owners:
  - architecture
last_updated: 2026-08-05
related:
  - COP-ARCH-001
  - COP-ARCH-003
  - COP-DOM-001
  - COP-INFRA-001
rfc: []
adr: []
---

# COP 逻辑架构

## Purpose

定义 COP 的责任平面、跨平面 Contracts、数据所有权和跨切面治理，明确同步请求与异步任务的故障边界。本文描述可审查的逻辑责任，不把逻辑关系误读为部署拓扑或产品实现方案。

## Scope

本文覆盖 Experience Boundary、Control Plane、Data Plane、Platform Data Services、External Adapter Boundary、Managed and Telemetry Systems，以及横跨这些责任的 Contracts、身份、授权、审计和可观测性。责任平面取代误导性的 Portal -> API -> Control -> Data -> Storage -> AI 线性链路：一次用户操作可以沿受治理的同步 Contract 查询，也可以提交异步意图并由 Data Plane 返回事实。

## Non-goals

- 不定义物理拓扑、集群或部署单元。
- 不定义 service、process、package 或 database 分解。
- 不规定具体 API、event、broker、protocol、port、timeout 或数值 SLO。
- 不负责 resource/workload 生命周期控制。
- 不把 AI Capability 作为 MVP 或关键路径，也不替代专门 RFC/ADR 的决策。

## Context

COP-ARCH-001 负责系统上下文和完整外部系统边界；COP-ARCH-003 负责 Control Plane 与 Data Plane 的详细信任和运行时关系；COP-DOM-001 负责 bounded contexts；COP-INFRA-001 负责物理基础设施。本文保持这些权威边界，使用责任平面表达职责与依赖方向。

## Architecture or Model

### Responsibility Model

责任平面是逻辑职责集合而非服务、进程、包或数据库。每个平面通过明确的 Contract 暴露能力并拥有其数据语义；跨平面依赖只沿授权方向发生。

### Logical Architecture Diagram

```mermaid
flowchart TB
  USERS[Users]
  CLIENTS[External API Clients]
  AI[Future AI Capability<br/>(optional governed consumer)]
  subgraph COP[COP Logical Architecture]
    subgraph EXPERIENCE[Experience Boundary]
      PORTAL[Portal]
      API[API Boundary]
    end
    CONTROL[Control Plane]
    DATA[Data Plane]
    DATA_SERVICES[Platform Data Services]
    CROSS[Cross-cutting Contracts]
    ADAPTERS[External Adapter Boundary]
  end
  EXTERNAL[Managed and Telemetry Systems]
  USERS --> PORTAL
  PORTAL -->|"Versioned Contracts"| API
  CLIENTS -->|"Versioned Contracts"| API
  AI -.->|"Future governed Contracts"| API
  API -->|"Commands and control queries"| CONTROL
  API -->|"Governed query Contracts"| DATA_SERVICES
  CONTROL -.->|"Authorized intent and tasks"| DATA
  DATA -.->|"Facts · progress · freshness · failure"| CONTROL
  CONTROL -->|"Domain Contracts"| DATA_SERVICES
  DATA -->|"Domain Contracts"| DATA_SERVICES
  DATA --> ADAPTERS --> EXTERNAL
  CROSS -.-> EXPERIENCE
  CROSS -.-> CONTROL
  CROSS -.-> DATA
  CROSS -.-> DATA_SERVICES
```

节点表示逻辑责任，不是服务或进程。实线表示同步 Contract 或逻辑依赖；虚线表示异步意图/事实或跨切面约束。External Adapter Boundary 仅强调 Data Plane 与 cloud、Kubernetes、Telemetry 执行集成；identity provider、notification 和 object storage 不被强制经过 Data Plane，而分别由 IAM、Alert/Notification 或拥有相应 Contract 的 Platform Data Services 对接。COP-ARCH-001 仍是完整外部系统的权威来源。

### Plane Responsibilities

#### Experience Boundary

Portal 负责交互、视图和导航。API Boundary 为 Portal 与外部客户端提供相同的 versioned Contracts，并处理 identity/tenant context、路由、protocol adaptation 和输入校验；它不承载领域规则或私有存储。Portal 不得绕过 API。

#### Control Plane

负责连接配置和 credential references、policy、desired state、task intent、orchestration 与生命周期。Control Plane 在验证 tenant/auth context 后接受或拒绝命令，向 Data Plane 发送 authorized intent；不处理 raw Telemetry，也不控制 resource/workload 生命周期。

#### Data Plane

负责经授权的 discovery、collection、sync、normalization 和 status reporting，并隔离 Adapter。它提供 backpressure、retry、checkpoint、duplicate protection 和局部故障隔离，报告 facts、progress、freshness、errors 与 recovery；不决定 tenant policy、final authorization、task intent 或 scope expansion。

#### Platform Data Services

拥有领域写模型、受治理读模型、cache/index 和 object reference，并且只通过 owner Contracts 访问。读模型声明 owner、source、update、freshness 和 failure 语义；derived cache/index 只是加速层，不产生新的 authority。raw Telemetry 与外部 object data 的 authority 仍在外部系统。

#### Cross-cutting Contracts

提供 IAM context、authorization、Audit 和 Platform Observability。不得使用共享可写数据库或绕过 ownership；不存在隐式内部信任。每次交互显式携带 org/tenant/subject/resource/auth context，关键动作必须审计，并提供 health/dependency 以及适用时的 processing、freshness、backlog、error、recovery signals。

#### Future AI Capability

Future AI Capability 是可选的未来消费者，不属于 MVP 或关键路径，必须使用相同的 governed versioned Contract，并遵循 tenant/auth/audit/source/freshness 语义。它不得拥有私有存储、credential 或 raw data 访问；接入前需要 roadmap 和 RFC/ADR review。

### Cross-plane Interactions

#### Dependency Rules

依赖以 Contract 为先并保持单向；不得形成循环同步依赖。跨平面不得直接写存储或读取私有存储。Experience 可使用受治理 Query Contracts/read models，不得访问共享数据库。Control 定义 intent，Data 执行并报告 facts；facts 不得静默改变 policy 或 desired state。

#### Command Path

命令路径校验 identity、tenant、resource 和 auth context，并区分 accepted、rejected、timeout、dependency failure 与 partial result。结果通过相应 Contract 返回，不能把未知结果伪装为成功。

#### Long-running Task Path

Discovery、sync 和 status propagation 采用异步路径；intent 先持久化，随后返回 facts、progress、freshness、failure 和 recovery。重试必须幂等或具备 duplicate protection，并支持 checkpoint、cancellation 与 resynchronization。

#### Query Path

查询可直接使用受治理 read models，无需经过 Data Plane；必须明确 owner、source、freshness 和 failure 语义。需要实时外部数据时，通过所属 integration Contract 发起 live external query。

### Data Ownership and Storage Access

| Data category | Logical owner | Access and semantics |
| --- | --- | --- |
| connection config / policy / desired state | COP domain owner | 仅通过 owner Contract 访问 |
| resource identity / catalog / relationships | Resource/Metadata owner | observations 通过 Contract，禁止私有表写入 |
| task intent / progress / failure / recovery | Control Plane lifecycle owner | facts 不得覆盖 intent |
| governed read models | Declared owner | 显式声明 source、update、freshness、failure |
| cache / index / replica | Source owner | 仅为 derived acceleration，不是新 authority |
| raw Telemetry / external object data | External authority | COP 只保留 context、association、reference |

逻辑所有权不决定 database 数量、schema、messaging 或 deployment 产品；这些属于后续受治理设计。

### Cross-cutting Governance

所有 Contract 显式携带 org/tenant/subject/resource/auth context，默认拒绝并遵循 least privilege。外部 claims 必须验证并映射 tenant/role；credential 仅以受控 references 传递，ordinary data、task 和 log 中不得出现 secrets。Audit 记录 subject、target、time、result。Platform Observability 提供健康、依赖、处理、freshness、backlog、error 和 recovery signals。跨切面能力不得建立共享可写数据库。

### Failure and Degradation

同步结果明确区分 rejection、timeout、dependency failure、partial 和 unknown outcome。异步生命周期定义 retry、idempotency 或 duplicate protection、checkpoint、cancellation 和 resynchronization；duplicate、delay、out-of-order 不得破坏不变量。按 account、cluster、Adapter 和 dependency 隔离故障，保持无关配置和查询可用。实施有界 backpressure，不允许无界队列或同步等待扩散到 Experience。stale、unknown、degraded、failed 状态必须包含 source/freshness，不能宣称 real-time success。恢复可 resync、重建 read models 并验证结果。

### Success Criteria

- 责任平面、Contracts、数据所有权和同步/异步故障边界可由评审者从本文独立识别。
- 八个责任模型小节与 Mermaid 关系保持一致，且不暗示物理拓扑或线性服务链。
- 所有跨平面读写遵循 owner Contract；read model、cache/index 和外部 authority 语义明确。
- 命令、长任务、查询及其拒绝、重试、幂等、隔离、退化和恢复行为均可追溯到责任方。
- IAM、授权、审计和可观测性形成跨切面约束，不依赖共享可写数据库或隐式信任。
- AI 保持可选、非 MVP 关键路径，并受 roadmap 与 RFC/ADR 治理。
- 文档保持 `draft`，重大决策仍通过 RFC/ADR，且不引入具体实现产品选择。

## Constraints

- 本文不得替代 COP-ARCH-001、COP-ARCH-003、COP-DOM-001 或 COP-INFRA-001 的职责边界。
- 重大架构变更必须经过 RFC/ADR；`draft` 文档不是 `cop-platform` 的强制实现依据。
- 不在本文决定物理部署、组件产品或具体协议参数。

## Quality Attributes

架构应具备清晰的 ownership 和审计可追溯性、默认拒绝的安全性、跨平面故障隔离与可恢复性、受治理读模型的可用性与 freshness 透明度、可通过 Contracts 演进的兼容性，以及在不把 AI 引入 MVP 关键路径的前提下的可扩展性与可运维性。

## Implementation Guidance

在本文状态变为 `accepted` 前，Codex 只能使用本文理解设计范围，不得据此生成生产实现。

## References

- [COP-ARCH-001](system-context.md)
- [COP-ARCH-003](control-plane-data-plane.md)
- [COP-DOM-001](../domains/domain-landscape.md)
- [COP-INFRA-001](../infrastructure/infrastructure-overview.md)
