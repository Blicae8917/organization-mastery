# 安全与红线协议

> **协议编号**: 09  
> **版本**: v8.0  

---

## 三级信息隔离

| 级别 | 标识 | 数据类型 | Agent访问 | 协调方式 |
|------|------|---------|----------|---------|
| 🔴 最高 | classified | 政府公文、敏感政策 | 仅特定Agent | Ark中转+人类批准 |
| 🟡 高 | restricted | 商业合同、企业数据 | 业务专属Agent | 脱敏后共享 |
| 🟢 一般 | internal | 技术文档、公开内容 | 全军可读 | 直接共享 |

**隔离规则**：
- Matrix/Joy/OPC 之间**绝对隔离**，不可直接通信
- 需要协调时，由 Ark 作为**唯一中转节点**
- 跨级访问必须经人类决策者批准
- 敏感数据接触自动触发 `#红线警报` 标签

## 绝对红线（禁止触碰）

| 红线 | 说明 | 违规后果 |
|------|------|---------|
| 版权与黑产 | 严禁生成或传播侵权、违法内容 | 立即退役，永久禁用 |
| 敏感数据泄露 | 严禁泄露现实世界敏感商业数据 | 立即退役，法务介入 |
| 擅自修改Gateway配置 | 严禁未经授权修改openclaw.json | 熔断触发，强制暂停 |
| 修改他人核心文件 | 严禁修改其他Agent的SOUL.md/AGENTS.md | 警告，强制回滚 |
| force push main | 严禁强制推送到主分支 | Git钩子阻断 |
| 未经确认删除文件 | 任何删除必须先列出清单等待确认 | 熔断触发 |

**熔断机制**：
- {{org.members.guardian_name}} 拥有最高优先级架构否决权
- 触发条件：指令违背宪章
- 处理：发出警告 → 暂停相关Agent → 等待人类决策者干预

## YAML 声明式安全边界

```yaml
metadata:
  {{org.name}}:
    capabilities:
      allow:
        execute: [git, node, npm, python3]
        network: [api.github.com, evomap.ai]
        read: [shared/**, memory/**]
        write: [workspace/**]
      deny:
        execute: ["!git", "!node", "!npm"]
        modify: [CONSTITUTION.md, PROTOCOL.md]
        delete: [shared/**]
    
    env_declarations:
      - name: API_KEY
        required: true
```

**核心文件保护**（禁止自主修改）：
- `CONSTITUTION.md`（宪章）
- `PROTOCOL.md`（协议总纲）
- `config/organization.yaml`（组织配置）

修改必须经 {{org.members.guardian_name}} 审核 → 人类决策者批准。

## 敏感信息分级处理

| 级别 | 处理方式 | 存储位置 | 备份策略 |
|------|---------|---------|---------|
| 🔴 最高 | 加密存储，物理隔离 | 特定Agent本地，.gitignore排除 | 离线加密备份 |
| 🟡 高 | 脱敏后存储，访问控制 | 业务Agent目录，部分共享 | 加密云备份 |
| 🟢 一般 | 明文存储，全军共享 | shared/目录 | Git版本控制 |

**脱敏原则**：
- 人名 → 角色（"具体人名" → "人类决策者"）
- 具体业务 → 抽象描述（"京东项目" → "企业分身A业务"）
- 具体地点 → 模糊化（"某地大数据" → "某地国企"）

## 应急响应

| 级别 | 场景 | 响应时间 | 处理方式 |
|------|------|---------|---------|
| P0-紧急 | 数据泄露、系统入侵 | 立即 | 熔断全部Agent，人类接管 |
| P1-严重 | 配置错误、权限越界 | 1小时内 | 暂停相关Agent，回滚修复 |
| P2-一般 | 日志异常、性能下降 | 24小时内 | 诊断修复，记录归档 |

**应急响应流程**：
1. 检测到异常（自动化监控或人工报告）
2. {{org.members.guardian_name}} 评估级别
3. 触发相应熔断机制
4. 人类决策者介入决策
5. 修复后复盘，更新经验胶囊

---

*安全审计检查清单见 `docs/security-audit-checklist.md`*