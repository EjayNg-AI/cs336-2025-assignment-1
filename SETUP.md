# CS336 Assignment 1: Setup Instructions

## Prerequisites

- **Python 3.11 or higher** (required by `pyproject.toml`)
- **Git** (to clone the repository)
- **wget** and **gunzip** (for downloading datasets)

---

## Step 1: Install uv (Python Package Manager)

This project uses **uv** instead of pip/conda for environment management.

### On Linux/macOS/WSL:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### Alternative methods:

```bash
pip install uv        # via pip
brew install uv       # via Homebrew (macOS)
```

### Add uv to PATH:

After installation, add `~/.local/bin` to your PATH:

```bash
# For bash:
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# For zsh:
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

### Verify installation:

```bash
uv --version
```

---

## Step 2: Clone the Repository (if not already done)

```bash
git clone <repository-url>
cd cs336-2025-assignment-1
```

---

## Step 3: Create Virtual Environment and Install Dependencies

With **uv**, this happens automatically when you run any command. Simply run:

```bash
uv sync
```

This will:
1. Create a virtual environment (in `.venv/`)
2. Install the correct Python version if needed
3. Install all dependencies from `uv.lock`

The environment includes:
- **PyTorch** (~2.6.0, or ~2.2.2 on Intel Macs)
- **NumPy**, **einops**, **einx** (tensor operations)
- **jaxtyping** (type hints)
- **pytest** (testing)
- **regex** (advanced regex)
- **tiktoken**, **tqdm**, **wandb** (ML utilities)

---

## Step 4: Verify Setup by Running Tests

```bash
uv run pytest
```

Initially, **all tests will fail** with `NotImplementedError` - this is expected! The tests are waiting for your implementation.

To run a specific test file:

```bash
uv run pytest tests/test_model.py      # Model component tests
uv run pytest tests/test_tokenizer.py  # Tokenizer tests
uv run pytest tests/test_optimizer.py  # AdamW optimizer tests
```

---

## Step 5: Download Training Data

Create a data directory and download the datasets:

```bash
mkdir -p data
cd data

# TinyStories dataset
wget https://huggingface.co/datasets/roneneldan/TinyStories/resolve/main/TinyStoriesV2-GPT4-train.txt
wget https://huggingface.co/datasets/roneneldan/TinyStories/resolve/main/TinyStoriesV2-GPT4-valid.txt

# OpenWebText sample
wget https://huggingface.co/datasets/stanford-cs336/owt-sample/resolve/main/owt_train.txt.gz
gunzip owt_train.txt.gz
wget https://huggingface.co/datasets/stanford-cs336/owt-sample/resolve/main/owt_valid.txt.gz
gunzip owt_valid.txt.gz

cd ..
```

---

## Step 6: Start Implementing

Your implementation goes in **two places**:

### 1. `cs336_basics/` folder
Create your implementation modules here (e.g., `tokenizer.py`, `model.py`, `optimizer.py`).

### 2. `tests/adapters.py`
This file connects your implementation to the test suite. You need to fill in the stub functions that currently raise `NotImplementedError`.

Key functions to implement in `adapters.py`:

| Category | Functions |
|----------|-----------|
| **Tokenizer** | `run_train_bpe`, `get_tokenizer` |
| **Model** | `run_linear`, `run_embedding`, `run_swiglu`, `run_scaled_dot_product_attention`, `run_multihead_self_attention`, `run_rope`, `run_rmsnorm`, `run_transformer_block`, `run_transformer_lm` |
| **Training** | `run_softmax`, `run_cross_entropy`, `get_adamw_cls`, `run_gradient_clipping`, `run_get_lr_cosine_schedule` |
| **Data** | `run_get_batch` |
| **Checkpoints** | `run_save_checkpoint`, `run_load_checkpoint` |

---

## Common uv Commands

```bash
uv run <script.py>         # Run a Python script
uv run pytest              # Run tests
uv add <package>           # Add a new dependency
uv sync                    # Sync environment with lockfile
uv lock --upgrade          # Upgrade all packages
```

---

## Recommended Workflow

1. Read the full assignment PDF: `cs336_spring2025_assignment1_basics.pdf`
2. Start with the tokenizer (BPE training)
3. Then implement model components (attention, transformer blocks)
4. Then training infrastructure (loss, optimizer, data loading)
5. Run tests frequently: `uv run pytest tests/test_<component>.py`
6. Use the `_snapshots/` folder outputs as reference for expected behavior

---

## Troubleshooting

### uv not found after installation
```bash
# Check if uv exists
ls -l "$HOME/.local/bin/uv"

# Find where it was installed
find "$HOME" -maxdepth 5 -type f -name "uv" 2>/dev/null
```

### Dependency issues
```bash
uv lock --upgrade    # Regenerate lockfile
uv sync --reinstall  # Force reinstall all packages
```

### For more uv help
See the offline documentation in `uv-docs/`:
- `uv-docs/uv-projects.md` - Project configuration
- `uv-docs/uv-dependencies.md` - Dependency management
- `uv-docs/uv-github-actions.md` - CI/CD setup
