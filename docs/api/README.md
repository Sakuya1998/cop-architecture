# API Index

## 分类目的

定义 COP 的 API、事件和版本兼容契约，作为系统边界与集成协作的规范入口。

| ID | Document | Priority | Status | Purpose |
| --- | --- | --- | --- | --- |
| COP-API-001 | [COP API Design Guidelines](api-design-guidelines.md) | P1 | draft | 规定 REST、gRPC、资源命名、分页、错误和幂等性规则。 |
| COP-API-002 | [COP Event Contracts](event-contracts.md) | P1 | draft | 规定领域事件和集成事件的信封、命名、语义和交付约束。 |
| COP-API-003 | [COP Versioning and Compatibility](versioning-and-compatibility.md) | P2 | draft | 规定 API、事件、文档和服务契约的版本、兼容与废弃流程。 |

## 推荐阅读顺序

1. COP-API-001 COP API Design Guidelines
2. COP-API-002 COP Event Contracts
3. COP-API-003 COP Versioning and Compatibility

只有 `status: accepted` 的文档具有实现权威；`draft` 和 `review` 文档仅用于讨论与设计收敛。
