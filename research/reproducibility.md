# 🔁 Reproducible Research

> *"An experiment that cannot be reproduced is not science — it is a story."*

Reproducibility is the foundation of trustworthy research. It is also one of the most systematically undervalued skills in graduate training. This guide provides a concrete framework for making your research reproducible from day one, not as an afterthought.

---

## 1. The Reproducibility Spectrum

Not all reproducibility is equal. Understand the spectrum:

| Level | Definition |
| :--- | :--- |
| **Repeatable** | Same team, same setup, same results |
| **Reproducible** | Different team, same setup, same results |
| **Replicable** | Different team, different setup, same conclusions |
| **Generalizable** | Different context, the findings still hold |

Most papers claim reproducibility but only provide repeatability. Aim for at least the reproducible tier.

---

## 2. The Reproducibility Checklist for Every Paper

### Code and Software

- [ ] **Release all code** used to produce results (not just inference code — training code too).
- [ ] Code passes a fresh-environment install test (clone → install → run).
- [ ] All dependencies are pinned with exact versions (`requirements.txt` or `environment.yml`).
- [ ] Code is documented: README with install, run, and expected output instructions.
- [ ] Provide a `run_experiments.sh` or similar script that reproduces main results.

### Data

- [ ] All datasets used are publicly available or released with the paper.
- [ ] Data preprocessing scripts are included (not just the preprocessed data).
- [ ] Train/val/test splits are fixed and released (or seed-based, deterministic).
- [ ] For private/proprietary data: provide synthetic or subset alternatives if possible.

### Experiments

- [ ] **All** random seeds are reported and fixed.
- [ ] Hardware and software environment are described (GPU model, CUDA version, OS).
- [ ] All hyperparameters (not just the final ones) are reported or available in a config file.
- [ ] The model selection criterion is clearly stated (how did you pick the best checkpoint?).
- [ ] Error bars / confidence intervals / standard deviations are reported.

### Results

- [ ] Main results can be reproduced to within the reported standard deviation.
- [ ] Negative results and failed experiments are mentioned in the paper or appendix.
- [ ] Ablation studies are reproducible independently.

---

## 3. Practical Tools for Reproducibility

### Environment Reproducibility

```bash
# Conda: Export exact environment
conda env export > environment.yml
# Reproduce:
conda env create -f environment.yml

# Pip: Pin exact versions
pip freeze > requirements.txt
# Reproduce:
pip install -r requirements.txt

# Docker: Gold standard for full reproducibility
# Dockerfile example:
FROM nvcr.io/nvidia/pytorch:24.01-py3
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
```

### Seed Management

```python
import random, numpy as np, torch

def set_seed(seed: int = 42):
    random.seed(seed)
    np.random.seed(seed)
    torch.manual_seed(seed)
    torch.cuda.manual_seed_all(seed)
    # For full determinism (may reduce performance):
    torch.backends.cudnn.deterministic = True
    torch.backends.cudnn.benchmark = False

set_seed(42)
```

### Configuration Files

Use structured config files (YAML/JSON) instead of command-line arguments for complex experiments. Every experiment should be associated with an immutable config file committed to version control.

```yaml
# configs/experiments/resnet50_imagenet_baseline.yaml
experiment:
  name: resnet50_imagenet_baseline
  seed: 42

model:
  arch: resnet50
  pretrained: false

training:
  epochs: 90
  batch_size: 256
  optimizer: sgd
  lr: 0.1
  momentum: 0.9
  weight_decay: 0.0001
  lr_schedule: cosine

data:
  dataset: imagenet
  train_split: train
  val_split: val
  num_workers: 8
```

---

## 4. Containers for Full Reproducibility

Docker containers are the strongest reproducibility guarantee: they capture the entire software stack.

```dockerfile
# Dockerfile for ML experiment
FROM pytorch/pytorch:2.2.0-cuda12.1-cudnn8-runtime

WORKDIR /workspace

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git wget curl && \
    rm -rf /var/lib/apt/lists/*

# Install Python dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy source code
COPY src/ ./src/
COPY configs/ ./configs/
COPY scripts/ ./scripts/

# Default command
ENTRYPOINT ["python", "src/train.py"]
```

```bash
# Build and run
docker build -t my-experiment .
docker run --gpus all \
  -v /data:/workspace/data \
  -v /results:/workspace/results \
  my-experiment --config configs/baseline.yaml
```

---

## 5. The Reproducibility Paper Appendix

Every published paper should include a **reproducibility appendix** (many venues now require this):

```
Appendix A: Implementation Details
  A.1 Model architecture details (layer dimensions, activation functions)
  A.2 Training procedure (optimizer, schedule, epochs)
  A.3 Hyperparameter sensitivity analysis

Appendix B: Computational Resources
  B.1 Hardware (GPU model, count, VRAM)
  B.2 Training time per experiment
  B.3 Total compute used (GPU hours)

Appendix C: Data
  C.1 Dataset statistics
  C.2 Preprocessing details
  C.3 Train/validation/test split sizes

Appendix D: Reproducibility Checklist
  (Use the NeurIPS or ML Reproducibility Challenge checklist)
```

---

## 6. Pre-Registration

Pre-registration is the practice of committing your hypotheses, methods, and analysis plan **before** collecting data or running experiments. It prevents p-hacking, HARKing (Hypothesizing After Results are Known), and other forms of unconscious bias.

* **[OSF Preregistration](https://osf.io/prereg/)** — Free, timestamped registration of your research plan.
* **[AsPredicted](https://aspredicted.org/)** — Short-form pre-registration for concise studies.

---

## 7. Key Papers on Reproducibility

* [Reproducibility in Machine Learning](https://arxiv.org/abs/1907.11908) — Pineau et al.
* [Reporting Standards for ML](https://arxiv.org/abs/1909.03004) — Bouthillier et al.
* [ML Reproducibility Challenge](https://paperswithcode.com/rc2022) — Papers With Code

---

## Further Reading

* [ML Reproducibility Checklist (NeurIPS)](https://neurips.cc/public/guides/PaperChecklist)
* [The Turing Way — Reproducible Research](https://the-turing-way.netlify.app/reproducible-research/reproducible-research)
* [DVC Documentation](https://dvc.org/doc) — Data and model versioning
