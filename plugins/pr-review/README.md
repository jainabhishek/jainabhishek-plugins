# PR Review Plugin

A comprehensive PR review workflow for Claude Code that runs specialized review agents, verifies issues against the base branch, and implements fixes with your approval.

## Installation

### Via marketplace

```bash
/plugin marketplace add jainabhishek/jainabhishek-plugins
/plugin install pr-review@jainabhishek-plugins
```

### Direct install

```bash
/plugin add jainabhishek/jainabhishek-plugins --plugin pr-review
```

## Usage

```
/pr-review
```

## Workflow

1. **Run review agents** - Code quality, security, architecture, tests, and comments
2. **Verify against base branch** - Discard pre-existing issues, keep only new ones
3. **Present fix plan** - Numbered list of verified issues grouped by severity
4. **Wait for approval** - Choose which fixes to apply (all, specific numbers, or none)
5. **Implement fixes** - Apply changes and run lint/build to verify
6. **Re-verify** - Confirm all issues are resolved

## Why Verification Matters

Most review tools flag every issue they find, including problems that existed long before your changes. This plugin cross-references each finding against the base branch so you only see issues **you** introduced. No noise, no false positives from legacy code.

## License

MIT
