# COP Alerting Domain Content Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 将 `COP-DOM-006` 从初始化骨架更新为可评审的 Alerting 领域文档，同时保持稳定文档 ID、`draft` 门禁和既有引用。

**Architecture:** 只修改 `docs/domains/alerting-domain.md`，以 Contract-centered aggregates 描述 Rule、Evaluation、Alert Instance、Acknowledgement、Suppression Policy、Notification Intent 和 Delivery Result Reference。通过一个 Mermaid flowchart 固定 IAM/Secret、Resource Metadata、Observability、Alerting、外部通知系统、Audit 与 Dashboard/Experience 的权威边界，并用 PowerShell 对元数据、结构、生命周期、幂等、完整性、Tenant 隔离、链接、编码和提交范围执行确定性验证。

**Tech Stack:** Markdown、YAML front matter、Mermaid、PowerShell、Git

---

## File Structure

- Create: `docs/superpowers/plans/2026-08-10-alerting-domain-content.md` - 保存本实施计划、完整目标内容与可重复验证命令。
- Modify: `docs/domains/alerting-domain.md` - 定义 Alerting 领域当前设计；保持 `COP-DOM-006` 和 `draft` 状态。
- Do not modify: `docs/domains/README.md`、关联领域文档、API、Security、RFC 或 ADR；本次没有新增、移动、废弃或接受权威文档。

### Task 1: Define the Alerting domain document

**Files:**
- Modify: `docs/domains/alerting-domain.md`
- Reference: `docs/superpowers/specs/2026-08-10-alerting-domain-content-design.md`
- Reference: `docs/domains/domain-landscape.md`
- Reference: `docs/domains/observability-domain.md`
- Reference: `docs/api/event-contracts.md`
- Reference: `docs/security/audit-and-compliance.md`

- [ ] **Step 1: Run the requirement gate against the skeleton document (RED)**

```powershell
$ErrorActionPreference = 'Stop'
$path = 'docs/domains/alerting-domain.md'
$content = Get-Content -Raw -Encoding UTF8 $path
if (-not $content.Contains('version: 0.2.0')) { throw 'RED: expected Alerting version 0.2.0' }
if (-not $content.Contains('### Alerting Responsibility Boundary')) { throw 'RED: responsibility boundary is missing' }
if (-not $content.Contains('Notification Intent')) { throw 'RED: notification orchestration model is missing' }
throw 'RED gate unexpectedly passed against the skeleton'
```

Expected: command fails first with `RED: expected Alerting version 0.2.0`, proving the current `0.1.0` skeleton does not satisfy the approved design.

- [ ] **Step 2: Replace the skeleton with the approved Alerting domain content**

Replace `docs/domains/alerting-domain.md` with exactly:

````markdown
---
id: COP-DOM-006
title: COP Alerting Domain
status: draft
version: 0.2.0
owners:
  - architecture
last_updated: 2026-08-10
related:
  - COP-DOM-001
  - COP-DOM-005
  - COP-API-002
  - COP-SEC-003
rfc: []
adr: []
---

# COP 告警领域

## Purpose

定义受治理 Rule 如何消费不可变 Evaluation Input，形成可追溯的 Evaluation、Alert Instance、状态转换、人工处置、抑制判定和 Notification Intent，使 COP 在不接管原始 Telemetry、Resource Metadata 或通知渠道权威的前提下提供可靠、隔离且可审计的告警能力。

## Scope

- 稳定 Rule identity、不可变 Rule version、管理权类别和作用域。
- Evaluation 请求、结果、失败、幂等和顺序判定。
- Alert Instance 活跃去重、`Pending`、`Firing`、`Resolved` 生命周期和 predecessor 关联。
- Acknowledgement 与 Suppression Policy 的独立处置和编排语义。
- Notification Intent、Delivery Result Reference 和通知失败边界。
- Tenant isolation、双阶段授权、Secret Reference、领域事件和连续审计链。

## Non-goals

- 保存完整原始 Metrics、Logs、Traces 或成为新的 Telemetry Backend。
- 创建或修改 Resource Identity、Resource Metadata 私有事实或 Observability 私有事实。
- 实现通知渠道 Adapter、供应商协议或保存 credential material。
- 承担自动修复 Workflow、完整事件响应流程或 Dashboard 布局。
- 选择 Rule Engine、scheduler、消息系统、数据库、缓存、通知供应商或部署产品。
- 定义 REST 路径、表结构、retention 数值、重试次数、持续窗口默认值或 SLO。

