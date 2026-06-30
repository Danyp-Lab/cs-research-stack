# 🚀 Getting Started as a CS Graduate Student

> *"The first semester is about orientation. The second is about direction. The third is when real research begins."*

Whether you are beginning a Master's or a PhD program, the first few months are foundational. The decisions you make in this window — which lab to join, which tools to learn, which habits to build — echo throughout your entire graduate career. This guide gives you a structured checklist and rational framework for navigating that critical onboarding period.

---

## 1. Week-by-Week Onboarding Plan

### Week 1–2: Administrative Foundation

- [ ] Set up your institutional email and enable two-factor authentication.
- [ ] Register for university VPN access — you will need it for remote database and library access.
- [ ] Claim your student benefits (see [stack/academic-perks.md](../stack/academic-perks.md)).
- [ ] Activate your GitHub Student Developer Pack.
- [ ] Set up your university library proxy in your browser for free journal/paper access.
- [ ] Introduce yourself to lab members and administrative staff.
- [ ] Locate the department's graduate student handbook and read it in full.

### Week 3–4: Environment Setup

- [ ] Install and configure your core development environment.
- [ ] Set up a password manager (Bitwarden — free; 1Password — free with GitHub Education Pack).
- [ ] Configure your reference manager (Zotero recommended — see [stack/literature-tools.md](../stack/literature-tools.md)).
- [ ] Create your Google Scholar profile and add your institution.
- [ ] Set up a personal research website (GitHub Pages is free and sufficient).
- [ ] Begin your first literature review using Semantic Scholar and Connected Papers.

### Month 2–3: Research Direction

- [ ] Schedule one-on-ones with each faculty member in your area of interest.
- [ ] Attend all department seminars and colloquia — even the unrelated ones.
- [ ] Read the five most-cited papers in your prospective research area.
- [ ] Identify 2–3 potential research problems you find genuinely exciting.
- [ ] Present your problem understanding to your prospective advisor for feedback.

---

## 2. Setting Up Your Development Environment

A reproducible, portable development environment prevents the most common class of research failures: "It worked on my machine."

### Recommended Core Stack

| Layer | Tool | Purpose |
| :--- | :--- | :--- |
| **Containerization** | Docker + NVIDIA Container Toolkit | Reproducible ML environments |
| **Package Management** | Conda (Miniforge) or uv (Python) | Isolated Python environments |
| **Version Control** | Git + GitHub | Code, paper drafts, experiment configs |
| **Remote Dev** | VS Code Remote SSH / JetBrains Gateway | HPC cluster development |
| **Experiment Tracking** | Weights & Biases (W&B) or MLflow | Logging runs, metrics, artifacts |
| **Reference Management** | Zotero + Better BibTeX | Citation management for LaTeX |

### Initial Machine Setup Script (Ubuntu/Debian)

```bash
# System essentials
sudo apt update && sudo apt install -y git curl wget htop tmux nvtop build-essential

# Miniforge (conda replacement, faster)
wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh
bash Miniforge3-Linux-x86_64.sh

# Set up Git identity
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
git config --global core.editor "code --wait"

# Generate SSH key for GitHub
ssh-keygen -t ed25519 -C "your@email.com"
cat ~/.ssh/id_ed25519.pub  # Copy this to GitHub Settings → SSH Keys
```

> [!TIP]
> Store your dotfiles (`.bashrc`, `.tmux.conf`, `.gitconfig`) in a private GitHub repository. When you get a new machine or HPC allocation, a single `git clone` restores your entire environment.

---

## 3. Establishing Your Reading Habit

The most important skill in graduate school is the ability to read, comprehend, and synthesize research papers efficiently.

### The 10-Paper Rule

In your first month, read **10 foundational papers** in your area cover-to-cover. Not skimming — reading, annotating, summarizing. This builds the conceptual vocabulary you need to understand everything else.

