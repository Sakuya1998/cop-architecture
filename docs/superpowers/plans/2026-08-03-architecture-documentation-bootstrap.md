# COP Architecture Documentation Bootstrap Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Initialize `cop-architecture` with a governed, navigable documentation system containing five templates, complete indexes, and 30 correctly identified `draft` architecture document skeletons.

**Architecture:** Keep authoritative current-state documents under stable knowledge categories, and keep RFC/ADR history in separate top-level directories. Every authoritative document receives machine-readable YAML metadata, a stable ID, explicit scope and non-goals, and a status gate that prevents draft material from becoming implementation authority.

**Tech Stack:** Markdown, YAML front matter, Mermaid, Git, PowerShell structural validation

---

## Source of Truth

Implement this plan against:

- `docs/superpowers/specs/2026-08-03-architecture-documentation-system-design.md`

Do not introduce platform architecture decisions beyond that approved design. All 30 authoritative documents remain `draft` after bootstrap.

## File Structure

```text
cop-architecture/
├── README.md
├── AGENTS.md
├── CONTRIBUTING.md
├── docs/
│   ├── README.md
│   ├── vision/README.md + 3 documents
│   ├── principles/README.md + 2 documents
│   ├── architecture/README.md + 4 documents
│   ├── domains/README.md + 6 documents
│   ├── infrastructure/README.md + 5 documents
│   ├── api/README.md + 3 documents
│   ├── security/README.md + 3 documents
│   ├── roadmap/README.md + 2 documents
│   └── standards/README.md + 2 documents
├── rfc/README.md
├── adr/README.md
└── templates/
    ├── architecture-template.md
    ├── domain-template.md
    ├── rfc-template.md
    ├── adr-template.md
    └── service-design-template.md
```

## Shared Skeleton Rules

Use `apply_patch` for all file creation and editing.

Every authoritative document must contain this exact metadata shape with values supplied by the catalog tables in Tasks 3–5:

```yaml
---
id: <catalog ID>
title: <catalog English title>
status: draft
version: 0.1.0
owners:
  - architecture
last_updated: 2026-08-03
related: <catalog related ID list>
rfc: []
adr: []
---
```

Every non-domain authoritative skeleton must contain these sections:

```markdown
# <Chinese title>

## Purpose

<Catalog purpose sentence>

## Scope

<Catalog scope bullets>

## Non-goals

<Catalog non-goal bullets>

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

<Relative links derived from related IDs>
```

Every domain skeleton must use the same metadata and introductory sections, then use these domain-specific sections instead of `Architecture or Model`:

```markdown
## Ubiquitous Language

本领域的正式术语将在领域评审中定义，并与 `COP-STD-001` 保持一致。

## Bounded Context

当前尚未形成已接受的边界上下文设计。

## Aggregates and Entities

当前尚未形成已接受的聚合、实体和值对象设计。

## Commands and Queries

当前尚未形成已接受的命令与查询模型。

## Domain Events

当前尚未形成已接受的领域事件模型；未来事件必须符合 `COP-API-002`。

## Invariants

当前尚未形成已接受的领域不变量。

## Relationships

<Relative links derived from related IDs>
```

The explicit statements above are intentional draft-state declarations, not hidden implementation placeholders. Do not add `TBD`, `TODO`, lorem ipsum, or speculative technology choices.

### Task 1: Create Repository Entry and Governance Documents

**Files:**

- Create: `README.md`
- Create: `AGENTS.md`
- Create: `CONTRIBUTING.md`
- Create: `docs/README.md`

- [ ] **Step 1: Verify the entry documents do not exist yet**

Run:

```powershell
$paths = @('README.md', 'AGENTS.md', 'CONTRIBUTING.md', 'docs/README.md')
$paths | Where-Object { -not (Test-Path $_) }
```

Expected: all four paths are printed.

- [ ] **Step 2: Create `README.md`**

Use `apply_patch` to create a Chinese repository introduction with these exact sections and links:

```markdown
# Cloud Operations Platform Architecture

本仓库是 Cloud Operations Platform（COP，云运营平台）的架构事实来源，独立保存产品愿景、架构、领域模型、基础设施、API、安全、RFC、ADR、路线图和 AI 开发约束。

实现代码位于独立的 `cop-platform` 仓库。本仓库不保存 Go、React、Helm、Terraform 或 Kubernetes 业务实现。

## Start Here

1. [文档地图](docs/README.md)
2. [平台愿景](docs/vision/platform-vision.md)
3. [范围与非目标](docs/vision/scope-and-non-goals.md)
4. [架构原则](docs/principles/architecture-principles.md)
5. [术语表](docs/standards/terminology.md)
6. [MVP 定义](docs/roadmap/mvp-definition.md)

## Governance

- [贡献规范](CONTRIBUTING.md)
- [AI Agent 规范](AGENTS.md)
- [RFC 索引](rfc/README.md)
- [ADR 索引](adr/README.md)
- [文档模板](templates/)

## Implementation Gate

只有状态为 `accepted` 的权威文档和 ADR 才能作为 `cop-platform` 的强制实现依据。`draft` 和 `review` 文档仅用于讨论和设计收敛。
```

