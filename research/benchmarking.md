# 📏 Benchmarking & Evaluation

> A benchmark is only as meaningful as it is fair. The most common source of invalid claims in ML research is not fraud — it is unconscious favoritism in experimental design.

---

## 1. Why Benchmarking Is Hard

Evaluation in machine learning is deceptively difficult. Subtle choices in experimental setup can produce dramatic differences in reported numbers without any change in the underlying method.

### The Most Common Benchmarking Failures

| Failure | Description | Impact |
| :--- | :--- | :--- |
| **Unfair baselines** | Comparing your tuned method to an untuned baseline | Inflated improvements |
| **Dataset snooping** | Using test set performance to make design decisions | Over-optimistic results |
| **HARKing** | Hypothesizing After Results are Known — framing post-hoc discoveries as predictions | Invalid scientific claims |
| **p-hacking** | Running many random seeds and reporting only the best | Misleading statistics |
| **Metric gaming** | Optimizing a metric that doesn't capture true performance | Results that don't transfer |
| **Incomplete ablations** | Not isolating the contribution of individual components | Unclear what actually works |

---

## 2. Setting Up Fair Baselines

A comparison is only as credible as the quality of your baselines.

### The Baseline Fairness Standard

For each baseline you compare against, ask:
1. **Did you tune their hyperparameters?** If you tuned yours and not theirs, the comparison is unfair.
2. **Is this the strongest version of the baseline?** Use the best available implementation, not the one in the original paper (which may have bugs or be outdated).
3. **Are you using the same data splits?** Exactly the same preprocessing, splits, and evaluation protocol.
4. **Are you using the same evaluation metric?** Different implementations compute the same metric differently (e.g., BLEU, mAP, Accuracy).

### Recommended Baseline Strategy

```
1. Find the official code for the baseline method.
2. Run it on your data with its default hyperparameters.
3. Optionally: tune baseline hyperparameters on the validation set.
4. Report the tuned baseline result if you tuned yours.
5. Explicitly state in the paper whether baselines were tuned.
```

---

## 3. Statistical Significance

**A single number is not a result.** Every empirical claim should include a measure of uncertainty.

### Reporting Best Practices

```python
import numpy as np
from scipy import stats

# Run experiment across multiple seeds
results = [run_experiment(seed=s) for s in [0, 1, 2, 3, 4]]
# Output: [84.1, 83.8, 84.5, 84.2, 83.9]

mean = np.mean(results)           # 84.1
std  = np.std(results)            # 0.25
sem  = stats.sem(results)         # standard error of mean
ci95 = stats.t.interval(0.95, df=len(results)-1,
                          loc=mean, scale=sem)

print(f"Accuracy: {mean:.1f} ± {std:.1f} (5 seeds)")
print(f"95% CI: [{ci95[0]:.1f}, {ci95[1]:.1f}]")
```

**Report format in papers:**
- `84.1 ± 0.3` (mean ± std over N seeds)
- Or: `84.1 (std: 0.3, N=5)`

### Statistical Tests

For comparing two methods:

```python
from scipy import stats

method_a = [84.1, 83.8, 84.5, 84.2, 83.9]
method_b = [82.3, 81.9, 82.7, 82.1, 82.4]

# Paired t-test (same data splits across runs)
t_stat, p_value = stats.ttest_rel(method_a, method_b)
print(f"p-value: {p_value:.4f}")  # p < 0.05 → statistically significant
```

> [!WARNING]
> Statistical significance does not imply practical significance. A 0.1% improvement with p=0.001 may not be meaningful. Always discuss effect size, not just p-values.

---

## 4. Ablation Studies

Ablation studies are the most rigorous way to understand what actually works in your method.

### What to Ablate

For a method with components A, B, C:

| Variant | A | B | C | Purpose |
| :--- | :--- | :--- | :--- | :--- |
| **Full method** | ✓ | ✓ | ✓ | Your final result |
| **w/o A** | ✗ | ✓ | ✓ | Measure A's contribution |
| **w/o B** | ✓ | ✗ | ✓ | Measure B's contribution |
| **w/o C** | ✓ | ✓ | ✗ | Measure C's contribution |
| **w/o A,B** | ✗ | ✗ | ✓ | Test for interaction effects |

Run ablations on **validation set**, not test set.

### Sensitivity Analysis

Test your method's sensitivity to key hyperparameters:

```python
import matplotlib.pyplot as plt
import numpy as np

# Vary learning rate, measure validation accuracy
learning_rates = [1e-5, 3e-5, 1e-4, 3e-4, 1e-3]
val_accs = [72.3, 78.1, 84.1, 83.5, 79.2]

plt.figure(figsize=(6, 4))
plt.semilogx(learning_rates, val_accs, 'bo-', linewidth=2)
plt.xlabel('Learning Rate')
plt.ylabel('Validation Accuracy (%)')
plt.title('Sensitivity to Learning Rate')
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.savefig('figures/lr_sensitivity.pdf', dpi=300, bbox_inches='tight')
```

---

## 5. Choosing and Reporting Metrics

### Metric Selection Principles

1. **Use the metric the community uses** — Report what other papers in your area report, even if you think a different metric is better. (Then also report the better metric.)
2. **Understand what the metric captures and misses** — Accuracy ignores class imbalance; FID ignores mode coverage; BLEU ignores semantic similarity.
3. **Report multiple metrics** — A single metric rarely tells the full story.

### Common CS Research Metrics

| Area | Primary Metrics |
| :--- | :--- |
| **Image classification** | Top-1 Accuracy, Top-5 Accuracy |
| **Object detection** | mAP@50, mAP@50:95 |
| **Image segmentation** | mIoU, Dice Score |
| **NLP generation** | BLEU, ROUGE, BERTScore |
| **Information retrieval** | NDCG, MRR, Recall@K |
| **Recommendation** | Hit Rate, NDCG, Precision@K |
| **Systems** | Latency (p50/p99), Throughput (QPS), Memory |
| **Network** | Throughput (Mbps), Latency (ms), Packet loss (%) |

---

## 6. Reproducibility of Evaluation

Your evaluation code must be as reproducible as your training code:

```python
# evaluation/evaluate.py
"""
Evaluates a trained model on the test set.
Usage: python evaluation/evaluate.py --checkpoint checkpoints/best.pt --split test
Reports: Accuracy, F1, Confusion Matrix
"""

import torch
import json
from pathlib import Path

def evaluate(model, dataloader, device):
    model.eval()
    all_preds, all_labels = [], []
    
    with torch.no_grad():
        for batch in dataloader:
            inputs, labels = batch
            outputs = model(inputs.to(device))
            preds = outputs.argmax(dim=1)
            all_preds.extend(preds.cpu().numpy())
            all_labels.extend(labels.numpy())
    
    # Compute metrics
    from sklearn.metrics import accuracy_score, f1_score, classification_report
    accuracy = accuracy_score(all_labels, all_preds)
    f1 = f1_score(all_labels, all_preds, average='macro')
    
    results = {
        "accuracy": float(accuracy),
        "f1_macro": float(f1),
        "n_samples": len(all_labels),
    }
    
    # Save results for reproducibility
    Path("results").mkdir(exist_ok=True)
    with open("results/test_results.json", "w") as f:
        json.dump(results, f, indent=2)
    
    return results
```

---

## Further Reading

* [Troubling Trends in ML Scholarship](https://arxiv.org/abs/1807.03341) — Lipton & Steinhardt
* [Evaluation Pitfalls in NLP](https://arxiv.org/abs/2008.02577) — Bender & Koller
* [SciPy Statistics Reference](https://docs.scipy.org/doc/scipy/reference/stats.html)
