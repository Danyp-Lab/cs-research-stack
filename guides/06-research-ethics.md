# ⚖️ Research Ethics in Computer Science

> *"Science advances on trust. Every violation of research integrity corrodes the foundation of the entire enterprise."*

Research ethics in computer science spans a broad terrain: from plagiarism and data fabrication, to privacy violations in human-subjects research, to the societal impact of deployed AI systems. Understanding these norms is not optional — violations can end careers, trigger paper retractions, and cause real-world harm.

---

## 1. Academic Integrity Fundamentals

### What Constitutes Misconduct

| Type | Definition | Examples |
| :--- | :--- | :--- |
| **Fabrication** | Inventing data or results that were never collected | Reporting benchmark scores that were not actually run |
| **Falsification** | Manipulating data or experiments to misrepresent results | Selectively deleting unfavorable data points; cherry-picking random seeds |
| **Plagiarism** | Using another's ideas, text, or code without attribution | Copying related work text from another paper; using unreferenced code |
| **Duplicate Publication** | Submitting substantially identical work to multiple venues simultaneously | Submitting the same paper to ICML and NeurIPS at the same time |
| **Gift Authorship** | Including authors who did not meaningfully contribute | Adding an advisor's name to a paper they had no role in |
| **Ghost Authorship** | Excluding contributors who should be listed as authors | Omitting a collaborator who wrote the methodology section |

> [!CAUTION]
> These are not abstract rules. Fabrication and falsification are career-ending, and in some contexts (medical AI, safety-critical systems), they can cause direct physical harm. Retracted papers remain permanently marked in databases.

### The Reproducibility Standard

A core ethical obligation in computational research is **reproducibility**: your results must be achievable by an independent researcher following your description. This requires:

- Releasing **code and model weights** used to produce results.
- Reporting **all hyperparameters** used in experiments.
- Disclosing **negative results and failed runs**, not just the best run.
- Specifying **hardware, software, and dataset versions** precisely.

See [research/reproducibility.md](../research/reproducibility.md) for a full reproducibility framework.

---

## 2. Authorship and Credit

### The CRediT Taxonomy

The **Contributor Roles Taxonomy (CRediT)** defines 14 distinct roles in research:

1. Conceptualization
2. Data Curation
3. Formal Analysis
4. Funding Acquisition
5. Investigation
6. Methodology
7. Project Administration
8. Resources
9. Software
10. Supervision
11. Validation
12. Visualization
13. Writing – Original Draft
14. Writing – Review & Editing

Most CS conferences and journals now support (and increasingly require) CRediT statements in submitted papers. Use them honestly.

### Authorship Order Conventions in CS

| Convention | Field | Meaning |
| :--- | :--- | :--- |
| **First author** | Universally | Primary contributor (usually the PhD student) |
| **Last author** | ML/AI/Systems | Senior researcher / PI / advisor |
| **Alphabetical** | Theory / Math | Equal contribution; no seniority implied |
| **Co-first authorship** | Common in biology | Explicitly denoted with † symbol; equal primary contributors |

> [!IMPORTANT]
> Discuss authorship and author order **before** the project begins, not after results are produced. Authorship disputes are among the most damaging conflicts in academic labs.

---

## 3. Human Subjects Research and IRB

Any research involving **human participants** — including user studies, surveys, interviews, dataset collection involving people, or analysis of personal data — is subject to ethical oversight.

### When IRB Review Is Required

- User studies with human participants
- Analysis of data collected from people (e.g., scraped social media data, medical records)
- Crowdsourced labeling tasks (e.g., Amazon Mechanical Turk)
- Deployment of systems that interact with people in research contexts

### IRB Process Overview

1. Submit a **protocol** describing your study: purpose, participants, data collection, privacy protections, risks, and benefits.
2. Receive **approval** (or exemption) before beginning data collection.
3. Obtain **informed consent** from participants.
4. Store data securely and anonymize where possible.
5. Report any **adverse events** to the IRB immediately.

> [!WARNING]
> Publishing results from human-subjects research conducted without IRB approval is grounds for paper retraction and professional sanctions, regardless of the research quality.

---

## 4. Data Ethics

### Responsible Dataset Collection

| Concern | Guidance |
| :--- | :--- |
| **Consent** | Collect data only with explicit informed consent or clear public domain status |
| **Privacy** | Anonymize personally identifiable information (PII) before storage or release |
| **Bias** | Document demographic composition of datasets and known biases explicitly |
| **Copyright** | Respect intellectual property in training datasets (especially for generative models) |
| **Provenance** | Document data sources, collection methods, and preprocessing steps |

