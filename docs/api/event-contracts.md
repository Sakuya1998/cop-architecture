---
id: COP-API-002
title: COP Event Contracts
status: draft
version: 0.1.0
owners:
  - architecture
last_updated: 2026-08-03
related:
  - COP-ARCH-004
  - COP-DOM-005
  - COP-DOM-006
  - COP-API-003
rfc: []
adr: []
---

# COP 事件契约规范

## Purpose

规定领域事件和集成事件的信封、命名、语义和交付约束。

## Scope

- Event envelope
- 命名
- 版本
- 幂等
- 排序
- 重试
- 死信

## Non-goals

- 具体 Topic 全集
- Broker 配置
- 消费者代码

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

- [COP-ARCH-004](../architecture/integration-architecture.md)
- [COP-DOM-005](../domains/observability-domain.md)
- [COP-DOM-006](../domains/alerting-domain.md)
- [COP-API-003](api-versioning-and-compatibility.md)