## Context

Alerting 是消费 Resource Context 和不可变 Evaluation Input 的 Core bounded context，拥有 Rule、Evaluation outcome、Alert Instance、state transition 和 notification orchestration。Observability 保留 Signal、Binding、Query 和 Evaluation Input 权威；Telemetry Backend 保留原始 Telemetry；Resource Metadata 保留 Resource Identity；外部通知系统保留渠道执行与最终 Delivery Result；Audit and Compliance 定义审计与保留约束。本文保持 `draft`，仅用于设计讨论，不能作为 `cop-platform` 的强制实现依据。

## Ubiquitous Language

| Term | Meaning |
| --- | --- |
| Rule | Tenant 内具有稳定身份、管理权、作用域和当前激活版本的告警规则 |
| Rule Version | 固定条件、持续窗口、严重级别、Resource selector、输入 Contract 和通知策略引用的不可变版本 |
| Evaluation Input | Observability 已提交、不可变、带完整性和 provenance 的 Signal 输入事实 |
| Evaluation | 固定 Rule/Input/Resource/授权上下文的一次求值事实，结果为 Matched、NotMatched 或 Failed |
| Alert Instance | 同一去重身份的一次连续告警发生，具有不可重开的生命周期 |
| Acknowledgement | 不改变 Alert lifecycle 的独立人工处置事实 |
| Suppression Policy | 只影响 Notification Orchestration 的版本化、带作用域和有效期的策略 |
| Notification Intent | 状态转换提交后生成、固定策略与目标引用的不可变通知编排事实 |
| Delivery Result | 外部通知系统拥有的最终投递事实，Alerting 只保存受控结果引用 |

## Bounded Context

### Alerting Responsibility Boundary

Alerting 拥有 Rule identity/version/management authority、Evaluation request/outcome/failure、Alert Instance identity/lifecycle、Acknowledgement、Suppression decision、Notification Intent 和连续审计关联。IAM 拥有 Principal、Tenant 与授权决策；Secret/KMS 拥有 credential material；Resource Metadata 拥有 Resource Identity 和 Resource Context；Observability 拥有 Signal 与不可变 Evaluation Input；Telemetry Backend 拥有原始遥测；外部通知系统拥有渠道 Adapter 和最终 Delivery Result。

Alerting 不读取或改写其他领域私有存储，不复制完整原始 Telemetry，不创建 Resource Identity，不实现通知供应商协议。Dashboard/Experience 只能发出授权 Command 和读取受治理 read model，不能直接产生 Evaluation outcome、Alert state 或 Delivery Result。

### Rule and Management Authority Model

Rule 使用稳定 `rule_id`，只属于一个 Tenant，并通过不可变 Rule version 演进。Rule version 固定条件、持续窗口、严重级别、Resource selector、Signal/Evaluation Input Contract、通知策略引用和管理权类别。条件、窗口、严重级别、作用域或关联 Contract 的语义变化创建新版本，历史 Evaluation、Alert Instance 和 Notification Intent 继续引用原版本。

Rule 分为 `Tenant-managed` 与 `Platform-managed`。两者共享 Evaluation 和 Alert lifecycle Contract，但创建、修改、激活、禁用和通知策略权限分离；Tenant 不得修改平台规则，平台也不得静默改写 Tenant 规则。只有已激活版本接受新 Evaluation。新版本激活、Rule 禁用或 Resource scope 撤销时，以受治理终止动作结束旧版本活跃实例，并记录 actor、reason、expected version、time 和 correlation。

### Evaluation and Input Contract

Evaluation 使用稳定 `evaluation_id` 和 idempotency key，并固定 Tenant、Rule version、计划求值时间、Evaluation Input、Resource Context version 和授权上下文。Alerting 只消费 Observability 已提交、不可变的 Evaluation Input；执行前重新验证 Tenant、Rule version、Resource Context、完整性、授权、敏感级别和引用状态，任一项无法确认时 fail closed。

完整成功的 outcome 只能是 `Matched` 或 `NotMatched`；失败使用 `Failed` 和标准失败分类。`Partial`、`Unavailable`、timeout、Contract violation 或敏感级别冲突不能转换为正常布尔结果、零值、最近值或“无数据”。Evaluation failure 不推进 Alert state，也不得推断告警已恢复。

同一告警身份按 Rule version 和计划求值时间推进。duplicate、late、out-of-order 或 replay 结果保留审计事实，但不得覆盖较新的 Alert 状态。Alerting 不依赖 exactly-once、跨 Tenant 全局顺序或跨领域分布式事务。

