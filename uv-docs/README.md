# uv Documentation

> Offline documentation for [uv](https://docs.astral.sh/uv/) - An extremely fast Python package and project manager, written in Rust.

This folder contains curated documentation covering **project management**, **dependency handling**, and **GitHub Actions integration** with uv.

---

## Documentation Files

| File | Description |
|------|-------------|
| [uv-projects.md](./uv-projects.md) | Project creation, structure, configuration, and workspaces |
| [uv-dependencies.md](./uv-dependencies.md) | Dependency management, locking, syncing, and resolution |
| [uv-github-actions.md](./uv-github-actions.md) | CI/CD integration with GitHub Actions |

---

## How to Use This Documentation

### Finding Information

**For project setup and structure:**
- Creating new projects → `uv-projects.md` > Creating Projects
- Understanding `pyproject.toml` → `uv-projects.md` > Project Structure and Files
- Configuring entry points/scripts → `uv-projects.md` > Configuring Projects
- Multi-package repositories → `uv-projects.md` > Using Workspaces

**For dependency management:**
- Adding/removing packages → `uv-dependencies.md` > Managing Dependencies
- Git/URL/path dependencies → `uv-dependencies.md` > Dependency Sources
- Development dependencies → `uv-dependencies.md` > Optional and Development Dependencies
- Environment sync issues → `uv-dependencies.md` > Locking and Syncing
- Version conflicts → `uv-dependencies.md` > Resolution

**For CI/CD:**
- Basic GitHub Actions setup → `uv-github-actions.md` > Installation
- Matrix testing → `uv-github-actions.md` > Multiple Python Versions
- Caching strategies → `uv-github-actions.md` > Caching
- Publishing to PyPI → `uv-github-actions.md` > Publishing to PyPI

### Quick Command Reference

```bash
# Project Management
uv init [name]              # Create new project
uv init --lib [name]        # Create library project
uv run [script.py]          # Run in project environment
uv build                    # Build distributions

# Dependencies
uv add <package>            # Add dependency
uv add --dev <package>      # Add dev dependency
uv remove <package>         # Remove dependency
uv sync                     # Sync environment with lockfile
uv lock                     # Update lockfile
uv lock --upgrade           # Upgrade all packages

# Environment
uv python install           # Install Python version from .python-version
uv venv                     # Create virtual environment
```

---

## Installation (summary of official instructions)

- **Unix/macOS**: `curl -LsSf https://astral.sh/uv/install.sh | sh` (installs to `$HOME/.local/bin` by default).
- **Add to PATH (current shell)**: `source $HOME/.local/bin/env` (or add the file to your shell startup).
- **Verify**: `uv --version` and `uvx --version`.
- **Windows**: Use the PowerShell installer from the official guide; see the link below for the latest command.
- Official docs: https://docs.astral.sh/uv/getting-started/installation/

---

## Post uv installation setup

### 1. Check which shell you’re using

In WSL, run:

```bash
echo "$SHELL"
```

* If you see something like `/bin/bash` → use `~/.bashrc`.
* If you see `/usr/bin/zsh` → use `~/.zshrc`.
* If you’re not sure, just follow the bash instructions; that’s most likely what you’re on by default.

---

### 2. Add `$HOME/.local/bin` to PATH (bash)

In WSL, run:

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
```

Now reload your shell config:

```bash
source ~/.bashrc
```

or just close the WSL terminal and open a new one.

Test:

```bash
which uv
uv --version
```

You should see something like `/home/<user>/.local/bin/uv`.

---

### 3. If you’re using zsh instead

Add the line to `~/.zshrc` instead:

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

---

### 4. If it still doesn’t work

A quick diagnostic sequence:

```bash
echo "$PATH"
ls -l "$HOME/.local/bin"
which uv
```

* Confirm `$HOME/.local/bin` appears somewhere in `PATH`.
* Confirm `uv` (and/or `uvx`) actually exist in `$HOME/.local/bin`.
* If they don’t, the installer might have placed them elsewhere; in that case:

```bash
find "$HOME" -maxdepth 5 -type f -name "uv" 2>/dev/null
```

then add **that** directory to PATH in the same way.

---

## Topics NOT Covered in This Documentation

The following topics are **not included** in these offline docs. Visit the official documentation for:

### Tools & Scripts
- **Running scripts**: https://docs.astral.sh/uv/guides/scripts/
- **Using tools (uvx)**: https://docs.astral.sh/uv/guides/tools/
- **Publishing packages**: https://docs.astral.sh/uv/guides/publish/

### Advanced Concepts
- **Python version management**: https://docs.astral.sh/uv/concepts/python-versions/
- **Package indexes**: https://docs.astral.sh/uv/concepts/indexes/
- **Authentication**: https://docs.astral.sh/uv/concepts/authentication/
- **Caching**: https://docs.astral.sh/uv/concepts/cache/
- **The pip interface**: https://docs.astral.sh/uv/concepts/pip/

### Other Integrations
- **Docker**: https://docs.astral.sh/uv/guides/integration/docker/
- **Jupyter**: https://docs.astral.sh/uv/guides/integration/jupyter/
- **Pre-commit**: https://docs.astral.sh/uv/guides/integration/pre-commit/
- **GitLab CI/CD**: https://docs.astral.sh/uv/guides/integration/gitlab/
- **PyTorch**: https://docs.astral.sh/uv/guides/integration/pytorch/
- **FastAPI**: https://docs.astral.sh/uv/guides/integration/fastapi/

### Reference
- **CLI reference**: https://docs.astral.sh/uv/reference/cli/
- **Settings reference**: https://docs.astral.sh/uv/reference/settings/
- **Environment variables**: https://docs.astral.sh/uv/reference/environment/
- **Troubleshooting**: https://docs.astral.sh/uv/reference/troubleshooting/

---

## Official Resources

- **Official Documentation**: https://docs.astral.sh/uv/
- **GitHub Repository**: https://github.com/astral-sh/uv
- **setup-uv Action**: https://github.com/astral-sh/setup-uv
- **Changelog**: https://github.com/astral-sh/uv/blob/main/CHANGELOG.md

---

## Documentation Version

- **Source**: docs.astral.sh/uv
- **Downloaded**: December 2025
- **uv Version Coverage**: 0.9.x

> **Note**: uv is actively developed. For the latest features and changes, always check the official documentation.
