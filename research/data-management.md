# 🗄️ Data Management for Research

> Bad data management is the silent killer of research productivity. Papers get delayed because datasets cannot be found. Results become irreproducible because preprocessing was done manually and undocumented. Storage runs out at the worst possible moment.

---

## 1. Research Data Lifecycle

```
Raw Data Collection / Download
         ↓
   Preprocessing & Cleaning
         ↓
   Processed Dataset (versioned)
         ↓
   Train / Val / Test Splits (fixed, versioned)
         ↓
   Experimental Results (logs, predictions)
         ↓
   Published Artifacts (released datasets, model weights)
```

Every step should be scripted, versioned, and reproducible from the previous step.

---

## 2. Dataset Discovery and Acquisition

### Major Dataset Repositories

| Repository | Content | Access |
| :--- | :--- | :--- |
| **[Hugging Face Datasets](https://huggingface.co/datasets)** | NLP, CV, multimodal, audio — 80,000+ datasets | Free API |
| **[Papers With Code Datasets](https://paperswithcode.com/datasets)** | ML benchmark datasets with leaderboards | Free |
| **[Kaggle Datasets](https://www.kaggle.com/datasets)** | Competitions and community datasets | Free (account required) |
| **[UCI ML Repository](https://archive.ics.uci.edu/)** | Classic ML datasets | Free |
| **[Google Dataset Search](https://datasetsearch.research.google.com/)** | Cross-repository dataset search engine | Free |
| **[OpenML](https://www.openml.org/)** | 3,000+ ML datasets with standardized splits | Free API |
| **[Zenodo](https://zenodo.org/)** | Research outputs including datasets (CERN-hosted) | Free |
| **[Dataverse Network](https://dataverse.org/)** | Academic research data across disciplines | Free |

### Downloading Large Datasets Efficiently

```bash
# aria2c: parallel multi-connection download
aria2c -x 16 -s 16 https://example.com/large_dataset.tar.gz

# wget with resume capability
wget -c https://example.com/dataset.tar.gz

# rsync for incremental updates
rsync -avhP --compress user@dataserver:/datasets/imagenet/ ./data/imagenet/

# Hugging Face datasets API
python -c "from datasets import load_dataset; ds = load_dataset('imagenet-1k'); ds.save_to_disk('./data/imagenet')"
```

---

## 3. Dataset Versioning with DVC

Large datasets cannot be versioned with Git (100MB file size limit). [DVC](https://dvc.org/) solves this by storing data in remote storage and tracking metadata in Git.

```bash
pip install dvc dvc-s3  # or dvc-gdrive, dvc-azure, dvc-ssh

# Initialize
dvc init

# Track dataset
dvc add data/imagenet/
git add data/imagenet.dvc data/.gitignore
git commit -m "Add imagenet dataset tracking"

# Push to remote storage (S3, GDrive, SFTP, etc.)
dvc remote add -d myremote s3://my-bucket/dvc/
dvc push

# On a new machine: restore data
git clone https://github.com/me/project
dvc pull
```

### DVC Data Versioning

```bash
# Update dataset and version the change
dvc add data/imagenet/
git add data/imagenet.dvc
git commit -m "Update imagenet to v2 (fixed corrupted images)"
dvc push

# Revert to previous data version
git checkout v1 -- data/imagenet.dvc
dvc checkout
```

---

## 4. Data Preprocessing Best Practices

### The Golden Rule

**Never modify raw data**. Raw data is sacred. All preprocessing produces new, versioned datasets.

```
data/
├── raw/              # Never touched after download; read-only
│   └── imagenet_raw/
├── processed/        # Output of preprocessing scripts; tracked by DVC
│   └── imagenet_processed/
└── splits/           # Fixed train/val/test splits; committed to Git
    ├── train.txt
    ├── val.txt
    └── test.txt
```

### Preprocessing Pipeline Example

```python
# scripts/preprocess.py
"""
Converts raw ImageNet tar files to preprocessed, resized JPEG images.
Input:  data/raw/imagenet_raw/ (original downloaded files)
Output: data/processed/imagenet_224/ (224x224 images, JPEG quality 95)

Usage: python scripts/preprocess.py --input data/raw/imagenet_raw --output data/processed/imagenet_224
"""

import argparse
from pathlib import Path
from PIL import Image
from tqdm import tqdm
import multiprocessing as mp

def process_image(args):
    src, dst, size = args
    dst.parent.mkdir(parents=True, exist_ok=True)
    img = Image.open(src).convert("RGB")
    img = img.resize((size, size), Image.LANCZOS)
    img.save(dst, "JPEG", quality=95)

def main(input_dir: Path, output_dir: Path, size: int = 224):
    files = list(input_dir.rglob("*.JPEG"))
    tasks = [(f, output_dir / f.relative_to(input_dir), size) for f in files]
    with mp.Pool() as pool:
        list(tqdm(pool.imap(process_image, tasks), total=len(tasks)))

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description=__doc__)
    parser.add_argument("--input", type=Path, required=True)
    parser.add_argument("--output", type=Path, required=True)
    parser.add_argument("--size", type=int, default=224)
    args = parser.parse_args()
    main(args.input, args.output, args.size)
```

---

## 5. Data Splits and Leakage Prevention

**Dataset contamination / leakage** is a critical validity issue: if test set data appears in training, your results are invalid.

### Rules for Train/Val/Test Splits

- **Fix splits before starting experiments** — never re-split after seeing results.
- **Use a deterministic seed** for random splits and commit the split files.
- **Verify no overlap** between splits:

```python
train_ids = set(train_df['id'])
val_ids   = set(val_df['id'])
test_ids  = set(test_df['id'])

assert len(train_ids & val_ids) == 0, "Train/val overlap!"
assert len(train_ids & test_ids) == 0, "Train/test overlap!"
assert len(val_ids & test_ids) == 0, "Val/test overlap!"
```

- **Test set is touched ONCE** — only to report final results. Never use test set performance to make modeling decisions.

---

## 6. Storage Architecture

### Local vs. Remote Storage Decision Tree

```
Dataset size < 10GB?  → Git LFS or DVC with local remote
Dataset size 10–100GB? → DVC with S3/GDrive/cluster storage
Dataset size > 100GB? → Cluster scratch filesystem + DVC metadata
Model checkpoints?    → DVC + cloud storage OR Hugging Face Hub
Final released models? → Hugging Face Hub (free, versioned, public)
```

### Hugging Face Hub for Model and Dataset Release

```python
from huggingface_hub import HfApi

api = HfApi()

# Upload dataset
api.create_repo(repo_id="username/my-dataset", repo_type="dataset")
api.upload_folder(
    folder_path="data/processed/my_dataset",
    repo_id="username/my-dataset",
    repo_type="dataset",
)

# Upload model weights
api.create_repo(repo_id="username/my-model")
api.upload_folder(
    folder_path="checkpoints/best_model",
    repo_id="username/my-model",
)
```

---

## 7. Data Documentation: Datasheets

For any dataset you create or release, write a **Datasheet for Dataset** covering:

1. **Motivation** — Why was this dataset created? Who created it and with what funding?
2. **Composition** — What does the dataset represent? How many instances? What format?
3. **Collection** — How was data collected? Who collected it? What tools?
4. **Preprocessing** — What preprocessing was applied? What was excluded and why?
5. **Uses** — What is the dataset for? What should it NOT be used for?
6. **Distribution** — How is it distributed? What license?
7. **Maintenance** — Who maintains it? How to report errors?

Template: [Datasheets for Datasets](https://arxiv.org/abs/1803.09010) — Gebru et al.

---

## Further Reading

* [DVC Documentation](https://dvc.org/doc)
* [Hugging Face Datasets Documentation](https://huggingface.co/docs/datasets)
* [Datasheets for Datasets](https://arxiv.org/abs/1803.09010)
