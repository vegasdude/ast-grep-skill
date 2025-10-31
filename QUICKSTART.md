# ast-grep Quick Start Guide

This guide will help you quickly get started with the ast-grep skill for Claude Code.

## Prerequisites

Install ast-grep if you haven't already:

```bash
# macOS
brew install ast-grep

# Using cargo (Rust)
cargo install ast-grep

# Using npm
npm install -g @ast-grep/cli

# Verify installation
ast-grep --version
```

## Testing the Skill

### 1. Try a Simple Search

**JavaScript Example:**

Create a test JavaScript file:

```bash
cat > test.js << 'EOF'
var x = 1;
var y = 2;
console.log(x + y);
const z = 3;
EOF
```

Search for `var` declarations:

```bash
ast-grep run -p 'var $VAR = $VAL' -l javascript test.js
```

**Java Example:**

Create a test Java file:

```bash
cat > Test.java << 'EOF'
public class Test {
    public static void main(String[] args) {
        System.out.println("Hello");
        new Date();
        Optional<String> opt = Optional.empty();
        opt.get();
    }
}
EOF
```

Search for `System.out.println` calls:

```bash
ast-grep run -p 'System.out.println($MSG)' -l java Test.java
```

### 2. Try a Rewrite

**JavaScript Example:**

First, find all matches with JSON output:

```bash
ast-grep run -p 'var $VAR = $VAL' -l javascript test.js --json
```

After reviewing the matches, apply the rewrite:

```bash
ast-grep run -p 'var $VAR = $VAL' -r 'let $VAR = $VAL' -l javascript test.js -U
```

**Java Example:**

Find all legacy Date usages:

```bash
ast-grep run -p 'new Date()' -l java Test.java --json
```

Apply the replacement:

```bash
ast-grep run -p 'new Date()' -r 'LocalDate.now()' -l java Test.java -U
```

**Note:** When using this skill with Claude Code, Claude will analyze the JSON output and present findings before applying changes with `-U` after your approval. Do not use `--interactive` mode as it requires manual input.

### 3. Try Rule-Based Scanning

Create a custom rule file for Java:

```bash
mkdir -p rules
cat > rules/optional-misuse.yml << 'EOF'
id: optional-get-without-check
language: Java
message: Optional.get() called without isPresent() check
note: Use orElse(), orElseGet(), or orElseThrow() instead
severity: warning
rule:
  pattern: $OPT.get()
  not:
    inside:
      any:
        - pattern: if ($OPT.isPresent()) { $$$ }
        - pattern: $OPT.orElse($$$)
        - pattern: $OPT.orElseGet($$$)
EOF
```

Create an `sgconfig.yml`:

```yaml
ruleDirs:
  - rules
```

Run the scan:

```bash
ast-grep scan Test.java
```

You should see the Optional.get() issue detected!

### 4. Ask Claude for Help

Now that the skill is installed, you can ask Claude questions like:

**JavaScript/TypeScript queries:**
- "Use ast-grep to find all console.log statements"
- "Help me find all functions that use setTimeout"
- "Find all places where we're not handling Promise rejections"
- "Replace all var declarations with let using ast-grep"
- "Convert all function declarations to arrow functions"

**Java queries:**
- "Find all empty catch blocks in my Java code"
- "Detect Optional.get() calls without isPresent() checks"
- "Find potential NullPointerException risks using ast-grep"
- "Identify resources that should use try-with-resources"
- "Find JUnit test methods without assertions"
- "Detect potential SQL injection from string concatenation"
- "Find all @Deprecated method usages"

**Rule creation:**
- "Create an ast-grep rule to prevent using eval()"
- "Write a rule that ensures all async functions have error handling"
- "Create a rule to enforce Java Stream API best practices"
- "Write a rule to detect hardcoded credentials in Java"

**Analysis:**
- "Use ast-grep to find complex nested conditionals"
- "Find all database queries that aren't parameterized"
- "Analyze where we're using deprecated APIs"

## Common Patterns

### JavaScript Patterns

