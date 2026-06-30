# 🖥️ HPC & Environment Management

> High-performance computing is the infrastructure of modern CS research. Mastering environment management prevents the most common class of research-blocking failures.

---

## 1. Understanding the HPC Ecosystem

Most universities provide access to shared HPC clusters. Understanding the architecture prevents wasted time and broken jobs.

### Typical Cluster Architecture

```
Your Laptop
    │ (SSH)
    ▼
Login Node (shared, no heavy compute here!)
    │ (SLURM job submission)
    ▼
Compute Nodes
    ├── CPU nodes (general compute)
    ├── GPU nodes (deep learning, ML)
    └── High-memory nodes (large-scale data processing)
    │
    └── Shared Filesystem (Lustre / GPFS / NFS)
            ├── /home/[user]    (small, backed up)
            ├── /scratch/[user] (large, NOT backed up)
            └── /project/[lab]  (shared lab storage)
```

> [!CAUTION]
> **Never run compute-heavy jobs on the login node.** It is a shared resource. Running `python train.py` on a login node affects all users on the cluster and will get your account suspended at most institutions.

---

## 2. SLURM Job Scheduler

SLURM is the most common HPC job scheduler. Learn it well — it is the gateway to all cluster resources.

### Basic SLURM Commands

```bash
# Submit a job
sbatch my_job.sh

# Check your job queue
squeue -u $USER

# Cancel a job
scancel <job_id>

# View cluster partitions and limits
sinfo

# Check a finished job's resource usage
sacct -j <job_id> --format=JobID,CPUTime,MaxRSS,Elapsed
```

### Job Script Template (GPU Training)

```bash
#!/bin/bash
#SBATCH --job-name=my_experiment
#SBATCH --partition=gpu
#SBATCH --gres=gpu:a100:1          # Request 1 A100 GPU
#SBATCH --cpus-per-task=8          # CPU cores per GPU
#SBATCH --mem=64G                  # RAM
#SBATCH --time=24:00:00            # Max walltime (HH:MM:SS)
#SBATCH --output=logs/%j.out       # Stdout file (%j = job ID)
#SBATCH --error=logs/%j.err        # Stderr file

# Load modules
module load cuda/12.1 python/3.11

# Activate environment
source ~/miniconda3/etc/profile.d/conda.sh
conda activate myenv

# Run experiment
python train.py \
  --config configs/experiment.yaml \
  --seed 42 \
  --output-dir /scratch/$USER/results/$SLURM_JOB_ID
```

### Array Jobs (Running Many Experiments)

```bash
#!/bin/bash
#SBATCH --array=0-19              # 20 jobs (indices 0–19)
#SBATCH --gres=gpu:1

# Map array index to hyperparameter
LR_VALUES=(0.0001 0.0003 0.001 0.003 0.01)
SEED_VALUES=(0 1 2 3)

LR=${LR_VALUES[$((SLURM_ARRAY_TASK_ID / 4))]}
SEED=${SEED_VALUES[$((SLURM_ARRAY_TASK_ID % 4))]}

python train.py --lr $LR --seed $SEED
```

---

## 3. Environment Management

### Conda / Miniforge (Recommended)