### Alert Instance Lifecycle

Alert Instance 使用稳定 `alert_instance_id` 表示一次连续告警发生。活跃去重键为 `Tenant + Rule version + Resource Identity + normalized dimensions`；同一去重键同时只能存在一个活跃实例。

首次完整 `Matched` 创建 `Pending`；持续窗口满足后进入 `Firing`。立即触发规则可以在一次受控处理内记录创建并进入 `Firing`，但必须保留创建和触发原因。`Pending` 或 `Firing` 收到完整 `NotMatched` 后进入 `Resolved`。Rule version superseded、Rule disabled 或 Resource scope revoked 也可以通过受治理终止动作进入 `Resolved`，并使用区别于 condition cleared 的明确原因。

`Resolved` 是终态，不得重新打开。相同条件再次命中时创建新的 Alert Instance，并通过 predecessor reference 关联前一实例。Evaluation failure、Acknowledgement、Suppression 和 Delivery status 不改变核心生命周期。

### Acknowledgement and Suppression Model

Acknowledgement 是独立、可审计的处置事实，记录 Alert Instance、actor、time、reason、expected version 和 correlation。它可以影响后续通知策略，但不能直接把 Alert Instance 转为 `Resolved`，也不能改写 Evaluation outcome。

Suppression Policy 使用稳定身份和不可变版本，固定 Tenant、作用域、有效期、管理权和通知编排语义。Suppression 只影响 Notification Orchestration；Evaluation 与 Alert lifecycle 继续推进。命中 Suppression 时记录 policy version、作用域、原因和时间，但不请求外部投递。到期、撤销或版本变化不反向修改历史编排决定。

### Notification Orchestration Model

Alert state transition 提交后，Alerting 使用固定通知策略版本和当前 Suppression 判定形成编排决定。未被抑制时生成不可变 Notification Intent，固定 Tenant、Alert Instance、触发转换、Rule version、通知策略版本、target Reference、idempotency key、time 和 correlation。

Alerting 只保存受治理目标和 Secret/KMS Reference，不保存 credential material 或供应商可执行 token。外部通知系统负责渠道适配、供应商协议、投递重试和最终 Delivery Result；Alerting 记录 Delivery Result Reference、标准结果分类、time 和 correlation。Delivery failure 不回滚 Alert state，也不改变 Evaluation outcome。Intent 和 Result 的 duplicate、out-of-order 与 replay 必须幂等处理。

### Alerting Relationship Map

```mermaid
flowchart LR
  IAM["IAM / Secret-KMS"]
  RM["Resource Metadata"]
  OBS["Observability"]
  AL["Alerting"]
  NS["External Notification System"]
  AUD["Audit and Compliance"]
  DX["Dashboard / Experience"]

  IAM -.->|"authorization / secret reference"| AL
  RM -->|"Resource Context"| AL
  OBS -->|"immutable Evaluation Input"| AL
  DX -->|"authorized Command"| AL
  AL -->|"governed read model"| DX
  AL -->|"Notification Intent"| NS
  NS -->|"Delivery Result"| AL
  AL -->|"domain event / audit fact"| AUD
```

箭头不表示共享可写存储、credential material 复制、跨领域同步事务或部署拓扑。Observability 保留 Signal 输入权威，Resource Metadata 保留 Resource 权威，外部通知系统保留最终投递权威，Dashboard/Experience 不产生核心告警事实。

### Failure, Security, and Consistency

仅标准化为 transient/retryable 的依赖故障执行有界 backoff；无效 Rule、授权拒绝、Tenant/作用域不匹配、Contract violation 和敏感级别冲突不自动重试。重试耗尽记录明确 Evaluation failure 或 Delivery failure，不推断 condition cleared，也不改变无关 Alert Instance。

重试、并发、限流和故障隔离至少按 Tenant 与 Rule/Notification target scope 隔离。核心事实先持久化，领域事件和 Notification Intent 使用稳定 ID 支持幂等重放；Observability、Alerting 与外部通知系统之间不要求分布式事务。read model 允许最终一致，但必须声明 `as_of`、更新时间、完整性或 stale 状态。

所有核心对象、Command 和 Event 携带 Tenant identity。Rule 管理与 Evaluation/Notification 执行分别校验 Tenant、授权、Resource scope、敏感级别和引用状态，任何一层无法确认时 fail closed。日志、事件、审计和普通查询只保存最小必要字段及受控证据引用，不泄漏 Secret、原始 Telemetry、完整敏感 Evaluation Input 或通知内容。

