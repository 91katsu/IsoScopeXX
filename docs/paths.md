# Path Assembly Reference

All paths are assembled from `cfg/env.json` (per-machine paths) and `cfg/{experiment}.yaml` (training parameters).

## Data Paths (Input)

```
$DATASET
└── {dataset}/
    ├── train/
    │   ├── {dir0}/
    │   └── {dir1}/          (if paired)
    └── val/
        ├── {dir0}/
        └── {dir1}/
```

`direction` is split by `_` to support paired directories:
- `x3d0` — single: `train/x3d0/`
- `x3d0_x3d1` — paired: `train/x3d0/` + `train/x3d1/`

Only `direction` uses `_` for splitting. Other parameters (`dataset`, `prj`, etc.) are used as-is.

## Log Paths (Output)

```
$LOGS
└── {dataset}/
    └── {prj}/
        ├── logs/
        │   ├── TensorBoardLogger/{run_timestamp}/
        │   ├── mlflow.db               (server mode only)
        │   ├── mlartifacts/            (server mode only)
        │   └── MLFlowLogger/           (file-based fallback only)
        └── checkpoints/
            └── {run_timestamp}/
                ├── config.json
                ├── {yaml_name}.yaml
                ├── {models}.py
                ├── {netg}_model_epoch_{N}.pth
                └── {netd}_model_epoch_{N}.pth   (if save_d)
```

`{run_timestamp}` = `YYYYMMDD_HHMMSS`, shared across TensorBoard, MLflow, and checkpoints.

**MLflow modes:** When an MLflow tracking server is running (see README), metadata is stored in `mlflow.db` (SQLite) and artifacts in `mlartifacts/`. When no server is available, MLflow falls back to file-based tracking under `MLFlowLogger/`.
