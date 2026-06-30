# ✍️ Writing Research Papers

> *"Writing is thinking made visible. A paper that cannot be written clearly is a paper whose ideas are not yet clear."*

Writing a research paper is the primary output of academic research. It is also one of the most underteached skills in graduate programs. This guide covers the full lifecycle: from structuring your argument to navigating peer review and managing rejections.

---

## 1. The Anatomy of a CS Research Paper

Understanding the function of each section prevents the most common structural mistakes.

| Section | Core Function | Common Mistakes |
| :--- | :--- | :--- |
| **Abstract** | Compress the entire paper into 150–250 words | Too vague; missing numbers; reads like an introduction |
| **Introduction** | Motivate the problem, state contributions explicitly | Buries contributions; too long; no clear problem statement |
| **Related Work** | Position your work relative to prior art | Dismissive of competitors; placed too early or too late |
| **Methodology** | Describe your approach precisely and reproducibly | Missing implementation details; assumes too much prior knowledge |
| **Experiments** | Empirically validate your claims | Weak baselines; missing ablations; cherry-picked results |
| **Results & Discussion** | Interpret and contextualize findings | Over-claiming; ignoring failure cases; no error bars |
| **Conclusion** | Summarize contributions and future work | Rehashing the abstract word-for-word |

---

## 2. The Writing Process

### Step 1: Write the Outline First

Before writing a single sentence, create a structured outline. A good outline for a systems/ML paper looks like:

```
1. Introduction
   1.1 Problem statement (1 paragraph)
   1.2 Why existing solutions fail (1 paragraph)
   1.3 Our approach and key insight (1 paragraph)
   1.4 Contributions (bullet list, 3–5 items)
   1.5 Paper organization (1 sentence per section)

2. Background / Problem Formulation
3. Related Work
4. [System Name] / Our Method
   4.1 Overview
   4.2 Component A
   4.3 Component B
5. Evaluation
   5.1 Experimental Setup
   5.2 Main Results
   5.3 Ablation Study
6. Discussion and Limitations
7. Conclusion
```

> [!TIP]
> Write the **contributions list** first. This forces you to articulate what is genuinely novel before writing the paper around it. If you cannot list 3 concrete, verifiable contributions, you do not yet have a paper.

### Step 2: Write Bottom-Up

Counterintuitively, experienced researchers often write papers in this order:

1. **Figures and tables first** — Create your key result graphs and architecture diagrams before writing any prose.
2. **Methodology** — You know this section best; it is easiest to start.
3. **Experiments** — Describe what you did while the details are fresh.
4. **Introduction** — Write this after the paper exists; you now know what you are introducing.
5. **Abstract** — Last. It summarizes a finished paper.
6. **Related Work** — Can be written any time, but best after the paper is stable.

### Step 3: Draft Quickly, Revise Deeply

Write your first draft without editing. The goal is a complete draft, not a perfect one. Then revise in multiple passes:

- **Pass 1**: Structure — Does every paragraph have a clear topic sentence? Does each section serve its function?
- **Pass 2**: Clarity — Can a reader in your area follow every argument without ambiguity?
- **Pass 3**: Precision — Are all claims quantified? Are all terms defined?
- **Pass 4**: Polish — Grammar, word choice, sentence rhythm, transitions.

---

## 3. Writing the Introduction

The introduction is the most important section. Reviewers form their overall impression here.

### The CARS Model (Create A Research Space)

Developed by John Swales, this is the canonical structure:

1. **Establishing territory**: Why is this research area important? (1–2 paragraphs)
2. **Establishing a niche**: What gap, problem, or challenge exists? (1 paragraph)
3. **Occupying the niche**: What does your paper do about it? (1 paragraph + contributions list)

### Contribution Bullet Format

Be concrete and falsifiable:

```
We make the following contributions:
• We propose [X], a novel approach to [problem] that achieves [Y].
• We show theoretically that [X] has [property Z] under [assumptions].
• We evaluate [X] on [benchmarks] and demonstrate [N]% improvement over [baseline].
• We release code and datasets at [URL].
```

---

## 4. Writing for Clarity

### Sentence-Level Rules

| Rule | Example (Bad) | Example (Good) |
| :--- | :--- | :--- |
| **Prefer active voice** | "It was shown by..." | "We show that..." |
| **One idea per sentence** | "We propose X which builds on Y and extends Z..." | "We propose X. X builds on Y by extending Z." |
| **Avoid nominalizations** | "We performed an evaluation of..." | "We evaluated..." |
| **Quantify claims** | "Our method is fast" | "Our method runs in O(n log n)" |
| **Avoid weasel words** | "significantly better" | "15.3% better (p < 0.01)" |