### Validation Strategy

- **Rule contract:** 验证稳定 Rule ID、不可变版本、激活校验、旧版本终止和 Tenant-managed/Platform-managed 权限分离。
- **Evaluation contract:** 验证稳定 ID、幂等键、固定引用及 `Matched`、`NotMatched`、`Failed` 互斥语义。
- **Input completeness:** 验证 `Partial`、`Unavailable`、timeout 和 Contract violation 均不触发正常 Alert state transition。
- **Alert lifecycle:** 验证 `Pending -> Firing -> Resolved`、终态不可重开、终态后再次命中新建实例。
- **Deduplication and concurrency:** 验证同一去重键只有一个活跃实例，以及 duplicate、late、out-of-order、replay、expected version 和并发冲突。
- **Disposition and suppression:** 验证 Acknowledgement 不等于 Resolved，Suppression 只影响 Notification Orchestration。
- **Notification boundary:** 验证 Intent 在状态提交后形成、Delivery failure 不回滚状态、外部系统保留最终投递权威。
- **Tenant isolation and secrecy:** 验证跨 Tenant 操作默认拒绝，Secret、credential 和敏感完整载荷不进入普通事件、日志或审计。
- **Traceability:** 验证 Rule version 到 Delivery Result 的连续关联链，以及 Command/Event 与 `COP-API-002` 的兼容方向。
- **Ownership separation:** 验证 Alerting 不创建 Resource Identity，不保存原始 Telemetry，不实现渠道 Adapter 或自动修复 Workflow。

### Success Criteria

读者能识别 Alerting、Observability、Telemetry Backend、Resource Metadata、IAM、Secret/KMS、外部通知系统、Audit 与 Dashboard/Experience 的事实权威。Rule version、Evaluation、Alert lifecycle、处置与抑制、Notification Intent、双重授权、完整性、故障隔离、幂等和审计语义均可验证；文档不引入未经批准的产品、API、部署、数值或跨领域实现决策。

## Aggregates and Entities

| Aggregate or entity | Ownership | Key references and boundaries |
| --- | --- | --- |
| Rule | 稳定 Rule identity、Tenant、管理权、激活版本和作用域 | 不可变版本引用 Evaluation Input Contract、Resource selector 和通知策略；不包含 Backend 查询语言 |
| Evaluation | 一次固定上下文的 Matched、NotMatched 或 Failed 事实 | 引用 Rule version、Evaluation Input、Resource Context 与授权；失败不推进 Alert state |
| Alert Instance | 活跃去重身份、生命周期、状态转换原因和 predecessor | 不复制 Resource 主数据；Resolved 不重开 |
| Acknowledgement | 独立人工处置事实 | 可以影响通知策略，但不等于 Resolved |
| Suppression Policy | 版本化作用域、有效期和编排判定 | 只影响 Notification Orchestration，不暂停 Evaluation |
| Notification Intent | 状态提交后的不可变通知编排事实 | 固定 Rule/Policy/target Reference；不保存 credential material |
| Delivery Result Reference | 外部最终投递事实的受控关联 | Delivery failure 不回滚 Alert state |

Alert Instance 状态转换如下：

| Current state | Input or command | Next state | Condition |
| --- | --- | --- | --- |
| None | Complete `Matched` | `Pending` | 创建新的活跃 Alert Instance |
| `Pending` | Complete `Matched` | `Firing` | 持续窗口满足；立即触发语义保留创建与触发原因 |
| `Pending` | Complete `NotMatched` | `Resolved` | condition cleared |
| `Firing` | Complete `Matched` | `Firing` | 保持实例并记录最新成功 Evaluation |
| `Firing` | Complete `NotMatched` | `Resolved` | condition cleared |
| `Pending` or `Firing` | Governed termination | `Resolved` | rule version superseded、Rule disabled 或 Resource scope revoked |
| `Resolved` | Complete `Matched` | New instance | 原实例不重开；新实例引用 predecessor |

`Failed` Evaluation 不进入该转换表；它只记录失败事实并保持当前 Alert state。

## Commands and Queries

### Commands

