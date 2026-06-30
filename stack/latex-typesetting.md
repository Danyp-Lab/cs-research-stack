# 📝 LaTeX Typesetting for Researchers

> LaTeX is the lingua franca of academic publishing in CS, mathematics, physics, and engineering. Mastering it transforms your paper-writing workflow from a formatting struggle into a creative act.

---

## 1. Getting Started

### Installation

| Platform | Distribution | Command |
| :--- | :--- | :--- |
| **Ubuntu/Debian** | TeX Live | `sudo apt install texlive-full` |
| **macOS** | MacTeX | `brew install --cask mactex` |
| **Windows** | MiKTeX | [miktex.org](https://miktex.org/) |
| **Cloud** | Overleaf | [overleaf.com](https://www.overleaf.com/) (no install) |

> [!TIP]
> For local installation, **TeX Live Full** installs everything. While large (~7GB), it prevents "missing package" errors mid-compile. On Overleaf, all packages are pre-installed.

### Minimal Working Example

```latex
\documentclass[12pt,a4paper]{article}

\usepackage[utf8]{inputenc}
\usepackage{amsmath, amssymb}
\usepackage{graphicx}
\usepackage{hyperref}
\usepackage{booktabs}
\usepackage{microtype}

\title{My Research Paper}
\author{Your Name \\ \texttt{your@email.com}}
\date{\today}

\begin{document}
\maketitle
\begin{abstract}
  Your abstract here.
\end{abstract}

\section{Introduction}
Your introduction here~\cite{key}.

\bibliographystyle{plain}
\bibliography{references}
\end{document}
```

---

## 2. Conference and Journal Templates

Always use the official template for your target venue. Never submit without it.

| Venue Family | Template |
| :--- | :--- |
| **NeurIPS, ICML, ICLR** | `\usepackage{neurips_2024}` — from venue website |
| **CVPR, ICCV, ECCV** | CVPR LaTeX template from [cvf.com](https://cvf.com/) |
| **ACM (SIGCOMM, SOSP, etc.)** | `\documentclass[sigconf]{acmart}` |
| **IEEE (TPAMI, ICDM, etc.)** | `\documentclass[journal]{IEEEtran}` |
| **USENIX (ATC, OSDI, etc.)** | USENIX template from venue website |
| **Springer (LNCS)** | `\documentclass{llncs}` |
| **Thesis** | Your university's official thesis template |

---

## 3. Essential Packages Reference

### Mathematics

```latex
\usepackage{amsmath}      % align, equation, cases environments
\usepackage{amssymb}      % \mathbb{R}, \mathcal{L}, etc.
\usepackage{amsthm}       % Theorem, Lemma, Proof environments
\usepackage{mathtools}    % Extends amsmath (dcases, pmatrix, etc.)
\usepackage{bm}           % Bold math symbols: \bm{\theta}
\usepackage{physics}      % Dirac notation, derivatives made easy
```

**Common math patterns:**

```latex
% Equation with label
\begin{equation}
  \mathcal{L}(\theta) = -\frac{1}{N}\sum_{i=1}^{N} y_i \log \hat{y}_i
  \label{eq:cross-entropy}
\end{equation}

% Aligned multi-line equations
\begin{align}
  f(x) &= ax^2 + bx + c \\
       &= a\left(x + \frac{b}{2a}\right)^2 + c - \frac{b^2}{4a}
\end{align}

% Matrix
\begin{pmatrix} a & b \\ c & d \end{pmatrix}

% Cases (piecewise function)
f(x) = \begin{cases} 1 & \text{if } x > 0 \\ 0 & \text{otherwise} \end{cases}
```

### Tables

```latex
\usepackage{booktabs}     % \toprule, \midrule, \bottomrule
\usepackage{multirow}     % Multi-row cells
\usepackage{tabularx}     % Auto-width tables
\usepackage{siunitx}      % Aligned decimal numbers in tables

% Professional table example
\begin{table}[t]
  \centering
  \caption{Comparison of methods on Dataset X.}
  \label{tab:results}
  \begin{tabular}{lSSS}
    \toprule
    Method & {Accuracy (\%)} & {F1 Score} & {Time (s)} \\
    \midrule
    Baseline & 72.3 & 0.71 & 1.2 \\
    Our Method & \textbf{84.7} & \textbf{0.85} & 1.8 \\
    \bottomrule
  \end{tabular}
\end{table}
```

### Figures

```latex
\usepackage{graphicx}
\usepackage{subcaption}   % Subfigures with (a), (b) labels

% Single figure
\begin{figure}[t]
  \centering
  \includegraphics[width=0.9\linewidth]{figures/architecture.pdf}
  \caption{System architecture overview.}
  \label{fig:arch}
\end{figure}

% Side-by-side subfigures
\begin{figure}[t]
  \centering
  \begin{subfigure}{0.48\linewidth}
    \includegraphics[width=\linewidth]{fig_a.pdf}
    \caption{Result A}
  \end{subfigure}
  \hfill
  \begin{subfigure}{0.48\linewidth}
    \includegraphics[width=\linewidth]{fig_b.pdf}
    \caption{Result B}
  \end{subfigure}
  \caption{Comparison of results.}
  \label{fig:comparison}
\end{figure}
```

### Algorithms

```latex
\usepackage[ruled,vlined]{algorithm2e}

\begin{algorithm}[t]
\caption{Gradient Descent}
\KwIn{Learning rate $\eta$, initial $\theta_0$}
\KwOut{Optimal $\theta^*$}
\For{$t = 1, 2, \ldots, T$}{
  Compute gradient $g_t = \nabla_\theta \mathcal{L}(\theta_{t-1})$\;
  Update $\theta_t \leftarrow \theta_{t-1} - \eta g_t$\;
}
\Return $\theta_T$
\end{algorithm}
```

---

## 4. TikZ Diagrams

TikZ is the most powerful LaTeX drawing library. Use it for architecture diagrams, flowcharts, and neural network figures.

```latex
\usepackage{tikz}
\usetikzlibrary{shapes, arrows.meta, positioning}

% Simple neural network layer diagram
\begin{tikzpicture}[
  node/.style={circle, draw, minimum size=0.8cm},
  layer/.style={rectangle, draw, minimum height=2cm, minimum width=0.6cm, fill=blue!20}
]
  \node[layer] (l1) {Input};
  \node[layer, right=1cm of l1] (l2) {Hidden};
  \node[layer, right=1cm of l2] (l3) {Output};
  \draw[->] (l1) -- (l2);
  \draw[->] (l2) -- (l3);
\end{tikzpicture}
```

**TikZ Resources:**
* **[Awesome LaTeX Drawing](https://github.com/xinychen/awesome-latex-drawing)** — Pre-built source code for neural networks, Bayesian graphs, and more.
* **[TikZiT](https://tikzit.github.io/)** — GUI editor for TikZ graphs.
* **[PGFPlots](https://pgfplots.sourceforge.net/)** — Native LaTeX plotting (no Python/matplotlib required).

---

## 5. Bibliography Management

### BibTeX with Zotero

Use [Better BibTeX](https://retorque.re/zotero-better-bibtex/) + Zotero to auto-export `.bib` files:

1. In Zotero: `File → Export Library → Better BibTeX → Auto-update`
2. Point your LaTeX project to this exported `.bib` file.
3. Add references: `\cite{authorYearKeyword}`

### Bibliography Styles by Venue

```latex
% Author-year (NeurIPS, ICML)
\usepackage{natbib}
\bibliographystyle{abbrvnat}

% Numbered (IEEE, ACM)
\bibliographystyle{IEEEtran}

% Alpha style (theory papers)
\bibliographystyle{alpha}
```

### BibLaTeX (Modern Alternative)

```latex
\usepackage[backend=biber, style=authoryear]{biblatex}
\addbibresource{references.bib}
% In document body:
\printbibliography
```

---

## 6. Compilation Workflow

```bash
# Standard compilation (with bibliography)
pdflatex paper.tex
bibtex paper
pdflatex paper.tex
pdflatex paper.tex

# Or use latexmk (automated, handles reruns)
latexmk -pdf paper.tex

# Continuous compilation with live preview
latexmk -pdf -pvc paper.tex

# Word count (excluding LaTeX commands)
texcount paper.tex
detex paper.tex | wc -w
```

---

## 7. Common Errors and Fixes

| Error | Cause | Fix |
| :--- | :--- | :--- |
| `Undefined control sequence` | Missing package | Add `\usepackage{package}` |
| `Overfull \hbox` | Line too long | Add `\sloppy` or rewrite the sentence |
| Citation `[?]` | `.bib` file not found or not compiled | Run `bibtex`, then recompile twice |
| Font not found | Missing font package | Install `texlive-fonts-extra` |
| `\textbf` in math mode | Wrong command | Use `\mathbf{}` or `\bm{}` |

---

## Further Reading

* [The Not So Short Introduction to LaTeX](https://tobi.oetiker.ch/lshort/lshort.pdf) — Oetiker et al. (canonical beginner's guide)
* [LaTeX Wikibook](https://en.wikibooks.org/wiki/LaTeX)
* [Detexify](http://detexify.kirelabs.org/) — Draw a symbol, get the LaTeX command
