# Allen's Claude Code Skills

Custom skills for Claude Code.

## Skills

| Skill | Description |
|-------|-------------|
| `/project-workflow` | Universal 5-phase development workflow (TRIAGE → DISCOVER → PLAN → BUILD → VERIFY → SHIP) |

## Install

```bash
# Add as marketplace
/plugin marketplace add samsara0xgg/claude-skills

# Install
/plugin install allen-skills@samsara0xgg/claude-skills
```

Or use locally:
```bash
claude --plugin-dir /Users/alllllenshi/Projects/claude-skills
```

## Structure

```
.claude-plugin/
  plugin.json          # Plugin manifest
skills/
  project-workflow/
    SKILL.md           # Main skill
    reference.md       # Activation matrix, multi-language guide
```
