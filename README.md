# Steven's Awesome Code Workflow

一个面向任意代码项目的 Codex 工程证据闭环。

它把需求澄清、项目规则发现、风险路由、实施授权、验证、独立审查和交付证据串成一条完整路径，同时允许每个仓库用自己的 `AGENTS.md`、工程规范和质量门禁覆盖通用默认值。

## 能解决什么

- 在修改代码前识别项目约束、工作区状态和真实入口
- 区分分析、诊断、审查、实施和外部发布的授权边界
- 独立记录风险、紧急模式、工作量和 Review Lane
- 根据 `Direct / Fast / Guarded / Audit` 选择最小充分流程
- 对 PRD、原型和模糊需求执行逐项澄清
- 将验收项映射到可重复、可证伪的验证证据
- 为高风险改动组织独立 Reviewer、回滚和可观测性证据
- 适配不同语言、框架、构建工具和仓库规范

## 核心原则

```text
目标
  → 事实取证
  → 需求澄清
  → 风险路由
  → Plan 与授权
  → 最小实现
  → 验证
  → 独立审查
  → 证据化交付
```

项目规则始终优先于本技能的通用默认值。需求描述“要实现什么”，当前代码、配置和运行证据描述“现在是什么”，仓库规范描述“必须如何工作”；三者冲突时应显式报告，而不是静默覆盖。

## 安装

使用已登录 GitHub 的 `gh` CLI 克隆私有仓库：

```bash
gh repo clone CodeDestiny/steven-awesome-code-workflow \
  ~/.codex/skills/steven-awesome-code-workflow
```

更新已有安装：

```bash
git -C ~/.codex/skills/steven-awesome-code-workflow pull --ff-only
```

## 使用

在 Codex 中显式调用：

```text
Use $steven-awesome-code-workflow to handle this repository task.
```

也可以直接描述非平凡的代码项目任务；当触发条件匹配时，Codex 会加载该技能。

## 目录结构

```text
steven-awesome-code-workflow/
├── README.md
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── evidence-and-delivery.md
    └── risk-and-review.md
```

- [`SKILL.md`](SKILL.md)：项目发现、任务路由、授权、实施、验证和交付主流程
- [`risk-and-review.md`](references/risk-and-review.md)：风险模型、Review Lane、Reviewer 职责和动态升级
- [`evidence-and-delivery.md`](references/evidence-and-delivery.md)：验证 bundle、Evidence Receipt、finding 生命周期和交付结论
- [`openai.yaml`](agents/openai.yaml)：Codex 技能列表中的展示元数据

README 仅用于仓库展示和安装说明，不参与技能运行时指令。
