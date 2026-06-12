# Experiments

Experiment utilities, W&B cache tooling, and report-figure generation.

Main offline report workflow:

```bash
uv run python run.py
```

That command restores `experiments/report_wandb_bundle.jsonl.gz` into
`results/report_wandb_cache/` and regenerates the report figures in
`report/figures/` without requiring W&B access.

Useful scripts:

- `report_figures.py`: builds the report and appendix figures from cached run
  summaries/histories.
- `pull_wandb.py`: maintainer utility for refreshing and rebundling W&B runs.
- `lr_bowl.py`: standalone LR-bowl plotter for W&B sweep projects.
- `baselines_step_efficiency.py`: local baseline comparison utility.
- `cost_table.py`: computes step/time-to-target summaries from result dumps.
- `depth_routing.py`: analyzes depth-resolved routing histories.
- `probe_proxies.py`: inspects routing-proxy distributions for calibration.
- `spatial_ablation.py`: restricts RelMuon-style updates to selected parameter
  groups.

Refresh the offline bundle only when intentionally updating cached experiment
data:

```bash
uv run python run.py --refresh-wandb
```
