# 战术面板与任务全生命周期协议（MISSION_CONTROL）

> **协议编号**: 07  
> **版本**: v8.2 (Config-driven & Rubric-gated)  
> **类型**: 核心法源

---

## 质量门控 (Validation Rubric)

> **Contract**: 任何导致 `MISSION_CONTROL.md` 状态变更或任务流转的操作，必须通过以下强制校验：

- [ ] **立项合法性**：新增任务是否具有明确的负责人（Owner）、优先级（Priority）和可验收的下一步（Next Action）？
- [ ] **闭环完整性**：标记为“完成（Completed）”的任务，是否已按要求生成并存储了 `Completion Report`？
- [ ] **阻塞显式化**：标记为“阻塞（Blocked）”的任务，是否清晰记录了 `blocker_owner` 和明确的解锁条件？
- [ ] **状态同步**：任务状态的变更是否同步更新到了对应的 `business-os/tasks/` 任务卡真源中？

---

## 核心定位与基础定义

`MISSION_CONTROL.md` 是**跨节点协同的实时任务看板**。

> **核心铁律：没进任务板，不算正式任务。**

聊天提到、口头答应、文件起草，都不等于正式任务。只有登记进入 `shared/MISSION_CONTROL.md`，才算被组织承认。

---

## 角色边界与派发协议

### Echo 职责边界
- **负责**：战略设计、协议、结构、审批、架构收敛
- **不负责**：直接向执行节点派发正式任务

### Ark 职责边界
- **负责**：任务拆解、正式派发、ACK 跟踪、阻塞升级、重派与接管
- **定位**：全军任务分发控制面的唯一入口

### 执行节点职责
- **负责**：执行任务、汇报进度、生成 Completion Report、遇阻上报

---

## 任务全生命周期流程

### 1. 正式派发协议（必须经过 Ark）
执行节点收到任务后必须在 **15 分钟 ACK 超时窗口**内回复（ACK / BLOCKED）。未 ACK 的任务不算分发成功。

### 2. 阻塞处理与升级路径
- **L0 - Ark 自行处理**：卡住 ≥ 30 分钟。
- **L1 - Ark → Echo**：需要设计决策，卡住 ≥ 2 小时。
- **L2 - Ark → 指挥官**：需要优先级重排，卡住 ≥ 24 小时。

### 3. 任务完成与闭环 (Completion Report)
执行节点完成任务后，必须生成包含任务状态、断点标识和下一步动作的报告，存入 `shared/logs/completion-reports/`。
**没有 Completion Report 的任务，不算真正闭环完成。**

---

## Business OS 连续性增强规范

> 核心目标：依靠任务板系统和任务卡，不需要人类重新复述，即可在断线后持续推进主线。

### 1. 文件结构与真源关系
**真源优先级**（从高到低）：
`business-os/tasks/*.md` (任务卡) > `CONTINUITY_SNAPSHOT.md` > `MISSION_CONTROL.md`

### 2. Agent 触发时机
- **Session 启动时（必读）**：读取 `CONTINUITY_SNAPSHOT.md`，定位任务卡。
- **任务有实质进展时（必挂）**：立即更新任务卡的 `next_action` 和 `latest_assets`。
- **发现阻塞时（必报）**：更新 `blocker_summary`，状态设为 `stale`。

### 3. 断线恢复标准流程
收到恢复指令或发现上下文丢失时：
1. 读取 `CONTINUITY_SNAPSHOT.md`。
2. 定位对应的 Task Card。
3. 确认当前主线、已定决策、本轮推进范围。
4. 按 `next_action` 继续推进。

---
*附录：具体任务状态枚举与优先级定义已移至全局配置域，具体执行看板见 `shared/MISSION_CONTROL.md`*
