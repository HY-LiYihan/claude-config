# CLAUDE.md — Project Conventions for Claude Code

## Doc Sync Rules

After modifying any file, check whether the following docs need updating and apply changes proactively:

| Modified file(s) | Docs to sync |
|---|---|
| `scripts/run_*.py`, `scripts/viz_*.py`, `scripts/audit/*.py` | `README.md` (usage examples, CLI flags), `README_zh.md` (Chinese version, keep in sync with README.md), `docs/scripts_reference.md`, `runbook.md` (workflow steps), `docs/` (any relevant doc) |
| `scripts/traversability/model.py`, `scripts/traversability/backbone.py` | `docs/model_architecture.md` (architecture, tensor shapes, param counts) |
| `scripts/traversability/losses.py` | `docs/model_architecture.md` (loss function section) |
| `scripts/traversability/train.py` | `docs/model_architecture.md` (training config section) |
| `scripts/traversability/dataset.py` | `docs/dataset_architecture.md` |
| `scripts/preprocess/*.py` | `docs/dataset_architecture.md`, `README.md`, `README_zh.md` |
| `scripts/utils/*.py` | `docs/scripts_reference.md` |
| `src/` (any file) | `README.md` (API/library section if applicable) |

After every code change, proactively update **all** of the following that exist and are affected:
- `README.md` and `README_zh.md` (keep Chinese in sync with English)
- `runbook.md` (workflow / operational steps)
- Any file under `docs/` that covers the modified script or feature

Do not create new doc files unless explicitly requested. Update existing ones in-place.

---

## Code Style

### Module docstrings
Every Python file must have a module-level docstring in this format:
```python
"""<一句中文摘要，说明模块用途>。

<English body: what the module does, key inputs/outputs, usage notes.>
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

## Architecture Constraints

- **Depth output format**: 16-bit PNG (uint16, millimetres). Metadata (scale, shift, camera model) stored in a separate `.npz` sidecar under `depth_meta/`.
- **Mask output format**: `.npz` per frame with keys `ground_raw`, `obstacle_raw` (after segmentation) and `ground`, `obstacle` (after refinement).
- **Elevation output format**: `.npz` per frame, keys: `profile_data (N,2)`, `profile_offsets (361,)`, `has_data (360,)`, `vertical_mode`. Default vertical mode is `ransac` (heights relative to fitted ground plane). Angle convention: clockwise, forward=0°, right=+90°, left=−90°.
- **Splits JSON**: lives under `<data_root>/splits/<dataset>.json`. Each scene entry has a `paths` dict with keys `raw_images`, `depth`, `masks`, `elevation`, and a `status` dict with per-stage completion counts.

---

## Pipeline Stage Order

1. Preprocess (`scripts/preprocess/`)
2. Depth estimation (`scripts/run_depth_da3.py` / `run_depth_dap.py` / `run_depth_unik3d.py`)
3. Obstacle segmentation (`scripts/run_segment_masks.py`)
4. Mask refinement (`scripts/run_refine_masks.py`)
5. Elevation map (`scripts/run_elevation.py`)
6. Traversability training (`scripts/traversability/train.py`)
7. Sync splits status (`scripts/audit/sync_splits.py`)

---

## Session Summary Style

After completing a set of code changes, provide a summary in **Chinese** describing what was modified and why.

---

## What NOT to Do

- Do not add backwards-compatibility shims or unused variables.
- Do not add error handling for scenarios that cannot happen in normal operation.
- Do not create helper abstractions for one-off operations.
- Do not commit `command.md`, `.env`, or anything listed in `.gitignore`.
- Do not push to remote unless explicitly asked.
