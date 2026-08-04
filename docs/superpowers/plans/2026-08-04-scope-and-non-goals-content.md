# COP Scope and Non-goals Content Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the `COP-VIS-002` initialization prose with the approved platform responsibility, deployment, stage, and exclusion boundaries while preserving its metadata contract and draft gate.

**Architecture:** Keep the stable authoritative-document shape unchanged. Organize the approved content under responsibility, deployment, stage, scope-decision, and success-criteria subsections; retain the existing catalog `Purpose`, `Scope`, `Non-goals`, `Implementation Guidance`, and `References` contracts.

**Tech Stack:** Markdown, YAML front matter, PowerShell verification, Git.

---

### Task 1: Write and Verify Scope and Non-goals

**Files:**
- Modify: `docs/vision/scope-and-non-goals.md`
- Test: PowerShell structural and link checks in the worktree

- [ ] **Step 1: Preserve the authoritative metadata and fixed sections**

Keep the current YAML front matter byte-for-byte equivalent, including `id: COP-VIS-002`, `status: draft`, version, owners, update date, related IDs, and empty RFC/ADR arrays. Keep the H1, `Purpose`, `Scope`, `Non-goals`, `Implementation Guidance`, and three existing `References` links unchanged.

- [ ] **Step 2: Replace `Context` with the approved boundary context**

State that this document applies the platform direction in `COP-VIS-001`, defines responsibility and stage boundaries, and delegates detailed MVP completion conditions to `COP-ROAD-002` and system relationships to `COP-ARCH-001`. State that COP is an integration and coordination layer rather than a replacement for cloud providers, business platforms, or observability storage engines.

- [ ] **Step 3: Replace `Architecture or Model` with five exact subsections**

Add `### Responsibility Boundary` with these groups:

- **COP 负责:** unified resource model and identities; resource catalog and relationships; cloud-account/cluster connection configuration and credential references; discovery and sync tasks; Metadata/CMDB semantics; resource-to-telemetry/alert/IAM/audit correlation; unified queries, operational views, Dashboard, Alert, IAM, and Audit; organization, tenant, and access boundaries.
- **COP 集成但不替代:** cloud-provider APIs, Kubernetes APIs, external identity providers, Telemetry Collectors, metric/log/trace storage, notifications, object storage, and infrastructure systems. COP may deploy or connect these components without owning their internal implementation in this document.
- **外部系统负责:** actual cloud-resource and Kubernetes-workload execution and final state; business-application build/release/hosting; cloud-provider product semantics and infrastructure implementation; observability-engine internal data organization, query execution, and capacity management.

Add `### Deployment Boundary` with these rules:

- Self-hosted is the primary delivery form; the adopting organization operates the management plane.
- One management plane can connect multiple cloud accounts, Kubernetes clusters, and managed environments.
- Managed environments contain only the minimum components needed for collection, connection, and status reporting.
- Identity, credential, network, and data-transfer boundaries must be explicit.
- Business workloads do not need to move into the COP cluster.
- Organization, tenant, membership, resource ownership, and access context remain explicit for future SaaS.
- Data models and APIs cannot assume one global organization or tenant.
- Current scope excludes SaaS billing, subscriptions, operator back office, customer lifecycle, and formal service-level commitments.

Add `### Stage Boundary` with first-release scope:

- Cloud-account and Kubernetes-cluster onboarding.
- Resource discovery, collection, and continuous synchronization.
- Metadata/CMDB and unified resource catalog.
- Metrics, logs, and basic observability-signal correlation.
- Dashboard and alerting.
- IAM, access control, and audit.

Explicitly exclude business-application hosting; a single-cloud-provider platform; resource creation, mutation, deletion, and automated remediation; Workflow, Automation, FinOps, marketplace; detailed AI/RAG; SaaS commercial operations; and implementation choices. State that future automation and AI require roadmap, RFC/ADR, and authoritative-document review.

Add `### Scope Decision Rules` with these six questions:

1. Does the requirement directly serve platform engineering, SRE, or operations?
2. Does it strengthen the unified resource model, operational context, governance, or operational loop?
3. Is it COP-owned responsibility or an external integration?
4. Does it require COP to replace a cloud provider, business platform, or storage engine?
5. Is it current MVP, a later roadmap phase, or unreviewed capability?
6. Does it affect organization, tenant, credential, network, or data-isolation boundaries?

State that unclear ownership, boundary changes, or major responsibility changes require RFC/ADR and authoritative-document review.

Add `### Success Criteria` with these outcomes:

- Every capability maps to COP-owned, COP-integrated, externally owned, or currently excluded.
- Self-hosted operation does not depend on an official COP SaaS.
- Multiple cloud accounts and clusters do not change the core resource model.
- Business workloads do not move into the COP management plane.
- MVP does not depend on resource mutation, Workflow, Automation, or AI to form a complete loop.
- Future SaaS does not require replacing organization, tenant, or resource-ownership models.

- [ ] **Step 4: Update constraints and quality attributes**

Keep the existing RFC/ADR and accepted-document constraints. Add constraints that self-hosted deployments run independently, external systems retain execution authority, and SaaS-ready boundaries remain explicit. Define quality expectations for clear ownership, secure connections, tenant isolation, evolvability, operability, and compatibility without technical parameters.

- [ ] **Step 5: Run focused verification**

Run from the worktree root:

```powershell
$file = Get-Item 'docs/vision/scope-and-non-goals.md'
$content = Get-Content -Raw -Encoding UTF8 $file.FullName
$required = @(
  'id: COP-VIS-002',
  'status: draft',
  'Responsibility Boundary',
  'Deployment Boundary',
  'Stage Boundary',
  'Scope Decision Rules',
  'Success Criteria',
  '自托管',
  'SaaS',
  'COP-ROAD-002',
  'RFC/ADR'
)
foreach ($value in $required) {
  if (-not $content.Contains($value)) { throw "Missing: $value" }
}
if ($content -match '\b(TBD|TODO)\b|lorem ipsum') { throw 'Forbidden filler found' }
$links = [regex]::Matches($content, '\[[^\]]+\]\(([^)#]+\.md)(?:#[^)]+)?\)')
foreach ($link in $links) {
  if (-not (Test-Path (Join-Path $file.DirectoryName $link.Groups[1].Value))) { throw "Broken link: $($link.Groups[1].Value)" }
}
'PASS: scope and non-goals content'
```

Expected output: `PASS: scope and non-goals content`.

- [ ] **Step 6: Verify scope and formatting**

Run `git diff --check`. Compare the first 15 lines of the target document with the baseline and require equality. Require `git diff --name-only` for the implementation commit to contain only `docs/vision/scope-and-non-goals.md`.

- [ ] **Step 7: Commit the document update**

```powershell
git add docs/vision/scope-and-non-goals.md
git commit -m "docs: define platform scope and non-goals"
```
