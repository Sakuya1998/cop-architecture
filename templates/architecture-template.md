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

# <架构文档标题>

> 权威文档状态仅可为 `draft` → `review` → `accepted` → `deprecated`。`accepted` 表示当前有效的架构基线；`deprecated` 表示不再推荐使用，并应链接替代文档（如有）。
>
> YAML 中的 `related`、`rfc` 和 `adr` 仅填写稳定文档 ID；相对链接应放在 References 中。

## Purpose

<说明本架构文档要解决的问题、预期价值及其作为权威依据的用途。>

## Scope

<说明覆盖的业务能力、系统边界、参与者和时间范围。>

## Non-goals

<明确本文件不决定、不实现或不覆盖的事项，以避免将设计细节误作架构约束。>

## Context

<描述促成此架构的业务背景、已有约束、相关现状与待解决的核心问题。>

## Architecture or Model

<描述系统结构、关键模型、边界、交互关系和主要数据或控制流；必要时附图并说明图例。>

## Constraints

<列出必须遵守的业务、法规、兼容性、成本、组织或技术约束，并标明来源。>

## Quality Attributes

<说明可用性、性能、安全性、可维护性等质量属性，以及可验证的目标或权衡。>

## Implementation Guidance

<给出实现必须遵循的架构性指导、接口边界和验证方式；不要在此虚构具体平台、产品或部署决策。>

## References

<列出相关权威文档、RFC、ADR、规范、图表及外部依据，并使用稳定链接或文档 ID。>
