---
id: COP-INFRA-002
title: COP Kubernetes Topology
status: draft
version: 0.1.0
owners:
  - architecture
last_updated: 2026-08-03
related:
  - COP-INFRA-001
  - COP-INFRA-004
  - COP-INFRA-005
rfc: []
adr: []
---

# COP Kubernetes 拓扑

## Purpose

描述 COP 工作负载的集群、命名空间、隔离和调度边界。

## Scope

- Management
- Observability
- 受管集群
- 命名空间
- 故障域
- 资源隔离

## Non-goals

- Helm values
- 节点规格
- 云厂商集群创建步骤

## Context

本文是 COP 架构文档体系初始化产生的 `draft` 文档。详细设计必须经过专项讨论和评审；在状态变为 `accepted` 前，不得作为 `cop-platform` 的强制实现依据。

## Architecture or Model

当前尚未形成已接受的详细设计。本节后续只记录经过评审的当前有效架构，不复制 RFC 的讨论过程。

## Constraints

- 不得绕过 RFC/ADR 流程引入重大架构决策。
- 不得与相关 `accepted` 权威文档产生冲突。

## Quality Attributes

后续评审必须明确与本文主题相关的安全性、可靠性、可扩展性、可运维性和兼容性要求。

## Implementation Guidance

在本文状态变为 `accepted` 前，Codex 只能使用本文理解设计范围，不得据此生成生产实现。

## References

- [COP-INFRA-001](infrastructure-overview.md)
- [COP-INFRA-004](observability-stack.md)
- [COP-INFRA-005](network-and-ingress.md)
