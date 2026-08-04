# COP Architecture Principles Content Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the `COP-PRI-001` initialization prose with the approved decision hierarchy, core architecture principles, verification rules, and exception governance while preserving its metadata contract and draft gate.

**Architecture:** Keep the stable authoritative-document shape unchanged. Organize the approved rules under decision priority, core principles, review evidence, exception governance, and success criteria; retain the existing catalog `Purpose`, `Scope`, `Non-goals`, `Implementation Guidance`, and `References` contracts.

**Tech Stack:** Markdown, YAML front matter, PowerShell verification, Git.

---

### Task 1: Write and Verify Architecture Principles

**Files:**
- Modify: `docs/principles/architecture-principles.md`
- Test: PowerShell structural, fixed-section, and link checks in the worktree

- [ ] **Step 1: Preserve the authoritative metadata and fixed sections**

Keep the current YAML front matter byte-for-byte equivalent, including `id: COP-PRI-001`, `status: draft`, version, owners, update date, related IDs, and empty RFC/ADR arrays. Keep the H1, `Purpose`, `Scope`, `Non-goals`, `Implementation Guidance`, and three existing `References` links unchanged.

- [ ] **Step 2: Run the failing structural check**

Run from the worktree root before editing:

```powershell
$file = Get-Item 'docs/principles/architecture-principles.md'
$content = Get-Content -Raw -Encoding UTF8 $file.FullName
$required = @(
  '### Decision Priority',
  '### Core Principles',
  '### Review and Evidence',
  '### Exception Governance',
  '### Success Criteria'
)
foreach ($value in $required) {
  if (-not $content.Contains($value)) { throw "Missing: $value" }
}
```

Expected: FAIL with `Missing: ### Decision Priority`, proving the initialization skeleton does not satisfy the approved structure.

- [ ] **Step 3: Replace `Context` and add the decision model**

Replace `Context` with prose that states:

- The document applies the platform direction and scope from `COP-VIS-001` and `COP-VIS-002`.
- It provides common decision rules for later architecture and domain designs.
- `COP-ARCH-002` and `COP-DOM-001` retain responsibility for detailed logical architecture and domain boundaries.
- The principles remain non-binding until the document becomes `accepted`.

Under `Architecture or Model`, add `### Decision Priority` with this exact precedence:

1. Security, authorization, and isolation boundaries.
2. Data correctness and stable resource identity.
3. Reliability and operability.
4. Evolvability and open integration.
5. Simplicity and delivery efficiency.

State that higher priorities override lower priorities. At the same level, prefer the simpler, verifiable, reversible option. Unclear conflicts or changes to precedence require RFC/ADR and authoritative-document review.

- [ ] **Step 4: Add `Core Principles` with eight exact subsections**

Add `### Core Principles` and these subsections:

#### Secure by Default

- Default deny and least privilege.
- Explicit organization, tenant, identity, resource, and access context.
- Store controlled credential references rather than propagating secret values as ordinary domain data.
- Audit the actor, target, result, and time of sensitive operations.
- Do not trust management-plane, managed-environment, or external-system traffic solely because of network location.

#### Stable Resource Identity and Data Correctness

- Stable, traceable unified resource identity with an explicit authority.
- Authority governs identity semantics and write responsibility without requiring one service or database.
- Expose source, synchronization status, and freshness for external data.
- Represent conflict, unknown, and stale states explicitly; do not silently overwrite them into apparent success.

#### Domain-oriented Boundaries

- Divide domains by capability, data ownership, and reason to change.
- Give each boundary clear responsibilities, inputs, outputs, and dependencies.
- Do not equate a domain boundary with a microservice, package, or database count.
- Technical layers and workflows may organize internals but cannot replace domain responsibility boundaries.

#### Domain Data Ownership

- Each domain owns its write model, invariants, and write semantics.
- Prohibit one domain from directly writing another domain's storage outside a Contract.
- Use a stable Contract or governed read model for cross-domain reads.
- Replicated read models cannot become undeclared authorities.

