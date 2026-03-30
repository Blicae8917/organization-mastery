# 组织架构协议

> **协议编号**: 03  
> **版本**: v8.1 (Config-driven & Rubric-gated)  
> **类型**: 核心原则与架构定义

本文件定义 [Org_Code] 数字集团军的组织架构原则。具体的编制名单、角色职责与权限矩阵已抽离，请通过 `shared/config/organization.yaml` 进行动态配置。

---

## 质量门控 (Validation Rubric)

> **Contract**: 任何新增 Agent 或对现有编制进行修改的操作，必须通过以下强制校验，否则不得合入配置：

- [ ] **职能互斥性**：新角色的核心职能是否与现有 15 Agent 编制中的任何角色重叠？如果是，必须选择合并或降级。
- [ ] **物理沙箱**：新角色的工作目录是否严格遵循 `protocol/02`（脑机分离）建立，并完成了 `workspace/` PARA 结构的初始化？
- [ ] **权限登记**：新角色是否在 `shared/config/organization.yaml` 中明确了其读写边界与隔离级别？

---

## 组织演进与激活原则

### 1. 生命周期 (Lifecycle)
[Org_Code] 的 Agent 遵循按需加载原则：
`休眠 (Dormant) → 激活 (Active) → 工作 (Working) → 归档/休眠 (Archived)`
- **休眠**：默认状态，不消耗常驻资源。
- **激活**：由指挥官或 Ark 下达明确任务后，加载灵魂三件套并挂载上下文。

### 2. 升级与迭代 (Evolution)
| 升级类型 | 触发方式 | 审批流程 |
|---------|---------|---------|
| 自我迭代 | Agent发现不足 → 写入 `.learnings` | Echo 提审 → 批准后合入 |
| 架构扩容 | 业务扩张需要新职能接入 | 提出扩容提案 → 批准后修改 config |
| 紧急授权 | 遇到任务阻塞或权限死锁 | Echo 代批应急权限 → 24h内复盘 |

---

*具体的 15 Agent 名单、角色、Emoji 及隔离级别，请读取 `shared/config/organization.yaml`*