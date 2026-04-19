# /hpc-status — Check job status and tail logs on MareNostrum 5

## Configuration — fill these in before use

- **SSH alias:** `YOUR_MN5_SSH_ALIAS` (e.g. `mn5` — set up in `~/.ssh/config`)
- **MN5 username:** `YOUR_MN5_USERNAME` (e.g. `bsc123456`)
- **Remote repo path:** `YOUR_REMOTE_REPO_PATH` (e.g. `/gpfs/scratch/bsc123/bsc123456/my-repo/`)
- **Logs dir:** `YOUR_LOGS_DIR` (path relative to repo root where sbatch writes `.out`/`.err` files, e.g. `logs/`)

---

Check the status of running/recent jobs on MareNostrum 5 and optionally tail log output.

**Remote host:** `YOUR_MN5_SSH_ALIAS`
**Remote repo:** `YOUR_REMOTE_REPO_PATH`
**Logs dir:** `YOUR_LOGS_DIR` (relative to repo root)

Parse `$ARGUMENTS`:

- If a job ID number is provided, show that specific job and tail its log
- If `--all` is provided, show all recent jobs (last 24h via sacct)
- Default: show current queue for the configured user

Run the following steps via SSH:

**Step 1 — Queue status:**

```bash
ssh YOUR_MN5_SSH_ALIAS "squeue -u YOUR_MN5_USERNAME --format='%.18i %.9P %.30j %.8u %.8T %.10M %.6D %R'"
```

**Step 2 — If a specific job ID was given, also run sacct:**

```bash
ssh YOUR_MN5_SSH_ALIAS "sacct -j <jobid> --format=JobID,JobName,State,ExitCode,Elapsed,Start,End"
```

**Step 3 — Tail the most recent (or specified job's) log file:**

```bash
ssh YOUR_MN5_SSH_ALIAS "ls -t YOUR_REMOTE_REPO_PATH/YOUR_LOGS_DIR/*.out 2>/dev/null | head -1 | xargs tail -n 50"
```

If a specific job ID was provided in arguments, find and tail the log matching that job ID instead of the most recent.

Report:

1. Current queue status (formatted table)
2. Last 50 lines of the relevant log file
3. If the job has finished, report final state and exit code
