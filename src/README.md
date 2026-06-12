# Source Package

Core Python package for models, optimizers, training, configuration, and
analysis helpers.

Top-level modules:

- `config.py`: YAML loading, inheritance via `extends`, and device selection.
- `trainer.py`: training loop, validation, logging, checkpoints, and routing
  diagnostics.
- `cli.py`: command-line argument parsing for `train.py`.
- `analysis.py`: history/series helpers used by experiment scripts.
- `models/`: GPT model implementation.
- `optimizers/`: optimizer implementations and registry.
- `data/`: tokenized dataset loading.

Run quick validation from the repository root:

```bash
uv run pytest validate_math.py validate_reference.py
```