- [ ] **Step 3: Create `AGENTS.md`**

Use `apply_patch` with this exact policy content:

```markdown
# COP Architecture Agent Instructions

## Repository Boundary

- This repository contains architecture assets only.
- Do not add product implementation code.
- `cop-platform` is the implementation repository.

## Authority Rules

1. Read `README.md` and `docs/README.md` before making changes.
2. Treat only `accepted` authoritative documents and ADRs as implementation constraints.
3. Never promote a document from `draft` or `review` to `accepted` without explicit human approval.
4. Never make a major architecture decision inside an implementation task; create or update an RFC first.
5. When an RFC is accepted, create or link an ADR and update all affected authoritative documents.
6. Preserve stable document IDs and relative links.
7. Use Chinese prose, English filenames, and established English technical terms.
8. Prefer Mermaid for diagrams.
9. Do not add `TBD`, `TODO`, filler text, or invented requirements to accepted documents.

## Change Discipline

- Keep each document focused on one responsibility.
- State scope and non-goals explicitly.
- Update directory indexes when adding, moving, deprecating, or superseding documents.
- Keep RFC discussion, ADR decisions, and current-state architecture separate.
```

- [ ] **Step 4: Create `CONTRIBUTING.md`**

Use `apply_patch` with sections covering: change types, proposal flow, document status changes, link/index updates, review checklist, and commit scope. Include the exact workflow:

```text
Issue or design need → RFC → Review → ADR → Update authoritative documents → Implementation task
```

State that no contributor may mark a document `accepted` without explicit maintainer approval.

- [ ] **Step 5: Create `docs/README.md`**

Use `apply_patch` to define the nine documentation categories, their responsibilities, and this reading order:

```text
Vision → Principles → System Architecture → Domain Landscape → MVP Definition
       → Domain Details → Infrastructure → API → Security → Roadmap
```

Include a table with columns `Category`, `Purpose`, `Index`, and `Implementation Authority`. Only accepted documents have implementation authority.

- [ ] **Step 6: Verify the four entry documents exist and contain the implementation gate**

Run:

```powershell
$paths = @('README.md', 'AGENTS.md', 'CONTRIBUTING.md', 'docs/README.md')
if (($paths | Where-Object { -not (Test-Path $_) }).Count -ne 0) { throw 'Missing entry document' }
if (-not (Select-String -Path 'README.md' -Pattern '只有状态为 `accepted`')) { throw 'Missing implementation gate' }
if (-not (Select-String -Path 'AGENTS.md' -Pattern 'Never promote')) { throw 'Missing agent status rule' }
'PASS: repository entry and governance documents'
```

Expected: `PASS: repository entry and governance documents`.

- [ ] **Step 7: Commit Task 1**

```powershell
git add README.md AGENTS.md CONTRIBUTING.md docs/README.md
git commit -m "docs: add architecture repository governance"
```

### Task 2: Create Standard Document Templates

**Files:**

- Create: `templates/architecture-template.md`
- Create: `templates/domain-template.md`
- Create: `templates/rfc-template.md`
- Create: `templates/adr-template.md`
- Create: `templates/service-design-template.md`

- [ ] **Step 1: Verify all five templates are absent**

Run:

```powershell
$templates = @(
  'templates/architecture-template.md',
  'templates/domain-template.md',
  'templates/rfc-template.md',
  'templates/adr-template.md',
  'templates/service-design-template.md'
)
$templates | Where-Object { -not (Test-Path $_) }
```

Expected: all five paths are printed.

- [ ] **Step 2: Create `architecture-template.md`**

Use the authoritative metadata shape from Shared Skeleton Rules and include: Purpose, Scope, Non-goals, Context, Architecture or Model, Constraints, Quality Attributes, Implementation Guidance, and References. In template-only fields use descriptive angle-bracket instructions such as `<stable document ID>`; do not use `TBD` or `TODO`.

- [ ] **Step 3: Create `domain-template.md`**

Use the authoritative metadata shape and include: Purpose, Scope, Non-goals, Context, Ubiquitous Language, Bounded Context, Aggregates and Entities, Commands and Queries, Domain Events, Invariants, Relationships, Constraints, Quality Attributes, Implementation Guidance, and References.

- [ ] **Step 4: Create `rfc-template.md`**

Use metadata fields `id`, `title`, `status`, `authors`, `created`, `updated`, `related`, and `adr`. Allowed status values must be documented as `draft`, `review`, `accepted`, `rejected`, and `superseded`. Include: Summary, Motivation, Goals, Non-goals, Proposal, Alternatives, Security Impact, Operational Impact, Migration, Rollout and Rollback, Risks, Decision, and References.

