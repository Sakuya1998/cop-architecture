---
id: COP-STD-001
title: COP Terminology
status: draft
version: 0.1.0
owners:
  - architecture
last_updated: 2026-08-03
related:
  - COP-DOM-001
  - COP-API-001
  - COP-STD-002
rfc: []
adr: []
---

# COP 术语表

## Purpose

建立全仓库统一语言并禁止 Resource、Asset、Metadata、CMDB 等概念随意混用。

## Scope

- 正式术语
- 定义
- 允许别名
- 禁止用法
- 所属领域

## Non-goals

- API 字段全集
- 数据库词典
- 第三方产品术语翻译大全

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

- [COP-DOM-001](../domains/domain-landscape.md)
- [COP-API-001](../api/api-design-guidelines.md)
- [COP-STD-002](documentation-standard.md)