### Paragraph-Level Rules

Every paragraph should:
1. Begin with a **topic sentence** stating the paragraph's main claim.
2. Provide **evidence** for that claim (data, equations, citations).
3. End with a **transition** or synthesis that connects to the next paragraph.

---

## 5. LaTeX Best Practices

CS papers are written in LaTeX. See [stack/latex-typesetting.md](../stack/latex-typesetting.md) for detailed setup.

### Essential Packages

```latex
\usepackage{booktabs}     % Professional tables
\usepackage{microtype}    % Better text rendering
\usepackage{hyperref}     % Clickable links and refs
\usepackage{cleveref}     % Smart cross-references (Sec. 3, Fig. 2)
\usepackage{algorithm2e}  % Pseudocode
\usepackage{tikz}         % Diagrams and figures
\usepackage{pgfplots}     % Result graphs
\usepackage{siunitx}      % Consistent number/unit formatting
```

### Figure Quality Rules

- Export at **300+ DPI** for raster images; prefer **vector formats (PDF, SVG)** for diagrams.
- Always include **axis labels and units**.
- Use **colorblind-safe palettes** (e.g., [Colorbrewer](https://colorbrewer2.org/), [Okabe-Ito](https://jfly.uni-koeln.de/color/)).
- Include **error bars** or confidence intervals on all empirical results.

---

## 6. Navigating Peer Review

### Understanding the Review Process

Most CS conferences use a **double-blind review** process (authors cannot see reviewers; reviewers cannot see authors in ideal implementations). Journals often use single-blind or open review.

The typical timeline for a systems/ML conference:

```
Submission deadline
   ↓ (~6 weeks)
Reviews released (scores + comments)
   ↓ (~2 weeks)
Author rebuttal period
   ↓ (~2 weeks)
Final decisions released
   ↓ (~4–8 weeks)
Camera-ready deadline
```

### Writing a Strong Rebuttal

The rebuttal is not an argument — it is a clarification and a set of commitments.

- **Acknowledge legitimate concerns**: "Reviewer 2 raises a valid point about..."
- **Correct factual misunderstandings**: "We respectfully note that our method does X, not Y. This is described in Section 3.2."
- **Commit to concrete revisions**: "We will add a baseline comparison with [Method Z] in the revision."
- **Prioritize**: Address the most critical concerns first. Every word counts.

> [!IMPORTANT]
> Reviewers are volunteers. Even harsh reviews are valuable. A rejection with three detailed reviews is more useful than an acceptance with two superficial ones.

### Managing Rejection

- The top venues (NeurIPS, ICML, CVPR, SIGCOMM, SOSP) have **15–25% acceptance rates**. Most submissions are rejected.
- A rejection is not a verdict on your career — it is feedback on a specific paper at a specific time.
- Standard practice: revise using review feedback, and resubmit to the next deadline (often 3–6 months away).

---

## 7. Tools for Academic Writing

| Tool | Purpose | Cost |
| :--- | :--- | :--- |
| **[Overleaf](https://www.overleaf.com/)** | Collaborative online LaTeX editor | Free (premium via institution) |
| **[Grammarly](https://www.grammarly.com/)** | Grammar and clarity checking | Free tier + premium |
| **[DeepL](https://www.deepl.com/)** | High-quality translation (non-native English writers) | Free tier |
| **[Writefull](https://writefull.com/)** | Academic English suggestions, trained on published papers | Free for students |
| **[Hemingway App](https://hemingwayapp.com/)** | Readability scoring and simplification suggestions | Free |
| **[detex / chktex](https://www.nongnu.org/chktex/)** | LaTeX linting and style checking | Free (CLI) |

---

## Further Reading

* [Writing Tips for PhD Students](https://www.johnhcochrane.com/research-tips) — John Cochrane
* [How to Write a Great Research Paper](https://www.microsoft.com/en-us/research/academic-program/write-great-research-paper/) — Simon Peyton Jones (Microsoft Research)
* [Mathematical Writing](https://jmlr.org/reviewing-papers/knuth_mathematical_writing.pdf) — Donald Knuth
* [The Elements of Style](https://www.gutenberg.org/files/37134/37134-h/37134-h.htm) — Strunk & White (classic)