- [ ] **Step 5: Create `adr-template.md`**

Use metadata fields `id`, `title`, `status`, `date`, `decision_makers`, `rfc`, `supersedes`, and `superseded_by`. Allowed status values must be documented as `proposed`, `accepted`, `deprecated`, and `superseded`. Include: Context, Decision, Consequences, Alternatives Considered, Follow-up Changes, and References.

- [ ] **Step 6: Create `service-design-template.md`**

Include: Service Responsibility, Domain Ownership, APIs, Events, Data Ownership, Dependencies, Security, Reliability, Observability, Deployment, Migration, Test Strategy, Acceptance Criteria, and References. State that a service design may only be produced from accepted platform and domain documents.

- [ ] **Step 7: Verify template section contracts**

Run:

```powershell
$checks = @{
  'templates/architecture-template.md' = @('## Purpose', '## Constraints', '## Implementation Guidance')
  'templates/domain-template.md' = @('## Ubiquitous Language', '## Domain Events', '## Invariants')
  'templates/rfc-template.md' = @('## Alternatives', '## Rollout and Rollback', '## Decision')
  'templates/adr-template.md' = @('## Context', '## Decision', '## Consequences')
  'templates/service-design-template.md' = @('## Data Ownership', '## Observability', '## Acceptance Criteria')
}
foreach ($file in $checks.Keys) {
  $content = Get-Content -Raw $file
  foreach ($heading in $checks[$file]) {
    if (-not $content.Contains($heading)) { throw "$file missing $heading" }
  }
}
'PASS: five document templates'
```

Expected: `PASS: five document templates`.

- [ ] **Step 8: Commit Task 2**

```powershell
git add templates
git commit -m "docs: add architecture document templates"
```

### Task 3: Create P0 Authoritative Document Skeletons

**Files:**

- Create the 11 P0 paths listed below.

- [ ] **Step 1: Verify the P0 files are absent**

Run a `Test-Path` check for all 11 catalog paths. Expected: missing count is `11`.

- [ ] **Step 2: Create all 11 P0 documents using Shared Skeleton Rules and this exact catalog**

| ID | Path | English title | Chinese title | Purpose | Scope | Non-goals | Related IDs |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `COP-VIS-001` | `docs/vision/platform-vision.md` | COP Platform Vision | COP 平台愿景 | 定义 COP 的使命、目标用户、核心价值和长期方向。 | 目标用户；平台价值；长期成果；成功信号 | 产品功能明细；技术实现；商业定价 | `COP-VIS-002`, `COP-VIS-003`, `COP-ROAD-001` |
| `COP-VIS-002` | `docs/vision/scope-and-non-goals.md` | COP Scope and Non-goals | COP 范围与非目标 | 明确 COP 的系统边界、阶段性范围和明确排除项。 | 自托管优先；SaaS-ready；平台职责；外部系统边界 | 单一云厂商专用平台；业务应用托管；首版完整自动化和 AI | `COP-VIS-001`, `COP-ROAD-002`, `COP-ARCH-001` |
| `COP-PRI-001` | `docs/principles/architecture-principles.md` | COP Architecture Principles | COP 架构原则 | 定义所有后续设计必须遵守的长期原则和决策优先级。 | 领域边界；松耦合；API-first；可观测；安全；可演进 | 具体产品选型；服务清单；部署参数 | `COP-VIS-001`, `COP-ARCH-002`, `COP-DOM-001` |
| `COP-PRI-002` | `docs/principles/ai-development-principles.md` | COP AI Development Principles | COP AI 开发原则 | 定义人、GPT 和 Codex 在设计、拆解、实现与评审中的职责边界。 | 文档门禁；Agent 权限；任务输入；评审与追踪 | 允许 AI 自主改变架构；用生成代码替代架构决策 | `COP-PRI-001`, `COP-STD-002`, `COP-ROAD-002` |
| `COP-ARCH-001` | `docs/architecture/system-context.md` | COP System Context | COP 系统上下文 | 描述 COP、用户、受管环境和外部平台之间的系统级关系。 | 用户角色；受管云和集群；身份系统；通知系统；对象存储 | COP 内部组件拆分；服务 API；数据库表 | `COP-VIS-002`, `COP-ARCH-002`, `COP-DOM-001` |
| `COP-ARCH-002` | `docs/architecture/logical-architecture.md` | COP Logical Architecture | COP 逻辑架构 | 描述 Portal、API、Control Plane、Data Plane、Storage 和 AI 边界。 | 逻辑层；核心职责；依赖方向；跨层交互 | 物理集群参数；服务代码结构；供应商安装步骤 | `COP-ARCH-001`, `COP-ARCH-003`, `COP-DOM-001`, `COP-INFRA-001` |
| `COP-ARCH-003` | `docs/architecture/control-plane-data-plane.md` | COP Control Plane and Data Plane | COP 控制面与数据面 | 定义控制面、数据面和受管环境内组件的职责及信任边界。 | 配置下发；采集；状态上报；数据传输；故障隔离 | 具体 Collector 实现；存储选型细节；网络端口清单 | `COP-ARCH-002`, `COP-DOM-004`, `COP-DOM-005`, `COP-INFRA-004` |
| `COP-DOM-001` | `docs/domains/domain-landscape.md` | COP Domain Landscape | COP 领域全景 | 定义 COP 的领域、子域、边界上下文和上下游关系。 | MVP 核心领域；支撑领域；通用领域；领域关系 | 微服务数量；代码包结构；数据库拆分 | `COP-ARCH-002`, `COP-DOM-002`, `COP-DOM-003`, `COP-DOM-004`, `COP-DOM-005`, `COP-DOM-006` |
| `COP-ROAD-001` | `docs/roadmap/platform-roadmap.md` | COP Platform Roadmap | COP 平台路线图 | 定义从 MVP、完整可观测、企业级、多云自动化到 AI Native 的能力演进。 | 阶段目标；能力依赖；进入和退出条件 | 精确发布日期；人员排期；未经评审的技术承诺 | `COP-VIS-001`, `COP-VIS-003`, `COP-ROAD-002` |
| `COP-ROAD-002` | `docs/roadmap/mvp-definition.md` | COP MVP Definition | COP MVP 定义 | 固化首个可用版本的核心闭环、非目标和完成条件。 | 云账号/集群接入；采集；Metadata/CMDB；指标日志；Dashboard；Alert；IAM/Audit | Workflow；Automation；FinOps；插件市场；AI/RAG 详细能力 | `COP-VIS-002`, `COP-DOM-001`, `COP-INFRA-001` |
| `COP-STD-001` | `docs/standards/terminology.md` | COP Terminology | COP 术语表 | 建立全仓库统一语言并禁止 Resource、Asset、Metadata、CMDB 等概念随意混用。 | 正式术语；定义；允许别名；禁止用法；所属领域 | API 字段全集；数据库词典；第三方产品术语翻译大全 | `COP-DOM-001`, `COP-API-001`, `COP-STD-002` |

