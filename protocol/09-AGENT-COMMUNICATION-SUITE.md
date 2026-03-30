# Agent 通信套件协议（AGENT-COMMUNICATION-SUITE）

> **协议编号**: 12  
> **版本**: v8.1（合并 16/17 内容）  
003e **生效日期**: 2026-03-23

---

## 第一部分：异步收件箱协议

### 目的
解决 Ark（及其他 Agent）在执行正式任务时，无法稳定响应同步追问的问题。

核心原则：
> **当目标 Agent 正在执行正式任务时，新增请求默认走异步收件箱，不走同步打断。**

### 核心目录
- 请求入口：`shared/workspace/tasks/agent-inbox/`
- 回执出口：`shared/workspace/tasks/agent-replies/`

### 适用场景
**优先使用异步收件箱**：
- 目标 Agent 正在执行正式任务
- 请求不要求秒回
- 需要处理、判断、拆解或补充方案
- 不希望因 `sessions_send` 超时导致协作断裂

**仍可同步沟通**：
- 小问题
- 快判断
- 目标 Agent 当前空闲

### 请求文件命名规范
```text
AGENT-REQ-<target>-<timestamp>-<slug>.md
```

示例：
- `AGENT-REQ-ark-2026-03-22T19-45-rollout-order.md`
- `AGENT-REQ-echo-2026-03-22T19-50-design-review.md`

### 请求文件最小格式
```md
# Agent Async Request

- request_id: AGENT-REQ-ark-2026-03-22T19-45-rollout-order
- target_agent: ark
- requested_by: echo
- priority: P1
- task_id: CMD-PLATFORM-001
- created_at: 2026-03-22 19:45 GMT+8
- summary: 请从接管与分发视角补充 rollout 顺序建议
- expected_output: 简明建议 / 分发意见 / 风险提醒
- due_mode: async
```

### Agent 回执格式
目标 Agent 处理完成后，在 `shared/workspace/tasks/agent-replies/` 写回：

```md
# Agent Async Reply

- request_id: AGENT-REQ-ark-2026-03-22T19-45-rollout-order
- target_agent: ark
- replied_at: 2026-03-22 20:10 GMT+8
- status: DONE
- summary: 建议第一轮先推 Inspector / Scout / Reverb
- output_ref: workspace/00-Pending/xxx.md
- next_action: Echo 合并方案
```

若无法完成：
- `status: BLOCKED`
- 必须写明阻塞原因

### 执行纪律
1. Agent 忙时，新增请求默认先入 inbox
2. 不要求实时回复的事项，不应反复同步打断
3. 回执后，相关结论再进入任务真源或正式方案
4. inbox 是异步队列，不是长期堆积箱，应定期清空或归档

### 与任务真源的关系
- `MISSION_CONTROL.md` 记录正式任务状态
- `agent-inbox` 记录给 Agent 的异步请求
- `agent-replies` 记录 Agent 的异步回执

三者分工不同，不可混用。

---

## 第二部分：体系级反馈默认投递群规则

### 规则正文
凡属 **[Org_Code] 体系级反馈**，除非指挥官明确要求与某节点单线对接，否则默认投递到 [Org_Code] 飞书群：

- **Feishu Group ID**: `oc_b21f1c5425b3ee081ba5cb0f375b8e3e`

### 适用范围
以下类型的反馈，默认进入该群：

1. **Heartbeat 异常反馈**
   - 心跳发现的关键异常
   - 漏记风险
   - 双核链路异常

2. **自动任务结果反馈**
   - cron / 自动化任务关键结果
   - 失败告警
   - 关键完成播报

3. **Agent 协作反馈**
   - 重要跨节点协作结果
   - 异步收件箱处理完成的关键回执
   - 需要全局知悉的状态更新

4. **治理 / 审计 / Reviewer 反馈**
   - 重大升级 PASS / FAIL
   - 协议违约警报
   - 需要全军知悉的结构性问题

### 例外情况
以下情况不默认发群：
1. **指挥官明确单线对接**
2. **敏感/私密内容**
3. **低层调试信息**

### 使用纪律
1. **质量优先**：默认发群不代表事无巨细都发，仍需判断"是否属于体系级"
2. **脱敏原则**：即使发群，敏感细节仍需脱敏或转至单线
3. **不回执原则**：群消息默认不需要每个人回执，避免刷屏
4. **归档原则**：重要群内反馈应在当日 L2 journal 留痕

---

## 第三部分：中枢双核链路健康检查机制

### 背景
当前 [Org_Code] 已形成明显的中枢双核：
- **Echo**：主架构 / 治理 / 记忆 / 审计中枢
- **Ark**：任务分发 / 作战协调 / 任务板运转中枢

若双核链路异常，将直接影响任务分发、方案统筹、治理协议落实。

### 检查目标
1. **寻址正确性**：通信目标是否仍正确映射
2. **可达性**：是否能成功发起消息、获得正常回执
3. **分工稳定性**：是否存在错路由或漂浮协作

### 最小检查项

#### Echo 链路检查
- Echo → main 映射是否正常
- Echo 是否能收到跨节点异步备案 / 请求
- Echo 是否能返回正常回执

#### Ark 链路检查
- Ark 的目标寻址是否正常
- Ark 是否能接收异步收件箱请求
- Ark 是否能正常回执 / 更新任务板相关状态

#### 双核协同检查
- Echo 与 Ark 对同一任务的任务真源认知是否一致
- 双核之间的关键交接是否有记录可追溯

### 检查方式

#### 方式 1：轻量人工巡检（当前推荐）
- 按周期或关键节点进行
- 先查 `COMMUNICATION_SSOT.md`
- 再做一次轻量验证与记录

#### 方式 2：Heartbeat 辅助巡检（后续）
- heartbeat 只做"链路异常发现"
- 不直接做复杂交互

#### 方式 3：升级 Reviewer 复核（后续）
- 当双核节点升级后，质检其链路与寻址是否仍兼容

### 异常分级

| 级别 | 状态 | 含义 |
|------|------|------|
| 🟢 绿色 | 健康 | 映射正确、消息可达、回执正常 |
| 🟡 黄色 | 警告 | 可达但有延迟/依赖异步、需优化但未失效 |
| 🔴 红色 | 严重 | 映射错误、无法到达、无回执/超时/错路由 |

### 记录纪律
每次双核链路健康检查后，至少在当日 `memory/journal/YYYY-MM-DD.md` 留下一笔记录，说明：
- 检查时间
- 检查对象
- 结果（绿/黄/红）
- 异常详情
- 后续动作

### 与其他章节的关系
- **红色异常**应通过第二部分"体系级反馈默认投递群"播报
- 检查时必须以 `COMMUNICATION_SSOT.md` 为真源

---

## 第四部分：与其他协议的关系

| 协议 | 关系 |
|------|------|
| Protocol 07（MISSION_CONTROL）| 异步请求与任务板协同 |
| INFRASTRUCTURE.md | 群 ID 与通信映射登记 |
| COMMUNICATION_SSOT.md | 链路检查的真源依据 |

---

## 第五部分：一句话归档

Agent 通信套件统一规范：忙时走异步收件箱（inbox/replies）、体系级反馈默认投群、双核链路定期检查，确保 [Org_Code] 通信既不丢消息也不制造噪音。
