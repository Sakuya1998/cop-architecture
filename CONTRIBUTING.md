# Contributing to COP Architecture

## Change Types
- 文档勘误：修正已确认事实、链接或索引。
- 架构提案：通过 RFC 讨论新的或有影响的架构决策。
- 决策记录：将已接受的 RFC 结论记录为 ADR，并同步权威文档。

## Proposal Flow
Issue or design need → RFC → Review → ADR → Update authoritative documents → Implementation task

架构变化先创建或更新 RFC。评审完成并获得维护者批准后，创建或关联 ADR，再更新受影响的权威文档，最后才能创建实现任务。

## Document Status Changes
文档状态使用 `draft`、`review` 和 `accepted`。任何贡献者均不得在没有明确维护者批准的情况下将文档标记为 `accepted`。状态变化应在文档和相关索引中同步反映。

## Link and Index Updates
新增、移动、弃用或替代文档时，更新所属目录索引、入站相对链接和引用该文档的导航页。保留稳定文档 ID；被替代的文档应链接到其替代项或 ADR。

## Review Checklist
- 范围、非目标和文档状态明确。
- 架构提案已有 RFC，接受的决策已有 ADR。
- 所有相对链接和目录索引已更新。
- 已接受文档不包含 `TBD`、`TODO`、填充文本或未经确认的需求。
- 内容与现有权威文档一致，且不包含产品实现代码。

## Commit Scope
每个提交只覆盖一个可审查的文档变更主题。将 RFC 讨论、ADR 决策、当前架构描述和实现代码分开提交；本仓库不提交 `cop-platform` 的实现代码。
