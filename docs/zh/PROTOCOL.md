# 8917 执行协议总纲

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
| 01 | TAXONOMY_TAGS | 8核心标签体系 | 写入记忆时 |
| 02 | COMMUNICATION_SHORTCODES | 通信简码 | 跨节点通信时 |
| 03 | ORGANIZATION | 15 Agent编制 | 初始化/扩容时 |
| 04 | BRAIN-COMPUTER-SEPARATION | 脑机分离V4 | 目录结构相关 |
| 05 | MEMORY-GOVERNANCE | 记忆治理 | 经验管理相关 |
| 06 | EXECUTION-EVOLUTION | DCE 2.0 / 进化策略 | 执行任务时 |
| 07 | MISSION-CONTROL | 战术面板协议 | 跨节点协调时 |
| 08 | TOKEN-AUTOMATION | Token节约 / 自动化 | 设计自动化时 |
| 09 | SECURITY-REDLINES | 安全红线 / 隔离 | 涉及敏感操作时 |
| 10 | SKILL-GOVERNANCE | Skill治理规范 | 创建/审查Skill时 |
| 11 | OPEN-SOURCE | 开源传承 | 对外发布时 |

---

## 速查表

### 标签速查
```
#创作素材  → 自媒体内容
#技术踩坑  → 避免重复踩坑
#决策节点  → 理解决策原因
#SOP沉淀   → 固化为Skill
#价值观升级 → 宪章修订轨迹
#编年史素材 → 汇入史册
#人类决策者偏好 → 同步USER.md
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
跨节点通信前 → 必须查询 COMMUNICATION_SSOT.md 确认实际路由
```

---

## 法源优先级

当本地文件与共享文件冲突时：

```
shared/CONSTITUTION.md > shared/PROTOCOL.md > 本地 AGENTS.md
```

**无条件以共享文件为最高准则。**

---

## 启动序列

所有 Agent 每次 Session 启动时必须：

1. 读取 `../shared/CONSTITUTION.md`
2. 读取 `../shared/PROTOCOL.md`
3. 读取 `../shared/MISSION_CONTROL.md`
4. 读取 `../shared/INFRASTRUCTURE.md`
5. 读取 `SOUL.md` / `MEMORY.md`

---

_版本：v8.0_  
_更新日期：2026-03-21_
