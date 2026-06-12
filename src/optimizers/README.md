# Optimizers

Optimizer implementations used in the project.

- `muon.py`: Muon baseline and exact/SVD polar variants.
- `dynmuon.py`: DynMuon spectral exponent schedules, routing math, and
  Newton-Schulz/SVD compute paths.
- `homogeneous_muon.py`: fixed exponent Homogeneous Muon.
- `relmuon.py`: RelMuon update scaling variants.
- `kaon.py`: Kaon-style spectral control.
- `param_groups.py`: GPT parameter grouping utilities.
- `registry.py`: maps `matrix_optimizer` config values to optimizer instances.

The registry is the main integration point used by `src/trainer.py`; add new
optimizers there only after adding a config and validation coverage.
