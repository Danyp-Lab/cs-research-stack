---
title: "Data Management Guide"
description: "Versioning, storage, privacy, and reproducibility best practices for research data in CS."
tags: ["guide", "data-management"]
---

# Data Management Guide

*Effective data handling is essential for reproducibility, collaboration, and compliance.*

## 1. Planning Your Data Lifecycle
- **Define data types** (raw, processed, intermediate, results).
- **Create a Data Management Plan (DMP)** – many funders require it. Include storage, backup, sharing, and preservation.
- **Assign persistent identifiers** (DOI via Zenodo, Figshare) for datasets you intend to publish.

## 2. Version Control for Data
- **Small datasets (<100 MB)** – store directly in the Git repo (use `.gitignore` for large binaries).
- **Large datasets** – use **DVC** (Data Version Control) or **Git‑LFS**.
  - Initialize DVC: `dvc init`
  - Track a file: `dvc add data/raw.csv`
  - Remote storage (S3, GCS, Azure, SSH): `dvc remote add -d myremote s3://mybucket/dvc`
  - Push: `dvc push`
- **Check integrity** – DVC stores SHA256 hashes.

## 3. Storage & Backup Strategies
| Tier | Typical Use | Recommended Service |
|---|---|---|
| **Hot** | Active development, quick access | Institutional HPC/shared drive, NFS mount |
| **Warm** | Intermediate results, collaboration | Cloud bucket (AWS S3, GCP Cloud Storage, Azure Blob) |
| **Cold** | Archival (≥ 5 years) | Institutional tape archive, Zenodo, Figshare |

- **Automate backups** with cron jobs or workflow managers (e.g., `rsync` nightly).

## 4. Privacy & Compliance
- **Personal data** – Follow GDPR or local regulations; anonymize before storage.
- **License** – Choose appropriate open‑data license (CC‑BY, CC0).
- **Secure access** – Use IAM policies; avoid public read/write buckets for sensitive data.

## 5. Documentation & Metadata
- Store a `README.md` alongside each dataset describing:
  - Origin & collection method
  - Schema (column names, types)
  - Pre‑processing steps
  - Usage examples (code snippets)
- Use **JSON‑LD** or **BIDS** (for neuro‑imaging) if applicable.

## 6. Reproducible Pipelines
- Integrate data steps into **Snakemake**, **Make**, or **CMake** workflows.
- Example Snakemake rule:
```yaml
rule preprocess:
    input: "data/raw.csv"
    output: "data/clean.parquet"
    script: "scripts/preprocess.py"
```
- Combine with DVC to track input/output.

## 7. Sharing & Publication
- Before publishing, **clean** data (remove personal identifiers).
- Upload to a repository (Zenodo, Figshare, OpenML) and **cite** the DOI in your paper.
- Provide a short `data/README.md` with instructions for reproducing experiments.

## 8. Checklist
- [ ] Write a Data Management Plan.
- [ ] Choose version‑control strategy (Git/DVC).
- [ ] Set up remote storage and backups.
- [ ] Document dataset schema & provenance.
- [ ] Apply appropriate license.
- [ ] Ensure privacy compliance.
- [ ] Create reproducible pipeline scripts.
- [ ] Publish dataset with DOI before paper submission.

---
