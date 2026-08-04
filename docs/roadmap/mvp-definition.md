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

- [COP-VIS-002](../vision/scope-and-non-goals.md)
- [COP-DOM-001](../domains/domain-landscape.md)
- [COP-INFRA-001](../infrastructure/infrastructure-overview.md)
