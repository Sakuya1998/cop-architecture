---
id: COP-PRI-002
title: COP AI Development Principles
status: draft
version: 0.1.0
owners:
  - architecture
last_updated: 2026-08-03
related:
  - COP-PRI-001
  - COP-STD-002
  - COP-ROAD-002
rfc: []
adr: []
---

# COP AI 开发原则

## Purpose

定义人、GPT 和 Codex 在设计、拆解、实现与评审中的职责边界。

## Scope

- 文档门禁
- Agent 权限
- 任务输入
- 评审与追踪

## Non-goals

- 允许 AI 自主改变架构
- 用生成代码替代架构决策

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

- [COP-PRI-001](architecture-principles.md)
- [COP-STD-002](../standards/documentation-standard.md)
- [COP-ROAD-002](../roadmap/mvp-definition.md)
