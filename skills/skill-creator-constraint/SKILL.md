---
name: skill-creator-constraint
description: skill-creator 的前置约束。任何会触发 skill-creator 的请求（创建/修改/优化/评估 skill）都必须先经过本 skill，确认以下两条硬约束后再进入 skill-creator。
---

# skill-creator-constraint

调用 `Skill(skill-creator)` 之前，先确认两件事：

## 1. 源放在 `<repo_root>/skills/<name>/`

- 源文件只放这里。`repo_root` 用 `git rev-parse --show-toplevel`。
- 用户明确要求放别处时，用 `AskUserQuestion` 确认。
- 修改已有 skill 也只改源，不改 symlink 副本。
- 让 skill 生效：`ln -s ../../skills/<name> .claude/skills/<name>` 和 `.agents/skills/<name>`（相对路径）。

## 2. 目录结构遵循 Agent Skills 规范

skill 是一个**目录**，不是单个文件。布局：

```
<name>/                # 目录名 == skill name，kebab-case
├── SKILL.md           # 必需，入口文件，只放精简的"做什么 + 怎么做"
├── scripts/           # 可选，可执行脚本
├── references/        # 可选，长文/规范/示例，按需引用
└── assets/            # 可选，模板、图片等静态资源
```

要点：

- 入口文件名必须是 `SKILL.md`（区分大小写）。
- `SKILL.md` 保持精简；细节、长说明、模板放进 `references/` 或 `assets/`，在正文里按需引用（相对路径）。
- 子目录命名固定用上述三个，不要自创 `docs/`、`lib/` 等替代名。

> 规范来源：agentskills.io（已内联，无需联网）。
