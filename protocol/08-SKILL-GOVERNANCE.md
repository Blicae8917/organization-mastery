# [Org_Code] Skill Governance

> 适用范围：`shared/skills/native/` 下所有 native skills，以及准备纳入 native 的新 skill / 改造 skill。
> 全军通用沟通暗号另见：`shared/protocol/02-COMMUNICATION_SHORTCODES.md`

---

## 一、定位

`shared/skills/native/` 只允许存在三类东西：
1. [Org_Code] 独有能力
2. 已主权化的外部能力
3. 高价值编排器 / 基础设施级 skill

native 不是垃圾场，也不是 external 镜像区。

### external / native 总原则

- 外部 skill 默认保留在 `shared/skills/external/`，并尽量随源更新。
- [Org_Code] 优先通过 wrapper 层增加规则、约束和使用边界，而不是直接 fork 外部 skill 进 native。
- 只有当外部 skill 经过深度改造，形成明确的 [Org_Code] 主权价值后，才允许进入 `shared/skills/native/`。
- 严禁把仅做轻度包装或简单改名的 external skill 伪装成 native skill。

---

## 二、双层治理框架

每个 native skill 都必须回答两个问题：

1. **它属于哪种类型？**（治理层）
2. **它采用哪种设计模式？**（内容层）

### A. 类型层（治理层）

#### 1. 规范型 Skill
回答“怎么做才对”。
- 负责规则、标准、方法论、判断框架
- 不应塞入过多执行细节

#### 2. 工具型 Skill
回答“具体怎么干”。
- 负责聚焦问题的实际执行
- 往往带 scripts / 输入输出约束

#### 3. 编排型 Skill
回答“多个能力怎么串起来形成流程”。
- 负责顺序、checkpoint、恢复机制
- 不应吞并所有步骤细节

### B. 模式层（设计层）

#### 1. Tool Wrapper
给 agent 包一层领域知识或工具使用规则。

#### 2. Generator
基于模板 / 结构约束生成稳定产物。

#### 3. Reviewer
基于 checklist / rubric 做结构化审查。

#### 4. Inversion
先采访用户，收集需求，再开始生成/执行。

#### 5. Pipeline
严格多步骤流程，带 checkpoint，不允许跳步。

**注意**：模式不是互斥的，但每个 skill 必须明确主模式；必要时可标注“主模式 + 次模式”。

---

## 三、命名规范

### 1. 主权技能默认使用 `[Org_Code]-` 前缀
凡是 [Org_Code] 主权 skill，优先使用：
- `[Org_Code]-xxx`

### 2. 裸名 skill 尽量清退
如 `docx-official`、`minimax-toolkit` 这类裸名 skill，原则上应：
- 改造成 `[Org_Code]-xxx`
- 或在治理文档中明确来源与保留理由

### 3. 名称必须体现职责
禁止使用模糊命名：
- `[Org_Code]-helper`
- `[Org_Code]-tools`
- `[Org_Code]-core`

---

## 四、frontmatter 规范

### 必填字段
```yaml
name:
description:
```

### 允许字段
```yaml
metadata:
```

### 默认不建议字段
除非确有必要，native skill 默认不建议加入：
- `license`
- `version`
- `author`
- `compatibility`

---

## 五、目录结构规范

每个 native skill 目录只允许：

```text
skill-name/
├── SKILL.md
├── scripts/
├── references/
└── assets/
```

### 禁止项
默认禁止堆放：
- README.md
- CHANGELOG.md
- 临时说明
- 备份副本
- 无关测试文件

---

## 六、SKILL.md 编写标准

### 1. description 负责触发，不是简介
必须明确：
- 做什么
- 什么场景触发
- 用户会怎么说

### 2. SKILL.md 只保留核心流程
保留：
- 目标
- 判断路径
- 执行步骤
- 何时读 references
- 何时调 scripts

长模板、FAQ、详表应下沉到 `references/`。

### 3. 不要复制平台通识
native skill 应只保留：
- [Org_Code] 独有约束
- [Org_Code] 独有路径
- [Org_Code] 独有流程差异

---

## 七、进入 native 的门槛

任何 skill 想进入 `shared/skills/native/`，必须回答：

1. 它属于哪一类（规范型 / 工具型 / 编排型）？
2. 它的主设计模式是什么（Tool Wrapper / Generator / Reviewer / Inversion / Pipeline）？
3. 它和现有 native skill 是否重复？
4. 它的主权价值是什么？为什么不是 external？
5. 是否已经验证“保留 external + 增加 wrapper”不足以满足需求？

答不清，就不进 native。

---

## 八、合并 / 拆分 / 瘦身规则

### 什么时候该合并
- 功能重复
- 触发语义重叠
- 只是轻重两版同类 skill
- 标准表述互相打架

### 什么时候该拆分
- 同时承担抓取、分析、归档三种职责
- 同时承担规范、工具、编排三种内容
- 一改动牵一大片
- SKILL.md 越来越长，没人敢改

### 什么时候该瘦身
- 大段复制官方 skill
- 编排器吞掉所有细节
- 工具型 skill 写成哲学论文

---

## 九、skill 引入与收编流程

1. 发现（新需求 / external / 已有替代）
2. 审查（`[Org_Code]-skill-vetting`）
3. 判断归属，优先按以下顺序决策：
   - 保留在 `external/`
   - 保留 `external/`，再增加 [Org_Code] wrapper
   - 仅在 wrapper 不足且已形成明确主权价值时，才进入 `native/`
   - 若无价值或有风险，则拒绝引入
4. 若确定进入 native，再执行主权化改造（`[Org_Code]-skill-creator`）
5. 入库（`shared/skills/native/`）
6. 登记（台账 + 里程碑 / MEMORY 索引）

---

## 十、治理铁律

- native 只收主权资产，不收功能幻觉。
- 重复的合并，过胖的拆分，照搬的瘦身，空壳的不入 native。
- 新建或改造 native skill 时，必须优先经过 `[Org_Code]-skill-vetting` 与 `[Org_Code]-skill-creator`。
- 所有结构性调整完成后，必须更新台账；必要时写入 milestone。
