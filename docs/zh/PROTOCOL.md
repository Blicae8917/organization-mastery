# [Org_Code] 执行协议总纲

> **版本**: v8.0  
> **配套**: CONSTITUTION.md / MISSION_CONTROL.md  

---

## D.C.E. 协议速查

| 阶段 | 关键动作 | 确认词 |
|------|---------|--------|
| **D - Discuss** | 2-3方案 + 5W2H挖掘 | - |
| **C - Confirm** | 等待明确批准 | "执行"/"确认"/"Proceed" |
| **E - Execute** | 策略选择 + Pre-flight + 执行 | - |

**信息挖掘框架**: Why / What / Who / When / Where / How / How much / 极限场景

---

## 协议索引

| 编号 | 协议 | 核心内容 | 何时读取 |
|------|------|---------|---------|
| 01 | ORGANIZATION | 15 Agent编制 (将抽离配置) | 初始化/扩容时 |
| 02 | BRAIN-COMPUTER-SEPARATION | 脑机分离V4 | 目录结构相关 |
| 03 | MEMORY-GOVERNANCE | 记忆治理 (含Taxonomy Tags) | 经验管理相关 |
| 04 | EXECUTION-EVOLUTION | DCE 2.0 / 进化策略 | 执行任务时 |
| 05 | MISSION-CONTROL | 任务全生命周期协议 | 跨节点协调时 |
| 06 | TOKEN-AUTOMATION | Token节约 / 自动化 | 设计自动化时 |
| 07 | SECURITY-REDLINES | 安全红线 / 隔离 | 涉及敏感操作时 |
| 08 | SKILL-GOVERNANCE | Skill治理规范 | 创建/审查Skill时 |
| 09 | AGENT-COMMUNICATION-SUITE | Agent通信套件 | 跨节点通信时 |
| 10 | PROJECT-DOCUMENT-SEMANTICS | 项目文档语义分层 | 文件治理时 |

---

## 质量门控层 (Rubrics & Contracts)

> **强制纪律**：根据 [Org_Code] Harness “第7项实用性标尺”，长任务必须附带验收条件。
> 所有执行后的协议落地，需执行以下总控 Check：

- [ ] **执行门控**：本操作是否符合 `D.C.E. 2.0` 的 Confirm 前置授权？
- [ ] **配置调取**：引用的环境变量、组织参数是否从 `shared/config/` 获取，而非在代码或 Prompt 中硬编码？
- [ ] **留痕收尾**：执行完后是否在 `memory/journal/` 或对应任务卡中完成了符合 Taxonomy 标签格式的强制留痕？

---

## 速查表 (待抽离 Config)

### 标签速查
```
#创作素材  → 自媒体内容
#技术踩坑  → 避免重复踩坑
#决策节点  → 理解决策原因
#SOP沉淀   → 固化为Skill
#价值观升级 → 宪章修订轨迹
#编年史素材 → 汇入史册
#指挥官偏好 → 同步USER.md
#红线警报  → 安全审计
```

### 进化策略速查
```
harden      → 配置修改、安全相关
innovate    → 内容创作、新功能
repair-only → 紧急修复、故障恢复
balanced    → 常规任务（默认）
```

### 路径前缀速查
```
✅ workspace/01-Projects/...    # 正确
❌ 01-Projects/...              # 错误，污染根目录
```

### 记忆层级速查
```
L0   → MEMORY.md (≤4KB, 常驻)
L0.5 → .learnings/ (临时经验)
L1   → memory/milestones/ (阶段性)
L2   → memory/journal/ (完整日志)
L3   → shared/knowledge/lessons/ (全军共享)
```

### 通信与基础设施速查
```
资源登记 → INFRASTRUCTURE.md (知识库/Git仓库)
详细指南 → INFRASTRUCTURE-GUIDE.md (故障排查/使用手册)
跨节点通信前 → 必须查询 COMMUNICATION_SSOT.md 确认实际路由(例如 Echo → main)
工具/能力相关 → 统一先看 shared/tools/README.md
  ├─ internal/  → 内部工具怎么用
  └─ external/  → 外部能力有没有、状态如何
```

### Self-Improvement触发器
```
纠正: "不对"/"错了"/"应该是"
错误: 非零退出码 / 异常堆栈
缺口: 未知信息 / 过时文档
优化: 发现更好方案
```

---

## 一事一议边界

**必须等待人类决策者**:
- 首次任务类型
- 配置修改 / 外部通信 / 数据删除
- 资源消耗大的操作

**回答前路由纪律（强制）**:
- 属于“位置 / 路由 / 权限 / 基础设施 / 当前任务状态”问题时，禁止凭记忆、目录扫描或印象直接回答
- 必须先查对应 SSOT（INFRASTRUCTURE / COMMUNICATION_SSOT / MISSION_CONTROL / MEMORY）再作答
- 如 SSOT 未命中，必须明确说明“已查 SSOT，但未找到”，再进入搜索或提问

**{{org.members.guardian_name}}可代批**:
- 已验证SOP（2次以上）
- 紧急修复（24h内补报）
- 纯内部文件读写

---

## 快速开始

```bash
# 1. 配置
cp config/organization.template.yaml config/organization.yaml

# 2. 初始化
./scripts/init-workspace.sh

# 3. 创建Agent
./scripts/create-agent.sh --name "MyAgent"

# 4. 启动
# 按照CONSTITUTION.md第二章指引激活Agent
```

---

_执行协议与宪章配套使用_  
_详情见 protocol/ 目录_��见 protocol/ 目录_