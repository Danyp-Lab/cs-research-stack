# 🔀 Version Control for Research

> Git is not just a software tool — it is the memory of your research. Every experiment, every draft, every failed approach becomes recoverable. This guide covers Git workflows optimized specifically for academic research.

---

## 1. Git Fundamentals for Researchers

### The Research Git Mental Model

Unlike software development, research Git has different priorities:
- **Experiments are ephemeral** — many branches will be created and discarded.
- **Papers are precious** — LaTeX source history is your version control for revisions.
- **Collaboration is async** — advisors, co-authors, and collaborators work on different schedules.
- **Reproducibility is critical** — every published result must be tied to a specific commit.

### Core Configuration

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
git config --global core.editor "code --wait"   # or vim, nano
git config --global push.default current
git config --global pull.rebase false

# Better git log visualization
git config --global alias.lg "log --oneline --graph --decorate --all"
git config --global alias.st "status -s"
```

---

## 2. Repository Structure for Research Projects

### Recommended Layout

```
my-research-project/
├── README.md               # Project overview and quick-start
├── paper/                  # LaTeX source for the paper
│   ├── main.tex
│   ├── references.bib
│   └── figures/
├── src/                    # Source code
│   ├── models/
│   ├── datasets/
│   ├── training/
│   └── evaluation/
├── configs/                # Experiment configurations (YAML/JSON)
│   ├── base.yaml
│   └── experiments/
├── scripts/                # Shell scripts for data processing and job submission
│   ├── download_data.sh
│   └── run_experiments.sh
├── notebooks/              # Exploratory analysis (Jupyter)
├── results/                # Processed results and tables (NOT raw model weights)
├── tests/                  # Unit tests
├── environment.yml         # Conda environment
├── requirements.txt        # Pip dependencies
└── .gitignore
```

### `.gitignore` for ML Research

```gitignore
# Python
__pycache__/
*.pyc
*.pyo
.pytest_cache/
.mypy_cache/

# Data and large files (use DVC instead)
data/
datasets/
*.h5
*.hdf5
*.pkl
*.npy
*.npz

# Model checkpoints (use DVC or HuggingFace Hub)
checkpoints/
*.pt
*.pth
*.ckpt
*.safetensors

# Experiment outputs
runs/
wandb/
mlruns/
outputs/

# Jupyter
.ipynb_checkpoints/

# Environment
.env
*.env
venv/
env/
.conda/

# IDE
.vscode/
.idea/
*.swp

# OS
.DS_Store
Thumbs.db

# LaTeX auxiliary files
*.aux
*.log
*.bbl
*.blg
*.fls
*.fdb_latexmk
*.synctex.gz
*.out
```

---

## 3. Branching Strategy for Research

### The Research Branch Model

```
main (stable, published results only)
├── dev (active development integration)
│   ├── experiment/attention-variant-1
│   ├── experiment/larger-batch-size
│   └── experiment/new-dataset-split
├── paper/submission-neurips-2025
│   ├── paper/revision-round-1
│   └── paper/camera-ready
└── feature/new-baseline
```

### Tagging Submissions and Results

Always tag the exact commit used to produce submitted results:

```bash
# Tag for paper submission
git tag -a "neurips2025-submission" -m "Exact code used for NeurIPS 2025 submission"
git push origin neurips2025-submission

# Tag for camera-ready
git tag -a "neurips2025-camera-ready" -m "Camera-ready version"

# List all tags
git tag -l

# Checkout a specific tag (for reproducing results)
git checkout neurips2025-submission
```

---

## 4. Managing Large Files with DVC

Git cannot handle large datasets and model weights efficiently. [DVC (Data Version Control)](https://dvc.org/) extends Git with versioning for large files.

```bash
pip install dvc dvc-s3  # Or dvc-gdrive, dvc-azure, etc.

# Initialize DVC in a Git repo
dvc init

# Track a large dataset directory
dvc add data/imagenet/
git add data/imagenet.dvc .gitignore
git commit -m "Track imagenet dataset with DVC"

# Configure remote storage (e.g., S3)
dvc remote add -d myremote s3://my-bucket/dvc-store
dvc push  # Push large files to remote

# Pull data on a new machine
git clone https://github.com/user/repo
dvc pull  # Fetches large files from remote
```

### DVC Pipelines (Reproducible Experiments)

```yaml
# dvc.yaml
stages:
  preprocess:
    cmd: python src/preprocess.py
    deps: [data/raw/, src/preprocess.py]
    outs: [data/processed/]
  
  train:
    cmd: python src/train.py --config configs/base.yaml
    deps: [data/processed/, src/train.py, configs/base.yaml]
    outs: [checkpoints/model.pt]
    metrics: [results/metrics.json]
```

```bash
dvc repro   # Run the pipeline (only re-runs changed stages)
dvc dag     # Visualize the pipeline DAG
```

---

## 5. Collaborating on Papers (LaTeX + Git)

### Overleaf Git Sync

Overleaf Pro supports full Git synchronization:

```bash
# Clone your Overleaf project locally
git clone https://git.overleaf.com/YOUR_PROJECT_ID paper/

# Push local changes to Overleaf
cd paper && git add . && git commit -m "Add related work section"
git push

# Pull co-author changes from Overleaf
git pull
```

### Handling LaTeX Merge Conflicts

LaTeX merge conflicts are manageable if you enforce one sentence per line:

```latex
% BAD (causes large diff blocks):
The proposed method leverages attention mechanisms to efficiently process long sequences and achieves state-of-the-art performance on five benchmarks.

% GOOD (one sentence per line — each line is a diff unit):
The proposed method leverages attention mechanisms to efficiently process long sequences.
It achieves state-of-the-art performance on five benchmarks.
```

With one-sentence-per-line formatting, `git diff` shows exactly which sentences changed, making reviews and conflict resolution trivial.

---

## 6. GitHub Actions for Research CI/CD

Automate testing and PDF compilation on every commit:

```yaml
# .github/workflows/paper.yml
name: Compile Paper

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Install TeX Live
        run: sudo apt install -y texlive-full
      
      - name: Compile paper
        run: |
          cd paper
          latexmk -pdf main.tex
      
      - name: Upload PDF artifact
        uses: actions/upload-artifact@v4
        with:
          name: paper-pdf
          path: paper/main.pdf
```

```yaml
# .github/workflows/tests.yml
name: Run Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.11'
      - run: pip install -r requirements.txt
      - run: pytest tests/ -v --tb=short
```

---

## 7. Useful Git Commands for Researchers

```bash
# See what changed in the last commit
git show --stat

# Find when a specific result was introduced
git log --all -S "accuracy=84.7" -- "results/"

# Recover a deleted file
git checkout HEAD~1 -- src/old_baseline.py

# Create a clean archive of your code for supplementary material
git archive --format=zip HEAD -o submission_code.zip

# Compare two experiment branches
git diff experiment/baseline experiment/our-method -- src/models/

# Interactive rebase to clean up messy commit history before submission
git rebase -i HEAD~10
```

---

## Further Reading

* [Pro Git](https://git-scm.com/book/en/v2) — Scott Chacon (free, comprehensive)
* [DVC Documentation](https://dvc.org/doc) — Official DVC docs
* [The Missing Semester — Version Control](https://missing.csail.mit.edu/2020/version-control/) — MIT
