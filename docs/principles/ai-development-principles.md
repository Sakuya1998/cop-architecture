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

本文档是由 COP 文档引导创建的草案；详细设计需要经过专题讨论和评审；在被接受前，本文档不是 cop-platform 实现的强制性权威依据。

## Architecture or Model

尚不存在已接受的详细设计；后续仅记录经过评审的当前态架构，不记录 RFC 讨论历史。

## Constraints

不得绕过 RFC/ADR 作出重大决策；不得与相关已接受的权威文档冲突。

## Quality Attributes

后续评审必须定义相关的安全性、可靠性、可扩展性、可运维性和兼容性要求。

## Implementation Guidance

在被接受前，Codex 仅可用本文档理解范围，不得据此生成生产实现。

## References

- [COP-PRI-001](architecture-principles.md)
- [COP-STD-002](../standards/documentation-standard.md)
- [COP-ROAD-002](../roadmap/mvp-definition.md)
