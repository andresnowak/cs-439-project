# Data

Dataset preparation scripts live here. Generated token files are intentionally
ignored by git.

Prepare WikiText-103 for local smoke tests:

```bash
uv run python data/prepare_wikitext.py
```

Prepare FineWeb shards for the 124M-scale runs:

```bash
uv run python data/prepare_fineweb.py 500M
```

The training code expects tokenized `.bin` files under paths configured in
`configs/base.yaml` and `configs/gpt124m.yaml`. On the cluster, `scripts/run_job.sh
prep` and `scripts/run_job.sh prep-fineweb 500M` submit these preparation jobs
through RunAI.
