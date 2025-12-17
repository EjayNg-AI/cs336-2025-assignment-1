# Claude Code Instructions

This file provides context for Claude Code when working in this repository.

## CS336 Spring 2025 Assignment 1: Project Overview

This is **CS336 Spring 2025 Assignment 1: Basics** - an educational assignment for implementing fundamental components of a transformer-based language model from scratch in PyTorch.

### Overview (what this assignment is really doing)

You’re rebuilding a “standard” decoder-only Transformer **Language Model (LM)** training stack from scratch: tokenizer → model → loss/optimizer → training loop → sampling/eval. Concretely, the handout says you will implement: a **Byte-Pair Encoding (BPE)** tokenizer, the Transformer LM, **cross-entropy** loss + **AdamW** optimizer, and a training loop with checkpoint save/load. 
Then you’ll run end-to-end experiments: train tokenizer on **TinyStories**, tokenize the dataset into integer IDs, train the Transformer LM, generate samples + evaluate perplexity, and (optionally) train on **OpenWebText (OWT)** and submit to a leaderboard. 

A key constraint: you’re expected to build “from scratch” and **cannot** use most of `torch.nn`, `torch.nn.functional`, or `torch.optim` (beyond `Parameter`, container modules like `ModuleList`, and the `torch.optim.Optimizer` base class). 

### Systematic breakdown of tasks demanded (deliverables + what you must build)

#### 0) Repo integration + tests (your “outer loop”)

* Clone the starter repo; implement your code under `cs336_basics/*`.
* Fill in `adapters.py` as *glue code only* that calls your implementations.
* Pass the provided `test_*.py` tests (don’t edit tests). 

#### 1) Tokenizer track (BPE)

**1.1 BPE training function**

* Write the function that trains a byte-level BPE tokenizer:

  * inputs: `input_path`, `vocab_size`, `special_tokens`
  * outputs: `vocab: dict[int, bytes]` and `merges: list[tuple[bytes, bytes]]` in creation order. 
* Implement speed/robustness details (special-token splitting; profiling; downscaling/debug subset). 

**1.2 Tokenizer class**

* Implement `Tokenizer` with:

  * `encode`, `decode`, plus `encode_iterable` for streaming tokenization, and a `from_files` constructor. 
    This is not optional busywork: you need streaming (`encode_iterable`) so you can tokenize huge corpora without loading everything at once. 

**1.3 Tokenizer experiments**

* Train + serialize tokenizers:

  * TinyStories vocab 10,000; report time/RAM; longest token; profile bottleneck. 
  * OWT vocab 32,000; report longest token; compare vs TinyStories; note resource budget (≤12h CPU, ≤100GB RAM). 
* Compute compression ratios, throughput estimates, and serialize token-ID arrays (they recommend `uint16` for token IDs). 

#### 2) Transformer LM track (model components)

You implement each building block and wire them up into a full model:

* Core attention + blocks:

  * **scaled dot-product attention** with optional boolean mask and arbitrary leading batch dims. 
  * **causal multi-head self-attention** (causal mask; apply **Rotary Positional Embeddings (RoPE)** to Q/K). 
  * full **Transformer block** and then full **Transformer LM** assembly. 
* Also implement the required “mathy stability” bits (e.g., RMSNorm upcast guidance is explicitly called out). 
* Do resource/FLOPs accounting exercises (parameter counts + FLOPs) for GPT-2 sized configs. 

#### 3) Training track (loss, optimizer, schedules, clipping)

* Implement **cross-entropy** with numerical stability tricks; report perplexity as `exp(mean(loss))`. 
* Implement **AdamW** as a `torch.optim.Optimizer` subclass (stateful moments + decoupled weight decay). 
* Implement learning-rate schedule (cosine with warmup) + gradient clipping. 

#### 4) Training loop + infrastructure

* **Data loader**: sample `(inputs, targets)` batches from a (possibly memmapped) 1D token array; move tensors to device. 
* Use `np.memmap` / `mmap_mode='r'` for datasets too large for RAM. 
* **Checkpointing**: save/load model + optimizer state + iteration. 
* Build a training script that supports hyperparameter config, memmap loading, periodic eval/logging, and checkpoint paths. 

#### 5) Decoding (text generation)

* Implement decoding with:

  * max tokens, temperature scaling, and **top-p (nucleus) sampling**. 

#### 6) Experiments (the “science” part)

* Add experiment tracking + learning curves against steps and wallclock time; keep an experiment log. 
* TinyStories baseline model guidance: ~17M non-embedding params (4 layers, 16 heads), target total tokens 327,680,000; they claim ~30–40 minutes on 1×H100 if your code is efficient. 
* Required sweeps/ablations:

  * learning-rate sweep (budgeted “4 H100 hrs”) targeting TinyStories val loss ≤ 1.45. 
  * batch-size sweep (budgeted “2 H100 hrs”). 
  * generate 256+ tokens from your trained model; discuss quality + factors. 
  * ablations: remove RMSNorm, switch pre-norm↔post-norm, remove positional info (NoPE), SwiGLU vs SiLU. 
* OWT run + leaderboard are explicitly optional-ish for “online students,” but included:

  * Train on OWT and compare losses + output quality. 
  * Leaderboard run: ≤1.5 hours on an H100; submit PR; beat naive baseline loss 5.0. 

### Coding skills you’ll need (mapped to the assignment)

#### Core Python engineering

* Clean module/class design (`Tokenizer`, custom `nn.Module`s, custom `Optimizer`).
* Serialization/IO (save vocab/merges, token arrays, checkpoints).
* Iterators/generators (streaming tokenization via `encode_iterable`). 
* Multiprocessing + chunking for tokenizer training speed. 

#### PyTorch fundamentals (but “low-level”)

* Writing `torch.nn.Module` with `torch.nn.Parameter` (without using `nn.Linear`, `nn.Embedding`, etc.). 
* Tensor shape discipline + broadcasting over “batch-like” leading dims (attention explicitly demands this). 
* Device/dtype correctness (cpu/cuda/mps; fp16/bf16/fp32).
* `state_dict()` / `load_state_dict()` usage for checkpoints and loading test weights. 

#### Numerical stability + performance

* Stable softmax / stable cross-entropy. 
* Norm layers and precision pitfalls (they explicitly advise upcasting to float32 inside RMSNorm). 
* Profiling and optimization; caching where needed (BPE merge step) and avoiding Python loops in hot paths. 

#### ML experimentation hygiene

* Hyperparameter sweeps + logging learning curves + tracking wallclock time. 
* Debugging tensor shapes and “overfit 1 minibatch” sanity checks. 

---

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
