---
id: "<stable service-design ID defined by an accepted naming rule>"
title: "<English formal title>"
source_status_requirement: accepted
owners:
  - "<负责团队或角色>"
last_updated: "<YYYY-MM-DD>"
platform_documents: []
domain_documents: []
rfc: []
adr: []
---

# <服务设计标题>

> 生成门槛：仅可基于已 `accepted` 的平台/架构文档和领域文档编写本设计；每一份平台/架构和领域来源文档都必须为 `accepted`。
>
> 服务设计是派生的实现输入，不是权威文档生命周期，也不创建新的权威来源。YAML 中的 `platform_documents`、`domain_documents`、`rfc` 和 `adr` 仅填写 ID 或编号；相对链接应放在 References 中。

## Service Responsibility

<说明服务承担的业务能力、明确边界、输入输出和不承担的职责，并引用来源权威文档。>

## Domain Ownership

<说明服务拥有或消费的边界上下文、聚合及业务规则；不得扩展未被已接受领域文档定义的事实。>

## APIs

<说明对外和对内 API 的职责、契约、版本兼容性、认证授权及关联规范。>

## Events

<说明发布或订阅的事件、生产/消费边界、契约来源和兼容性处理。>

## Data Ownership

<明确本服务数据的所有权、访问边界、生命周期、共享方式和与权威模型的对应关系。>

## Dependencies

<列出运行时与构建时依赖、依赖方向、关键假设和故障影响。>

## Security

<说明身份、授权、数据分类、保护、审计和威胁缓解；具体机制应由已接受架构约束驱动。>

## Reliability

<说明服务级目标、故障处理、超时、重试、幂等和降级原则，避免预设具体平台实现。>

## Observability

<说明日志、指标、追踪、告警、审计信号及其如何支持服务责任和验收。>

## Deployment

<说明发布依赖、配置边界、环境差异和变更控制；具体部署技术应遵循已接受架构文档。>

## Migration

<说明接口、事件、数据或责任变化时的兼容性、迁移步骤、验证和回退前提。>

## Test Strategy

<说明单元、契约、集成、端到端和恢复性验证如何覆盖责任、约束和质量目标。>

## Acceptance Criteria

<列出可验证的完成条件，并将每项条件追溯到已接受的平台/架构或领域文档。>

## References

<列出所有已接受的来源文档、相关 RFC/ADR、API/事件规范和验证证据。>