For each Scope and Non-goals cell, render the semicolon-separated items as Markdown bullets. Resolve every related ID to the exact relative path from the catalog in the approved spec.

- [ ] **Step 3: Verify P0 count, unique IDs, status, and absence of forbidden filler**

Run:

```powershell
$files = Get-ChildItem docs -Recurse -File -Filter '*.md' | Where-Object {
  $_.Name -ne 'README.md' -and $_.FullName -notlike '*\superpowers\*'
}
$p0Ids = Select-String -Path $files.FullName -Pattern '^id: COP-' | ForEach-Object { $_.Line.Substring(4) }
if ($p0Ids.Count -ne 11) { throw "Expected 11 P0 IDs, found $($p0Ids.Count)" }
if (($p0Ids | Sort-Object -Unique).Count -ne 11) { throw 'Duplicate P0 IDs' }
if ((Select-String -Path $files.FullName -Pattern '^status: (?!draft$)' -AllMatches).Count -ne 0) { throw 'Non-draft P0 document' }
if ((Select-String -Path $files.FullName -Pattern '\b(TBD|TODO)\b|lorem ipsum' -AllMatches).Count -ne 0) { throw 'Forbidden filler found' }
'PASS: 11 P0 skeletons'
```

Expected: `PASS: 11 P0 skeletons`.

- [ ] **Step 4: Commit Task 3**

```powershell
git add docs/vision docs/principles docs/architecture docs/domains docs/roadmap docs/standards
git commit -m "docs: add P0 architecture document skeletons"
```

### Task 4: Create P1 Authoritative Document Skeletons

**Files:**

- Create the 14 P1 paths listed below.

- [ ] **Step 1: Verify the P1 files are absent**

Run a `Test-Path` check for all 14 catalog paths. Expected: missing count is `14`.

- [ ] **Step 2: Create all 14 P1 documents using Shared Skeleton Rules and this exact catalog**

