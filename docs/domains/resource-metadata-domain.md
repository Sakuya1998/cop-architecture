---
id: COP-DOM-003
title: COP Resource Metadata Domain
status: draft
version: 0.1.0
owners:
  - architecture
last_updated: 2026-08-03
related:
  - COP-DOM-001
  - COP-DOM-004
  - COP-DOM-005
  - COP-STD-001
rfc: []
adr: []
---

# COP 资源与元数据领域

## Purpose

定义 Resource、Resource Type、Attribute、Relationship 和 CMDB 语义。

## Scope

- 资源身份
- 类型
- 属性
- 关系
- 来源
- 生命周期

## Non-goals

- 遥测时序数据
- 告警状态
- 云凭据

## Context

本文是 COP 架构文档体系初始化产生的 `draft` 文档。详细设计必须经过专项讨论和评审；在状态变为 `accepted` 前，不得作为 `cop-platform` 的强制实现依据。

## Ubiquitous Language

本领域的正式术语将在领域评审中定义，并与 `COP-STD-001` 保持一致。

## Bounded Context

当前尚未形成已接受的边界上下文设计。

## Aggregates and Entities

当前尚未形成已接受的聚合、实体和值对象设计。

## Commands and Queries

当前尚未形成已接受的命令与查询模型。

## Domain Events

当前尚未形成已接受的领域事件模型；未来事件必须符合 `COP-API-002`。

## Invariants

当前尚未形成已接受的领域不变量。

## Relationships

- [COP-DOM-001](domain-landscape.md)
- [COP-DOM-004](cloud-access-domain.md)
- [COP-DOM-005](observability-domain.md)
- [COP-STD-001](../standards/terminology.md)

## Constraints

- 不得绕过 RFC/ADR 流程引入重大架构决策。
- 不得与相关 `accepted` 权威文档产生冲突。

## Quality Attributes

后续评审必须明确与本文主题相关的安全性、可靠性、可扩展性、可运维性和兼容性要求。

## Implementation Guidance

在本文状态变为 `accepted` 前，Codex 只能使用本文理解设计范围，不得据此生成生产实现。

## References

- [COP-DOM-001](domain-landscape.md)
- [COP-DOM-004](cloud-access-domain.md)
- [COP-DOM-005](observability-domain.md)
- [COP-STD-001](../standards/terminology.md)
