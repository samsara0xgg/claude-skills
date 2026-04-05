---
name: project-workflow
description: Universal 5-phase project workflow with intelligent triage, first-principles planning, reflection system, and parallel execution. Combines Superpowers + ECC + Planning-with-Files.
user-invocable: true
---

# Project Workflow — Universal 5-Phase Development Process

<HARD-GATE>
You MUST complete each phase in order. You MUST output the TRIAGE assessment BEFORE doing anything else. You MUST NOT skip to implementation without completing DISCOVER and PLAN phases (unless Path S). You MUST NOT write any code until Phase 2. Violation of phase ordering is a CRITICAL error.
</HARD-GATE>

<HARD-GATE>
DO NOT invoke `superpowers:brainstorming` or `superpowers:writing-plans` as separate skills. They have their own competing workflows that will override this one. Instead, follow the inline instructions in each phase below. You ARE the workflow controller — never hand off control to another process-level skill.
</HARD-GATE>

## How to Use

The task description is: $ARGUMENTS

If no arguments provided, ask the user: "What are we building or fixing?"

---

## Phase -1: TRIAGE

<HARD-GATE>
This phase is MANDATORY. You MUST output the triage box below BEFORE any other action. No exploring files, no asking questions, no brainstorming. TRIAGE FIRST.
</HARD-GATE>

Evaluate the task on 3 dimensions and output:

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

> **Skip if Path S.** Go directly to Phase 2.

<HARD-GATE>
DO NOT invoke superpowers:brainstorming. Follow the steps below directly.
</HARD-GATE>

### Step 0.1: Context (do silently)
Read relevant files, docs, recent commits to understand the current state. Do NOT output a long summary — just absorb context.

### Step 0.2: Clarifying Questions
Ask the user **one question at a time**. Prefer multiple choice. Focus on:
- Purpose: What problem does this solve?
- Constraints: Platform, performance, dependencies?
- Success criteria: How do we know it's done?

Keep to 2-4 questions max. If the user says "继续" or similar, move forward with reasonable defaults.

### Step 0.3: Approaches (Path M: 2 options, Path L: 3 options)
Present approaches with trade-offs. Lead with your recommendation.

### Step 0.4: Design
Present the design in sections:
- Architecture overview
- Key components and their responsibilities
- Data flow
- Tech stack choices

After presenting:

> **深度思考并优化：**
> - 需求理解对不对？遗漏了什么？
> - 有没有更优解？
> - 这个方案是最简的吗？

Ask: "Design 可以吗？还是要调整？"

### Step 0.5: Research (Path L only)
If Path L, launch research agents in parallel:
- Use `Agent` tool with `mcp__exa__web_search_exa` for web research
- Use `mcp__context7__query-docs` for library documentation
- Synthesize findings into the design

> **思考并优化：** research 结果够全面吗？有没有遗漏的关键来源？

### Step 0.6: Write Spec
Save design to `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md` and commit.

→ Get user approval before proceeding.

---

## Phase 1: PLAN

> **Skip if Path S.** Go directly to Phase 2.

<HARD-GATE>
DO NOT invoke superpowers:writing-plans. Follow the steps below directly.
</HARD-GATE>

### Step 1.1: First Principles Decomposition

Output explicitly:

> **第一性原则分解：**
> 1. 这个问题的本质约束是什么？
> 2. 不可绕过的技术限制是什么？
> 3. 从零开始，最优解应该长什么样？
> 4. 现有代码中哪些部分接近最优，哪些需要改？

> **思考并优化：** 分解是否触及了真正的约束？

### Step 1.2: Search Existing Solutions
Use `everything-claude-code:search-first` — check if existing libraries/tools already solve part of the problem.

### Step 1.3: Write Implementation Plan

Create a plan with **bite-sized tasks** (each 2-5 minutes):
- Exact file paths for every change
- Code snippets for each step
- Test commands with expected output
- TDD order: write test → verify fail → implement → verify pass → commit

For Path L: Also create `task_plan.md` / `findings.md` / `progress.md` for tracking.

Save plan to `docs/superpowers/plans/YYYY-MM-DD-<feature>.md`

> **深度思考并优化：**
> - 方案是从第一性原则推导的，还是在抄现有方案？
> - 步骤分解合理吗？依赖关系对吗？

### Step 1.4: Feasibility Validation

