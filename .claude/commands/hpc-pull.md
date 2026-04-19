# /hpc-pull — Sync results from MareNostrum 5 back to local

## Configuration — fill these in before use

- **SSH alias:** `YOUR_MN5_SSH_ALIAS` (e.g. `mn5` — set up in `~/.ssh/config`)
- **Remote repo path:** `YOUR_REMOTE_REPO_PATH` (e.g. `/gpfs/scratch/bsc123/bsc123456/my-repo/`)
- **Local repo path:** `YOUR_LOCAL_REPO_PATH` (e.g. `/home/user/projects/my-repo/`)
- **Remote logs dir:** `YOUR_REMOTE_LOGS_DIR` (relative to remote repo root, e.g. `logs/`)
- **Local logs dir:** `YOUR_LOCAL_LOGS_DIR` (relative to local repo root, e.g. `logs/`)
- **Remote runs dir:** `YOUR_REMOTE_RUNS_DIR` (relative to remote repo root, e.g. `runs/`)
- **Local runs dir:** `YOUR_LOCAL_RUNS_DIR` (relative to local repo root, e.g. `runs/`)
- **Remote wandb dir:** `YOUR_REMOTE_WANDB_DIR` (relative to remote repo root, e.g. `wandb/`)
- **Local wandb dir:** `YOUR_LOCAL_WANDB_DIR` (relative to local repo root, e.g. `wandb/`)

---

Rsync results (logs, checkpoints, wandb runs) from MareNostrum 5 back to the local machine.

**Remote host:** `YOUR_MN5_SSH_ALIAS`
**Remote repo:** `YOUR_REMOTE_REPO_PATH`
**Local repo:** `YOUR_LOCAL_REPO_PATH`

Parse `$ARGUMENTS` to determine what to pull:

- `logs` or default (no args): pull only logs
- `runs`: pull only runs/ (checkpoints — can be large, warn user)
- `wandb`: pull only wandb/ offline runs
- `all`: pull logs + runs + wandb

**Pull logs (always included):**

```bash
rsync -avz --progress \
  YOUR_MN5_SSH_ALIAS:YOUR_REMOTE_REPO_PATH/YOUR_REMOTE_LOGS_DIR \
  YOUR_LOCAL_REPO_PATH/YOUR_LOCAL_LOGS_DIR
```

**Pull runs/ (checkpoints, only if requested):**

```bash
rsync -avz --progress \
  YOUR_MN5_SSH_ALIAS:YOUR_REMOTE_REPO_PATH/YOUR_REMOTE_RUNS_DIR \
  YOUR_LOCAL_REPO_PATH/YOUR_LOCAL_RUNS_DIR
```

**Pull wandb/ offline runs (only if requested):**

```bash
rsync -avz --progress \
  YOUR_MN5_SSH_ALIAS:YOUR_REMOTE_REPO_PATH/YOUR_REMOTE_WANDB_DIR \
  YOUR_LOCAL_REPO_PATH/YOUR_LOCAL_WANDB_DIR
```

After pulling, report:

1. What was synced and total data transferred
2. If logs were pulled, show the last few lines of the most recent `.out` log
3. If wandb was pulled, remind user to run `wandb sync --sync-all ./wandb` from the local wandb dir to upload to W&B
