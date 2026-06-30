---
title: "Experiment Tracking Guide"
description: "How to log, version, and visualize machine‑learning experiments for reproducibility."
tags: ["guide", "experiment-tracking"]
---

# Experiment Tracking Guide

*Track every run so you can reproduce, compare, and share results effortlessly.*

## 1. Why Track Experiments?
- Reproducibility – exact hyper‑parameters, dataset version, code commit.
- Collaboration – teammates can inspect runs, reuse pipelines.
- Hyper‑parameter search – aggregate metrics to pick the best model.

## 2. Choose a Tracking Tool
| Tool | Language Support | Cloud / Self‑hosted | Key Features |
|---|---|---|---|
| **MLflow** | Python, R, Java | Cloud (databricks) or local | UI, model registry, REST API |
| **Weights & Biases (W&B)** | Python, R, JavaScript | SaaS (free tier) | Real‑time dashboards, sweep, artifact storage |
| **Sacred** | Python | Self‑hosted | SQLite/JSON DB, simple API |
| **Comet.ml** | Python, R, Java, Scala | SaaS | Experiment comparison, dataset versioning |

> **Tip:** For a small research group, start with MLflow locally; later migrate to W&B if you need more visual analytics.

## 3. Basic MLflow Setup
```bash
# Install
pip install mlflow
# Initialise a tracking server (optional)
mlflow server --backend-store-uri sqlite:///mlflow.db --default-artifact-root ./mlruns
```
```python
import mlflow
import mlflow.sklearn

mlflow.start_run()
mlflow.log_param("learning_rate", 0.01)
mlflow.log_metric("accuracy", 0.87)
mlflow.sklearn.log_model(model, "model")
mlflow.end_run()
```
- Use `mlflow ui` to view the web UI.

## 4. Integrating with DVC / Git
- Store large artifacts (trained models, logs) with **DVC** and reference them in the MLflow run.
- Example: `dvc add models/model.pt` then `mlflow.log_artifact('models/model.pt')`.

## 5. Advanced Features
- **Parameter Sweeps** – use `mlflow.start_run(run_name="sweep")` in a loop or integrate with `optuna`.
- **Metrics Plotting** – add custom plots via `mlflow.log_figure(fig, "confusion.png")`.
- **Artifact Storage** – configure remote storage (S3, GCS) for long‑term preservation.

## 6. Checklist
- [ ] Choose a tracking tool and install.
- [ ] Initialise a tracking server or use SaaS UI.
- [ ] Log parameters, metrics, and artifacts in each script.
- [ ] Version data and code alongside experiments (Git + DVC).
- [ ] Regularly review runs and prune obsolete ones.
- [ ] Backup the tracking database (e.g., `mlflow.db` to remote storage).

---