#### Contract-first Interaction

- State that Contract-first extends rather than rejects the existing `API-first` scope, and that an API is one form of cross-boundary Contract.
- Define semantics, ownership, compatibility, and failure behavior before choosing communication technology.
- Use synchronous APIs for queries or commands that need an immediate result; use events for state propagation and decoupling.
- Do not require all interactions to be synchronous or all to be event-driven.
- Version Contracts and define rejection, timeout, retry, partial-failure, and delayed-propagation behavior.

#### Observable and Recoverable Operations

- Each runnable component exposes health, dependency, processing-state, and data-freshness signals.
- Isolate failures so one external dependency cannot spread failure without bounds.
- Make retries idempotent or provide equivalent duplicate-processing protection.
- Define verifiable degradation, recovery, and resynchronization paths.
- Event consumers handle duplicates, reordering, and delay without assuming undeclared arrival order.

#### Open and Evolvable Architecture

- Isolate cloud-provider, Kubernetes, and observability-product semantics behind Adapters or equivalent stable boundaries.
- Keep the core domain model independent of one cloud provider, cluster, or storage engine.
- Give Contract changes a compatibility strategy and major changes migration and rollback paths.
- Keep self-hosted operation independent without breaking future organization and tenant isolation.

#### Simplicity and Incremental Evolution

- Add only complexity justified by current reviewed needs.
- Prefer the smallest understandable, verifiable, replaceable, reversible design.
- Do not prebuild architecture for capabilities absent from the roadmap or RFC/ADR review.
- Never trade away security, data correctness, or recovery merely for delivery efficiency.

- [ ] **Step 5: Add review, failure, exception, and success rules**

Add `### Review and Evidence`:

- Treat changes to domain boundaries, data authority, cross-domain Contracts, security or tenant boundaries, failure behavior, external integration boundaries, or migration paths as major designs.
- Require each major design to state applicable principles and priority; domain, data, and Contract ownership; security, isolation, failure, and compatibility impact; verification evidence; migration, rollback, and alternatives.
- Put automatable Contract compatibility, authorization boundaries, idempotency, data invariants, migration, and rollback checks into tests or CI.
- Put domain boundaries, responsibility ownership, complexity, vendor coupling, and operational risk into an architecture review checklist.
- Define synchronous rejection, timeout, retry, and partial-failure behavior; asynchronous duplicate, reordering, delay, and consumption-failure behavior; and external synchronization source, state, freshness, and conflict behavior.
- Keep unknown, degraded, and failed states distinguishable from success.

Add `### Exception Governance`:

- Block designs that violate higher-priority principles by default.
- Require RFC/ADR records for exceptions, including reason, scope, alternatives, owner, verification, and expiry or exit condition.
- Allow an accepted ADR to approve a bounded exception without implicitly changing the principle document.
- Require updates to `COP-PRI-001` and affected authoritative documents for lasting principle or priority changes.
- Reject “temporary” as a reason to bypass review.

Add `### Success Criteria` with these outcomes:

- Major designs identify applicable principles, precedence, and evidence.
- Security, authorization, tenant, and credential boundaries stay explicit.
- Stable resource identity, source, synchronization state, and freshness are traceable.
- Domain write models and invariants have one responsible owner with no direct cross-domain storage writes.
- Cross-domain interactions define Contracts, failure semantics, and compatibility.
- Failure isolation, idempotent retry, degradation, and recovery are verifiable.
- External dependencies use stable boundaries without binding the core model to one vendor.
- Major changes are migratable and reversible; exceptions have approved RFC/ADR records.

- [ ] **Step 6: Update constraints and quality attributes**

Keep the existing RFC/ADR and accepted-document constraints. Add constraints that delivery speed cannot bypass higher-priority principles, domains cannot directly write other domains' storage, and external product semantics cannot leak into the core model.

