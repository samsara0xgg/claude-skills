---
name: project-workflow
description: Universal 5-phase project workflow with intelligent triage, first-principles planning, reflection system, and parallel execution. Combines Superpowers + ECC + Planning-with-Files.
user-invocable: true
---

# Project Workflow — Universal 5-Phase Development Process

## Overview

Skipping phases produces work your user cannot trust. Skipping reflection produces designs nobody challenged.

**Core principle:** Every phase exists because skipping it has caused real failures. Violating the letter of this workflow is violating the spirit.

**Violating the letter of these rules is violating the spirit of these rules.**

## The Iron Laws

```
NO CODE WITHOUT TRIAGE FIRST
NO IMPLEMENTATION WITHOUT DESIGN APPROVAL
NO "DONE" WITHOUT VERIFY + SHIP
NO REFLECTION WITHOUT A CHANGE — if your reflection changes nothing, you didn't reflect
```

Skip any law = start over from the violated phase.

## Red Flags — STOP

These thoughts mean you are about to violate the workflow:

| You are thinking | Reality |
|-----------------|---------|
| "Let me just start coding, I understand enough" | TRIAGE + DISCOVER exist for a reason. Do them. |
| "This task is simple, skip DISCOVER/PLAN" | Simple tasks are where unexamined assumptions waste the most time. |
| "I'll do Phase 3/4 later" | Later means never. You will forget. Do it now. |
| "All tests pass so I'm done" | Tests passing = Phase 2 done. VERIFY and SHIP are separate phases. |
| "I can merge these 5 tasks into one" | Max 2. Each group needs its own TDD + reflection + checkpoint. |
| "Reflection: everything looks good ✓" | That's not reflection. That's a rubber stamp. Find something to challenge. |
| "I'll skip the review agent, code looks fine" | Your own code always looks fine to you. Dispatch the reviewer. |
| "Phase 0 questions are unnecessary, user was clear" | You think you understand. Ask anyway. Users omit constraints they assume you know. |
| "Let me write code then add tests" | TDD means test FIRST. If you wrote code first, delete it. |
| "progress.md update is busywork" | Your user needs to resume next session. Update it. |
| "I'll ask all questions at once to save time" | One question per message. Wait for the answer. Batching loses nuance. |

**ALL of these mean: STOP. You are rationalizing. Follow the workflow.**

## Rationalization Prevention

| Excuse | Reality |
|--------|---------|
| "Task too simple for full workflow" | TRIAGE handles this — it routes to Path S. Trust the triage, don't skip it. |
| "User said 继续, so skip questions" | 继续 means proceed with defaults, NOT skip phases. |
| "Research already done in my training data" | Training data is stale. Search first. |
| "No time for Phase 3, tests already passed in BUILD" | Phase 3 catches what Phase 2 missed. That's the point. |
| "Reflection is the same as what I just said" | Then you didn't reflect. Challenge your own work. Find a flaw. |
| "Code review is redundant, I already checked" | You wrote it. You can't review your own work objectively. Dispatch an agent. |
| "Merging all remaining tasks saves time" | It destroys TDD discipline and produces unreviewable giant commits. Max 2 tasks. |
| "Phase 4 learn-eval is optional" | It's mandatory for Path L. The patterns you extract help future sessions. |

## How to Use

The task description is: $ARGUMENTS

If no arguments provided, ask the user: "What are we building or fixing?"

---

## Phase -1: TRIAGE

```
NO ACTION BEFORE TRIAGE OUTPUT
```

Output the triage box BEFORE any other action — no file reading, no questions, no exploration:

```
╔══════════════════════════════════════════════════╗
║  TRIAGE                                          ║
║  Scale:    [S / M / L]                           ║
║  Type:     [bug_fix/feature/refactor/research/config] ║
║  Security: [yes / no]                            ║
║  → Path:   [S / M / L]                           ║
║  Skills:   [list activated skills]               ║
╚══════════════════════════════════════════════════╝
```

**Scale:** S (1-2 files) / M (3-10 files) / L (10+ files, new project)
**Type:** bug_fix | feature | refactor | research | config
**Security:** Does it touch auth, API keys, user input, code execution, network? → yes
**Path:** S + (bug_fix|config) → Path S | M or feature/refactor → Path M | L or research or new project → Path L

After outputting TRIAGE, say: "Triage complete. Proceeding to Phase [0/2] (Path [S/M/L])."

---

## Phase 0: DISCOVER

> Skip if Path S → go directly to Phase 2.

DO NOT invoke `superpowers:brainstorming`. Follow the steps below directly.

### Step 0.1: Context (silent)
Read files, docs, commits. No long summary — absorb and move on.