[Miniforge](https://github.com/conda-forge/miniforge) is the recommended conda distribution — it defaults to `conda-forge` (community-maintained, often more up-to-date than Anaconda's defaults) and excludes proprietary licensing terms.

```bash
# Install Miniforge
wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh
bash Miniforge3-Linux-x86_64.sh -b  # -b for batch/non-interactive

# Create project environment
conda create -n myproject python=3.11 -y
conda activate myproject

# Install PyTorch with CUDA (always get the exact command from pytorch.org)
conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia

# Export environment for reproducibility
conda env export > environment.yml
# Reproduce on another machine:
conda env create -f environment.yml
```

### uv (Modern Alternative)

[uv](https://github.com/astral-sh/uv) is a blazingly fast Python package manager written in Rust. 10–100× faster than pip for dependency resolution.

```bash
# Install uv
curl -LsSf https://astral.sh/uv/install.sh | sh

# Create a new project
uv init myproject && cd myproject

# Add dependencies
uv add torch torchvision numpy pandas

# Run a script
uv run python train.py

# Sync exact dependencies
uv sync
```

### Lmod (On Shared Clusters)

```bash
# List available modules
module avail

# Search for a specific module
module spider cuda

# Load specific versions
module load cuda/12.1 gcc/11.4 python/3.11

# Save your module configuration
module save my_default

# Restore saved configuration
module restore my_default
```

---

## 4. NVIDIA GPU Management

### Driver and CUDA Setup

```bash
# Check GPU status and driver version
nvidia-smi

# Monitor GPU utilization in real-time
watch -n 1 nvidia-smi

# Better GPU monitoring (install nvtop)
sudo apt install nvtop
nvtop
```

### NVIDIA Container Toolkit

The toolkit exposes host GPUs to Docker containers, eliminating CUDA version conflicts:

```bash
# Install NVIDIA Container Toolkit
distribution=$(. /etc/os-release; echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/libnvidia-container/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list \
  | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
sudo apt update && sudo apt install -y nvidia-container-toolkit
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker

# Test GPU in Docker
docker run --rm --gpus all nvidia/cuda:12.1.1-base-ubuntu22.04 nvidia-smi
```

### Preventing Driver/Kernel Conflicts with apt-pinning

```bash
# Pin NVIDIA driver version to prevent auto-upgrades
sudo tee /etc/apt/preferences.d/nvidia > /dev/null <<EOF
Package: nvidia-*
Pin: version 535.*
Pin-Priority: 1001
EOF
```

---

## 5. Remote Development

### VS Code Remote SSH

The most ergonomic way to develop on a remote cluster:

1. Install the **Remote - SSH** extension in VS Code.
2. Add your cluster to `~/.ssh/config`:
```
Host my-cluster
    HostName cluster.university.edu
    User your_username
    IdentityFile ~/.ssh/id_ed25519
    ServerAliveInterval 60
```
3. `Ctrl+Shift+P` → `Remote-SSH: Connect to Host` → `my-cluster`

### tmux (Session Management)

Never lose a running session due to SSH disconnection:

```bash
# Start new named session
tmux new -s training

# Detach (session keeps running)
Ctrl+B then D

# List sessions
tmux ls

# Reattach to session
tmux attach -t training

# Kill a session
tmux kill-session -t training
```

**Essential tmux config** (`~/.tmux.conf`):
```bash
# Enable mouse scrolling
set -g mouse on
# Increase scrollback buffer
set -g history-limit 50000
# Use vim keys in copy mode
setw -g mode-keys vi
```

---

## 6. Storage and Data Management

### Efficient Data Transfer

```bash
# Fast transfer to/from cluster (rsync with compression and progress)
rsync -avhP --compress local_data/ user@cluster:/scratch/user/data/

# Transfer large datasets efficiently with rclone (supports S3, GDrive, etc.)
rclone copy s3://my-bucket/dataset /scratch/user/dataset --progress

# Parallel transfer with aria2 (for HTTP/FTP sources)
aria2c -x 16 -s 16 https://example.com/large_dataset.tar.gz
```

### Data Compression

```bash
# Fast compression with zstd (better than gzip for large ML datasets)
tar -I zstd -cf dataset.tar.zst dataset/

# Decompress
tar -I zstd -xf dataset.tar.zst

# Ultra-fast parallel compression
pigz -9 -p 8 large_file.txt  # 8 threads
```

---

## Further Reading

* [HPC Carpentry](https://hpc-carpentry.org/) — Hands-on HPC tutorials for researchers
* [SLURM Documentation](https://slurm.schedmd.com/documentation.html) — Official SLURM reference
* [Lambda Stack](https://lambda.ai/blog/lambda-stack-deep-learning-software/) — One-command deep learning environment setup for Ubuntu
