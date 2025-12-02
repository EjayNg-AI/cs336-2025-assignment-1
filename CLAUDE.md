# Claude Code Instructions

This file provides context for Claude Code when working in this repository.

## Project Overview

This is **CS336 Spring 2025 Assignment 1: Basics** - an educational assignment for implementing fundamental components of a transformer-based language model from scratch in PyTorch.

## uv Documentation

This repository includes comprehensive offline documentation for **uv** (the Python package manager) in the `uv-docs/` folder:

| File | Content |
|------|---------|
| `uv-docs/README.md` | Master index and usage instructions |
| `uv-docs/uv-projects.md` | Project creation, structure, configuration, workspaces, building |
| `uv-docs/uv-dependencies.md` | Dependencies, sources, locking, syncing, resolution, exporting |
| `uv-docs/uv-github-actions.md` | GitHub Actions CI/CD integration |

### When to Use This Documentation

**Always refer to the `uv-docs/` folder when the user asks about:**
- How to use uv commands (`uv add`, `uv sync`, `uv run`, `uv build`, etc.)
- Managing dependencies in this project
- Understanding `pyproject.toml` configuration
- Lockfile management (`uv.lock`)
- Setting up GitHub Actions for this project
- Workspace configuration
- Dependency resolution issues

### How to Use

1. Read the relevant file from `uv-docs/` to answer uv-related questions
2. The documentation includes code examples and command references
3. For topics not covered in `uv-docs/`, refer the user to https://docs.astral.sh/uv/

## Key Project Files

- `pyproject.toml` - Project configuration and dependencies
- `tests/adapters.py` - Main implementation interface (where student code goes)
- `tests/test_*.py` - Test suites for each component
- `tests/fixtures/` - Test data and reference files

## Common Commands

```bash
uv run pytest              # Run all tests
uv run pytest tests/test_model.py  # Run specific test file
uv add <package>           # Add a dependency
uv sync                    # Sync environment with lockfile
uv run <script.py>         # Run a Python script
```