Rule 命令包括 CreateRule、CreateRuleVersion、ActivateRuleVersion、DisableRule、SupersedeRuleVersion；Evaluation 命令包括 RequestEvaluation、RecordEvaluationResult、RecordEvaluationFailure；Alert Instance 命令包括 ApplyEvaluationResult、TerminateAlertInstance；处置与抑制命令包括 AcknowledgeAlert、CreateSuppressionPolicy、CreateSuppressionPolicyVersion、ActivateSuppressionPolicyVersion、ExpireSuppressionPolicy、RevokeSuppressionPolicy；通知命令包括 RecordNotificationDecision、RecordDeliveryResult。

所有管理命令携带 Tenant、actor/source、idempotency key、expected version、reason 和 correlation 中适用的字段，并校验 Rule/Policy version、Resource Context、Evaluation Input、授权、敏感级别和状态转换。非法命令与并发冲突显式拒绝，不使用 last-write-wins 或跨领域分布式事务。

### Queries

查询提供 Rule identity、当前/历史版本、管理权类别、作用域、激活/禁用状态；Evaluation outcome、失败分类、输入完整性、固定引用、计划时间；Alert Instance 当前状态、转换历史、去重身份、predecessor 和 Resource reference；Acknowledgement、Suppression decision、Notification Intent 和 Delivery Result Reference；以及带 `as_of`、完整性或 stale 标记的受治理 read model。

查询应用 Tenant isolation、Resource scope、Rule ownership、敏感级别和字段级授权。普通接口不返回 Secret、credential material、原始 Telemetry、完整敏感 Evaluation Input、供应商可执行 token 或未授权通知内容。

## Domain Events

### Events

事件覆盖 Rule version created/activated/superseded 和 Rule disabled；Evaluation requested/matched/not matched/failed；Alert Instance created/pending/firing/resolved；Alert acknowledged；Suppression Policy version created/activated/expired/revoked 和 Notification suppressed；Notification Intent created 与 Delivery Result recorded。

事件采用与 `COP-API-002` 兼容的最小 Contract，携带稳定 event ID、Tenant、aggregate ID/version、Rule/Policy version、状态或结果分类、time、correlation 和必要受控 reference，不携带 Secret、credential material、原始 Telemetry、完整敏感输入或供应商可执行 token。消费者使用 event ID、aggregate version 和业务 idempotency key 处理 duplicate、out-of-order 和 replay，不依赖全局顺序。

### Audit

Rule version、Evaluation Input、Evaluation、Alert transition、Acknowledgement/Suppression decision、Notification Intent 和 Delivery Result 形成连续审计链。审计保留 actor/source、Tenant、版本、状态或结果、reason、time、correlation 和受控证据引用；具体 retention 由 `COP-SEC-003` 治理，不在本文决定。审计与普通日志不得复制 credential、原始 Telemetry 或完整敏感 payload。

## Invariants

- Rule、Evaluation、Alert Instance、Suppression Policy 和 Notification Intent 各自只属于一个 Tenant。
- Rule 与 Suppression Policy 使用稳定 ID 和不可变版本；历史引用不得漂移到最新版本。
- 只有已激活 Rule version 可以接受新的 Evaluation。
- 只有完整成功的 `Matched` 或 `NotMatched` 可以推进 Alert lifecycle。
- 同一活跃去重键同时只能存在一个 Alert Instance；`Resolved` 实例不得重新打开。
- Acknowledgement 不等于 Resolved；Suppression 不暂停 Evaluation 或 Alert state transition。
- Notification Intent 只在核心事实提交后形成；Delivery failure 不回滚 Alert state。
- 所有状态变更使用 idempotency key 和 expected version；冲突显式拒绝。
- late、duplicate、out-of-order 和 replay 结果不得覆盖更新状态。
- Tenant、授权、Resource scope、敏感级别或引用状态无法确认时 fail closed。
- Alerting 不拥有原始 Telemetry、Resource 主数据、渠道 Adapter、credential material 或自动修复 Workflow。

## Relationships

- [COP-DOM-001](domain-landscape.md)
- [COP-DOM-005](observability-domain.md)
- [COP-API-002](../api/event-contracts.md)
- [COP-SEC-003](../security/audit-and-compliance.md)

## Constraints

- 不得绕过 RFC/ADR 流程引入重大架构决策。
- 不得将 `Partial`、`Unavailable` 或 Failed Evaluation 转换为正常 Alert state transition。
- 不得跨 Tenant 关联、去重、查询或路由 Rule、Alert Instance、Suppression 或 Notification Intent。
- 不得把 Acknowledgement、Suppression 或 Delivery status 混入核心 Alert lifecycle。
- 不得依赖 exactly-once、全局顺序、共享可写存储或跨领域分布式事务。
- 不得在本文选择 Rule Engine、通知供应商、存储、消息、部署产品或数值参数。
- 不得与相关 `accepted` 权威文档产生冲突。

