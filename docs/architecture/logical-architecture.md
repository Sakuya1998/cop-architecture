---
id: COP-ARCH-002
title: COP Logical Architecture
status: draft
version: 0.1.0
owners:
  - architecture
last_updated: 2026-08-03
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

描述 Portal、API、Control Plane、Data Plane、Storage 和 AI 边界。

## Scope

- 逻辑层
- 核心职责
- 依赖方向
- 跨层交互

## Non-goals

- 物理集群参数
- 服务代码结构
- 供应商安装步骤

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

- [COP-ARCH-001](system-context.md)
- [COP-ARCH-003](control-plane-data-plane.md)
- [COP-DOM-001](../domains/domain-landscape.md)
- [COP-INFRA-001](../infrastructure/infrastructure-overview.md)
