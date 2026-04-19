# /hpc-setup — Configure HPC skills for MareNostrum 5

Walk the user through filling in all `YOUR_*` placeholders across the four HPC skill files.

---

## Steps

### Step 1 — Check current state

Scan the following files for any remaining `YOUR_*` placeholders and report which ones still need values:

- `.claude/commands/hpc-push.md`
- `.claude/commands/hpc-submit.md`
- `.claude/commands/hpc-status.md`
- `.claude/commands/hpc-pull.md`

If no placeholders remain, tell the user the skills are already configured and exit.

### Step 2 — Collect values interactively

Ask the user for each value **one at a time**, in this order. Only ask for values that are still placeholders (skip any already filled in). Show a short explanation with each question.

1. **SSH alias** (`YOUR_MN5_SSH_ALIAS`)
   > The alias you use in `~/.ssh/config` to connect to MN5 (e.g. `mn5`). If you haven't set one up yet, see the README for a template.

2. **MN5 username** (`YOUR_MN5_USERNAME`)
   > Your BSC username (e.g. `bsc123456`). Used in `squeue` to filter your jobs.

3. **Local repo path** (`YOUR_LOCAL_REPO_PATH`)
   > Absolute path to your project on this machine, with a trailing slash (e.g. `/home/user/projects/my-repo/`).

4. **Remote repo path** (`YOUR_REMOTE_REPO_PATH`)
   > Absolute path to your project on MN5 scratch, with a trailing slash (e.g. `/gpfs/scratch/bsc123/bsc123456/my-repo/`).

5. **Production allocation** (`YOUR_PRODUCTION_ALLOCATION`)
   > Your BSC project code used with `sbatch -A` (e.g. `bsc123`).

6. **EuroHPC allocation** (`YOUR_EHPC_ALLOCATION`)
   > Your EuroHPC allocation code, if you have one (e.g. `ehpc999`). Type `none` to skip — the placeholder will be removed from the submit skill.

7. **sbatch script** (`YOUR_SBATCH_SCRIPT`)
   > Path to your sbatch script, relative to the repo root (e.g. `scripts/submit.sbatch`).

8. **Default config** (`YOUR_DEFAULT_CONFIG`)
   > Default argument passed to the sbatch script (e.g. `configs/default.json`). Can be empty if your script needs no config argument — type `none` to leave it blank.

9. **Logs directory** (`YOUR_LOGS_DIR`, `YOUR_REMOTE_LOGS_DIR`, `YOUR_LOCAL_LOGS_DIR`)
   > Directory where sbatch writes `.out`/`.err` files, relative to repo root (e.g. `logs/`). Used for both local and remote unless you specify different paths.

10. **Runs/checkpoints directory** (`YOUR_REMOTE_RUNS_DIR`, `YOUR_LOCAL_RUNS_DIR`)
    > Directory for model checkpoints, relative to repo root (e.g. `runs/`). Used for `/hpc-pull runs`.

11. **W&B directory** (`YOUR_REMOTE_WANDB_DIR`, `YOUR_LOCAL_WANDB_DIR`)
    > Directory for wandb offline runs, relative to repo root (e.g. `wandb/`). Type `none` if you don't use W&B.

### Step 3 — Apply replacements

For each value collected, use the Edit tool to replace all occurrences of the placeholder string across all four skill files.

Special cases:
- If the user typed `none` for `YOUR_EHPC_ALLOCATION`: remove the EuroHPC bullet line from `hpc-submit.md` entirely.
- If the user typed `none` for `YOUR_DEFAULT_CONFIG`: replace with an empty string in the sbatch command.
- If the user typed `none` for the wandb dir: remove the wandb-related blocks from `hpc-pull.md`.
- For logs/runs/wandb dirs: if the user gave a single path for both local and remote, use it for both `YOUR_REMOTE_*` and `YOUR_LOCAL_*` variants.

Also update `.claude/settings.json`: replace `YOUR_MN5_SSH_ALIAS` with the actual SSH alias the user provided.

### Step 4 — Verify SSH connectivity

After all replacements are done, test the SSH connection:

```bash
ssh -o ConnectTimeout=10 <ssh_alias> "echo connected && hostname"
```

Report whether the connection succeeded. If it failed, remind the user to check their `~/.ssh/config` and that they can reach MN5 from this machine.

### Step 5 — Summary

Show a table of every value that was set, then list the commands they can now use:

- `/hpc-push` — sync code to MN5
- `/hpc-submit` — submit a job
- `/hpc-status` — check queue and tail logs
- `/hpc-pull` — bring results back
