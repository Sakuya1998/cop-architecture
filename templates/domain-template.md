---
id: "<stable document ID from the approved catalog>"
title: "<English formal title>"
status: "<draft | review | accepted | deprecated>"
version: "<semantic version, e.g. 0.1.0>"
owners:
  - "<负责团队或角色>"
last_updated: "<YYYY-MM-DD>"
related: []
rfc: []
adr: []
---

# <领域文档标题>

> 权威文档状态仅可为 `draft` → `review` → `accepted` → `deprecated`。`accepted` 表示当前有效的领域基线；`deprecated` 表示不再推荐使用，并应链接替代文档（如有）。
>
> YAML 中的 `related`、`rfc` 和 `adr` 仅填写稳定文档 ID；相对链接应放在 References 中。

## Purpose

<说明此领域模型服务的业务目的、读者及作为权威依据的用途。>

## Scope

<说明该领域覆盖的业务能力、术语范围和边界。>

## Non-goals

<说明不属于此领域模型的能力、流程或技术实现事项。>

## Context

<描述业务背景、现有系统关系、外部约束和需要统一的概念。>

## Ubiquitous Language

<逐项定义业务术语、别名、禁止混用的名称及适用语境。>

## Bounded Context

<定义边界上下文的职责、边界、上下游关系和与其他上下文的集成方式。>

## Aggregates and Entities

<描述聚合、实体、值对象、标识和一致性边界；不要填入未经确认的领域事实。>

## Commands and Queries

<说明命令和查询的业务意图、前置条件、结果语义、授权和可见性约束；传输层和 wire schema 应维护在 API 文档中。>

## Domain Events

<定义领域事件的触发条件、载荷语义、生产者和消费者边界；不要填入未经确认的领域事实。>

领域事件必须引用 COP-API-002；仅当 COP-API-002 的状态为 `accepted` 时，该规范具有约束力。

## Invariants

<明确任何状态变更都必须满足的业务规则、一致性要求和违反时的处理方式。>

## Relationships

<说明上下文映射、实体/聚合关系、所有权和依赖方向。>

## Constraints

<列出法规、隐私、保留期、兼容性、组织及其他不可违反的约束，并说明来源。>

## Quality Attributes

<说明领域模型所需的正确性、安全性、可审计性、性能或演进质量目标及其权衡。>

## Implementation Guidance

<给出保持领域边界和不变量的通用实现指导；不要在此指定未经架构确认的平台或产品。>

## References

<列出关联架构文档、API 规范、RFC、ADR、词汇表及外部依据。>
