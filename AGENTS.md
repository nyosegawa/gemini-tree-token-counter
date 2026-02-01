# AGENTS.md

## Project Overview
**gemini-tree-token-counter (gtc)** is a CLI tool to estimate tokens for Gemini models.
It visualizes file trees with token counts and supports "Time Travel" (analyzing specific Git commits/dates).

- **Core Logic**: Regex-based tokenization (ported from `smartprocure/gemini-token-estimator`).
- **Goal**: Fast estimation without API calls.
- **Languages**: Python 3.7+.

## Build & Installation
- **Dependency Management**: This project aims for **Zero Dependencies** (standard library only) for runtime.
- **Installation**:
  ```bash
  pip install -e .
  ```
- **Build System**: Uses `pyproject.toml` (setuptools).

## Testing
- **Test Runner**: `pytest`
- **Command**:
  ```bash
  pytest
  ```
- **Critical Tests**: `tests/test_tokenizer.py` contains vital test cases for multilingual support (French, Spanish, German, Ukrainian, etc.) and edge cases. **Always run these** after touching `tokenize` logic.

## Code Style & Conventions
- **Python Version**: Must remain compatible with Python 3.7+. Avoid features introduced in newer Python versions (e.g., `match/case`, `|` for union types in hints) unless using `from __future__`.
- **Type Hinting**: Use standard `typing` module (e.g., `List`, `Dict`, `Optional`) for 3.7 compatibility.
- **Docstrings**: Maintain docstrings for core functions in `main.py`.
- **Imports**: Avoid introducing external PyPI dependencies unless absolutely necessary.

## Key Architecture Notes
1.  **Tokenizer (`main.py`)**:
    - Strictly regex-based.
    - Do **not** replace with an API call to Google Vertex AI or Gemini API. The tool must work offline.
    - Logic is derived from GovSpend's ISC licensed code. Preserve the license notice in `main.py` headers.

2.  **Git Context (`GitContext` class)**:
    - Uses `subprocess` to call the system `git` CLI.
    - Clones/Checkouts to a temporary directory (`tempfile.mkdtemp`).
    - Cleans up automatically via Context Manager (`__enter__`, `__exit__`).

3.  **Tree Traversal**:
    - Respects `.gitignore` patterns.
    - Recursive `Node` structure used to aggregate token counts.

## Documentation & Release
- **Versioning**:
    - Update version in `src/gemini_tree_token_counter/__init__.py`.
    - Update version in `pyproject.toml`.
- **Multilingual Docs**:
    - `README.md` (English)
    - `README_ja.md` (Japanese)
    - **Rule**: When updating documentation, changes must be reflected in **both** English and Japanese files.

## Workflow Instructions for Agents
- When asked to "analyze this repo", assume the user wants to run `gtc .` logic conceptually.
- If modifying the tokenizer regex, add a corresponding test case in `tests/test_tokenizer.py` to ensure no regression in token counts.
- If adding features that require external libraries, stop and ask the user for confirmation first (preference is to stay dependency-free).