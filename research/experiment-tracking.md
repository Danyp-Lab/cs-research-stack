# 📊 Experiment Tracking

> Running experiments without systematic tracking is like navigating without a map. You cannot reproduce your best result, compare approaches, or explain to your advisor what you tried last week.

---

## 1. Why Experiment Tracking Matters

Without tracking:
- You run the same failed experiment twice because you forgot you tried it.
- Your best model checkpoint is from an unnamed run six weeks ago and you cannot reproduce it.
- Paper writing takes twice as long because you cannot find the exact hyperparameters used.

With proper tracking:
- Every experiment is logged, tagged, and searchable.
- Best results are reproducible from the exact config + seed + checkpoint.
- You can compare 50 runs in a visualization dashboard in 2 minutes.

---

## 2. Tool Comparison

| Tool | Hosting | Free Tier | Best For |
| :--- | :--- | :--- | :--- |
| **[Weights & Biases (W&B)](https://wandb.ai/)** | Cloud | Generous (unlimited projects, 100GB storage) | ML experiments, real-time dashboards |
| **[MLflow](https://mlflow.org/)** | Self-hosted or cloud | Fully free (self-hosted) | Full experiment lifecycle, model registry |
| **[Comet ML](https://www.comet.com/)** | Cloud | Free academic tier | Similar to W&B |
| **[Neptune](https://neptune.ai/)** | Cloud | Free academic tier | Metadata-heavy experiments |
| **[TensorBoard](https://www.tensorflow.org/tensorboard)** | Local | Fully free | Simple local visualization |
| **[Sacred + Omniboard](https://github.com/IDSIA/sacred)** | Self-hosted | Fully free | Academic, MongoDB-backed |

---

## 3. Weights & Biases (W&B) — Primary Recommendation

W&B is the most widely used experiment tracker in academic ML research.

### Quick Start

```bash
pip install wandb
wandb login  # Authenticate with your account
```

```python
import wandb

# Initialize a run
run = wandb.init(
    project="my-research-project",
    name="baseline-resnet50",
    config={
        "architecture": "resnet50",
        "learning_rate": 0.001,
        "batch_size": 256,
        "epochs": 90,
        "seed": 42,
    },
    tags=["baseline", "imagenet"],
)

# Log metrics during training
for epoch in range(config.epochs):
    train_loss, val_acc = train_one_epoch(...)
    wandb.log({
        "epoch": epoch,
        "train/loss": train_loss,
        "val/accuracy": val_acc,
    })

# Save model checkpoints
wandb.save("checkpoints/best_model.pt")

run.finish()
```

### PyTorch Lightning Integration

```python
from lightning.pytorch.loggers import WandbLogger
from lightning import Trainer

logger = WandbLogger(project="my-project", log_model=True)
trainer = Trainer(logger=logger, max_epochs=90)
trainer.fit(model, datamodule)
```

### Hugging Face Transformers Integration

```python
from transformers import TrainingArguments

training_args = TrainingArguments(
    output_dir="./outputs",
    report_to="wandb",  # That's it!
    run_name="bert-finetune-v1",
)
```

### W&B Sweeps (Hyperparameter Search)

```yaml
# sweep_config.yaml
program: train.py
method: bayes  # or random, grid
metric:
  name: val/accuracy
  goal: maximize
parameters:
  learning_rate:
    distribution: log_uniform_values
    min: 1e-5
    max: 1e-1
  batch_size:
    values: [64, 128, 256]
  dropout:
    distribution: uniform
    min: 0.0
    max: 0.5
```

```bash
# Create sweep
wandb sweep sweep_config.yaml
# Run sweep agents (can distribute across machines)
wandb agent user/project/sweep_id
```

---

## 4. MLflow — Self-Hosted Option

MLflow is the preferred choice for environments with data privacy constraints (no data leaving your cluster).

### Setup

```bash
pip install mlflow

# Start tracking server (stores to local filesystem or S3)
mlflow server \
  --backend-store-uri sqlite:///mlflow.db \
  --default-artifact-root s3://my-bucket/mlflow \
  --host 0.0.0.0 \
  --port 5000
```

### Logging

```python
import mlflow

with mlflow.start_run(run_name="baseline"):
    mlflow.log_params({
        "learning_rate": 0.001,
        "batch_size": 256,
        "architecture": "resnet50",
    })
    
    for epoch in range(epochs):
        loss, acc = train(...)
        mlflow.log_metrics({
            "train_loss": loss,
            "val_accuracy": acc
        }, step=epoch)
    
    mlflow.pytorch.log_model(model, "model")
    mlflow.log_artifact("configs/experiment.yaml")
```

---

## 5. What to Log

### Always Log

- All hyperparameters (complete config, not just the interesting ones)
- Training metrics per epoch/step (loss, accuracy, learning rate)
- Validation metrics
- Final test metrics
- Random seeds
- Hardware specs
- Git commit hash

### Log When Relevant

- Gradient norms (for debugging training instability)
- Model weight histograms (for detecting vanishing/exploding gradients)
- Sample predictions (images, text) for qualitative assessment
- Confusion matrices and ROC curves
- Resource usage (GPU memory, training time per epoch)

### Code to Capture Git State

```python
import subprocess

def get_git_info():
    return {
        "git_commit": subprocess.check_output(
            ["git", "rev-parse", "HEAD"]
        ).decode().strip(),
        "git_branch": subprocess.check_output(
            ["git", "rev-parse", "--abbrev-ref", "HEAD"]
        ).decode().strip(),
        "git_dirty": bool(subprocess.check_output(
            ["git", "status", "--porcelain"]
        ).decode().strip()),
    }
```

---

## 6. Organizing Your Experiment Hierarchy

A flat list of 200 unnamed runs is useless. Establish a naming convention from day one:

```
project: thesis-main
├── group: chapter2-baseline-comparison
│   ├── run: resnet50-imagenet-seed0
│   ├── run: resnet50-imagenet-seed1
│   └── run: vit-imagenet-seed0
├── group: chapter2-ablations
│   ├── run: no-augmentation
│   └── run: no-dropout
└── group: chapter3-new-method
    └── run: attention-variant-v1
```

Use **tags** for cross-cutting concerns: `submitted-neurips25`, `best-result`, `failed`, `debug`.

---

## Further Reading

* [W&B Documentation](https://docs.wandb.ai/) — Official docs with tutorials
* [MLflow Documentation](https://mlflow.org/docs/latest/index.html)
* [Experiment Tracking in ML](https://madewithml.com/courses/mlops/experiment-tracking/) — Made with ML
