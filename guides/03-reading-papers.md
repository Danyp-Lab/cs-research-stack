# 📄 How to Read Research Papers

> *"You don't read papers — you interrogate them."*

Reading research papers is a learnable skill, not a talent. The average senior PhD student reads **3–5 papers per week** and can extract the core contribution of any paper within 15–20 minutes. This guide teaches the systematic methods that experienced researchers use — from skimming to deep comprehension — and the tools to manage what you read.

---

## 1. Why Naive Reading Fails

Most graduate students read papers the wrong way: linearly, from abstract to conclusion, trying to understand every sentence before moving on. This approach has two critical flaws:

1. **It is inefficient**: You spend equal time on filler and on critical contributions.
2. **It lacks context**: Without a framework for where this paper fits in the literature, individual sentences are difficult to interpret.

The structured methods below fix both problems.

---

## 2. The Three-Pass Method

Developed by S. Keshav ([How to Read a Paper](http://blizzard.cs.uwaterloo.ca/keshav/home/Papers/data/07/paper-reading.pdf)), this method is the most widely taught in CS graduate programs.

### Pass 1 — The Bird's-Eye View (5–10 minutes)

**Goal**: Decide whether the paper deserves further reading.

Read only:
- Title, abstract, introduction
- Section headings and subheadings
- Conclusion
- References (skim for familiar names and venues)

**Output**: Answers to the five Cs:
1. **Category**: What type of paper is this? (Measurement, analysis, new system, survey?)
2. **Context**: What related work does it build on?
3. **Correctness**: Do the assumptions seem valid?
4. **Contributions**: What are the paper's main claims?
5. **Clarity**: Is the paper well-written enough to understand?

### Pass 2 — The Engaged Reading (30–60 minutes)

**Goal**: Understand the main content, not the implementation details.

Read carefully, but skip proofs and mathematical derivations for now:
- Read the Introduction carefully — it contains the paper's argument in compressed form.
- Study all figures, tables, and graphs. Caption text is especially information-dense.
- Annotate key claims, methods, and results.
- Note any unfamiliar terms — look them up immediately.

**Output**: A 3–5 sentence summary of the paper in your own words. If you cannot produce this, you have not understood the paper.

### Pass 3 — The Deep Dive (1–5 hours)

**Goal**: Virtually re-implement the paper.

- Read every section, including proofs and appendices.
- Challenge every assumption and experimental choice.
- Identify the paper's weaknesses.
- Reproduce the key equations and algorithms on paper.

**Output**: Full understanding sufficient to (a) explain the paper to someone else, and (b) build on it in your own research.

> [!TIP]
> Not every paper warrants Pass 3. Pass 1 for most papers, Pass 2 for relevant ones, Pass 3 only for papers directly in your research core.

---

## 3. Critical Reading Framework

Good researchers do not just understand papers — they critique them.

### Questions to Ask During Reading

#### On the Problem

- Is this problem important and well-motivated?
- Is the problem statement precise, or is it vague enough to allow easy wins?
- What would happen if this problem were left unsolved?

#### On the Method

- Are the assumptions realistic?
- Is there a simpler baseline that would achieve similar results?
- What are the failure modes of this approach?

#### On the Evaluation

- Is the evaluation on a benchmark that the authors may have overfit to?
- Are the baselines fair and up-to-date?
- Are the improvements statistically significant?
- Do the experiments support the claims in the introduction?

#### On the Contribution

- What is the **actual** novel contribution vs. the claimed contribution?
- Could this result be an artifact of experimental setup?
- How does this change what you thought you knew?

---

## 4. Annotation Strategies

### Margin Annotation System

Use a consistent annotation vocabulary across all papers. Suggested symbols:

| Symbol | Meaning |
| :--- | :--- |
| `★` | Key claim or contribution |
| `?` | Something I don't understand yet |
| `!` | Surprising or counterintuitive result |
| `→` | Leads to a follow-up question |
| `✗` | I disagree with or doubt this |
| `📖` | New reference I should read |
| `💡` | My own idea sparked by this |

### Digital Annotation Tools

* **[Zotero](https://www.zotero.org/) + PDF reader** — Annotate PDFs and sync notes directly to your library. Annotations are searchable.
* **[Hypothes.is](https://hypothes.is/)** — Web annotation layer for HTML papers (arXiv, ACM, IEEE).
* **[Readwise Reader](https://readwise.io/read)** — Highlight and annotate PDFs and web articles with AI-assisted synthesis.
* **[Obsidian](https://obsidian.md/)** — Write permanent notes linked to paper entries.

---

## 5. Building a Paper Notes System

Reading without note-taking is reading with a leaky bucket. A systematic notes system transforms consumed papers into reusable research capital.

### The Atomic Note Template

For every paper you read at Pass 2 or deeper, create a note with this structure:

```markdown
# [Paper Title] — [Year]

**Authors**: 
**Venue**: 
**Link**: 
**Read Date**: 

## Core Contribution (1–3 sentences)

## Key Method

## Key Results

## Limitations / Weaknesses

## My Questions

## How This Relates to My Research

## References to Follow Up
```

> [!NOTE]
> The "How This Relates to My Research" field is the most important. If you cannot write something here, the paper may not belong in your permanent notes at all.

### The Zettelkasten Approach

The Zettelkasten method (popularized by sociologist Niklas Luhmann, who wrote over 70 books using it) builds a network of atomic notes that link to each other. In Obsidian, this creates a visual knowledge graph where concepts in your research area are interconnected.

---

## 6. Managing Your Reading Queue

### Literature Discovery Pipeline

```
New paper alert (Google Scholar, arXiv, Twitter)
           ↓
    Pass 1 filter (5 min)
           ↓
    Add to Zotero library
           ↓
    Tag and categorize
           ↓
    Schedule for Pass 2/3 based on relevance
```

### Setting Up Paper Alerts

* **[Google Scholar Alerts](https://scholar.google.com/)** — Set keyword and author alerts to receive email notifications of new publications.
* **[arXiv.org RSS feeds](https://arxiv.org/)** — Subscribe to specific subject areas (cs.LG, cs.CV, cs.NI, etc.) via RSS.
* **[Semantic Scholar Research Feeds](https://www.semanticscholar.org/)** — Personalized paper recommendations based on your reading history.
* **[Papers With Code](https://paperswithcode.com/)** — Tracks new papers with associated code and leaderboard results.
* **[Twitter/X Lists](https://twitter.com/)** — Follow the top 20 researchers in your area; they share the important papers as they come out.

---

## 7. Speed and Efficiency

### Increasing Reading Speed

1. **Read the abstract and conclusion first** — they contain 80% of the paper's message.
2. **Read figures before text** — figures are the paper's argument compressed to its most visual form. If you understand all figures, you likely understand the paper.
3. **Use 1.5× playback speed for video explanations** — Many major papers now have [Papers with Code explanations](https://paperswithcode.com/) or YouTube walkthroughs.
4. **Read in batches** — Read 3–5 papers in the same cluster consecutively. Context from Paper 1 makes Paper 2 dramatically faster.

### When to Stop Reading

- If a paper is clearly outside your research scope after Pass 1 — stop.
- If the methodology is so flawed that the results are meaningless — stop.
- If a better, more recent paper has superseded it entirely — read the newer one instead.

---

## 8. Talking About Papers

Reading is passive. The test of understanding is explanation.

### Weekly Lab Reading Groups

A structured weekly reading group where members take turns presenting a paper is one of the highest-leverage activities in a research lab. The presenter gains deeper understanding through the act of teaching; the audience gets a curated survey.

### Paper Presentation Structure

A 20-minute paper presentation should cover:
1. **Motivation** (2 min): Why does this problem matter?
2. **Prior art** (2 min): What did people do before this paper?
3. **Core contribution** (5 min): What is genuinely new?
4. **Key results** (5 min): Walk through the most important figures and tables.
5. **Critique** (3 min): What are the weaknesses?
6. **Discussion** (3 min): How does this affect our own research?

---

## Further Reading

* [How to Read a Paper](http://blizzard.cs.uwaterloo.ca/keshav/home/Papers/data/07/paper-reading.pdf) — S. Keshav (canonical 3-pass method)
* [How to Read a Research Paper](https://www.eecs.harvard.edu/~michaelm/postscripts/ReadPaper.pdf) — Michael Mitzenmacher (Harvard)
* [Efficient Reading of Papers in Science and Technology](https://www.cs.columbia.edu/~hgs/netbib/efficientReading.pdf) — Michael Hanson
