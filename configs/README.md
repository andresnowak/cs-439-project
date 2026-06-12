# Configs

YAML configuration files for training and ablation runs. Most configs extend
`base.yaml`, while `gpt124m.yaml` sets the main 124M-model training scale.

Common entry points:

```bash
uv run python train.py --config configs/small.yaml --train-steps 50
uv run python train.py --config configs/route.yaml --wandb
```

Core methods:

- `adamw.yaml`: AdamW baseline.
- `muon.yaml`: Muon baseline using Newton-Schulz polar updates.
- `muon_svd.yaml`: exact SVD polar Muon.
- `dynmuon.yaml`: DynMuon global spectral-exponent schedule.
- `route.yaml`: DynMuon-Route layer-wise modulation.
- `homogeneous_muon.yaml`: fixed exponent Homogeneous Muon.
- `kaon.yaml`: Kaon-style spectral control.
- `relmuon_*.yaml`: RelMuon variants.

The shell sweep scripts in `scripts/` mostly pass CLI overrides on top of these
configs, so changes here affect both local and RunAI-submitted runs.
