# Source Data Utilities

Runtime loaders for tokenized `.bin` datasets.

- `cached_tokens.py`: memory-mapped uint16 token arrays, random/fixed spans, and
  deterministic validation offsets.

Dataset preparation lives in the top-level `data/` folder; this package only
loads already prepared token files during training and validation.
