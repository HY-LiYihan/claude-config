# CLAUDE.md — Project Conventions for Claude Code

## Initial Setup

Before starting work on a project, read the following documents to build project context:
- `README.md` - Project overview, setup, and usage
- `Runbook` (if exists) - Operational workflows and procedures
- `docs/` folder (if exists) - Detailed documentation on architecture, components, and workflows

This ensures you understand the project structure, conventions, and current state before making changes.

---

## Doc Sync Rules

After modifying any file, proactively update the following documentation if it exists and is affected:
- `README.md` - Update usage examples, CLI flags, API documentation
- `Runbook` - Update workflow steps and operational procedures
- `docs/` folder - Update any relevant architecture or component documentation

**Key principle**: Documentation is a first-class artifact. Keep it in sync with code changes.

Do not create new doc files unless explicitly requested. Update existing ones in-place.

---

## Code Style

### Module docstrings
Every Python file must have a module-level docstring in this format:
```python
"""<Brief description of module purpose in English>.

<Detailed explanation: what the module does, key inputs/outputs, usage notes.>
"""
```

### Inline comments and docstrings
- All inline comments (`#`) must be in **English**.
- All function/class docstrings must be in **English**, Google style (Args / Returns / Raises).
- No emoji anywhere in source code (comments, prints, docstrings, log messages).

### Print output style
- Use `[ERROR]` prefix for fatal errors.
- Use `[WARN]` prefix for non-fatal warnings.
- No emoji in any `print()` or `logging` call.

### Section banners
Use this style for section separators inside files:
```python
# ---------------------------------------------------------------------------
# Section name
# ---------------------------------------------------------------------------
```
Not `# ===...===` or `# ***...***`.

---

## Session Summary Style

After completing a set of code changes, provide a summary describing what was modified and why.

---

## What NOT to Do

- Do not add backwards-compatibility shims or unused variables.
- Do not add error handling for scenarios that cannot happen in normal operation.
- Do not create helper abstractions for one-off operations.
- Do not commit `.env`, `command.md`, or anything listed in `.gitignore`.
- Do not push to remote unless explicitly asked.

