# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a Claude Code skill repository that teaches Claude how to use ast-grep programmatically. It is NOT an ast-grep tool itself, but rather documentation that gets loaded as a skill when Claude operates in projects.

## Repository Structure

```
.claude/skills/ast-grep/
└── SKILL.md          # Main skill file with YAML frontmatter (loaded by Claude Code)

README.md             # User-facing documentation
QUICKSTART.md         # Tutorial for users
LICENSE              # MIT License
```

The entire skill is self-contained in a single file: `.claude/skills/ast-grep/SKILL.md`

## Architecture: Claude Code Skills

**Key Concept:** This repository follows the Claude Code Skills structure:

1. **Skill Directory:** `.claude/skills/ast-grep/` contains the skill
2. **SKILL.md Required:** The main file MUST be named `SKILL.md` (not `ast-grep.md` or other names)
3. **YAML Frontmatter Required:** Must have `name` and `description` fields
4. **Installation:** Users copy `.claude/skills/ast-grep/` to:
   - Personal: `~/.claude/skills/` (all projects)
   - Project-specific: `.claude/skills/` in their project

## Critical Design Principles

### 1. Programmatic Usage Only
This skill teaches Claude to use ast-grep **programmatically**, not interactively:

- ✓ Use `--json` for analysis
- ✓ Use `-U` to apply changes after user approval
- ✗ NEVER use `--interactive` mode (requires human input, fails in automation)

### 2. SKILL.md Structure
The skill file must follow this pattern:
```yaml
---
name: skill-name-lowercase-with-hyphens
description: Clear description of what it does and when to activate (max 1024 chars)
---

# Content follows...
```

The `description` field is critical - it tells Claude when to activate the skill.

### 3. Content Organization in SKILL.md
The skill content is organized as:
1. **Core Commands** - Basic ast-grep CLI usage
2. **Pattern Syntax** - Meta-variables, wildcards
3. **Rule Configuration** - YAML rule structure
4. **Java-Specific Patterns** - Language-specific section (annotations, null checks, etc.)
5. **Using ast-grep with Claude Code** - Programmatic workflow section
6. **Best Practices** - Guidelines for effective usage
7. **Common Pitfalls** - What to avoid

## Making Changes to the Skill

### When updating SKILL.md:

1. **Keep examples inline** - No separate examples folder (was removed for simplicity)
2. **Multi-language examples** - Show JavaScript AND Java examples throughout
3. **Emphasize programmatic workflow** - Always show `--json` → analyze → `-U` pattern
4. **Java AST node types** - Keep the reference table updated for structural rules
5. **Update description field** - If adding major features, update YAML frontmatter

### When updating documentation:

- **README.md** - User-facing, explains what the skill is and how to install it
- **QUICKSTART.md** - Tutorial-style guide with concrete examples
- Both should stay consistent with SKILL.md's programmatic approach

## Validation Checklist

Before committing changes to SKILL.md:

1. ✓ YAML frontmatter has valid `name` (lowercase, hyphens, max 64 chars)
2. ✓ YAML frontmatter has `description` (max 1024 chars, mentions when to use)
3. ✓ No references to `--interactive` mode as a recommendation
4. ✓ Programmatic workflow emphasized (`--json` for analysis, `-U` for application)
5. ✓ File is named exactly `SKILL.md` (case-sensitive)
6. ✓ Located at `.claude/skills/ast-grep/SKILL.md`

## Language Coverage

The skill currently covers:
- **Java** (extensive: annotations, null safety, Stream API, exceptions, security patterns)
- **JavaScript/TypeScript** (moderate: async patterns, var/let/const, console.log)
- **Python** (basic: mentioned in examples)
- **Others** (listed as supported: C, C++, Rust, Go, C#, Kotlin, Swift)

### Adding new language coverage:

1. Add language-specific section in SKILL.md (follow Java pattern)
2. Include AST node types reference for that language
3. Document gotchas (like Java's modifier issues)
4. Add examples to README.md and QUICKSTART.md

## Testing the Skill

There's no automated test suite. To manually verify:

1. Copy skill to `~/.claude/skills/` or project `.claude/skills/`
2. Ask Claude Code a question that should trigger the skill
3. Verify Claude uses `--json` and `-U` flags, not `--interactive`
4. Check that Java-specific patterns are accessible

Example test prompts:
- "Find all empty catch blocks in my Java code using ast-grep"
- "Use ast-grep to replace console.log with logger.info"

## Important: What This Repository Is NOT

- ❌ Not an ast-grep implementation
- ❌ Not a plugin/extension for ast-grep
- ❌ Not executable code that runs
- ❌ Not a test suite for ast-grep

This is purely documentation/knowledge that Claude Code loads as a skill.
