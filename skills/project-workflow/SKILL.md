---
name: project-workflow
description: Universal 5-phase project workflow with intelligent triage, first-principles planning, reflection system, and parallel execution. Combines Superpowers + ECC + Planning-with-Files.
user-invocable: true
---

# Project Workflow — Universal 5-Phase Development Process

You are executing a structured development workflow. Follow each phase exactly. Do not skip phases unless TRIAGE explicitly allows it.

## How to Use

Invoke with: `/project-workflow <task description>`
The task description is: $ARGUMENTS

If no arguments provided, ask the user: "What are we building or fixing?"

---

## Phase -1: TRIAGE

Before doing ANYTHING, evaluate the task on 3 dimensions. Output your assessment explicitly.

### Evaluate:

**1. Scale:**
- S (1-2 files, single module)
- M (3-10 files, multiple related files)
- L (10+ files, cross-module, new subsystem)

**2. Type:**
- bug_fix | feature | refactor | research | config

**3. Risk — does the task touch any of these?**
- Authentication, authorization, permissions
- API keys, secrets, credentials
- User input handling, command execution
- Code generation (skill_factory, eval, exec)
- External API calls, network requests

If YES to any → mark `security-required`

### Route to Path:

```
S + (bug_fix | config)           → Path S (lightweight)
M or (feature | refactor)        → Path M (standard)  
L or research or unknown-tech    → Path L (full)
```

### Output format:

> **TRIAGE**: Scale=[S/M/L], Type=[type], Security=[yes/no] → **Path [S/M/L]**
> Activating: [list of skills that will be used]

### Always-On (all paths):
- Ruff hook (automatic via PostToolUse)
- python-review (any code change)
- verification-before-completion (any task completion)
- security-review (when security-required)
- docs / Context7 (when external library involved)

---

## Phase 0: DISCOVER

> **Skip if Path S.**

### Path M: Brainstorm only

Invoke: `superpowers:brainstorming`
- Explore requirements, constraints, success criteria
- Keep it focused, 2-3 questions max

After brainstorm produces a design:

> **深度思考并优化：**
> - 需求理解对不对？遗漏了什么？
> - 有没有更优解？
> - 这个方案是最简的吗？

→ Get user approval on design.

### Path L: Brainstorm + Research in parallel

Launch in parallel:
1. `superpowers:brainstorming` — requirements exploration
2. `everything-claude-code:deep-research` — investigate existing solutions, papers, community practices
3. `everything-claude-code:docs` / Context7 — look up library APIs if external deps involved

> **思考并优化：** research 结果够全面吗？有没有遗漏的关键来源？

Synthesize findings into design.

> **深度思考并优化：**
> - 需求理解对不对？遗漏了什么？
> - 有没有更优解？
> - research 发现了什么可以直接复用的？

→ Get user approval on design.

---

## Phase 1: PLAN (First Principles + Plan + Feasibility)

> **Skip if Path S.**

### Step 1: First Principles Decomposition

Before writing any plan, answer these questions explicitly:

> **第一性原则分解：**
> 1. 这个问题的本质约束是什么？（物理限制、API 限制、硬件限制）
> 2. 不可绕过的技术限制是什么？
> 3. 从零开始，如果没有任何现有代码，最优解应该长什么样？
> 4. 现有代码中哪些部分已经接近最优，哪些需要改？

> **思考并优化：**
> - 上面的分解是否触及了真正的约束，还是在重复表面假设？

### Step 2: Write the Plan

For Path M: Invoke `superpowers:writing-plans`
For Path L: Invoke `superpowers:writing-plans` AND `planning-with-files:plan` (complementary)

- writing-plans → implementation steps with exact file paths and code
- planning-with-files → task_plan.md / findings.md / progress.md for tracking

Also invoke `everything-claude-code:search-first` to check if existing libraries/tools solve part of the problem before writing custom code.