To find these 10 papers:
1. Ask your advisor: "What are the 5 papers I absolutely must read to work in this lab?"
2. Search your prospective topic on [Semantic Scholar](https://www.semanticscholar.org/), sort by Most Cited, read the top results.
3. Use [Connected Papers](https://www.connectedpapers.com/) on the most cited paper to find the foundational cluster.

> See [guides/03-reading-papers.md](./03-reading-papers.md) for a structured paper reading methodology.

---

## 4. Building Your Academic Digital Presence

Your online research presence is your professional reputation made visible. Build it early.

### Essential Profiles

* **[Google Scholar](https://scholar.google.com/)** — Create a public profile immediately. This is the canonical source for your citation count and h-index.
* **[ORCID](https://orcid.org/)** — Create a persistent researcher identifier. Required by many journals and grants. Free and takes 2 minutes.
* **[Semantic Scholar](https://www.semanticscholar.org/me/research)** — Claim your author profile to get citation alerts.
* **[GitHub](https://github.com/)** — Your public portfolio. Pin your best repositories.
* **Personal Website** — Even a simple GitHub Pages site with your name, institution, research interests, and publications list is sufficient.

### Website Generators for Researchers

| Tool | Complexity | Best For |
| :--- | :--- | :--- |
| **[Academic Pages](https://academicpages.github.io/)** | Low (GitHub Pages template) | Fast setup, minimal customization |
| **[al-folio](https://github.com/alshedivat/al-folio)** | Medium (Jekyll) | Clean, popular in ML/AI community |
| **[Hugo Academic](https://themes.gohugo.io/themes/hugo-academic/)** | Medium (Hugo) | Fast builds, excellent publication support |
| **[Quarto](https://quarto.org/)** | Medium | Integrates code, papers, and blog posts |

---

## 5. Financial Survival

Graduate stipends are notoriously low. Optimizing your finances is a real research-enabler.

### Academic Discounts and Free Resources

- **Software**: Claim everything in [stack/academic-perks.md](../stack/academic-perks.md) immediately.
- **Textbooks**: [Library Genesis](https://libgen.is/), [Z-Library](https://z-library.se/), [Anna's Archive](https://annas-archive.org/) — all major academic texts for free.
- **Journals**: Use your library VPN or [Unpaywall](https://unpaywall.org/) browser extension for legal open-access PDF retrieval.
- **Conference Travel**: Apply for student travel grants from IEEE, ACM, and the organizing conference. Always ask your advisor first — labs often have travel funds.
- **Tax**: In many countries, PhD stipends have special tax treatment. Consult your country-specific academic payroll office early.

---

## 6. Finding Your Research Community

Graduate school is not a solo endeavor. Your community is a research accelerant.

### Where CS Researchers Gather

| Platform | Purpose |
| :--- | :--- |
| **[Twitter/X](https://twitter.com/)** | Real-time paper discussions, researcher networking |
| **[LinkedIn](https://linkedin.com/)** | Professional networking, internship connections |
| **[Research Gate](https://www.researchgate.net/)** | Author profiles, paper requests |
| **[Reddit r/MachineLearning](https://www.reddit.com/r/MachineLearning/)** | ML community news and paper discussions |
| **[Discord servers](https://discord.com/)** | ML Collective, Eleuther AI, Papers with Code |
| **Local Meetups** | Department seminars, reading groups, local ACM/IEEE chapters |

---

## Further Reading

* [How to Succeed in Graduate School](https://www.cs.cmu.edu/~harchol/gradschooltalk.pdf) — Mor Harchol-Balter (CMU)
* [Applying to PhD Programs in CS](https://www.cs.cmu.edu/~harchol/gradschooltalk.pdf)
* [Notes on the PhD](https://matt.might.net/articles/phd-school-in-pictures/) — Matt Might's famous illustrated explanation
* [The Missing Semester of Your CS Education](https://missing.csail.mit.edu/) — MIT: shell, git, tmux, debugging
