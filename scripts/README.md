# Scripts

Shell wrappers for local sweeps, cluster submission, and RunAI/RCP workflow.

RunAI flow:

1. Sync this repo to the RunAI submit host.
2. Submit jobs from the synced checkout.
3. Inspect logs or list/delete jobs through `run_job.sh`.

```bash
scripts/sync_to_rcp.sh
ssh <submit-host>
cd ~/developer/cs-439-project
scripts/run_job.sh prep-fineweb 500M
scripts/run_job.sh sanity
scripts/run_job.sh single --config configs/route.yaml --wandb
```

Important scripts:

- `sync_to_rcp.sh`: rsyncs the repo to the submit host and makes cluster scripts
  executable. Override `REMOTE_HOST`, `REMOTE_USER`, and `REMOTE_DIR` as needed.
- `run_job.sh`: RunAI submission wrapper. It mounts the home PVC, forwards W&B/HF
  environment variables when present, and runs Python through `container_entry.sh`.
- `container_entry.sh`: pod entrypoint; changes into the project, sets
  `PYTHONPATH`, syncs uv dependencies, then executes the requested script.
- `sweeps.sh`: grouped report sweep submissions (`bowls`, `route`, `final`, etc.).

Dry-run sweep commands before submitting:

```bash
DRY_RUN=1 scripts/sweeps.sh bowls
DRY_RUN=1 scripts/sweeps.sh route 0.02
DRY_RUN=1 scripts/sweeps.sh final 0.02
```

`run_job.sh` requires the external cluster tools and permissions: `runai`, access
to the submit host/PVC, and any W&B/HF tokens supplied through the user
environment. No API keys are stored in this repository.
