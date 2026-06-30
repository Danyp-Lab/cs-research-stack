# 📚 Literature Tools & Research Discovery

> Finding, evaluating, and managing the research literature is a full-time meta-skill. This page covers the full pipeline from discovery to citation management.

---

## 1. Finding Papers

### Primary Search Engines

| Tool | Strengths | Best Used For |
| :--- | :--- | :--- |
| **[Semantic Scholar](https://www.semanticscholar.org/)** | AI-extracted TLDRs, influential citation detection, free API | Quick comprehension, citation impact analysis |
| **[Google Scholar](https://scholar.google.com/)** | Broadest coverage, citation tracking, alerts | General search, h-index tracking |
| **[dblp](https://dblp.org/)** | CS-specific, curated, zero duplicate entries | Finding all papers by an author in CS venues |
| **[arXiv](https://arxiv.org/)** | Preprints, immediate access, no paywall | Cutting-edge work before peer review |
| **[Papers With Code](https://paperswithcode.com/)** | Links papers to GitHub code + benchmark leaderboards | Finding reproducible ML implementations |
| **[ACM Digital Library](https://dl.acm.org/)** | Authoritative source for ACM papers | Systems, HCI, programming languages |
| **[IEEE Xplore](https://ieeexplore.ieee.org/)** | Authoritative for IEEE papers | Networking, hardware, signal processing |

### Specialized Discovery Tools

* **[Connected Papers](https://www.connectedpapers.com/)** — Visual force-directed graph of papers connected by co-citation. Explore a "galaxy" of related work from one seed paper.
* **[ResearchRabbit](https://www.researchrabbit.ai/)** — "Spotify for papers" — recommends related papers based on your collection. Free.
* **[Litmaps](https://www.litmaps.com/)** — Timeline-based citation map showing how ideas evolved over time.
* **[Inciteful](https://inciteful.xyz/)** — Finds the most important papers connecting two specific papers.
* **[OpenReview](https://openreview.net/)** — Access submitted and rejected papers with full reviewer comments at top ML venues.

---

## 2. Evaluating Venue Quality

### Conference Rankings

**[CORE Rankings Portal](http://portal.core.edu.au/conf-ranks/)** is the standard for CS conference prestige:

| Tier | Meaning | Examples |
| :--- | :--- | :--- |
| **A\*** | Flagship, highest prestige | NeurIPS, ICML, CVPR, SOSP, SIGCOMM, PLDI |
| **A** | Excellent, highly competitive | ICCV, ECCV, USENIX ATC, EuroSys, OOPSLA |
| **B** | Good, solid venue | BMVC, Middleware, LCTES |
| **C** | Acceptable | Lower-tier venues |

### Journal Rankings

* **[SCImago Journal Rank (SJR)](https://www.scimagojr.com/)** — Eigenvector-based prestige ranking. Q1 = top 25%, Q4 = bottom 25%.
* **[Clarivate JCR](https://clarivate.com/products/scientific-and-academic-research/research-analytics-evaluation-and-management-solutions/journal-citation-reports/)** — Journal Impact Factor (JIF). Requires institutional access.
* **[DOAJ (Directory of Open Access Journals)](https://doaj.org/)** — Verify that a journal is legitimate open-access (not predatory).

### Predatory Venue Detection

> [!WARNING]
> Predatory conferences and journals accept papers for a fee with no real peer review. Publishing there can permanently damage your reputation.

Red flags for predatory venues:
- Unsolicited email invitations with flattery
- Acceptance promised within 24–48 hours
- Article Processing Charges (APCs) revealed only after acceptance
- No verifiable editorial board
- URL variations of legitimate venue names (e.g., `ieeexplore-conf.com`)

Verification tools:
* **[Think. Check. Submit.](https://thinkchecksubmit.org/)** — Structured checklist for evaluating venues.
* **[Beall's List Archive](https://beallslist.net/)** — Historical record of known predatory publishers.
* **[COPE (Committee on Publication Ethics)](https://publicationethics.org/)** — Verify if a journal/publisher is COPE-compliant.

---

## 3. Reference Management

### Zotero (Recommended)

[Zotero](https://www.zotero.org/) is the gold standard for academic reference management. It is free, open-source, and deeply integrated with LaTeX.

**Setup:**
1. Install Zotero desktop app.
2. Install the Zotero browser connector (Chrome/Firefox).
3. Install [Better BibTeX](https://retorque.re/zotero-better-bibtex/) plugin for LaTeX integration.
4. Create collections for each project/topic.
5. Configure Better BibTeX citation key format: `[auth:lower][year][title:skipwords:lower:select=1,1]`

**Power features:**
- **Browser save**: Click connector on any paper page — Zotero saves metadata and PDF automatically.
- **Auto-sync `.bib` file**: Better BibTeX can auto-export a `.bib` file to your LaTeX project directory. It updates when you add papers.
- **PDF annotation**: Annotate PDFs directly in Zotero; annotations are stored as notes.
- **Group libraries**: Share a Zotero library with your lab group.

### Alternative Reference Managers

| Tool | Free Tier | LaTeX Integration | Cloud Sync | Best For |
| :--- | :--- | :--- | :--- | :--- |
| **Zotero** | 300MB free | Excellent (Better BibTeX) | Yes | Most users |
| **Mendeley** | 2GB free | Good | Yes | PDF annotation heavy users |
| **JabRef** | Unlimited | Native (BibTeX-native) | Manual | LaTeX purists |
| **Paperpile** | Paid ($3/mo student) | Good | Google Drive | Google Docs users |
| **EndNote** | Institutional | Good | Yes | Biomedical fields |

---

## 4. Staying Current

### Alert Systems

Set up automated alerts to stay current without spending hours searching:

```
# Google Scholar Alert
1. Go to scholar.google.com
2. Search your topic keywords
3. Click "Create alert" at the bottom of results
4. Set delivery frequency (weekly recommended)
```

* **[arXiv RSS](https://arxiv.org/help/rss)** — Subscribe to `cs.LG`, `cs.CV`, `cs.AI`, `cs.NI` etc. in any RSS reader.
* **[Semantic Scholar Research Feeds](https://www.semanticscholar.org/)** — AI-powered recommendations based on your reading history.
* **[Twitter/X lists](https://twitter.com/)** — Create a curated list of researchers in your area. They share important papers in real time.
* **[HuggingFace Daily Papers](https://huggingface.co/papers)** — Curated list of the day's most important ML preprints.

### Reading Group Structure

A weekly lab reading group is the most efficient way to stay current as a team:

| Role | Responsibility |
| :--- | :--- |
| **Presenter** | Selects and presents one paper (20 min talk + 10 min discussion) |
| **Critic** | Prepares 3 specific questions or criticisms in advance |
| **Connector** | Identifies how this paper connects to lab's current projects |
| **Moderator** | Keeps time, ensures everyone contributes |

Rotate roles each week. Record sessions if permitted.

---

## 5. Building a Literature Review

A systematic literature review for a thesis or survey paper requires a structured methodology:

### PRISMA Flow (for systematic reviews)

```
Identification: N papers found via search
        ↓
Screening: Remove duplicates; apply title/abstract filter
        ↓
Eligibility: Apply full-text inclusion/exclusion criteria
        ↓
Included: Final set of papers for review
```

### Tools for Systematic Reviews

* **[Rayyan](https://www.rayyan.ai/)** — Collaborative screening tool for systematic reviews. Free for academics.
* **[Covidence](https://www.covidence.org/)** — Cochrane's systematic review platform. Institutional access often available.
* **[Elicit](https://elicit.org/)** — AI-powered literature review assistant. Extracts structured data from papers.

---

## Further Reading

* [Literature Review Guide](https://www.library.cornell.edu/research/literature-review) — Cornell University Library
* [How to Do a Systematic Literature Review in CS](https://arxiv.org/abs/1512.08386) — Kitchenham & Charters guidelines
