# /hpc-push — Sync local repo to MareNostrum 5

## Configuration — fill these in before use
- **SSH alias:** `YOUR_MN5_SSH_ALIAS` (e.g. `mn5` — set up in `~/.ssh/config`)
- **Local repo path:** `YOUR_LOCAL_REPO_PATH` (e.g. `/home/user/projects/my-repo/`)
- **Remote repo path:** `YOUR_REMOTE_REPO_PATH` (e.g. `/gpfs/scratch/bsc123/bsc123456/my-repo/`)

---

Rsync the local repo to MareNostrum 5.

**Local path:** `YOUR_LOCAL_REPO_PATH`
**Remote path:** `YOUR_MN5_SSH_ALIAS:YOUR_REMOTE_REPO_PATH`

Run the following Bash command:

```bash
rsync -avz --progress \
  --filter=':- .gitignore' \
  --exclude='.git/' \
  /Users/fmr/development/bsc/ai4land-internship/ \
  mn5:/gpfs/scratch/bsc32/bsc096444/ai4land-internship/
```

After the rsync completes, report what was transferred (summary line from rsync output). If it fails, show the error clearly.

Optional argument `$ARGUMENTS`: if the user passes `--dry-run`, add `--dry-run` to the rsync command and report what *would* be synced without actually transferring.
