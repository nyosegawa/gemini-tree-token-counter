# Deployment Protocol for Coding Agent

This document outlines the Standard Operating Procedure (SOP) for releasing `gemini-tree-token-counter`.
Follow these steps sequentially. **Do not skip validation steps.**

## 0. Environment Setup
- Ensure `uv` is installed and valid.
- Ensure `gh` CLI is authenticated.

## 1. Test Verification (CRITICAL)
Before changing any version numbers, ensure the current codebase is stable.

1. **Run Tests**:
   Execute the test suite using `uv`:
   ```bash
   uv run pytest
   ```

2. **Validation**:
   - IF exit code is `0` (PASS): Proceed to Step 2.
   - IF exit code is non-zero (FAIL): **STOP IMMEDIATELY**. Report the errors to the user and request fixes.

## 2. Version Resolution
The version is dynamic. You must determine the correct next version.

1. **Check Current Version**:
   Read `pyproject.toml` to find the current `version`.
   *(Example: `0.1.3`)*

2. **Determine Target Version**:
   - Ask the user: "Current version is `<CURRENT>`. What should be the target version?"
   - OR, if the user already specified a version (e.g., "bump patch"), calculate it (e.g., `0.1.3` -> `0.1.4`).
   - Store this as `<TARGET_VERSION>`.

## 3. Apply Changes
Update the version strings in the codebase.

1. **Update `pyproject.toml`**:
   - Find: `version = "<CURRENT>"`
   - Replace with: `version = "<TARGET_VERSION>"`

2. **Update `src/gemini_tree_token_counter/__init__.py`**:
   - Find: `__version__ = "<CURRENT>"`
   - Replace with: `__version__ = "<TARGET_VERSION>"`

3. **Verify Changes**:
   - Run `git diff` to confirm only the version numbers have changed.

## 4. Release Execution
Push the changes and trigger the release workflow.

1. **Commit**:
   ```bash
   git add pyproject.toml src/gemini_tree_token_counter/__init__.py
   git commit -m "Bump version to <TARGET_VERSION>"
   ```

2. **Push Code**:
   ```bash
   git push origin main
   ```

3. **Create Release (Triggers CI)**:
   Create a GitHub Release. This will create the git tag automatically and trigger the `.github/workflows/publish.yml` action.
   ```bash
   gh release create v<TARGET_VERSION> --generate-notes
   ```

## 5. Post-Release Monitoring
Watch the GitHub Action to ensure PyPI upload succeeds.

```bash
gh run watch
```