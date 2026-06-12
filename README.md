# CS-439 Optimization Project

This repository contains the code, experiment utilities, and cached run metadata
for our CS-439 project on spectral-shaping variants of Muon for language-model
training.

The project compares Muon-style hidden-layer optimizers through a common GPT
training setup. The main question is how changing the singular-value map of the
matrix update affects validation loss, learning-rate sensitivity, step cost, and
late-stage training behavior.

## What Is Included

Implemented optimizers and ablations:

- AdamW baseline.
- Muon with Newton-Schulz polar updates.
- Exact-polar Muon using SVD.
- DynMuon global spectral-exponent schedule.
- DynMuon-Route layer-wise routing variants.
- Homogeneous Muon with fixed power `p`.
- RelMuon scale variants.
- Kaon-style spectral controls.

The repository also includes an offline W&B cache bundle:

```text
experiments/report_wandb_bundle.jsonl.gz
```

This bundle is enough to regenerate the report figures without W&B credentials.

## Quickstart

Install dependencies:

```bash
uv sync
```

Run validation checks:

```bash
uv run pytest validate_math.py validate_reference.py
```

Prepare small/local data:

```bash
uv run python data/prepare_wikitext.py
```

Run a smoke training job:

```bash
uv run python train.py --config configs/small.yaml --train-steps 50
```

Run a single method:

```bash
uv run python train.py --config configs/route.yaml
uv run python train.py --config configs/muon.yaml
uv run python train.py --config configs/homogeneous_muon.yaml
```

## Reproducing Figures

Regenerate all report figures from the bundled offline cache:

```bash
uv run python run.py
```

This restores cached W&B summaries/histories into `results/report_wandb_cache/`
and writes figures to `report/figures/`.

Generate figures from an already restored cache:

```bash
REPORT_WANDB_DIR=results/report_wandb_cache uv run python experiments/report_figures.py all
```

Generate individual figures:

```bash
REPORT_WANDB_DIR=results/report_wandb_cache uv run python experiments/report_figures.py bowls
REPORT_WANDB_DIR=results/report_wandb_cache uv run python experiments/report_figures.py homogeneous_p_lr_bowl
REPORT_WANDB_DIR=results/report_wandb_cache uv run python experiments/report_figures.py ns_power_map
REPORT_WANDB_DIR=results/report_wandb_cache uv run python experiments/report_figures.py fixed_power_map
```

Refresh the W&B cache only when intentionally updating the archived run data:

```bash
uv run python run.py --refresh-wandb
```

## Training Sweeps

The report training runs are submitted through the grouped sweep launcher:

```bash
scripts/sweeps.sh bowls
scripts/sweeps.sh route 0.02
scripts/sweeps.sh final 0.02
```

Use dry-run mode before submitting:

```bash
DRY_RUN=1 scripts/sweeps.sh bowls
DRY_RUN=1 scripts/sweeps.sh route 0.02
DRY_RUN=1 scripts/sweeps.sh final 0.02
```

For one-off RunAI jobs:

```bash
scripts/run_job.sh single --config configs/route.yaml --wandb
scripts/run_job.sh baselines --train-steps 20000
```

## RunAI / RCP Workflow

The cluster workflow is:

1. Sync the local checkout to the RunAI submit host.
2. Submit jobs from the synced checkout.
3. Inspect logs or list/delete jobs with `run_job.sh`.

```bash
scripts/sync_to_rcp.sh
ssh <submit-host>
cd ~/developer/cs-439-project
scripts/run_job.sh prep-fineweb 500M
scripts/run_job.sh sanity
scripts/run_job.sh single --config configs/route.yaml --wandb
```

Useful environment overrides:

```bash
REMOTE_HOST=myhost REMOTE_USER=me scripts/sync_to_rcp.sh
IMAGE=ic-registry.epfl.ch/mlo/mlo-base:uv1 GPUS=1 scripts/run_job.sh sanity
NODE_POOLS=h100 scripts/run_job.sh single --config configs/muon.yaml --wandb
UV_SYNC=0 scripts/run_job.sh single --config configs/small.yaml --train-steps 20
```

`run_job.sh` requires external cluster access and tools (`runai`, SSH/PVC access,
and any W&B/HF tokens supplied by the user environment). No API keys are stored in
this repository.

## Repository Layout

```text
configs/       YAML configs for methods and ablations
data/          dataset preparation scripts
experiments/   figure generation, W&B cache tooling, and analysis utilities
scripts/       RunAI/RCP submission and grouped sweep scripts
src/           model, optimizer, trainer, config, and data-loading code
run.py         offline report figure reproduction entry point
train.py       training CLI entry point
```

Each main folder has its own `README.md` with more specific notes.

## Configs

Common configs:

| config | method |
| --- | --- |
| `configs/adamw.yaml` | AdamW baseline |
| `configs/muon.yaml` | Muon baseline |
| `configs/muon_svd.yaml` | exact-polar Muon |
| `configs/dynmuon.yaml` | DynMuon global schedule |
| `configs/route.yaml` | DynMuon-Route |
| `configs/route_decoupled.yaml` | route with decoupled update magnitude |
| `configs/homogeneous_muon.yaml` | Homogeneous Muon fixed `p` |
| `configs/kaon.yaml` | Kaon-style spectral control |
| `configs/relmuon_*.yaml` | RelMuon variants |

Most configs extend `configs/base.yaml`; the main 124M-scale settings live in
`configs/gpt124m.yaml`.

## Reproduction Notes

- `uv run python run.py` should work offline from the bundled cache.
- Generated `results/` files are ignored by git.
- `report/figures/` is intentionally not ignored, so regenerated figures can be
  tracked if needed.
- This is a clean project repository.

## References

```bibtex
@misc{jordan2024muon,
  author = {Keller Jordan and others},
  title = {Muon: An Optimizer for Hidden Layers in Neural Networks},
  year = {2024},
  url = {https://kellerjordan.github.io/posts/muon/}
}

@misc{bernstein2025derivingmuon,
  author = {Jeremy Bernstein},
  title = {Deriving Muon},
  year = {2025},
  url = {https://jeremybernste.in/writing/deriving-muon}
}

@misc{wu2026dynmuondynamicspectralshaping,
  title = {DynMuon: A Dynamic Spectral Shaping View of Muon},
  author = {Fangzhou Wu and Rikhav Shah and Sandeep Silwal and Qiuyi Zhang},
  year = {2026},
  eprint = {2605.17109},
  archivePrefix = {arXiv},
  primaryClass = {cs.LG},
  url = {https://arxiv.org/abs/2605.17109}
}
```