| ID | Path | English title | Chinese title | Purpose | Scope | Non-goals | Related IDs |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `COP-VIS-003` | `docs/vision/product-capabilities.md` | COP Product Capabilities | COP 产品能力地图 | 描述 COP 能力分组、依赖和路线图阶段归属。 | MVP 能力；后续能力；能力依赖；用户价值 | 页面清单；服务清单；迭代排期 | `COP-VIS-001`, `COP-ROAD-001`, `COP-ROAD-002` |
| `COP-ARCH-004` | `docs/architecture/integration-architecture.md` | COP Integration Architecture | COP 集成架构 | 定义同步 API、异步事件、外部连接器和集成错误边界。 | REST/gRPC；事件总线；Webhook；Provider 接口；幂等性 | 每个接口的字段设计；特定 SDK 实现；消息主题全集 | `COP-ARCH-002`, `COP-API-001`, `COP-API-002`, `COP-DOM-004` |
| `COP-DOM-002` | `docs/domains/iam-domain.md` | COP IAM Domain | COP IAM 领域 | 定义身份、主体、组织、租户和访问管理领域。 | User；Service Account；Organization；Tenant；Membership | IdP 产品选型；登录页面设计；权限策略语法细节 | `COP-DOM-001`, `COP-SEC-001`, `COP-SEC-002` |
| `COP-DOM-003` | `docs/domains/resource-metadata-domain.md` | COP Resource Metadata Domain | COP 资源与元数据领域 | 定义 Resource、Resource Type、Attribute、Relationship 和 CMDB 语义。 | 资源身份；类型；属性；关系；来源；生命周期 | 遥测时序数据；告警状态；云凭据 | `COP-DOM-001`, `COP-DOM-004`, `COP-DOM-005`, `COP-STD-001` |
| `COP-DOM-004` | `docs/domains/cloud-access-domain.md` | COP Cloud Access Domain | COP 云接入领域 | 定义云账号、凭据引用、Provider、资源发现和同步边界。 | Cloud Account；Credential Reference；Provider；Discovery；Sync Job | 保存明文凭据；云资源业务操作自动化；计费分析 | `COP-DOM-001`, `COP-DOM-003`, `COP-ARCH-003`, `COP-SEC-001` |
| `COP-DOM-005` | `docs/domains/observability-domain.md` | COP Observability Domain | COP 可观测领域 | 定义遥测接入、信号类型、查询边界和资源关联。 | Metrics；Logs；Traces；OTel；查询；资源关联 | 存储引擎内部实现；Dashboard 布局；告警规则状态机 | `COP-DOM-001`, `COP-DOM-003`, `COP-INFRA-004`, `COP-API-002` |
| `COP-DOM-006` | `docs/domains/alerting-domain.md` | COP Alerting Domain | COP 告警领域 | 定义告警规则、评估结果、告警实例、状态和通知关系。 | Rule；Evaluation；Alert Instance；State；Notification Reference | 通知供应商实现；自动修复 Workflow；完整事件响应流程 | `COP-DOM-001`, `COP-DOM-005`, `COP-API-002`, `COP-SEC-003` |
| `COP-INFRA-001` | `docs/infrastructure/infrastructure-overview.md` | COP Infrastructure Overview | COP 基础设施总览 | 描述 COP 管理面和可观测基础设施的总体部署边界及阶段演进。 | Kubernetes；PostgreSQL；Redis；遥测栈；对象存储；基础高可用 | 生产容量数值；具体云资源模块；安装手册 | `COP-ARCH-002`, `COP-ROAD-002`, `COP-INFRA-003`, `COP-INFRA-004` |
| `COP-INFRA-003` | `docs/infrastructure/data-storage.md` | COP Data Storage Architecture | COP 数据存储架构 | 定义关系数据、缓存、遥测数据、对象数据和事件数据的职责边界。 | PostgreSQL；Redis；VictoriaMetrics；日志/追踪存储；S3 API；事件总线 | 表结构；索引参数；容量规划数字；备份命令 | `COP-INFRA-001`, `COP-DOM-003`, `COP-DOM-005`, `COP-SEC-003` |
| `COP-INFRA-004` | `docs/infrastructure/observability-stack.md` | COP Observability Stack | COP 可观测技术栈 | 定义 OTel Collector、VictoriaMetrics、日志、追踪和查询组件的职责与数据路径。 | Telemetry pipeline；采集；转换；存储；查询；平台自监控 | 精确 Helm values；保留期数值；Dashboard 内容 | `COP-ARCH-003`, `COP-DOM-005`, `COP-INFRA-001`, `COP-INFRA-003` |
| `COP-API-001` | `docs/api/api-design-guidelines.md` | COP API Design Guidelines | COP API 设计规范 | 规定 REST、gRPC、资源命名、分页、错误和幂等性规则。 | REST；gRPC；错误模型；分页；过滤；幂等；鉴权上下文 | 具体业务接口；SDK 语言实现；网关产品配置 | `COP-ARCH-004`, `COP-API-003`, `COP-SEC-002`, `COP-STD-001` |
| `COP-API-002` | `docs/api/event-contracts.md` | COP Event Contracts | COP 事件契约规范 | 规定领域事件和集成事件的信封、命名、语义和交付约束。 | Event envelope；命名；版本；幂等；排序；重试；死信 | 具体 Topic 全集；Broker 配置；消费者代码 | `COP-ARCH-004`, `COP-DOM-005`, `COP-DOM-006`, `COP-API-003` |
| `COP-SEC-001` | `docs/security/security-architecture.md` | COP Security Architecture | COP 安全架构 | 定义信任边界、威胁目标、凭据处理、加密和安全控制。 | 身份边界；网络边界；Secrets；传输与静态加密；安全日志 | 产品级威胁模型明细；合规认证承诺；工具部署参数 | `COP-ARCH-001`, `COP-ARCH-003`, `COP-SEC-002`, `COP-SEC-003` |
| `COP-SEC-002` | `docs/security/iam-and-authorization.md` | COP IAM and Authorization | COP 身份认证与授权 | 定义认证集成、租户隔离、角色、权限和策略评估边界。 | Authentication；Tenant Context；RBAC；Policy Decision；Service Identity | IdP UI；具体策略语言；审计保留期 | `COP-DOM-002`, `COP-API-001`, `COP-SEC-001`, `COP-SEC-003` |

