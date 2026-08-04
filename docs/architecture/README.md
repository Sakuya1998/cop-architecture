# Architecture Index

## 分类目的

描述 COP 当前有效的系统上下文、逻辑分层、控制面与数据面边界，以及跨系统集成方式。

| ID | Document | Priority | Status | Purpose |
| --- | --- | --- | --- | --- |
| COP-ARCH-001 | [COP System Context](system-context.md) | P0 | draft | 描述 COP、用户、受管环境和外部平台之间的系统级关系。 |
| COP-ARCH-002 | [COP Logical Architecture](logical-architecture.md) | P0 | draft | 描述 Portal、API、Control Plane、Data Plane、Storage 和 AI 边界。 |
| COP-ARCH-003 | [COP Control Plane and Data Plane](control-plane-data-plane.md) | P0 | draft | 定义控制面、数据面和受管环境内组件的职责及信任边界。 |
| COP-ARCH-004 | [COP Integration Architecture](integration-architecture.md) | P1 | draft | 定义同步 API、异步事件、外部连接器和集成错误边界。 |

## 推荐阅读顺序

1. COP-ARCH-001 COP System Context
2. COP-ARCH-002 COP Logical Architecture
3. COP-ARCH-003 COP Control Plane and Data Plane
4. COP-ARCH-004 COP Integration Architecture

只有 `status: accepted` 的文档具有实现权威；`draft` 和 `review` 文档仅用于讨论与设计收敛。
