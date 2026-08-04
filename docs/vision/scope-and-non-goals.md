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

- [COP-VIS-001](platform-vision.md)
- [COP-ROAD-002](../roadmap/mvp-definition.md)
- [COP-ARCH-001](../architecture/system-context.md)
