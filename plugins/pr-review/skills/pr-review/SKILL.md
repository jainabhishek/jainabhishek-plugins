---
name: pr-review
description: >
  Comprehensive PR review workflow that runs specialized review agents on uncommitted code,
  verifies each issue against the base branch, presents a numbered fix plan, and implements
  fixes after user approval. Use when: (1) User runs "/pr-review" before committing or pushing,
  (2) User wants a thorough code review with automated verification, (3) User needs to ensure
  code quality before creating a PR.
---

# PR Review & Fix Workflow

A multi-step review workflow that ensures only real, new issues are flagged and fixed.

## Workflow Steps

### 1. Run Review Agents

Run all available review agents on the uncommitted code changes (staged + unstaged):

- **Code quality**: Style, patterns, anti-patterns, naming, duplication
- **Security**: Input validation, injection vulnerabilities, auth issues, OWASP top 10
- **Architecture**: Pattern compliance, separation of concerns, dependency direction
- **Tests**: Coverage gaps, missing edge cases, test quality
- **Comments**: Accuracy, staleness, value of comments

Use `git diff` to scope reviews to only changed code. Do not review unchanged files.

### 2. Verify Against Base Branch

For **each issue found**, verify it exists on the current branch and NOT on the base branch:

1. Detect the base branch automatically:
   - Check for `develop`, `main`, or `master` (in that order)
   - Or use the branch configured in the project's PR settings
2. Compare the flagged code against the base branch version
3. **Discard** any issue that already exists on the base branch (pre-existing)
4. **Keep** only issues introduced by the current changes

This step prevents false positives from pre-existing code quality issues.

### 3. Present Verified Issues

Present only verified, new issues as a numbered fix plan:

```
Verified Issues (N total):

1. [severity] file.ts:42 - Description of the issue
   Why: Explanation of why this is a problem
   Fix: Proposed fix approach

2. [severity] other-file.ts:15 - Description
   Why: ...
   Fix: ...
```

Group by severity: critical > high > medium > low.

### 4. Wait for Approval

Ask the user which fixes to apply:

- **"all"** - Apply all fixes
- **Specific numbers** - e.g., "1, 3, 5" to apply only selected fixes
- **"none"** - Skip all fixes
- **Natural language** - e.g., "all except 2" or "only the security ones"

Do NOT implement any fixes without explicit user approval.

### 5. Implement Fixes

Apply the approved fixes:

1. Make the code changes
2. Run the project's lint command to verify (detect from `package.json` scripts, `Makefile`, `biome.json`, etc.)
3. Run the project's build command if available
4. Show a summary of changes made

### 6. Re-run Verification

After implementing fixes:

1. Re-run the review agents on the modified code
2. Verify all approved issues are resolved
3. Report any remaining or newly introduced issues
4. If new issues found, return to step 3

## Usage

```
/pr-review              # Full review workflow
```

## Notes

- Reviews are scoped to uncommitted changes only (not the full codebase)
- Base branch detection is automatic but can be overridden
- The workflow is iterative: fix -> verify -> fix until clean
- All fixes require explicit user approval before implementation
