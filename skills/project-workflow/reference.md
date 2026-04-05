# Project Workflow Reference

## Skill Activation Matrix

|                              | Path S | Path M | Path L |
|------------------------------|--------|--------|--------|
| brainstorm                   | -      | yes    | yes    |
| deep-research                | -      | -      | yes    |
| docs / Context7              | ?      | ?      | ?      |
| search-first                 | -      | ?      | yes    |
| writing-plans                | -      | yes    | yes    |
| planning-with-files          | -      | -      | yes    |
| tdd-workflow / python-testing| yes    | yes    | yes    |
| python-review                | yes    | yes    | yes    |
| security-review              | ?      | ?      | ?      |
| subagent / worktree          | -      | -      | yes    |
| code-reviewer                | -      | yes    | yes    |
| security-scan                | -      | -      | yes    |
| verification-before-done     | yes    | yes    | yes    |
| context-budget               | -      | -      | yes    |
| learn-eval                   | -      | -      | yes    |
| aside                        | ?      | ?      | ?      |

yes = always | ? = conditional | - = skip

## Parallel Strategies

Phase 0: brainstorm || deep-research || docs-lookup (up to 3-way)
Phase 1: writing-plans || planning-with-files (2-way)
Phase 2: task-A(worktree) || task-B(worktree) || task-C (N-way)
Phase 3: pytest || code-review || security-scan (3-way)

## Reflection Points

### Deep (6x)
1. Phase 0: Design Spec complete
2. Phase 1: Plan review (first-principles check)
3. Phase 1: Feasibility validation
4. Phase 3: Final verification (overall consistency)
5. Phase 4: Ship (long-term impact)
6. Phase 4: Learn-eval (pattern generality)

### Normal (8x)
1. Phase 0: Research completeness (Path L)
2. Phase 1: First-principles decomposition
3. Phase 2: Test coverage
4. Phase 2: Implementation simplicity
5. Phase 2: Review fixes
6. Phase 2: Security fixes
7. Phase 3: Verify aggregate
8. Phase 4: Commit message

## Adapting to Other Languages

Replace per-language tools:

| Language   | Formatter Hook      | Reviewer          | Testing            |
|------------|--------------------|--------------------|---------------------|
| Python     | ruff format + check | python-review     | python-testing      |
| Go         | gofmt              | go-review          | go-test             |
| Rust       | rustfmt            | rust-review        | rust-test           |
| TypeScript | prettier           | typescript-reviewer| tdd-workflow        |
| Java       | google-java-format | java-review        | springboot-tdd      |
| Kotlin     | ktlint             | kotlin-review      | kotlin-test         |
| C++        | clang-format       | cpp-review         | cpp-test            |

## TRIAGE Decision Examples

**"fix store.py timezone bug"**
→ S, bug_fix, no security → Path S
→ ~8K tokens

**"add function calling extraction to memory"**
→ M, feature, LLM API involved → Path M + docs(Context7)
→ ~50K tokens

**"redesign entire memory architecture based on Mem0/MemGPT"**
→ L, feature+research, data storage security → Path L (full)
→ ~200K tokens

**"investigate best ASR options for RPi5"**
→ research → Path L (deep-research only, no BUILD)

**"update config.yaml comments"**
→ S, config, no security → Path S (skip all planning)
→ ~5K tokens
