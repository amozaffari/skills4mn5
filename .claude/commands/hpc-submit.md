# /hpc-submit — Submit a training job on MareNostrum 5

## Configuration — fill these in before use

- **SSH alias:** `YOUR_MN5_SSH_ALIAS` (e.g. `mn5` — set up in `~/.ssh/config`)
- **Remote repo path:** `YOUR_REMOTE_REPO_PATH` (e.g. `/gpfs/scratch/bsc123/bsc123456/my-repo/`)
- **sbatch script:** `YOUR_SBATCH_SCRIPT` (path relative to repo root, e.g. `scripts/submit.sbatch`)
- **Default config:** `YOUR_DEFAULT_CONFIG` (path to default job config, e.g. `configs/default.json`)
- **Production allocation:** `YOUR_PRODUCTION_ALLOCATION` (e.g. `bsc123` — your BSC project code)
- **EuroHPC allocation:** `YOUR_EHPC_ALLOCATION` (e.g. `ehpc999` — if you have one, otherwise remove)

---

Submit an sbatch job on MareNostrum 5 via SSH.

**Remote host:** `YOUR_MN5_SSH_ALIAS`
**Remote repo:** `YOUR_REMOTE_REPO_PATH`
**sbatch script:** `YOUR_SBATCH_SCRIPT`

Parse `$ARGUMENTS` to determine:

- **allocation** (`-A`): look for the user's project codes in the arguments. Default: `YOUR_PRODUCTION_ALLOCATION`
- **queue** (`-q`): look for `acc_debug`, `acc_bsces`, `acc_ehpc`. Default: `acc_debug`
- **config**: any `.json` filename or path mentioned. Default: `YOUR_DEFAULT_CONFIG`

Available queues on MN5 (ACC partition):

- `acc_debug` — short debug jobs (fast start, limited time/resources)
- `acc_bsces` — BSC production allocation
- `acc_ehpc` — EuroHPC production allocation

Run the following via SSH:

```bash
ssh YOUR_MN5_SSH_ALIAS "cd YOUR_REMOTE_REPO_PATH && sbatch -A <allocation> -q <queue> YOUR_SBATCH_SCRIPT <config>"
```

After submitting, report:

1. The job ID (from `Submitted batch job XXXXXXX`)
2. The full command that was run
3. Where logs will appear (check your sbatch script's `#SBATCH --output` directive)
4. Remind user to run `/hpc-status` to monitor progress
