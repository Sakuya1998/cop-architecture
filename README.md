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