**Find Function Calls:**
```bash
ast-grep run -p '$FUNC($$$ARGS)' -l javascript
```

**Find Method Calls:**
```bash
ast-grep run -p '$OBJ.$METHOD($$$ARGS)' -l javascript
```

**Find Async Functions Without Try-Catch:**
```yaml
id: no-try-catch
language: JavaScript
rule:
  pattern: |
    async function $FUNC($$$PARAMS) {
      $$$BODY
    }
  not:
    has:
      pattern: |
        try {
          $$$
        } catch ($E) {
          $$$
        }
```

### Java Patterns

**Find Method Calls:**
```bash
ast-grep run -p '$OBJ.$METHOD($$$ARGS)' -l java
```

**Find Methods with @Deprecated:**
```yaml
id: find-deprecated
language: Java
rule:
  kind: method_declaration
  has:
    kind: marker_annotation
    pattern: "@Deprecated"
```

**Find Empty Catch Blocks:**
```yaml
id: empty-catch
language: Java
rule:
  kind: catch_clause
  has:
    pattern: |
      catch ($E) {
      }
message: Empty catch block - handle or log exception
```

**Find String Fields (any modifiers):**
```yaml
id: string-fields
language: Java
rule:
  kind: field_declaration
  has:
    field: type
    regex: ^String$
```

## Testing Your Rules

Create a test file:

```yaml
# tests/my-rule-test.yml
id: my-rule
language: JavaScript

testCases:
  - id: should-match-case
    match: |
      // Code that SHOULD trigger the rule
      console.log("test");

  - id: should-not-match-case
    match: |
      // Code that should NOT trigger the rule
      logger.info("test");
```

Run tests:

```bash
ast-grep test
```

## Next Steps

1. **Read the Skill:** Review `.claude/skills/ast-grep/SKILL.md` for comprehensive documentation including:
   - Java-specific patterns for annotations, null safety, Stream API, and more
   - JavaScript/TypeScript examples
   - Complete AST node type reference for Java
   - Java gotchas and best practices
2. **Visit the Playground:** Try patterns at https://ast-grep.github.io/playground.html
3. **Ask Claude:** Leverage the skill by asking Claude to help with ast-grep tasks for Java, JavaScript, TypeScript, or other supported languages
4. **Create Custom Rules:** Start building your own linting rules based on your project's needs

## Troubleshooting

### "command not found: ast-grep"
Make sure ast-grep is installed and in your PATH.

### "Pattern must be valid code"
Ensure your pattern is syntactically valid in the target language.

### "No files matched"
Check that:
- Your file paths are correct
- The language specification matches your files
- Your `sgconfig.yml` exclude patterns aren't too broad

### Meta-variable not working
Remember:
- Use UPPERCASE: `$VAR` not `$var`
- Start with `$`: `$VAR` not `VAR`
- No lowercase letters: `$MY_VAR` not `$myVar`

## Resources

- **Skill Documentation:** `.claude/skills/ast-grep/SKILL.md`
- **Official Docs:** https://ast-grep.github.io/
- **Playground:** https://ast-grep.github.io/playground.html
- **GitHub:** https://github.com/ast-grep/ast-grep

## Example Session with Claude

```
You: I need to refactor my codebase to replace all console.log with a proper logger

Claude: I'll help you use ast-grep to replace console.log with a logger. Let me:
1. First search to see what console.log usage we have
2. Present the findings to you
3. After your approval, apply the changes automatically

[Claude runs: ast-grep run -p 'console.log($A)' -l javascript --json]

Claude: I found 23 console.log statements across 8 files:
- src/app.js: 5 occurrences
- src/utils.js: 3 occurrences
- ...

Would you like me to replace them all with logger.info()?

You: Yes, please proceed

[Claude runs: ast-grep run -p 'console.log($A)' -r 'logger.info($A)' -l javascript -U]

Claude: Done! Replaced 23 console.log statements. Please review the changes and run your tests.
```

Enjoy using ast-grep with Claude Code!
