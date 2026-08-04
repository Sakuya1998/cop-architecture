---
id: COP-ROAD-002
title: COP MVP Definition
status: draft
version: 0.1.0
owners:
  - architecture
last_updated: 2026-08-03
related:
  - COP-VIS-002
  - COP-DOM-001
  - COP-INFRA-001
rfc: []
adr: []
---

# COP MVP 定义

## Purpose

固化首个可用版本的核心闭环、非目标和完成条件。

## Scope

- 云账号/集群接入
- 采集
- Metadata/CMDB
- 指标日志
- Dashboard
- Alert
- IAM/Audit

## Non-goals

- Workflow
- Automation
- FinOps
- 插件市场
- AI/RAG 详细能力

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

- [COP-VIS-002](../vision/scope-and-non-goals.md)
- [COP-DOM-001](../domains/domain-landscape.md)
- [COP-INFRA-001](../infrastructure/infrastructure-overview.md)