## Quality Attributes

- **Security:** Tenant 与权限在管理和执行阶段分别校验，Secret 只通过受控 Reference 使用。
- **Reliability:** 完整性、失败、幂等、并发和乱序语义明确，单一规则或渠道故障不扩散到无关 Tenant。
- **Auditability:** Rule version 到 Delivery Result 的事实链连续、可关联且使用最小必要载荷。
- **Consistency:** 核心状态使用 expected version；read model 声明 `as_of`、完整性或 stale 状态。
- **Compatibility:** Domain Event 与 `COP-API-002` 保持兼容方向，不假设 exactly-once 或全局顺序。
- **Evolvability:** Rule、Suppression Policy 和外部 Contract 使用不可变版本，历史引用不漂移。

## Implementation Guidance

本文不指定 Rule Engine、scheduler、数据库、消息系统、缓存、渠道 Adapter、API 路径或部署拓扑。`cop-platform` 的实现必须等待相关文档和 ADR 达到 `accepted`，并在实现任务中保持 Rule/Evaluation/Alert、Observability、Resource Metadata 与通知系统的私有存储和事实权威分离。

## References

- [COP-DOM-001](domain-landscape.md)
- [COP-DOM-005](observability-domain.md)
- [COP-API-002](../api/event-contracts.md)
- [COP-SEC-003](../security/audit-and-compliance.md)
````

- [ ] **Step 3: Run the structural and semantic checks (GREEN)**