- [ ] **Step 3: Verify cumulative authoritative count is 25 and every status is draft**

Run:

```powershell
$files = Get-ChildItem docs -Recurse -File -Filter '*.md' | Where-Object {
  $_.Name -ne 'README.md' -and $_.FullName -notlike '*\superpowers\*'
}
$ids = Select-String -Path $files.FullName -Pattern '^id: COP-' | ForEach-Object { $_.Line.Substring(4) }
if ($ids.Count -ne 25) { throw "Expected 25 IDs after P1, found $($ids.Count)" }
if (($ids | Sort-Object -Unique).Count -ne 25) { throw 'Duplicate IDs after P1' }
$nonDraft = Select-String -Path $files.FullName -Pattern '^status: (?!draft$)'
if ($nonDraft.Count -ne 0) { throw 'Non-draft authoritative document found' }
'PASS: 25 P0/P1 skeletons'
```

Expected: `PASS: 25 P0/P1 skeletons`.

- [ ] **Step 4: Commit Task 4**

```powershell
git add docs/vision docs/architecture docs/domains docs/infrastructure docs/api docs/security
git commit -m "docs: add P1 architecture document skeletons"
```

### Task 5: Create P2 Authoritative Document Skeletons

**Files:**

- Create the 5 P2 paths listed below.

- [ ] **Step 1: Verify the P2 files are absent**

Run a `Test-Path` check for all five catalog paths. Expected: missing count is `5`.

- [ ] **Step 2: Create all five P2 documents using Shared Skeleton Rules and this exact catalog**

| ID | Path | English title | Chinese title | Purpose | Scope | Non-goals | Related IDs |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `COP-INFRA-002` | `docs/infrastructure/kubernetes-topology.md` | COP Kubernetes Topology | COP Kubernetes 拓扑 | 描述 COP 工作负载的集群、命名空间、隔离和调度边界。 | Management；Observability；受管集群；命名空间；故障域；资源隔离 | Helm values；节点规格；云厂商集群创建步骤 | `COP-INFRA-001`, `COP-INFRA-004`, `COP-INFRA-005` |
| `COP-INFRA-005` | `docs/infrastructure/network-and-ingress.md` | COP Network and Ingress | COP 网络与入口架构 | 定义外部入口、Gateway、服务流量、管理连接和网络信任边界。 | DNS/CDN；Load Balancer；Gateway API；服务通信；出口；受管环境连接 | 防火墙命令；域名清单；证书签发操作手册 | `COP-ARCH-003`, `COP-INFRA-001`, `COP-INFRA-002`, `COP-SEC-001` |
| `COP-API-003` | `docs/api/versioning-and-compatibility.md` | COP Versioning and Compatibility | COP 版本与兼容性规范 | 规定 API、事件、文档和服务契约的版本、兼容与废弃流程。 | Semantic versioning；breaking change；deprecation；migration window | 产品发布节奏；Git 分支策略；数据库迁移实现 | `COP-API-001`, `COP-API-002`, `COP-STD-002` |
| `COP-SEC-003` | `docs/security/audit-and-compliance.md` | COP Audit and Compliance | COP 审计与合规架构 | 定义审计事件、不可抵赖性、访问控制、保留和导出边界。 | Actor；Action；Resource；Result；Tenant；correlation；retention policy boundary | 承诺具体认证；法律意见；精确保留期数字 | `COP-DOM-002`, `COP-DOM-006`, `COP-INFRA-003`, `COP-SEC-001`, `COP-SEC-002` |
| `COP-STD-002` | `docs/standards/documentation-standard.md` | COP Documentation Standard | COP 文档规范 | 固化文档元数据、状态、章节、链接、图表、评审和演进规则。 | Authoritative docs；RFC；ADR；templates；indexes；Mermaid；review gates | 代码风格；产品写作风格；外部站点发布流程 | `COP-PRI-002`, `COP-API-003`, `COP-STD-001` |

