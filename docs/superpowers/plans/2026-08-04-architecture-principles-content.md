# COP 架构原则内容实施计划

> **面向执行代理：** 必须使用子技能 superpowers:subagent-driven-development（推荐）或 superpowers:executing-plans，按任务逐项实施本计划。步骤使用 checkbox（`- [ ]`）语法跟踪。

**Goal:** 在保持元数据契约和 `draft` 门禁不变的前提下，将 `COP-PRI-001` 的初始化正文替换为已批准的决策层级、核心架构原则、验证规则和例外治理。

**Architecture:** 保持稳定的权威文档结构不变。在决策优先级、核心原则、评审证据、例外治理和成功标准下组织已批准规则；保留既有 `Purpose`、`Scope`、`Non-goals`、`Implementation Guidance` 和 `References` 契约。

**Tech Stack:** Markdown、YAML front matter、PowerShell 验证、Git。

---

### 任务 1：编写并验证架构原则

**文件：**
- 修改：`docs/principles/architecture-principles.md`
- 测试：在 worktree 中运行 PowerShell 结构、固定章节和链接检查

- [ ] **步骤 1：保留权威元数据和固定章节**

保持现有 YAML front matter 逐字节等价，包括 `id: COP-PRI-001`、`status: draft`、版本、owners、更新时间、related IDs 和空 RFC/ADR 数组。保持 H1、`Purpose`、`Scope`、`Non-goals`、`Implementation Guidance` 和三个既有 `References` 链接不变。

- [ ] **步骤 2：运行失败的结构检查（RED）**

编辑前从 worktree 根目录运行：

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

预期结果：以 `Missing: ### Decision Priority` 失败，证明初始化骨架不满足已批准结构。

- [ ] **步骤 3：替换 `Context` 并加入决策模型**

将 `Context` 替换为说明以下内容的正文：

- 本文承接 `COP-VIS-001` 与 `COP-VIS-002` 的平台方向和范围。
- 本文为后续架构与领域设计提供共同决策规则。
- `COP-ARCH-002` 与 `COP-DOM-001` 继续分别负责详细逻辑架构和领域边界。
- 本文在状态变为 `accepted` 前不形成实现约束。

在 `Architecture or Model` 下添加 `### Decision Priority`，并保持以下精确顺序：

1. 安全、权限与隔离边界。
2. 数据正确性与稳定资源身份。
3. 可靠性与可运维性。
4. 可演进性与开放集成。
5. 简单性与交付效率。

低优先级目标不得以违反更高优先级的适用约束为代价。只有满足所有适用的更高优先级约束的候选方案，才比较低优先级目标。同级冲突选择更简单、可验证、可回退或可恢复的方案。无法明确裁决或需要改变优先级时，必须进入 RFC/ADR 和相关权威文档评审。

- [ ] **步骤 4：添加包含八个精确子节的 `Core Principles`**

添加 `### Core Principles` 和以下子节：

#### Secure by Default

- 默认拒绝并实施最小权限。
- 显式表达组织、租户、身份、资源和访问上下文。
- 保存受控凭据引用，不把敏感值作为普通领域数据传播。
- 审计敏感操作的主体、目标、结果和时间。
- 管理面、受管环境和外部系统不能仅因网络位置而默认互信。

#### Stable Resource Identity and Data Correctness

- 统一资源身份稳定、可追踪，并具有明确权威来源。
- 权威来源约束身份语义和写入责任，但不预设单一服务或数据库。
- 外部数据暴露来源、同步状态和新鲜度。
- 冲突、未知和过期状态显式表达，不能静默覆盖为成功。

#### Domain-oriented Boundaries

- 按业务能力、数据所有权和变化原因划分领域。
- 每个边界具有明确职责、输入、输出和依赖。
- 领域边界不等同于微服务、代码包或数据库数量。
- 技术分层与流程可以组织领域内部，但不能替代领域责任边界。

#### Domain Data Ownership

- 每个领域拥有自己的写模型、不变量和写入语义。
- 禁止一个领域绕过 Contract 直接写入另一个领域的数据存储。
- 跨领域读取通过稳定 Contract 或受控读模型完成。
- 受控读模型必须声明所有者、来源 Contract、构建或更新方式、数据新鲜度和失败语义。
- 不得以受控读模型名义直接读取其他领域的私有存储。
- 复制的读模型不能成为未声明的新权威来源。

#### Contract-first Interaction

- Contract-first 扩展而非否定现有 `API-first` 范围；API 是跨边界 Contract 的一种形式。
- 先定义语义、所有权、兼容性和失败行为，再选择通信技术。
- 需要即时结果的查询或命令可以使用同步 API；状态传播和解耦可以使用事件。
- 不强制所有交互同步，也不强制所有交互事件化。
- Contract 支持版本演进，并按交互类型定义实际适用的失败语义；不要求每个 Contract 机械声明全部失败模式。

#### Observable and Recoverable Operations

- 所有可运行组件必须暴露健康和依赖信号；只有承担处理、复制或同步职责的组件才必须暴露处理状态和数据新鲜度信号。
- 隔离故障，避免单一外部依赖无边界扩散失败。
- 可重试操作及其处理必须幂等，或提供等价的重复处理保护。
- 定义可验证的降级、恢复和重新同步路径。
- 事件消费者处理重复、乱序和延迟，不能依赖未声明的到达顺序。