### The Datasheet for Datasets

Google Research introduced [Datasheets for Datasets](https://arxiv.org/abs/1803.09010) — a standardized documentation format that captures:
- Motivation and intended use
- Composition and collection process
- Preprocessing and cleaning steps
- Distribution, maintenance, and legal considerations

Publishing a datasheet alongside your dataset has become best practice.

---

## 5. AI Ethics and Responsible Research

### Dual-Use Research Concerns

Many CS research contributions have **dual-use potential** — they can be used for beneficial or harmful purposes. Examples:

- Face recognition → security applications OR surveillance abuse
- Language models → writing assistance OR disinformation generation
- Adversarial attacks → robustness research OR real-world attacks on deployed systems
- Network traffic analysis → intrusion detection OR mass surveillance

> [!NOTE]
> Most major venues (NeurIPS, ICML, ICLR, FAccT) now require a **Broader Impacts** or **Ethics Statement** in submitted papers. This is your opportunity to discuss potential harms and mitigation strategies honestly — not to minimize concerns.

### The ACM Code of Ethics

The [ACM Code of Ethics and Professional Conduct](https://www.acm.org/code-of-ethics) is the foundational ethical framework for computer scientists. Core principles:

1. Contribute to society and human well-being.
2. Avoid harm.
3. Be honest and trustworthy.
4. Be fair and take action not to discriminate.
5. Respect the work required to produce new ideas.
6. Respect privacy.
7. Honor confidentiality.

### Fairness and Bias in Systems

Research involving machine learning must grapple with:

- **Algorithmic bias**: Systems that discriminate against protected groups due to biased training data or model design.
- **Measurement bias**: Metrics that do not capture the experience of underrepresented users.
- **Deployment context**: A model accurate in aggregate may be harmful at the individual level.

Recommended frameworks and tools:
* **[Fairlearn](https://fairlearn.org/)** — Python toolkit for assessing and mitigating fairness issues.
* **[AI Fairness 360 (AIF360)](https://aif360.mybluemix.net/)** — IBM's comprehensive fairness toolkit.
* **[Model Cards](https://modelcards.withgoogle.com/about)** — Standardized model documentation including fairness evaluation.

---

## 6. Open Science and Transparency

### The Reproducibility Crisis

Meta-analyses across ML research have found that a significant fraction of published results cannot be reproduced by independent teams. Contributing causes:

- Undisclosed hyperparameter tuning
- Selective reporting of favorable random seeds
- Missing implementation details
- Dataset contamination (test set leakage)

### Open Science Practices

| Practice | Tool / Platform |
| :--- | :--- |
| **Preprint publication** | [arXiv](https://arxiv.org/), [OpenReview](https://openreview.net/) |
| **Code release** | [GitHub](https://github.com/), [Papers with Code](https://paperswithcode.com/) |
| **Data release** | [Hugging Face Datasets](https://huggingface.co/datasets), [Zenodo](https://zenodo.org/), [OSF](https://osf.io/) |
| **Model release** | [Hugging Face Hub](https://huggingface.co/models) |
| **Pre-registration** | [OSF Preregistration](https://osf.io/prereg/) — commit to methodology before seeing results |

---

## 7. Citing and Attribution

### What Must Be Cited

- Direct quotations
- Paraphrased ideas from other works
- Datasets used in experiments
- Code libraries and frameworks
- Statistical methods not invented by you

### Citation Tools

* **[Zotero](https://www.zotero.org/)** — Reference manager with browser plugin for one-click citation saving.
* **[Better BibTeX](https://retorque.re/zotero-better-bibtex/)** — Generates consistent citation keys for LaTeX.
* **[doi2bib](https://www.doi2bib.org/)** — Convert DOI to BibTeX entry instantly.
* **[Google Scholar](https://scholar.google.com/)** — Cite button generates BibTeX for any paper.

---

## Further Reading

* [ACM Code of Ethics](https://www.acm.org/code-of-ethics)
* [Datasheets for Datasets](https://arxiv.org/abs/1803.09010) — Gebru et al.
* [Model Cards for Model Reporting](https://arxiv.org/abs/1810.03993) — Mitchell et al.
* [Stochastic Parrots](https://dl.acm.org/doi/10.1145/3442188.3445922) — Bender et al. (on large language model risks)
* [The Ethics of Artificial Intelligence](https://nickbostrom.com/ethics/artificial-intelligence.pdf) — Bostrom & Cirkovic
