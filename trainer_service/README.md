# SaturnOS Trainer Service

This service runs YOLOv8 training jobs on a Linux GPU server by polling
Supabase `training_runs` rows with `status=queued`.

## Requirements

- Python 3.11+
- CUDA-capable GPU (optional, but recommended)
- Access to local datasets under `/mnt/d/datasets`

## Setup

1) Create a virtual environment and install deps:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

2) Configure environment variables (see `.env.example`):

```bash
export SUPABASE_URL="https://YOUR_PROJECT.supabase.co"
export SUPABASE_SERVICE_ROLE_KEY="YOUR_SERVICE_ROLE_KEY"
export SUPABASE_STORAGE_BUCKET="sops"
export DATASET_ROOT="/mnt/d/datasets"
export RUNS_DIR="/mnt/d/datasets/runs"
```

3) Run the service:

```bash
python trainer_service.py
```

## Dataset YAML

When creating a training run from the UI, set **Dataset YAML Path (Server)**
to the absolute path on the training server, for example:

```
/mnt/d/datasets/my_project/data.yaml
```

## Outputs

The service uploads artifacts to Supabase Storage under:

```
training_runs/<run_id>/
```

It updates `training_runs` with:
- `status`, `started_at`, `completed_at`, `error_message`
- `trained_model_url`
- `results` (metrics + artifact URLs)