> **深度思考并优化：**
> - 方案是从第一性原则推导的，还是在抄现有方案？
> - 步骤分解合理吗？依赖关系对吗？
> - 有没有过度设计或遗漏？

### Step 3: Feasibility Validation

For each critical step in the plan, verify:

> **可行性验证：**
> 1. 技术上能实现吗？有没有 API 限制、硬件约束？
> 2. 有没有隐含的依赖没列出来？
> 3. 最大风险点在哪？如果失败怎么回退？
> 4. 有没有因为惯性思维多加了不必要的步骤？

> **深度思考并优化：**
> - 这个计划真的可行吗？
> - 哪一步最可能出问题？

→ Get user approval on plan.

---

## Phase 2: BUILD

### For each task in the plan:

**1. TDD: Write tests first**
Invoke: `everything-claude-code:tdd` or `everything-claude-code:python-testing`

> **思考并优化：** 测试覆盖够吗？边界情况考虑了吗？

**2. Implement → GREEN**
Write minimal code to pass the tests.
[Ruff hook auto-formats on save — automatic]

> **思考并优化：** 有更简洁的写法吗？

**3. Review**
Invoke: `everything-claude-code:python-review` (agent)

> **思考并优化：** review 发现的问题都修了吗？

**4. Security (if security-required)**
Invoke: `everything-claude-code:security-review` (agent)

> **思考并优化：** 安全问题有没有漏掉的？

**5. Test gate**
Run: `python -m pytest tests/ -q`
PASS → mark task done, update progress.md
FAIL → fix and re-run

### Parallel execution (Path L only):
If plan has independent tasks, use `superpowers:subagent-driven-development` to dispatch parallel worktree agents, each executing the above flow independently.

---

## Phase 3: VERIFY

Run these in parallel:

1. **Full test suite**: `python -m pytest tests/ -v`
2. **Code review**: `superpowers:requesting-code-review` — review against the original plan
3. **Security scan** (if security-required): `everything-claude-code:security-scan`

> **思考并优化：** review 和 scan 发现的问题都修完了吗？

Then invoke: `superpowers:verification-before-completion`
- MUST run actual commands and show real output
- NO claiming "done" without evidence

> **深度思考并优化：**
> - 整体一致性如何？
> - 有没有遗留问题？
> - 改动的副作用想清楚了吗？

For long sessions, run `everything-claude-code:context-budget` to check context health.
Update progress.md with verification results.

---

## Phase 4: SHIP

**1. Commit**
Invoke: `superpowers:finishing-a-development-branch`
- Commit (DO NOT auto-push, wait for user to request)
- Or create PR if on feature branch

> **思考并优化：** commit message 准确反映了改动吗？

**2. Update tracking**
Update progress.md to mark completion.

**3. Learn (Path L only)**
Invoke: `everything-claude-code:learn-eval`
- Extract reusable patterns from this session

> **深度思考并优化：**
> - 提取的模式真的通用吗？
> - 这次改动的长期影响？
> - 下次做类似任务，哪里可以更快？

---

## Reflection System Summary

| Level | When | Focus |
|-------|------|-------|
| **深度思考并优化** (6x) | Design spec, plan review, feasibility, final verify, ship, learn | Direction, optimality, long-term impact |
| **思考并优化** (8x) | Research, first-principles, tests, implementation, reviews, security, verify-aggregate, commit | Completeness, correctness of current step |

---

## Quick Reference: Skill Sources

| Skill | From |
|-------|------|
| brainstorming, writing-plans, executing-plans, subagent-driven-development, requesting-code-review, verification-before-completion, finishing-a-development-branch | Superpowers |
| tdd, python-testing, python-review, security-review, security-scan, deep-research, search-first, docs, learn-eval, context-budget, aside | ECC |
| plan (task_plan.md/findings.md/progress.md) | Planning-with-Files |
| Context7 (resolve-library-id, query-docs) | MCP |
| Ruff auto-format | Hook (PostToolUse) |