#### Open and Evolvable Architecture

- 通过 Adapter 或等价稳定边界隔离云厂商、Kubernetes 和可观测产品语义。
- 核心领域模型不绑定单一云厂商、集群或存储引擎。
- Contract 变更具有兼容策略；重大变更优先具备迁移和回退路径。
- 确实不可回退时，RFC/ADR 必须明确不可逆点、备份/恢复或 forward-recovery 方案、验证证据和额外批准条件。
- 自托管可以独立运行，同时不破坏未来组织和租户隔离能力。

#### Simplicity and Incremental Evolution

- 只引入当前已评审需求证明需要的复杂度。
- 选择最小、可理解、可验证、可替换的设计，并优先选择可回退方案；不可回退方案遵循上述治理。
- 不为路线图之外或未经 RFC/ADR 评审的能力预建架构。
- 不得为交付效率降低安全、数据正确性或恢复能力。

- [ ] **步骤 5：添加评审、失败、例外和成功规则**

添加 `### Review and Evidence`：

- 将新增或改变领域边界、数据权威、跨领域 Contract、安全或租户边界、故障行为、外部集成边界或迁移路径的设计视为重大设计。
- 每项重大设计说明适用原则和优先级；领域、数据和 Contract 所有权；安全、隔离、故障和兼容性影响；验证证据；迁移、回退或 forward-recovery 方案及替代方案。
- 可自动化的 Contract 兼容、权限边界、幂等、数据不变量、迁移、回退或 forward-recovery 检查进入测试或 CI。
- 领域边界、责任归属、复杂度、供应商绑定和运营风险进入架构评审清单。
- 各类交互必须识别并定义实际适用的失败语义；不适用项无需机械声明，但不能留下未处理或无法区分的失败状态。
- 同步交互可考虑拒绝、超时、重试和部分失败；异步交互可考虑重复、乱序、延迟和消费失败；外部同步可考虑来源、同步状态、新鲜度和冲突状态。
- 未知、降级和失败状态必须与成功状态可区分。

添加 `### Exception Governance`：

- 违反任何适用原则的设计默认阻断；只有获批且有边界的例外才能继续。
- 例外通过 RFC/ADR 记录原因、范围、备选方案、责任人、验证措施以及到期或退出条件。
- RFC 接受后必须关联 `accepted` ADR，并在受影响的权威文档中记录该例外。
- `accepted` ADR 只能批准明确且有边界的例外，不能隐式改变原则文档。
- 长期改变原则或优先级时，更新 `COP-PRI-001` 和受影响的权威文档。
- “临时方案”不能成为绕过评审的理由。

添加 `### Success Criteria`，包含以下结果：

- 重大设计能够指出适用原则、优先级和验证证据。
- 安全、权限、租户和凭据边界保持显式。
- 稳定资源身份、来源、同步状态和新鲜度可追踪。
- 领域写模型和不变量具有唯一责任方，不发生跨领域直接写存储。
- 跨领域交互定义 Contract、实际适用的失败语义和兼容策略。
- 故障隔离、可重试操作的幂等处理、降级和恢复可验证。
- 外部依赖通过稳定边界接入，核心模型不绑定单一供应商。
- 重大变更具有迁移和回退路径，或具有经明确批准的 forward-recovery 方案；例外具有获批 RFC/ADR。

- [ ] **步骤 6：更新约束和质量属性**

保留既有 RFC/ADR 与 `accepted` 文档约束。增加以下约束：交付速度不得绕过更高优先级的适用约束；领域不得直接写入其他领域的数据存储；外部产品语义不得泄漏到核心模型。

定义安全性、正确性、可靠性、可运维性、兼容性和可演进性质量要求，不写技术参数、供应商选择、服务数量、数据库拆分、部署拓扑或数值化 SLO。

- [ ] **步骤 7：运行聚焦验证（GREEN）**

提交前从 worktree 根目录运行：

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
  '只有满足所有适用的更高优先级约束的候选方案',
  '受控读模型必须声明所有者',
  '不得以受控读模型名义直接读取其他领域的私有存储',
  '所有可运行组件必须暴露健康和依赖信号',
  '只有承担处理、复制或同步职责的组件',
  '按交互类型定义适用的失败语义',
  '可重试操作及其处理必须幂等',
  '各类交互必须识别并定义实际适用的失败语义',
  '违反任何适用原则默认阻断',
  '不可逆点、备份/恢复或 forward-recovery 方案',
  'forward-recovery',
  'RFC 接受后必须关联 `accepted` ADR',
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

预期输出：`PASS: architecture principles content`。

- [ ] **步骤 8：审查范围并提交**

审查完整差异，确认没有供应商选择、服务清单、API shape、event 字段、数据库拆分、部署拓扑或数值化 SLO。随后提交：

```powershell
git add docs/principles/architecture-principles.md
git commit -m "docs: define architecture principles"
```

提交后，要求 `git diff-tree --no-commit-id --name-only -r HEAD` 只包含 `docs/principles/architecture-principles.md`，并要求 `git status --porcelain` 输出为空。
