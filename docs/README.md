# COP Architecture Documentation Map

本文档地图定义架构资料的目录职责和阅读路径。只有状态为 `accepted` 的文档具有实现约束力。

## Reading Order
Vision → Principles → System Architecture → Domain Landscape → MVP Definition
       → Domain Details → Infrastructure → API → Security → Roadmap

## P0 Reading Path

1. [COP Platform Vision](vision/platform-vision.md)
2. [COP Scope and Non-goals](vision/scope-and-non-goals.md)
3. [COP Architecture Principles](principles/architecture-principles.md)
4. [COP System Context](architecture/system-context.md)
5. [COP Logical Architecture](architecture/logical-architecture.md)
6. [COP Control Plane and Data Plane](architecture/control-plane-data-plane.md)
7. [COP Domain Landscape](domains/domain-landscape.md)
8. [COP MVP Definition](roadmap/mvp-definition.md)
9. [COP Platform Roadmap](roadmap/platform-roadmap.md)
10. [COP Terminology](standards/terminology.md)

## Categories
| Category | Purpose | Index | Documents | Implementation Authority |
| --- | --- | --- | ---: | --- |
| vision | 产品愿景、范围与非目标。 | [vision](vision/README.md) | 3 | 仅 `accepted` 文档。 |
| principles | 架构原则与决策约束。 | [principles](principles/README.md) | 2 | 仅 `accepted` 文档。 |
| architecture | 系统架构、边界和关键交互。 | [architecture](architecture/README.md) | 4 | 仅 `accepted` 文档。 |
| domains | 领域全景、领域模型和领域细节。 | [domains](domains/README.md) | 6 | 仅 `accepted` 文档。 |
| infrastructure | 基础设施、部署和运行环境约束。 | [infrastructure](infrastructure/README.md) | 5 | 仅 `accepted` 文档。 |
| api | API 设计、契约和集成约束。 | [api](api/README.md) | 3 | 仅 `accepted` 文档。 |
| security | 安全架构、控制和安全要求。 | [security](security/README.md) | 3 | 仅 `accepted` 文档。 |
| roadmap | MVP 定义、阶段目标和路线图。 | [roadmap](roadmap/README.md) | 2 | 仅 `accepted` 文档。 |
| standards | 术语、文档标准和跨文档约定。 | [standards](standards/README.md) | 2 | 仅 `accepted` 文档。 |

总数：30 篇权威文档。

`vision` 说明平台为何存在；`principles` 确立架构判断标准；`architecture` 描述系统当前结构；`domains` 组织业务领域知识；`infrastructure` 记录运行基础；`api` 定义系统边界契约；`security` 规定安全约束；`roadmap` 管理演进顺序；`standards` 保持术语和文档一致性。