> **可行性验证：**
> 1. 技术上能实现吗？有没有 API/硬件约束？
> 2. 有没有隐含依赖没列出来？
> 3. 最大风险点在哪？失败怎么回退？
> 4. 有没有因为惯性思维多加了不必要的步骤？

> **深度思考并优化：** 这个计划真的可行吗？哪一步最可能出问题？

Ask: "Plan 可以吗？开始写代码？"

→ Get user approval before proceeding.

---

## Phase 2: BUILD

<HARD-GATE>
For EVERY task (not just scaffold), you MUST follow the 5-step cycle below IN ORDER. You MUST output the reflection checkpoint box after each task. Skipping TDD or skipping reflections is a CRITICAL violation. No exceptions.
</HARD-GATE>

### The BUILD Cycle (repeat for each task):

**Step 2.1: Write tests FIRST (TDD)**
- Write a failing test that defines the expected behavior
- Run the test to confirm it FAILS
- If the task is pure scaffold (no logic), write a smoke test instead (e.g., "module imports without error", "config file is valid")

**Step 2.2: Implement — make the test pass**
- Write MINIMAL code to pass the test
- Run the test to confirm it PASSES

**Step 2.3: Reflect and optimize**

You MUST output this box after implementation:

```
┌─ 思考并优化 ─────────────────────────────┐
│ 测试: [覆盖够吗？边界情况？]              │
│ 实现: [有更简洁的写法吗？]                │
│ 质量: [命名清晰吗？职责单一吗？]          │
└───────────────────────────────────────────┘
```

If any answer reveals an issue → fix it NOW before proceeding.

**Step 2.4: Review**
- Dispatch code review agent (python-review / typescript-reviewer depending on language)
- If security-required: also dispatch security-review agent
- Fix all issues found before proceeding

**Step 2.5: Test gate + checkpoint**

Run full test suite. Then output this checkpoint:

```
╔══ Task Checkpoint ═══════════════════════╗
║ Task:    [task name]                      ║
║ Tests:   [X passed / Y total]             ║
║ Status:  [PASS / FAIL]                    ║
╚══════════════════════════════════════════╝
```

PASS → proceed to next task
FAIL → fix and re-run until PASS

### For tasks with deep implications (architecture, data model, core systems):

After the checkpoint, ALSO output:

```
┌─ 深度思考并优化 ─────────────────────────┐
│ 这个实现会限制后续扩展吗？                │
│ 有没有做了不该做的假设？                  │
│ 如果要改，现在改成本最低                  │
└───────────────────────────────────────────┘
```

### Parallel execution (Path L only):
If plan has independent tasks, dispatch parallel worktree agents via `Agent` tool with `isolation: "worktree"`. Each agent MUST follow the same 5-step cycle.

---

## Phase 3: VERIFY

Run in parallel:
1. `python -m pytest tests/ -v` (full suite)
2. Dispatch `superpowers:code-reviewer` agent — review against plan
3. Dispatch `everything-claude-code:security-scan` agent (if security-required)

> **思考并优化：** review 和 scan 发现的问题都修完了吗？

Then verify:
- Run actual commands, show real output
- NO claiming "done" without evidence

> **深度思考并优化：**
> - 整体一致性如何？
> - 有没有遗留问题？
> - 改动的副作用想清楚了吗？

---

## Phase 4: SHIP

**1. Commit** (DO NOT auto-push)

> **思考并优化：** commit message 准确反映了改动吗？

**2. Update progress.md** to mark completion.

**3. Learn (Path L only)**

> **深度思考并优化：**
> - 这次有什么可复用的模式？
> - 长期影响？下次怎么更快？

---

## Always-On (all paths, all phases)

- Ruff hook (PostToolUse auto-format) — automatic
- python-review — any code change
- verification-before-completion — any task completion
- security-review — when security-required
- docs / Context7 — when external library involved

## Phase Transition Rules

```
TRIAGE ──→ Path S? ──→ Phase 2 (BUILD) directly
           Path M? ──→ Phase 0 → Phase 1 → Phase 2 → Phase 3 → Phase 4
           Path L? ──→ Phase 0 (with research) → Phase 1 → Phase 2 → Phase 3 → Phase 4
```

Each phase MUST complete and get user approval (where indicated) before the next phase starts. Never skip ahead. Never lose control to another skill's workflow.