- [ ] **Step 3: Verify all 30 authoritative documents**

Run:

```powershell
$files = Get-ChildItem docs -Recurse -File -Filter '*.md' | Where-Object {
  $_.Name -ne 'README.md' -and $_.FullName -notlike '*\superpowers\*'
}
$ids = Select-String -Path $files.FullName -Pattern '^id: COP-' | ForEach-Object { $_.Line.Substring(4) }
if ($files.Count -ne 30) { throw "Expected 30 authoritative files, found $($files.Count)" }
if ($ids.Count -ne 30) { throw "Expected 30 IDs, found $($ids.Count)" }
if (($ids | Sort-Object -Unique).Count -ne 30) { throw 'Duplicate authoritative IDs' }
if ((Select-String -Path $files.FullName -Pattern '^status: draft$').Count -ne 30) { throw 'All authoritative files must remain draft' }
if ((Select-String -Path $files.FullName -Pattern '\b(TBD|TODO)\b|lorem ipsum').Count -ne 0) { throw 'Forbidden filler found' }
'PASS: 30 authoritative skeletons'
```

Expected: `PASS: 30 authoritative skeletons`.

- [ ] **Step 4: Commit Task 5**

```powershell
git add docs/infrastructure docs/api docs/security docs/standards
git commit -m "docs: add P2 architecture document skeletons"
```

### Task 6: Create Directory, RFC, and ADR Indexes

**Files:**

- Create: `docs/vision/README.md`
- Create: `docs/principles/README.md`
- Create: `docs/architecture/README.md`
- Create: `docs/domains/README.md`
- Create: `docs/infrastructure/README.md`
- Create: `docs/api/README.md`
- Create: `docs/security/README.md`
- Create: `docs/roadmap/README.md`
- Create: `docs/standards/README.md`
- Create: `rfc/README.md`
- Create: `adr/README.md`
- Modify: `docs/README.md`

- [ ] **Step 1: Create each category index**

Each category `README.md` must contain:

1. Category purpose.
2. A table with columns `ID`, `Document`, `Priority`, `Status`, and `Purpose`.
3. Every document in that category exactly once.
4. A recommended reading order.
5. A statement that only `accepted` documents have implementation authority.

Use the exact IDs, paths, priorities, and purposes from Tasks 3–5. Links must be relative to the index file, for example `[Platform Vision](platform-vision.md)`.

- [ ] **Step 2: Create `rfc/README.md`**

Include:

- Purpose of RFCs.
- Filename rule `RFC-0001-topic.md`.
- Status flow `draft → review → accepted/rejected → superseded`.
- Required acceptance workflow: accepted RFC → linked ADR → authoritative document updates.
- An initially empty RFC index table with columns `ID`, `Title`, `Status`, `ADR`, and `Affected Documents`.
- A link to `../templates/rfc-template.md`.

- [ ] **Step 3: Create `adr/README.md`**

Include:

- Purpose of ADRs.
- Filename rule `ADR-0001-decision.md`.
- Status flow `proposed → accepted → deprecated/superseded`.
- A statement that ADRs explain decisions but do not replace current-state documents.
- An initially empty ADR index table with columns `ID`, `Title`, `Status`, `RFC`, and `Affected Documents`.
- A link to `../templates/adr-template.md`.

- [ ] **Step 4: Update `docs/README.md` with counts and links**

The category table must report counts `3, 2, 4, 6, 5, 3, 3, 2, 2`, totaling `30`. Add direct links to every category index and the P0 reading path.

- [ ] **Step 5: Verify category indexes cover every authoritative document exactly once**

Run:

```powershell
$authoritative = Get-ChildItem docs -Recurse -File -Filter '*.md' | Where-Object {
  $_.Name -ne 'README.md' -and $_.FullName -notlike '*\superpowers\*'
}
$indexFiles = Get-ChildItem docs -Recurse -File -Filter 'README.md' | Where-Object {
  $_.FullName -ne (Resolve-Path 'docs/README.md').Path
}
foreach ($doc in $authoritative) {
  $matches = Select-String -Path $indexFiles.FullName -SimpleMatch $doc.Name
  if ($matches.Count -ne 1) { throw "$($doc.Name) indexed $($matches.Count) times" }
}
'PASS: every authoritative document indexed exactly once'
```

Expected: `PASS: every authoritative document indexed exactly once`.

- [ ] **Step 6: Commit Task 6**

```powershell
git add docs rfc adr
git commit -m "docs: add architecture document indexes"
```

### Task 7: Run Final Documentation-System Verification

**Files:**

- Verify all files created in Tasks 1–6.
- Modify only files that fail a verification condition.

- [ ] **Step 1: Verify the expected structure and counts**

Run:

```powershell
$expectedAuthoritative = 30
$expectedTemplates = 5
$expectedCategoryIndexes = 9

$authoritative = Get-ChildItem docs -Recurse -File -Filter '*.md' | Where-Object {
  $_.Name -ne 'README.md' -and $_.FullName -notlike '*\superpowers\*'
}
$templates = Get-ChildItem templates -File -Filter '*.md'
$categoryIndexes = Get-ChildItem docs -Directory | ForEach-Object {
  Join-Path $_.FullName 'README.md'
} | Where-Object { Test-Path $_ }

if ($authoritative.Count -ne $expectedAuthoritative) { throw "Authoritative count: $($authoritative.Count)" }
if ($templates.Count -ne $expectedTemplates) { throw "Template count: $($templates.Count)" }
if ($categoryIndexes.Count -ne $expectedCategoryIndexes) { throw "Category index count: $($categoryIndexes.Count)" }
'PASS: structure and counts'
```

Expected: `PASS: structure and counts`.

- [ ] **Step 2: Verify metadata and ID uniqueness**

Run:

```powershell
$authoritative = Get-ChildItem docs -Recurse -File -Filter '*.md' | Where-Object {
  $_.Name -ne 'README.md' -and $_.FullName -notlike '*\superpowers\*'
}
$required = @('id:', 'title:', 'status:', 'version:', 'owners:', 'last_updated:', 'related:', 'rfc:', 'adr:')
foreach ($file in $authoritative) {
  $content = Get-Content -Raw $file.FullName
  foreach ($field in $required) {
    if (-not $content.Contains($field)) { throw "$($file.FullName) missing $field" }
  }
}
$ids = Select-String -Path $authoritative.FullName -Pattern '^id: COP-' | ForEach-Object { $_.Line.Substring(4) }
if (($ids | Sort-Object -Unique).Count -ne 30) { throw 'IDs are not unique' }
if ((Select-String -Path $authoritative.FullName -Pattern '^status: draft$').Count -ne 30) { throw 'Status gate failure' }
'PASS: metadata and unique IDs'
```

Expected: `PASS: metadata and unique IDs`.

- [ ] **Step 3: Verify relative Markdown links resolve**

Run:

```powershell
$markdown = Get-ChildItem . -Recurse -File -Filter '*.md' | Where-Object {
  $_.FullName -notlike '*\.git\*' -and $_.FullName -notlike '*\.superpowers\*'
}
foreach ($file in $markdown) {
  $content = Get-Content -Raw $file.FullName
  $links = [regex]::Matches($content, '\[[^\]]+\]\(([^)#]+\.md)(?:#[^)]+)?\)')
  foreach ($link in $links) {
    $target = Join-Path $file.DirectoryName $link.Groups[1].Value
    if (-not (Test-Path $target)) { throw "Broken link in $($file.FullName): $($link.Groups[1].Value)" }
  }
}
'PASS: relative Markdown links'
```

Expected: `PASS: relative Markdown links`.

- [ ] **Step 4: Verify no forbidden filler and no accidental implementation code**

Run:

```powershell
$authoritative = Get-ChildItem docs -Recurse -File -Filter '*.md' | Where-Object {
  $_.Name -ne 'README.md' -and $_.FullName -notlike '*\superpowers\*'
}
$bootstrapContent = @($authoritative.FullName) + @(Get-ChildItem templates -File -Filter '*.md' | ForEach-Object { $_.FullName })
if ((Select-String -Path $bootstrapContent -Pattern '\b(TBD|TODO)\b|lorem ipsum').Count -ne 0) {
  throw 'Forbidden filler found'
}
$codeExtensions = @('*.go', '*.ts', '*.tsx', '*.js', '*.jsx', '*.py', '*.tf', '*.yaml', '*.yml')
$implementationFiles = foreach ($pattern in $codeExtensions) {
  Get-ChildItem . -Recurse -File -Filter $pattern | Where-Object {
    $_.FullName -notlike '*\.git\*' -and $_.FullName -notlike '*\.superpowers\*'
  }
}
if ($implementationFiles.Count -ne 0) { throw 'Unexpected implementation file found' }
'PASS: no filler or implementation code'
```

Expected: `PASS: no filler or implementation code`.

- [ ] **Step 5: Verify formatting and repository state**

Run:

```powershell
git diff --check
git status --short
```

Expected: `git diff --check` exits successfully. `git status --short` is empty after all task commits.

- [ ] **Step 6: Review commit history**

Run:

```powershell
git log --oneline --decorate -8
```

Expected: the design commit plus separate governance, templates, P0, P1, P2, and index commits are present.

## Completion Conditions

Do not declare the bootstrap complete unless all Task 7 commands pass with their stated outputs. If a check fails, fix the smallest relevant document set, rerun the failed check, then rerun all Task 7 checks before reporting completion.
