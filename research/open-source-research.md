# 🌍 Open-Source Research

> Releasing your research code is no longer optional — it is the baseline expectation at top venues. A paper without code is increasingly treated as a second-class contribution.

---

## 1. Why Open-Source Your Research

### The Scientific Argument

Reproducibility requires access to your implementation. Without it, other researchers cannot:
- Verify your results
- Build on your method correctly
- Identify bugs in your evaluation
- Reuse your components in their work

### The Career Argument

A well-maintained research repository is a **career accelerator**:
- High GitHub stars signal community recognition
- Open-source users become paper citers and future collaborators
- Industry recruiters specifically look for research code quality
- It demonstrates software engineering competence alongside research depth

### The Evidence

Studies consistently show that papers releasing code receive **significantly more citations** than papers without code, even controlling for paper quality.

---

## 2. What to Release

### Minimum Viable Release

- [ ] All code used to produce results in the paper
- [ ] Pre-trained model weights (for applicable papers)
- [ ] Data preprocessing scripts
- [ ] Evaluation scripts
- [ ] A `README.md` with installation and quick-start instructions

### Ideal Release Package

- [ ] Clean, documented code (not your messy experiment branch)
- [ ] Training script with all hyperparameters configurable from CLI or config file
- [ ] Inference script / demo notebook
- [ ] Pre-trained weights for all model sizes/variants
- [ ] Example data or synthetic demo data
- [ ] Comprehensive README with method overview, installation, training, evaluation, results
- [ ] Docker image for full reproducibility
- [ ] Colab or Kaggle notebook for interactive demo
- [ ] `CITATION.cff` or `BibTeX` entry for easy citation

---

## 3. Repository Quality Standards

### README Template

```markdown
# [Method Name]: [One-sentence description]

[Paper badge] [License badge] [Stars badge]

> Brief description of what this repository contains.

## Abstract
[1-2 sentence summary of the paper's contribution]

## Key Results
| Dataset | Metric | Ours | Previous SOTA |
|---------|--------|------|---------------|
| CIFAR-10 | Top-1 Acc | 97.8% | 97.2% |

## Installation
\`\`\`bash
git clone https://github.com/username/method-name
cd method-name
pip install -e .
\`\`\`

## Quick Start
\`\`\`python
from method import Model
model = Model.from_pretrained("username/method-name-base")
result = model.predict(input_data)
\`\`\`

## Training
\`\`\`bash
python train.py --config configs/cifar10.yaml --seed 42
\`\`\`

## Evaluation
\`\`\`bash
python evaluate.py --checkpoint checkpoints/best.pt --split test
# Expected output: Accuracy: 97.8%
\`\`\`

## Pretrained Models
| Model | Dataset | Accuracy | Download |
|-------|---------|----------|----------|
| Method-Base | CIFAR-10 | 97.8% | [HuggingFace](https://huggingface.co/...) |

## Citation
\`\`\`bibtex
@inproceedings{yourname2024method,
  title={Your Paper Title},
  author={Your Name and Co-author},
  booktitle={Conference Name},
  year={2024}
}
\`\`\`

## License
MIT License
```

### Code Quality Checklist

- [ ] Consistent code style (use `black` for Python formatting)
- [ ] Type hints on all public functions
- [ ] Docstrings on all modules, classes, and non-trivial functions
- [ ] Unit tests for core components (aim for >60% coverage)
- [ ] No hardcoded paths or secrets
- [ ] Clear separation of configuration from code

```bash
# Code formatting
pip install black isort
black src/
isort src/

# Linting
pip install flake8 pylint
flake8 src/ --max-line-length 100

# Type checking
pip install mypy
mypy src/
```

---

## 4. Licensing Your Research Code

Choose the right license before releasing:

| License | Allows Commercial Use | Requires Attribution | Copyleft | Best For |
| :--- | :--- | :--- | :--- | :--- |
| **MIT** | ✓ | ✓ | ✗ | Most research code |
| **Apache 2.0** | ✓ | ✓ | ✗ | Research with patent protection |
| **GPL v3** | ✓ | ✓ | ✓ | Code you want kept open-source |
| **CC BY 4.0** | ✓ | ✓ | ✗ | Datasets and models |
| **CC BY-NC 4.0** | ✗ | ✓ | ✗ | Non-commercial research only |

> [!TIP]
> **MIT** is the community default for most CS research code. It maximizes adoption and places minimal burden on users, encouraging citation and contribution.

---

## 5. Publishing on Hugging Face Hub

[Hugging Face Hub](https://huggingface.co/) has become the de-facto standard for releasing ML models and datasets.

```python
from transformers import AutoModelForSequenceClassification, AutoTokenizer

model = AutoModelForSequenceClassification.from_pretrained("checkpoints/best")
tokenizer = AutoTokenizer.from_pretrained("bert-base-uncased")

# Upload model
model.push_to_hub("username/my-research-model")
tokenizer.push_to_hub("username/my-research-model")

# Create a model card (README on the hub)
# IMPORTANT: Write a model card at the model repo on HuggingFace
```

### Model Card Template

```markdown
---
language: en
license: mit
tags:
  - text-classification
  - research
datasets:
  - my_dataset
metrics:
  - accuracy
---

# Model Name

## Model Description
This model was released with the paper "[Title](arxiv link)".

## Intended Uses and Limitations

## Training Details

## Evaluation Results

## Citation
```

---

## 6. Papers With Code Submission

[Papers With Code](https://paperswithcode.com/) is the standard platform for linking papers to code and benchmark results.

1. Go to [paperswithcode.com](https://paperswithcode.com/)
2. Search for your paper (it may already be indexed from arXiv)
3. Add your GitHub repository link
4. Submit your results to the relevant benchmark leaderboards

This dramatically increases discoverability — researchers searching for implementations of your method find it immediately.

---

## 7. Maintaining Your Repository

Releasing code is the beginning, not the end:

### Post-Release Checklist

- [ ] Set up GitHub Issues for bug reports and feature requests
- [ ] Add a `CONTRIBUTING.md` if you want community contributions
- [ ] Respond to issues within 1–2 weeks (builds community trust)
- [ ] Tag releases with version numbers (`v1.0.0`)
- [ ] Update the README as the API evolves
- [ ] Add a `CHANGELOG.md`

---

## Further Reading

* [Papers With Code](https://paperswithcode.com/) — Submit your paper and results
* [Hugging Face Hub Documentation](https://huggingface.co/docs/hub)
* [Choose A License](https://choosealicense.com/) — Decision guide for licenses
* [The Hitchhiker's Guide to Python](https://docs.python-guide.org/) — Python project best practices
