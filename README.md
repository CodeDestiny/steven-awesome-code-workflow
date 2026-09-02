# Steven's Awesome Code Workflow

一个面向任意代码项目的轻量 Codex 工作流：理解目标、检查仓库、最小实现、针对性验证、简短交付。

它适用于分析、诊断、实现、重构和 Review。目标仓库的 `AGENTS.md` 及其他现役规则始终优先；技能只提供跨项目的默认做法。

## 特点

- 先确认任务是只读分析、代码变更还是持续观察
- 当前下达任务的用户拥有项目内决策权，无需额外身份或授权证明
- 只在真实歧义会改变范围、行为或验收时提问
- 优先最小实现，不加入推测性抽象和防御机制
- 选择最贴近改动的验证方式，不以测试数量为目标
- 高影响任务按实际后果补充证据，Reviewer 按需启用
- commit、push、merge、部署和生产写入等动作仍需明确点名

## 安装

使用已登录 GitHub 的 `gh` CLI：

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

当代码项目任务与技能描述匹配时，也可以自动加载。

## 目录

```text
steven-awesome-code-workflow/
├── README.md
├── SKILL.md
└── agents/
    └── openai.yaml
```

`README.md` 仅用于仓库介绍和安装说明，运行时规则全部位于 `SKILL.md`。