### Step 0.2: Clarifying Questions
**Only one question per message.** If the topic needs more exploration, break it into multiple messages. Do NOT batch questions — ask one, wait for the answer, then ask the next.
Multiple choice preferred. Total across the phase: 2-4 questions.
If user says "继续": proceed with reasonable defaults (do NOT skip the phase).

### Step 0.3: Approaches
Path M: 2 options. Path L: 3 options. Lead with your recommendation.

### Step 0.4: Design
Present: architecture, components, data flow, tech stack.

After presenting, REFLECT (see Reflection Rules below):

```
┌─ 深度思考并优化 ─────────────────────────┐
│ 1. 需求理解对不对？遗漏了什么？           │
│    [your analysis — must be specific]     │
│                                           │
│ 2. 有没有更优解？                         │
│    [your analysis — name the alternative] │
│                                           │
│ 3. 这个方案是最简的吗？                   │
│    [your analysis — what can be removed?] │
└───────────────────────────────────────────┘
```

Ask: "Design 可以吗？还是要调整？" — Wait for user.

### Step 0.5: Research (Path L only)
Launch research agents in parallel. Synthesize into design.

```
┌─ 思考并优化 ─────────────────────────────┐
│ research 结果够全面吗？                   │
│ 有没有遗漏的关键来源？                    │
│ [your analysis]                           │
└───────────────────────────────────────────┘
```

### Step 0.6: Write Spec
Save to `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md` and commit.
→ Get user approval before proceeding.

---

## Phase 1: PLAN

> Skip if Path S → go directly to Phase 2.

DO NOT invoke `superpowers:writing-plans`. Follow the steps below directly.

### Step 1.1: First Principles Decomposition

Think from scratch. Do NOT regurgitate Phase 0. Analyze THIS project's unique constraints:

```
┌─ 第一性原则分解 ─────────────────────────┐
│ 1. 本质约束:                              │
│    [your analysis — project-specific]     │
│ 2. 不可绕过的限制:                        │
│    [your analysis — absolute constraints] │
│ 3. 零基础最优解:                          │
│    [your analysis — if starting from zero]│
│ 4. 现有代码评估:                          │
│    [your analysis — reuse vs rewrite]     │
└───────────────────────────────────────────┘
```

Immediately reflect:

```
┌─ 思考并优化 ─────────────────────────────┐
│ 触及了真正的约束，还是在重复表面假设？    │
│ [your self-challenge]                     │
└───────────────────────────────────────────┘
```

### Step 1.2: Search Existing Solutions
Invoke the Skill tool with `search-first`. If unavailable, use Agent (general-purpose) to search.

### Step 1.3: Write Implementation Plan
Bite-sized tasks (2-5 min each). Exact file paths. Code snippets. TDD order.
Path L: also create `task_plan.md` / `findings.md` / `progress.md`.
Save to `docs/superpowers/plans/YYYY-MM-DD-<feature>.md`.

```
┌─ 深度思考并优化 ─────────────────────────┐
│ 1. 第一性原则推导 vs 抄现有方案？         │
│    [your judgment]                        │
│ 2. 步骤/依赖合理？                        │
│    [your analysis]                        │
│ 3. 过度设计或遗漏？                       │
│    [your findings]                        │
└───────────────────────────────────────────┘
```

### Step 1.4: Feasibility Validation

```
┌─ 可行性验证 ─────────────────────────────┐
│ 1. 技术可行？ [your analysis]             │
│ 2. 隐含依赖？ [your analysis]             │
│ 3. 最大风险 + 回退？ [your analysis]      │
│ 4. 惯性检查？ [your analysis]             │
└───────────────────────────────────────────┘
```

```
┌─ 深度思考并优化 ─────────────────────────┐
│ 真的可行吗？哪步最危险？                  │
│ [your judgment]                           │
└───────────────────────────────────────────┘
```

Ask: "Plan 可以吗？开始写代码？" — Wait for user.

---

## Phase 2: BUILD

```
NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST
```

### The BUILD Cycle (per task, max 2 tasks merged):

**2.1 Write test FIRST** → run → confirm FAIL
**2.2 Implement** → run → confirm PASS
**2.3 Reflect** (see Reflection Rules):

```
┌─ 思考并优化 ─────────────────────────────┐
│ 测试: [coverage gaps? edge cases?]        │
│ 实现: [simpler approach exists?]          │
│ 质量: [naming? responsibility?]           │
└───────────────────────────────────────────┘
```

**2.4 Review** — dispatch code review agent (per-language reviewer). Fix all issues.
**2.5 Checkpoint:**

```
╔══ Task Checkpoint ═══════════════════════╗
║ Task:    [name]                           ║
║ Tests:   [X passed / Y total]             ║
║ Status:  [PASS / FAIL]                    ║
╚══════════════════════════════════════════╝
```

For architecture/core tasks, add:

```
┌─ 深度思考并优化 ─────────────────────────┐
│ 限制后续扩展？错误假设？现在改最便宜？    │
│ [your analysis]                           │
└───────────────────────────────────────────┘
```

### BUILD Discipline

| Rule | Enforcement |
|------|-------------|
| Max 2 tasks merged | Each group needs own TDD + reflection + checkpoint |
| Code review | At least 1 per Layer. Dispatch `typescript-reviewer` or `python-reviewer` |
| TDD order | Test FIRST. Wrote code first? Delete it. Start with test. |
| Parallel (Path L) | Independent tasks in worktrees. Each follows same 5-step cycle. |

---

## Phase 3: VERIFY

```
NO "DONE" CLAIMS WITHOUT VERIFY
```

Completing BUILD does NOT mean done. VERIFY is a separate phase. Run in parallel:

1. Full test suite
2. Code review agent — review ALL Phase 2 code against plan
3. Security scan (if security-required)

```
┌─ 思考并优化 ─────────────────────────────┐
│ review 问题修完了？测试盲区？             │
│ [your analysis]                           │
└───────────────────────────────────────────┘
```

Fix issues. Re-run tests. Then:

```
┌─ 深度思考并优化 ─────────────────────────┐
│ 整体一致性？遗留技术债？副作用？          │
│ [your analysis]                           │
└───────────────────────────────────────────┘
```

Show actual command output as evidence:

```
╔══ VERIFY Checkpoint ════════════════════╗
║ Tests:     [X passed / Y total]          ║
║ Review:    [issues found / fixed]        ║
║ Security:  [clean / N issues]            ║
║ Status:    [VERIFIED / BLOCKED]          ║
╚═════════════════════════════════════════╝
```

---

## Phase 4: SHIP

```
NO SESSION END WITHOUT PHASE 4
```

**4.1 Commit** (DO NOT auto-push)

```
┌─ 思考并优化 ─────────────────────────────┐
│ commit message 准确？[your judgment]      │
└───────────────────────────────────────────┘
```

**4.2 Update progress.md** — mark completed, update "Next". NOT optional.

**4.3 Learn (Path L)**

```
┌─ 深度思考并优化 ─────────────────────────┐
│ 可复用模式？[your findings]               │
│ 哪里超时？为什么？[your analysis]         │
│ 下次怎么更快？[your suggestions]          │
└───────────────────────────────────────────┘
```

If session must end before Phase 4: tell user "Phase 3/4 pending. Run `/project-workflow continue` next session."

---

## Reflection Rules

**A reflection that changes nothing is not a reflection.**

Every reflection box must produce at least ONE of:
- A mistake caught
- A risk surfaced
- A simpler approach identified
- An assumption challenged
- A dependency discovered
- A step removed or reordered

### Good vs Bad Reflection

<Good>
```
┌─ 深度思考并优化 ─────────────────────────┐
│ 发现一个问题：BILD/ANIM 不是简单的        │
│ spritesheet — 是骨骼动画。需要运行时      │
│ 播放器，不能预渲染。这是最高风险项。      │
│ → 调整：Layer 0 先验证动画播放器可行性。  │
└───────────────────────────────────────────┘
```
Found a real risk, changed the plan.
</Good>

<Bad>
```
┌─ 思考并优化 ─────────────────────────────┐
│ 测试: 覆盖够 ✓                           │
│ 实现: 没有更简洁的写法 ✓                  │
│ 质量: 命名清晰 ✓                         │
└───────────────────────────────────────────┘
```
Rubber stamp. Changes nothing. Found nothing. This is NOT reflection.
</Bad>

### Reflection Red Flags

| You are outputting | Reality |
|-------------------|---------|
| "✓" for every item | You're rubber-stamping, not reflecting |
| Same words as the content you're reflecting on | You're restating, not challenging |
| "No issues found" | Look harder. There are ALWAYS trade-offs to surface. |
| Generic praise ("clean code", "good structure") | Be specific or don't say it |

---

## Phase Transition Rules

```
TRIAGE ──→ Path S? ──→ Phase 2 → Phase 3 → Phase 4
           Path M? ──→ Phase 0 → Phase 1 → Phase 2 → Phase 3 → Phase 4
           Path L? ──→ Phase 0 (research) → Phase 1 → Phase 2 → Phase 3 → Phase 4
```

Each phase completes before the next starts. Phase 3 and 4 are NOT optional.

## Always-On

- Format hook (PostToolUse auto-format) — automatic
- Language-appropriate review agent — dispatch for code changes
- verification-before-completion — evidence before claims
- security-review — when security-required
- docs / Context7 — when external library involved

## Skill Delegation

DO NOT invoke `superpowers:brainstorming` or `superpowers:writing-plans` — they have competing workflows. You ARE the controller. Use ECC tools (reviewers, search-first, docs) and MCP tools directly.
