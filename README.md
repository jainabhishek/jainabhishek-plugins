# jainabhishek-plugins

Claude Code plugins for productivity and code quality.

## Installation

Subscribe to the marketplace (one-time):

```bash
/plugin marketplace add jainabhishek/jainabhishek-plugins
```

Then install any plugin:

```bash
/plugin install reflect@jainabhishek-plugins
/plugin install pr-review@jainabhishek-plugins
```

Or install directly without subscribing:

```bash
/plugin add jainabhishek/jainabhishek-plugins
/plugin add jainabhishek/jainabhishek-plugins --plugin pr-review
```

---

## Plugins

### Reflect

A self-improving skill that learns from your corrections and remembers them across sessions.

**Correct Claude once, never again.**

#### How It Works

When you correct Claude ("No, always use `const` not `let`" or "Never commit directly to main"), Reflect extracts these learnings and saves them to skill files. Next session, Claude automatically applies what it learned.

```
Session 1: You correct Claude  →  /reflect  →  Saves to skill files
Session 2: Claude loads skill files at startup  →  Applies learnings automatically
```

#### Usage

```
/reflect           # Analyze conversation and extract learnings
reflect on         # Enable auto-learning on session end
reflect off        # Disable auto-learning
reflect status     # Check current mode
```

#### How Continuous Learning Works

This is **not** model fine-tuning. Reflect writes human-readable instructions to files that Claude reads at the start of each session — like a growing instruction manual.

```
~/.claude/skills/
├── code-style.md      # "Use const, not let"
├── security.md        # "Check for SQL injection"
├── workflow.md        # "Never push to main directly"
└── project-x.md       # Project-specific preferences
```

#### Signal Detection

| Confidence | Source | Examples |
|------------|--------|----------|
| **HIGH** | Explicit corrections | "Never do X", "Always check Y" |
| **MEDIUM** | Success patterns | Approaches that worked after iteration |
| **LOW** | Implied preferences | Patterns to review later |

---

### PR Review

A comprehensive PR review workflow that runs specialized review agents, verifies issues against the base branch, and implements fixes with your approval.

#### Usage

```
/pr-review
```

#### Workflow

1. **Run review agents** — Code quality, security, architecture, tests, and comments
2. **Verify against base branch** — Discard pre-existing issues, keep only new ones
3. **Present fix plan** — Numbered list of verified issues grouped by severity
4. **Wait for approval** — Choose which fixes to apply (all, specific numbers, or none)
5. **Implement fixes** — Apply changes and run lint/build to verify
6. **Re-verify** — Confirm all issues are resolved

#### Why Verification Matters

Most review tools flag every issue they find, including problems that existed long before your changes. This plugin cross-references each finding against the base branch so you only see issues **you** introduced. No noise, no false positives from legacy code.

---

## License

MIT
