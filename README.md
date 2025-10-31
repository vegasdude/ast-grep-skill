# ast-grep Skill for Claude Code

This repository contains a Claude Code skill that teaches Claude how to work effectively with ast-grep, a powerful AST-based code search, lint, and rewrite tool.

**This skill enables Claude to use ast-grep programmatically** - Claude will analyze code with `--json` output, present findings to you, and apply changes with `-U` after your approval. Interactive mode is not used as Claude operates ast-grep automatically.

## What's Included

- **Skill File:** `.claude/skills/ast-grep/SKILL.md` - Comprehensive guide for using ast-grep with extensive examples
- **Java Support:** Dedicated section with Java-specific patterns, gotchas, and AST node reference
- **Multi-Language Coverage:** Examples for JavaScript, TypeScript, Python, and Java

## Installation

### For Personal Use (All Projects)
Copy the skill directory to your personal Claude skills folder:
```bash
cp -r .claude/skills/ast-grep ~/.claude/skills/
```

### For Project-Specific Use
The skill is already configured in this repository at `.claude/skills/ast-grep/` and will be automatically available when using Claude Code in this project.

### For Other Projects
Copy the skill directory to any project:
```bash
cp -r .claude/skills/ast-grep /path/to/your/project/.claude/skills/
```

## How It Works

Once installed, Claude will automatically use ast-grep programmatically:

1. **Analysis:** Claude runs ast-grep with `--json` to find patterns or issues
2. **Presentation:** Claude shows you the findings with file locations and details
3. **Action:** After your approval, Claude applies changes using `-U` flag

The skill teaches Claude about:

- **Pattern Syntax:** How to write effective search patterns with meta-variables
- **Rule Configuration:** Creating YAML-based linting rules
- **Programmatic Usage:** Using `--json` for analysis and `-U` for applying changes
- **Java-Specific Patterns:** Annotations, null checks, Stream API, exception handling, and security patterns
- **Best Practices:** Common pitfalls and how to avoid them (including avoiding `--interactive` mode)
- **Integration Examples:** Using ast-grep in CI/CD and with other tools

## Quick Start with ast-grep

If you don't have ast-grep installed:

```bash
# macOS
brew install ast-grep

# Cargo (Rust)
cargo install ast-grep

# npm
npm install -g @ast-grep/cli
```

Verify installation:
```bash
ast-grep --version
```

## Example Use Cases

Ask Claude to help you with:

### JavaScript/TypeScript
1. **Code Search:** "Use ast-grep to find all console.log statements in my JavaScript files"
2. **Refactoring:** "Help me replace all var declarations with let using ast-grep"
3. **Custom Linting:** "Create an ast-grep rule to prevent using setTimeout in async functions"

### Java
1. **Null Safety:** "Find all method calls that could cause NullPointerException"
2. **Exception Handling:** "Detect empty catch blocks in my Java code using ast-grep"
3. **Optional Misuse:** "Find Optional.get() calls without isPresent() checks"
4. **Stream API:** "Detect streams that are created but never consumed"
5. **Security:** "Find potential SQL injection risks from string concatenation"
6. **Resource Management:** "Identify resources that should use try-with-resources"
7. **Test Quality:** "Find JUnit test methods without assertions"

### General
1. **Code Analysis:** "Find all functions that don't handle errors properly using ast-grep"
2. **Deprecated APIs:** "Find all usages of @Deprecated methods in my codebase"

## Skill Features

The skill teaches Claude about:

- ✓ AST-based pattern matching vs text-based search
- ✓ Meta-variable syntax and capturing
- ✓ Atomic, relational, and composite rules
- ✓ Programmatic workflow with `--json` analysis and `-U` application
- ✓ Rule testing and validation
- ✓ CI/CD integration
- ✓ Java-specific patterns (annotations, null safety, Stream API, exception handling)
- ✓ Java AST node types and structural rules
- ✓ Java gotchas (modifiers, annotations, generics)
- ✓ Language-specific considerations for JavaScript, TypeScript, Python
- ✓ Performance optimization techniques
- ✓ When NOT to use features (like `--interactive` mode)

## Documentation

For more information about ast-grep itself, visit:
- Official Documentation: https://ast-grep.github.io/
- Online Playground: https://ast-grep.github.io/playground.html

## Contributing

Feel free to enhance this skill with:
- Additional examples
- Language-specific patterns
- Advanced use cases
- Common rule templates

## License

MIT License - see [LICENSE](LICENSE) file for details.
