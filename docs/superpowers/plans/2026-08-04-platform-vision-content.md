# COP Platform Vision Content Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the `COP-VIS-001` initialization prose with the approved platform vision while preserving its authoritative metadata contract and draft governance gate.

**Architecture:** Keep the existing authoritative document shape and stable ID unchanged. Replace only the generic draft declarations in `Context`, `Architecture or Model`, `Constraints`, and `Quality Attributes` with the approved platform direction; retain the explicit draft-only `Implementation Guidance` and existing related-document links.

**Tech Stack:** Markdown, YAML front matter, PowerShell verification, Git.

---

### Task 1: Write and Verify the Platform Vision

**Files:**
- Modify: `docs/vision/platform-vision.md`
- Test: PowerShell structural and link checks in the worktree

- [ ] **Step 1: Preserve the document contract before editing**

Record the current front matter values for `id`, `title`, `status`, `version`, `owners`, `last_updated`, `related`, `rfc`, and `adr`. The implementation must preserve these exact values:

```yaml
id: COP-VIS-001
title: COP Platform Vision
status: draft
version: 0.1.0
owners:
  - architecture
last_updated: 2026-08-03
related:
  - COP-VIS-002
  - COP-VIS-003
  - COP-ROAD-001
rfc: []
adr: []
```

- [ ] **Step 2: Replace only the approved vision prose**

Keep the existing H1, `Purpose`, `Scope`, `Non-goals`, and `References` links. Replace the generic body under `Context`, `Architecture or Model`, `Constraints`, and `Quality Attributes` with the following content:

```markdown
## Context

COP 首要交付形态是开源、自托管优先，同时保持未来 SaaS 多租户能力。第一阶段 MVP 聚焦云账号/集群接入、资源采集、Metadata/CMDB、指标与日志接入、Dashboard、Alert、IAM/Audit 的运营闭环。

本文服务于企业内部的平台工程、SRE 和运维团队；安全、审计和管理角色是重要协作用户。本文不替代云厂商控制台、可观测存储引擎或业务应用平台，而是在这些系统之上建立统一的资源视图和运营入口。

## Architecture or Model

### Mission

为企业内部的平台工程、SRE 和运维团队提供开源、自托管优先的统一云运营平台，以资源为中心连接云账号、Kubernetes、Metadata/CMDB、指标、日志和告警，让分散的基础设施与运营信号形成可理解、可追踪的统一上下文。

### Target Users

- 平台工程团队：负责云平台、Kubernetes 和公共基础设施能力建设。
- SRE 团队：负责可靠性、可观测性、故障定位和运行风险控制。
- 运维团队：负责资源接入、日常监控、告警处理和运营管理。

### Core Problem

云账号、Kubernetes 集群、资源、Metadata/CMDB、指标、日志和告警分散在不同系统中，资源身份与运营信号缺少稳定关联。团队难以快速回答：有什么资源、当前状态如何、相关信号在哪里、告警影响什么。

### Value Proposition

COP 以统一资源模型为中心：

1. 建立跨云和 Kubernetes 的统一资源身份与目录。
2. 关联资源、元数据、遥测信号、告警、权限和审计信息。
3. 提供一致的资源入口，支持从资产发现到异常定位的日常运营闭环。
4. 通过开放集成与自托管交付降低平台锁定，同时保留未来 SaaS 演进边界。

### Long-term Direction

COP 是跨云与 Kubernetes 环境的统一运营层，从统一可见性起步，逐步演进为企业云与 Kubernetes 的统一运营平台：

1. 建立可信的资源目录和统一运营视图。
2. 形成身份、权限、审计和策略治理能力。
3. 在明确权限、审批和审计边界下引入自动化。
4. 基于统一资源上下文提供可解释、可追踪的 AI 辅助分析与运营建议。

具体阶段、能力依赖和退出条件由 `COP-ROAD-001` 管理，平台愿景不承诺发布日期或实现选型。

### Success Signals

- 团队能够接入云账号或集群并持续发现资源。
- 每个资源拥有稳定身份，并可关联元数据、指标、日志和告警。
- 运维人员能够从统一资源入口完成日常状态查看和异常定位。
- 告警能够追溯到受影响资源、相关信号和操作上下文。
- 权限与关键操作具备可审计性。
- 新环境和数据源能够通过稳定边界接入，而不破坏核心资源模型。
- 自托管交付可独立运行，同时不封死未来 SaaS 多租户演进。

## Constraints

- 不得绕过 RFC/ADR 流程引入重大架构决策。
- 不得与相关 `accepted` 权威文档产生冲突。
- 具体阶段、能力依赖和退出条件必须在 `COP-ROAD-001` 中维护。

## Quality Attributes

愿景相关的架构演进必须保持安全性、可靠性、可扩展性、可运维性和兼容性；具体质量目标在对应的 accepted 架构、领域、基础设施和安全文档中定义。
```

Keep `Implementation Guidance` exactly as the existing draft gate:

```markdown
## Implementation Guidance

在本文状态变为 `accepted` 前，Codex 只能使用本文理解设计范围，不得据此生成生产实现。
```

- [ ] **Step 3: Run focused verification**

Run from the worktree root:

```powershell
$file = Get-Item 'docs/vision/platform-vision.md'
$content = Get-Content -Raw -Encoding UTF8 $file.FullName
if (-not $content.Contains('id: COP-VIS-001')) { throw 'ID changed' }
if (-not $content.Contains('status: draft')) { throw 'Draft gate changed' }
if (-not $content.Contains('企业内部的平台工程、SRE 和运维团队')) { throw 'Primary users missing' }
if (-not $content.Contains('统一资源模型为中心')) { throw 'Value proposition missing' }
if (-not $content.Contains('COP-ROAD-001')) { throw 'Roadmap boundary missing' }
if ($content -match '\b(TBD|TODO)\b|lorem ipsum') { throw 'Forbidden filler found' }
$links = [regex]::Matches($content, '\[[^\]]+\]\(([^)#]+\.md)(?:#[^)]+)?\)')
foreach ($link in $links) {
  if (-not (Test-Path (Join-Path $file.DirectoryName $link.Groups[1].Value))) { throw "Broken link: $($link.Groups[1].Value)" }
}
'PASS: platform vision content'
```

Expected output: `PASS: platform vision content`.

- [ ] **Step 4: Run repository checks**

Run `git diff --check` and confirm only `docs/vision/platform-vision.md` is modified in addition to this plan's execution commit. Confirm the YAML front matter remains unchanged by comparing the first 15 lines with the baseline.

- [ ] **Step 5: Commit the document update**

```powershell
git add docs/vision/platform-vision.md
git commit -m "docs: define platform vision"
```
