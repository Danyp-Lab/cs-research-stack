# 📄 CV, Research Statement & Portfolio

> Your academic CV and research statement are the primary instruments by which the world forms its first impression of you as a researcher. Invest in them proportionally.

---

## 1. The Academic CV vs. Industry Résumé

These are fundamentally different documents with different audiences and purposes.

| Dimension | Academic CV | Industry Résumé |
| :--- | :--- | :--- |
| **Length** | 2–10+ pages (grows with career) | 1–2 pages maximum |
| **Goal** | Document complete scholarly record | Demonstrate impact and fit in 6 seconds |
| **Publications** | Full section, all papers listed | Selected; may summarize |
| **Teaching** | Dedicated section | Usually omitted |
| **Grants** | Dedicated section | Usually omitted |
| **Format** | Chronological, dense | Scannable, achievement-focused |
| **Objective** | Establish scholarly identity | Pass ATS filters, impress hiring manager |

---

## 2. Academic CV Structure

### Standard Section Order

```
1. Header (Name, contact info, website, Google Scholar, GitHub)
2. Research Interests (2–4 keywords or phrases)
3. Education (reverse chronological, GPA optional)
4. Publications
   4a. Peer-reviewed Conference Papers
   4b. Journal Articles
   4c. Workshop Papers / Extended Abstracts
   4d. Preprints / Under Review
5. Awards and Honors
6. Research Experience
7. Teaching Experience
8. Invited Talks and Presentations
9. Professional Service (reviewing, organizing)
10. Skills (languages, frameworks — brief)
11. References
```

### Publications Section Formatting

Follow the standard in your community. Most CS academics use:

```latex
\textbf{Your Name}, Co-author A, Co-author B.
``Paper Title.''
\textit{Proceedings of Conference Name (VENUE)}, Year.
[\href{https://arxiv.org/abs/...}{PDF}]
[\href{https://github.com/...}{Code}]
```

Bold your name in every author list. Include direct links to PDFs and code.

---

## 3. Writing the Research Statement

A research statement is a 2–5 page narrative document required for faculty job applications and some graduate fellowships. It answers three questions:

1. **What problems do you work on, and why do they matter?**
2. **What have you accomplished so far?**
3. **Where is your research going?**

### Structure Template

```
Introduction (0.5 page)
— Hook: compelling motivation for your research area
— Your positioning: what makes your approach distinctive
— Explicit statement of 2–3 research thrusts

Research Thrust 1 (0.75–1 page)
— Problem
— Your approach
— Key result (with numbers)
— Publication reference

Research Thrust 2 (0.75–1 page)
— Same structure

Research Thrust 3 (0.75–1 page)
— Same structure

Future Work (0.5–1 page)
— Near-term: 1–2 year concrete goals
— Long-term: 5-year vision for the research program

[Optional] Teaching/Mentoring Philosophy (0.5 page for faculty applications)
```

> [!IMPORTANT]
> Every factual claim must be backed by a publication or a concrete result. "My research improves efficiency" is weak. "My work reduces inference latency by 3.2× on standard benchmarks (VENUE 2024)" is strong.

---

## 4. Building a Research Portfolio Website

Your personal website is the public face of your research identity. A researcher without a website is invisible.

### Minimal Required Content

- [ ] Name and current affiliation
- [ ] Professional photo
- [ ] Research interests (2–3 sentences)
- [ ] Publications list with PDF/code links
- [ ] CV download link
- [ ] Contact information

### Recommended Templates

| Template | Framework | Demo |
| :--- | :--- | :--- |
| **[al-folio](https://github.com/alshedivat/al-folio)** | Jekyll | Most popular in ML community |
| **[Academic Pages](https://github.com/academicpages/academicpages.github.io)** | Jekyll | Easy GitHub Pages setup |
| **[Hugo Academic (Blox)](https://hugoblox.com/)** | Hugo | Fast, clean, flexible |
| **[Quarto](https://quarto.org/)** | Quarto | Integrates code and publications |

### Google Scholar Profile Setup

1. Go to [scholar.google.com](https://scholar.google.com/) and create a profile.
2. Set your profile to **public**.
3. Add your institution.
4. Verify all your papers are correctly attributed.
5. Set up **citation alerts** for your own papers.

---

## 5. Industry-Facing CV and Portfolio

When targeting industry roles (even research roles), adapt your presentation:

### Key Differences

- **Lead with impact**: "Reduced model training time by 40%" beats "Developed new training algorithm"
- **Quantify everything**: Users, latency, accuracy, scale
- **Emphasize code**: GitHub links, open-source contributions are taken seriously
- **Trim publications**: List only top venues; industry cares about NeurIPS, not your workshop
- **Show systems thinking**: Describe deployed systems, not just academic results

### GitHub as Portfolio

Your GitHub profile is your engineering portfolio:

- **Pin your best 6 repositories**
- Each repo should have a clean README with: What it does, how to install, how to run, results
- Include at least one implementation of a key paper
- Maintain consistent commit history (irregular commits signal low engagement)

---

## 6. LaTeX CV Templates

```latex
% Minimal professional CV header
\begin{center}
  {\LARGE \textbf{Your Name}} \\[0.5em]
  \href{mailto:you@university.edu}{you@university.edu} $\cdot$
  \href{https://yourwebsite.com}{yourwebsite.com} $\cdot$
  \href{https://github.com/you}{github.com/you} $\cdot$
  \href{https://scholar.google.com/citations?user=...}{Google Scholar}
\end{center}
```

**Recommended LaTeX CV packages:**
* **[moderncv](https://ctan.org/pkg/moderncv)** — Professional, clean; multiple style options
* **[awesome-cv](https://github.com/posquit0/Awesome-CV)** — Visually distinctive; great for industry applications
* **[altacv](https://github.com/liantze/AltaCV)** — Modern two-column layout

---

## Further Reading

* [Awesome PhD CV](https://github.com/LimHyungTae/Awesome-PhD-CV) — LaTeX templates and examples
* [How to Write an Academic CV](https://www.jobs.ac.uk/careers-advice/cv-templates/1168/academic-cv) — jobs.ac.uk
* [Research Statement Advice](https://cs.brown.edu/~sk/Memos/Grad-Advice/) — Shriram Krishnamurthi (Brown)