```powershell
$ErrorActionPreference = 'Stop'
$path = 'docs/domains/alerting-domain.md'
$bytes = [IO.File]::ReadAllBytes((Resolve-Path $path))
$content = [Text.UTF8Encoding]::new($false, $true).GetString($bytes).Replace(([string][char]13 + [char]10), [string][char]10)
if ($bytes.Length -ge 3 -and $bytes[0] -eq 0xEF -and $bytes[1] -eq 0xBB -and $bytes[2] -eq 0xBF) { throw 'UTF-8 BOM found' }
if ($content.Contains([char]0) -or $content.Contains([char]0xFFFD)) { throw 'Invalid text character found' }
$expectedFrontMatter = @'
---
id: COP-DOM-006
title: COP Alerting Domain
status: draft
version: 0.2.0
owners:
  - architecture
last_updated: 2026-08-10
related:
  - COP-DOM-001
  - COP-DOM-005
  - COP-API-002
  - COP-SEC-003
rfc: []
adr: []
---
'@
$frontMatter = [regex]::Match($content, '\A---\n.*?\n---\n', [Text.RegularExpressions.RegexOptions]::Singleline)
if (-not $frontMatter.Success -or $frontMatter.Value.TrimEnd() -cne $expectedFrontMatter.TrimEnd()) { throw 'Front matter mismatch' }
$h2Expected = @('Purpose','Scope','Non-goals','Context','Ubiquitous Language','Bounded Context','Aggregates and Entities','Commands and Queries','Domain Events','Invariants','Relationships','Constraints','Quality Attributes','Implementation Guidance','References')
$h2Actual = @([regex]::Matches($content, '(?m)^## (.+)$') | ForEach-Object { $_.Groups[1].Value })
if (($h2Actual -join '|') -cne ($h2Expected -join '|')) { throw 'H2 order mismatch' }
$h3Expected = @('Alerting Responsibility Boundary','Rule and Management Authority Model','Evaluation and Input Contract','Alert Instance Lifecycle','Acknowledgement and Suppression Model','Notification Orchestration Model','Alerting Relationship Map','Failure, Security, and Consistency','Validation Strategy','Success Criteria','Commands','Queries','Events','Audit')
$h3Actual = @([regex]::Matches($content, '(?m)^### (.+)$') | ForEach-Object { $_.Groups[1].Value })
if (($h3Actual -join '|') -cne ($h3Expected -join '|')) { throw 'H3 order mismatch' }
foreach ($value in @(
  'Alerting 是消费 Resource Context 和不可变 Evaluation Input 的 Core bounded context',
  'Observability 保留 Signal、Binding、Query 和 Evaluation Input 权威',
  '外部通知系统保留渠道执行与最终 Delivery Result',
  '稳定 `rule_id`', 'Tenant-managed', 'Platform-managed', '不可变 Rule version', 'rule_version_superseded',
  '稳定 `evaluation_id`', 'Matched', 'NotMatched', 'Failed', 'Partial', 'Unavailable', 'fail closed',
  'Tenant + Rule version + Resource Identity + normalized dimensions', 'Pending', 'Firing', 'Resolved', 'predecessor reference',
  'Acknowledgement 不等于 Resolved', 'Suppression 只影响 Notification Orchestration',
  '不可变 Notification Intent', 'Delivery failure 不回滚 Alert state', 'Secret/KMS Reference',
  'expected version', 'idempotency key', 'duplicate', 'late', 'out-of-order', 'replay',
  'exactly-once', '全局顺序', '分布式事务', 'CreateRule', 'RequestEvaluation', 'AcknowledgeAlert', 'RecordDeliveryResult'
)) { if (-not $content.Contains($value)) { throw "Missing requirement: $value" } }
$ticks = ([string][char]96) * 3
$mermaidPattern = '(?ms)^' + [regex]::Escape($ticks) + 'mermaid\n(.*?)^' + [regex]::Escape($ticks) + '$'
$mermaid = [regex]::Matches($content, $mermaidPattern)
if ($mermaid.Count -ne 1) { throw "Expected one Mermaid block, found $($mermaid.Count)" }
$diagram = $mermaid[0].Groups[1].Value
foreach ($value in @(
  'IAM -.->|"authorization / secret reference"| AL', 'RM -->|"Resource Context"| AL',
  'OBS -->|"immutable Evaluation Input"| AL', 'DX -->|"authorized Command"| AL',
  'AL -->|"governed read model"| DX', 'AL -->|"Notification Intent"| NS',
  'NS -->|"Delivery Result"| AL', 'AL -->|"domain event / audit fact"| AUD'
)) { if (-not $diagram.Contains($value)) { throw "Missing diagram relation: $value" } }
$aggregateTable = [regex]::Match($content, '(?ms)^\| Aggregate or entity \| Ownership \| Key references and boundaries \|\n(.*?)\n\n')
if (-not $aggregateTable.Success) { throw 'Aggregate table missing' }
$aggregateRows = @($aggregateTable.Groups[1].Value -split [char]10)
if ($aggregateRows.Count -ne 8) { throw "Expected separator and 7 aggregate rows, found $($aggregateRows.Count)" }
foreach ($row in $aggregateRows) { if (([regex]::Matches($row, '\|')).Count -ne 4) { throw "Aggregate table column mismatch: $row" } }
$stateTable = [regex]::Match($content, '(?ms)^\| Current state \| Input or command \| Next state \| Condition \|\n(.*?)\n\n')
if (-not $stateTable.Success) { throw 'State transition table missing' }
$stateRows = @($stateTable.Groups[1].Value -split [char]10)
if ($stateRows.Count -ne 8) { throw "Expected separator and 7 state rows, found $($stateRows.Count)" }
foreach ($row in $stateRows) { if (([regex]::Matches($row, '\|')).Count -ne 5) { throw "State table column mismatch: $row" } }
$forbidden = @(('T' + 'BD'), ('T' + 'ODO'), ('lorem' + ' ipsum'), '当前尚未形成')
foreach ($value in $forbidden) { if ($content.Contains($value)) { throw "Forbidden filler: $value" } }
$file = Get-Item $path
$links = [regex]::Matches($content, '\[[^\]]+\]\(([^)#]+\.md)(?:#[^)]+)?\)')
if ($links.Count -ne 8) { throw "Expected 8 relationship and reference links, found $($links.Count)" }
foreach ($link in $links) { if (-not (Test-Path -LiteralPath (Join-Path $file.DirectoryName $link.Groups[1].Value))) { throw "Broken link: $($link.Groups[1].Value)" } }
git diff --check
if ($LASTEXITCODE -ne 0) { throw 'git diff --check failed' }
$changed = @(git diff --name-only)
if ($changed.Count -ne 1 -or $changed[0] -ne $path) { throw "Unexpected file scope: $($changed -join ', ')" }
'PASS: Alerting domain structure and semantics'
```

Expected output: `PASS: Alerting domain structure and semantics`.

- [ ] **Step 4: Perform the manual authority, lifecycle and consistency review**

Read the target source or rendered Markdown and confirm:

- Alerting owns Rule, Evaluation outcome, Alert Instance, state transition, Acknowledgement/Suppression and Notification Intent; Observability, Telemetry Backend, Resource Metadata, IAM/Secret, external notification and Audit retain their approved authorities.
- Rule identity is stable and Tenant-bound; Rule versions are immutable; Tenant-managed and Platform-managed permissions remain separate; old active instances terminate explicitly when a version is superseded.
- Evaluation fixes Rule/Input/Resource/authorization context; only complete Matched or NotMatched results advance state; Partial, Unavailable and failures never imitate a normal result.
- the active deduplication key includes Tenant, Rule version, Resource Identity and normalized dimensions; one active instance exists per key; Resolved instances never reopen.
- Pending, Firing and Resolved are the only core lifecycle states; Acknowledgement, Suppression and Delivery status remain independent facts.
- Notification Intent is created after the state fact is committed; external delivery failure never rolls back Alert state; credential material never enters Alerting.
- duplicate, late, out-of-order and replay handling uses stable IDs, idempotency and expected version without implying exactly-once or global ordering.
- commands, queries, events and audit preserve Tenant, actor/source, versions, completeness, reason and correlation without exposing raw Telemetry or sensitive payload.
- the document does not select products, deployment topology, REST/database implementation, numerical policy, automatic remediation or incident-response workflow.
- the document remains `draft` and does not create RFC/ADR authority.

If any item fails, correct only the target document and rerun Step 3.

- [ ] **Step 5: Run full repository link and scope checks**

```powershell
$ErrorActionPreference = 'Stop'
$markdown = @(Get-Item README.md,CONTRIBUTING.md,AGENTS.md) + @(Get-ChildItem docs,adr,rfc,templates -Recurse -File -Filter '*.md' | Where-Object { $_.FullName -notmatch '[\\/]docs[\\/]superpowers[\\/]' })
foreach ($file in $markdown) {
  $content = Get-Content -Raw -Encoding UTF8 $file.FullName
  foreach ($link in [regex]::Matches($content, '\[[^\]]+\]\(([^)#]+\.md)(?:#[^)]+)?\)')) {
    if (-not (Test-Path -LiteralPath (Join-Path $file.DirectoryName $link.Groups[1].Value))) { throw "Broken link in $($file.FullName): $($link.Groups[1].Value)" }
  }
}
git diff --check
if ($LASTEXITCODE -ne 0) { throw 'git diff --check failed' }
$changed = @(git diff --name-only)
if ($changed.Count -ne 1 -or $changed[0] -ne 'docs/domains/alerting-domain.md') { throw "Unexpected file scope: $($changed -join ', ')" }
'PASS: repository links and scope'
```

Expected output: `PASS: repository links and scope`.

- [ ] **Step 6: Commit the Alerting domain document**

```powershell
git add docs/domains/alerting-domain.md
git commit -m "docs: define alerting domain content"
```

- [ ] **Step 7: Verify the implementation commit contains only the target document**

```powershell
$files = @(git diff-tree --no-commit-id --name-only -r HEAD)
if ($files.Count -ne 1 -or $files[0] -ne 'docs/domains/alerting-domain.md') { throw "Unexpected committed files: $($files -join ', ')" }
if (git status --porcelain) { throw 'Worktree is not clean' }
'PASS: Alerting domain commit'
```

Expected output: `PASS: Alerting domain commit`.

### Plan Self-Review

- [ ] Compare this plan with all 18 acceptance conditions in `docs/superpowers/specs/2026-08-10-alerting-domain-content-design.md`; conditions 1-15 are covered by the exact target content, GREEN assertions and manual review, condition 16 by unchanged references and repository link checks, condition 17 by byte-level encoding and filler checks, and condition 18 by diff and single-file commit verification.
- [ ] Confirm the plan preserves `COP-DOM-006` ID, owners, related, rfc and adr; changes version/date only as approved; keeps `draft`; preserves four existing references; and changes no index or related document.
- [ ] Confirm Rule authority/versioning, Evaluation completeness, Alert identity/lifecycle, Acknowledgement/Suppression separation, Notification boundary, Resource authority, Tenant isolation, idempotency, failure classification, audit and secrecy appear in both target content and validation.
- [ ] Confirm target content and plan introduce no product selection, deployment decision, REST/database schema, numerical policy, automatic remediation, incident-response workflow or unauthorized RFC/ADR decision.

### Commit the Plan

- [ ] **Step 8: Commit the implementation plan**

```powershell
git add docs/superpowers/plans/2026-08-10-alerting-domain-content.md
git commit -m "docs: plan alerting domain content"
```
