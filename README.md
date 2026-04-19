# Claude Code Skills for MareNostrum 5

Claude Code slash-command skills for developing locally and running jobs on [MareNostrum 5](https://www.bsc.es/marenostrum/marenostrum-5) (BSC) without leaving your editor.

## Skills

| Command | Description |
|---|---|
| `/hpc-setup` | Interactive setup — fill in all config values once |
| `/hpc-push` | Rsync local repo → MN5 (excludes large data files) |
| `/hpc-submit` | Submit an sbatch job with configurable allocation/queue/config |
| `/hpc-status` | Check job queue, sacct history, and tail logs |
| `/hpc-pull` | Rsync logs, checkpoints, and wandb runs back to local |

## Setup

### 1. SSH config

Add an entry to `~/.ssh/config` so the skills can reach MN5 without a password prompt:

```
Host mn5
    HostName mn5.bsc.es
    User YOUR_MN5_USERNAME
    IdentityFile ~/.ssh/id_ed25519
    ServerAliveInterval 60
```

> If you use a jump host or a different login node alias, adjust accordingly. The skills use whatever alias you configure as `YOUR_MN5_SSH_ALIAS`.

### 2. Copy the skills into your project

```bash
# From the root of your project repo
mkdir -p .claude/commands
cp /path/to/skills4mn5/.claude/commands/hpc-*.md .claude/commands/
```

### 3. Run `/hpc-setup`

Open Claude Code in your project and run:

```text
/hpc-setup
```

Claude will ask for each required value one at a time (SSH alias, username, local/remote paths, allocations, sbatch script, etc.), then automatically replace all placeholders across every skill file and test your SSH connection.

> **Manual alternative:** if you prefer to edit files directly, each skill has a `## Configuration` section at the top listing its placeholders. The full placeholder reference is in the table below.

### Placeholder reference (manual setup)

| Placeholder | What to put |
|---|---|
| `YOUR_MN5_SSH_ALIAS` | SSH alias from `~/.ssh/config` (e.g. `mn5`) |
| `YOUR_MN5_USERNAME` | Your MN5 username (e.g. `bsc123456`) |
| `YOUR_LOCAL_REPO_PATH` | Absolute path to your local repo (trailing `/`) |
| `YOUR_REMOTE_REPO_PATH` | Absolute path on MN5 scratch (trailing `/`) |
| `YOUR_PRODUCTION_ALLOCATION` | Your BSC project code (e.g. `bsc123`) |
| `YOUR_EHPC_ALLOCATION` | Your EuroHPC allocation, if any (e.g. `ehpc999`) |
| `YOUR_SBATCH_SCRIPT` | Path to your sbatch script (relative to repo root) |
| `YOUR_DEFAULT_CONFIG` | Default job config passed to sbatch (relative path) |
| `YOUR_LOGS_DIR` | Directory where sbatch writes `.out`/`.err` files |
| `YOUR_REMOTE_LOGS_DIR` / `YOUR_LOCAL_LOGS_DIR` | Log dirs for pull (can differ from each other) |
| `YOUR_REMOTE_RUNS_DIR` / `YOUR_LOCAL_RUNS_DIR` | Checkpoint dirs for pull |
| `YOUR_REMOTE_WANDB_DIR` / `YOUR_LOCAL_WANDB_DIR` | W&B offline run dirs for pull |

### 4. Use them

Once configured, the skills appear as slash commands inside Claude Code:

```
/hpc-push                          # sync code to MN5
/hpc-push --dry-run                # preview what would be synced
/hpc-submit                        # submit with defaults
/hpc-submit ehpc999 acc_ehpc       # submit to EuroHPC queue
/hpc-status                        # check queue + tail latest log
/hpc-status 12345678               # tail log for specific job
/hpc-pull                          # pull logs (default)
/hpc-pull all                      # pull logs + checkpoints + wandb
```

## MN5 ACC queue reference

| Queue | Use case |
|---|---|
| `acc_debug` | Short interactive/debug jobs (fast start, limited resources) |
| `acc_bsces` | BSC production allocation |
| `acc_ehpc` | EuroHPC production allocation |

## Notes

- The push skill excludes large binary files (`*.nc`, `*.zarr`, `*.npy`, `*.pt`, `*.ckpt`) by default. Edit `hpc-push.md` to adjust exclusions for your project.
- The pull skill warns before pulling `runs/` (checkpoints can be many GB).
- If you use W&B in offline mode on MN5, `hpc-pull wandb` + `wandb sync` is the recommended workflow.
