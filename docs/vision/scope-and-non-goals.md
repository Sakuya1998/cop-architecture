---
id: COP-VIS-002
title: COP Scope and Non-goals
status: draft
version: 0.1.0
owners:
  - architecture
last_updated: 2026-08-03
related:
  - COP-VIS-001
  - COP-ROAD-002
  - COP-ARCH-001
rfc: []
adr: []
---

# COP 范围与非目标

## Purpose

明确 COP 的系统边界、阶段性范围和明确排除项。

## Scope

- 自托管优先
- SaaS-ready
- 平台职责
- 外部系统边界

## Non-goals

- 单一云厂商专用平台
- 业务应用托管
- 首版完整自动化和 AI

## Context

本文承接 `COP-VIS-001` 的平台方向，定义 COP 的职责边界和阶段边界。MVP 的详细完成条件由 `COP-ROAD-002` 定义，COP 与外部系统的关系由 `COP-ARCH-001` 定义。COP 是集成与协调层，不替代云厂商、业务平台或可观测存储引擎。

## Architecture or Model

### Responsibility Boundary

**COP 负责：**

- 建立统一资源模型和稳定资源身份。
- 维护资源目录和资源关系。
- 管理云账号、Kubernetes 集群的接入配置与凭据引用。
- 执行资源发现和同步任务。
- 提供 Metadata/CMDB 语义。
- 建立资源与 Telemetry、Alert、IAM、Audit 的关联。
- 提供统一查询、运营视图、Dashboard、Alert、IAM、Audit。
- 维护组织、租户和访问边界。

**COP 集成但不替代：**

- 云厂商 API、Kubernetes API、外部身份提供方、Telemetry Collectors、指标、日志和追踪存储、通知、对象存储及基础设施系统。
- COP 可以部署或连接这些组件，但不负责其内部实现。

**外部系统负责：**

- 云资源与 Kubernetes 工作负载的实际执行及最终状态。
- 业务应用的构建、发布和托管。
- 云厂商产品语义与基础设施实现。
- 可观测引擎内部的数据组织、查询执行和容量管理。

### Deployment Boundary

- 自托管是首要交付形态，使用组织运营管理面。
- 一个管理面可以连接多个云账号、Kubernetes 集群和受管环境。
- 受管环境只包含采集、连接和状态上报所需的最小组件。
- 身份、凭据、网络和数据传输边界必须明确。
- 业务工作负载无需迁入 COP 集群。
- 面向未来 SaaS，组织、租户、成员、资源归属和访问上下文保持明确。
- 数据模型和 API 不得假设全局只有一个组织或租户。
- 当前不包含 SaaS 计费、订阅、运营后台、客户生命周期和正式服务等级承诺。

### Stage Boundary

首版包括：

- 云账号和 Kubernetes 集群接入。
- 资源发现、采集和持续同步。
- Metadata/CMDB 与统一资源目录。
- 指标、日志和基础可观测信号关联。
- Dashboard 和告警。
- IAM、访问控制和审计。

首版明确排除：

- 业务应用托管。
- 单一云厂商专用平台。
- 资源创建、变更、删除和自动修复。
- Workflow、Automation、FinOps 和插件市场。
- AI/RAG 详细能力。
- SaaS 商业运营。
- 具体 API、服务拆分、部署参数和技术选型。

未来引入 Automation 和 AI 能力，必须先经过路线图、RFC/ADR 和相应权威文档评审。

### Scope Decision Rules

评估能力是否进入 COP 范围时，必须回答以下问题：

1. 是否直接服务平台工程、SRE 或运维团队？
2. 是否增强统一资源模型、运营上下文、治理或运营闭环？
3. 属于 COP 自有职责，还是外部系统集成？
4. 是否要求 COP 替代云厂商、业务平台或存储引擎？
5. 属于当前 MVP、后续路线图阶段，还是尚未评审的能力？
6. 是否影响组织、租户、凭据、网络或数据隔离边界？

职责不清、边界改变或重大职责变化，必须经过 RFC/ADR 和相关权威文档评审。

### Success Criteria

- 每项能力都能归入 COP 负责、COP 集成、外部负责或当前排除。
- 自托管运行不依赖官方 COP SaaS。
- 多云账号和多集群不改变核心资源模型。
- 业务工作负载无需迁入 COP 管理面。
- MVP 不依赖资源变更、Workflow、Automation 或 AI 才能形成完整闭环。
- 未来 SaaS 无需推翻组织、租户或资源归属模型。

## Constraints

- 不得绕过 RFC/ADR 流程引入重大架构决策。
- 不得与相关 `accepted` 权威文档产生冲突。
- 自托管部署必须能够独立运行，不依赖官方 COP SaaS。
- 外部系统保留资源执行及最终状态的控制权，COP 不得越过集成边界接管该职责。
- 组织、租户、身份、凭据、网络、数据传输和资源归属边界必须明确，并保持 SaaS-ready。

## Quality Attributes

- **职责清晰性：** 平台能力和外部依赖具有可判定、可审查的责任归属。
- **安全连接：** 外部系统接入明确身份、凭据、网络和数据传输边界。
- **租户隔离：** 组织、租户、成员、资源和访问上下文保持清晰隔离。
- **可演进性：** 自托管向未来 SaaS 演进时无需推翻核心资源和归属模型。
- **可运维性：** 多云账号、多集群和受管环境能够通过统一运营上下文进行管理。
- **兼容性：** 核心资源模型不绑定单一云厂商、集群或可观测存储引擎。

## Implementation Guidance

在本文状态变为 `accepted` 前，Codex 只能使用本文理解设计范围，不得据此生成生产实现。

## References

- [COP-VIS-001](platform-vision.md)
- [COP-ROAD-002](../roadmap/mvp-definition.md)
- [COP-ARCH-001](../architecture/system-context.md)