Define quality expectations for security, correctness, reliability, operability, compatibility, and evolvability without technical parameters, vendor choices, service counts, database splits, deployment topology, or numeric SLOs.

- [ ] **Step 7: Run focused verification**

Run from the worktree root before committing:

```powershell
$ErrorActionPreference = 'Stop'
$path = 'docs/principles/architecture-principles.md'
$baseline = (git show "HEAD:$path") -join "`n"
$current = (Get-Content -Raw -Encoding UTF8 $path) -replace "`r`n", "`n"

function Get-FrontMatter([string]$text) {
  $match = [regex]::Match($text, '\A---\n.*?\n---\n', [Text.RegularExpressions.RegexOptions]::Singleline)
  if (-not $match.Success) { throw 'Front matter not found' }
  $match.Value
}

function Get-Section([string]$text, [string]$name) {
  $pattern = '(?ms)^## ' + [regex]::Escape($name) + '\n.*?(?=^## |\z)'
  $match = [regex]::Match($text, $pattern)
  if (-not $match.Success) { throw "Section not found: $name" }
  $match.Value.TrimEnd()
}

if ((Get-FrontMatter $baseline) -cne (Get-FrontMatter $current)) { throw 'Front matter changed' }
if (-not $current.Contains('status: draft')) { throw 'Status is not draft' }
if ([regex]::Match($baseline, '(?m)^# .+$').Value -cne [regex]::Match($current, '(?m)^# .+$').Value) { throw 'H1 changed' }

foreach ($section in @('Purpose', 'Scope', 'Non-goals', 'Implementation Guidance', 'References')) {
  if ((Get-Section $baseline $section) -cne (Get-Section $current $section)) { throw "Fixed section changed: $section" }
}

$required = @(
  '### Decision Priority',
  '### Core Principles',
  '#### Secure by Default',
  '#### Stable Resource Identity and Data Correctness',
  '#### Domain-oriented Boundaries',
  '#### Domain Data Ownership',
  '#### Contract-first Interaction',
  '#### Observable and Recoverable Operations',
  '#### Open and Evolvable Architecture',
  '#### Simplicity and Incremental Evolution',
  '### Review and Evidence',
  '### Exception Governance',
  '### Success Criteria',
  'RFC/ADR'
)
foreach ($value in $required) {
  if (-not $current.Contains($value)) { throw "Missing: $value" }
}

$forbidden = @(('TB' + 'D'), ('TO' + 'DO'), ('lorem' + ' ipsum'))
foreach ($value in $forbidden) {
  if ($current.Contains($value)) { throw "Forbidden filler: $value" }
}

$file = Get-Item $path
$links = [regex]::Matches($current, '\[[^\]]+\]\(([^)#]+\.md)(?:#[^)]+)?\)')
if ($links.Count -ne 3) { throw "Expected 3 links, found $($links.Count)" }
foreach ($link in $links) {
  $resolved = Join-Path $file.DirectoryName $link.Groups[1].Value
  if (-not (Test-Path -LiteralPath $resolved)) { throw "Broken link: $($link.Groups[1].Value)" }
}

$changed = @(git diff --name-only)
if ($changed.Count -ne 1 -or $changed[0] -ne $path) { throw "Unexpected file scope: $($changed -join ', ')" }

git diff --check
if ($LASTEXITCODE -ne 0) { throw 'git diff --check failed' }

'PASS: architecture principles content'
```

Expected output: `PASS: architecture principles content`.

- [ ] **Step 8: Review scope and commit**

Review the complete diff and confirm it contains no vendor choice, service list, API shape, event field, database split, deployment topology, or numeric SLO. Then commit:

```powershell
git add docs/principles/architecture-principles.md
git commit -m "docs: define architecture principles"
```

After committing, require `git diff-tree --no-commit-id --name-only -r HEAD` to contain only `docs/principles/architecture-principles.md`, and require `git status --porcelain` to be empty.
