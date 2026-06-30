# ✅ Paper Submission Checklist

> Use this checklist before every paper submission. A missed item at submission time can mean rejection, disqualification, or an embarrassing post-acceptance correction.

---

## Phase 1: Research Completeness (2–4 weeks before deadline)

### Experiments

- [ ] All baseline comparisons are fair and complete (same data, same evaluation protocol)
- [ ] Ablation studies isolate the contribution of each major component
- [ ] Results are reported over multiple seeds with mean ± std
- [ ] Statistical significance is assessed where appropriate
- [ ] Test set was used **only once** to report final results
- [ ] No data leakage between train/val/test splits

### Reproducibility

- [ ] All random seeds are fixed and reported
- [ ] Hardware and software environment is documented
- [ ] Full hyperparameter configuration is available (in paper or supplementary)
- [ ] Code is ready for release (or will be before camera-ready)
- [ ] Model checkpoints are saved for the results being reported

---

## Phase 2: Writing Completeness (1–2 weeks before deadline)

### Content

- [ ] Abstract clearly states the problem, approach, and key results (with numbers)
- [ ] Introduction has an explicit **contributions list** (3–5 bullet points)
- [ ] Related work addresses all directly competing methods
- [ ] Methodology section is sufficiently detailed for an independent implementation
- [ ] Experiments section explains the experimental setup completely
- [ ] Every claim is backed by evidence (citation, equation, or empirical result)
- [ ] Limitations are acknowledged (required by most venues)
- [ ] Ethics/Broader Impacts statement is complete (required by NeurIPS, ICML, etc.)

### Figures and Tables

- [ ] All figures have captions that are self-contained (understandable without reading the text)
- [ ] All tables have captions
- [ ] Axes are labeled with units
- [ ] All figures use colorblind-safe palettes
- [ ] Error bars or confidence intervals are visible where applicable
- [ ] Figures are in vector format (PDF) or ≥300 DPI raster

### Equations

- [ ] All variables in equations are defined
- [ ] Equation numbering is consistent
- [ ] Mathematical notation is consistent throughout
- [ ] Equations compile without errors

---

## Phase 3: Formatting and Compliance (3–7 days before deadline)

### Venue Compliance

- [ ] Correct venue template is being used (download fresh from venue website)
- [ ] Paper is within the page limit (not counting references and appendix, unless specified otherwise)
- [ ] Supplementary material is within the page/file-size limit
- [ ] Paper is **anonymized** if the venue uses double-blind review:
  - [ ] Author names and affiliations removed
  - [ ] "In our previous work [X]" → "In prior work [X]"
  - [ ] GitHub links removed or anonymized (use [Anonymous GitHub](https://anonymous.4open.science/))
  - [ ] Acknowledgments section removed
  - [ ] No identifying metadata in PDF properties

### Technical

- [ ] PDF compiles without errors or warnings
- [ ] PDF is below the file size limit (typically 10–50 MB)
- [ ] All fonts are embedded (check with `pdffonts paper.pdf`)
- [ ] All hyperlinks work (test every URL)
- [ ] References are complete — no `[?]` or `??` placeholders
- [ ] BibTeX entries are complete and formatted correctly
- [ ] No "TODO" or "FIXME" comments remaining in the paper

### Final Read-through

- [ ] Read the paper aloud once — catches awkward phrasing and missing words
- [ ] Have at least one person outside your immediate lab read the paper
- [ ] Check the abstract matches the paper's actual contributions
- [ ] Check the conclusion matches the paper's actual results

---

## Phase 4: Submission Day

### Submission System

- [ ] Account is created on the submission system (CMT, HotCRP, OpenReview, etc.)
- [ ] Paper is registered (often a separate step from submission with an abstract deadline)
- [ ] Authors are listed in the correct order with correct affiliations
- [ ] Keywords/topics are selected appropriately
- [ ] Conflicts of interest are declared correctly
- [ ] Supplementary materials uploaded in the correct format

### Final Checks

- [ ] Submit **at least 2 hours before the deadline** — systems crash under load at deadlines
- [ ] Download the submitted PDF and verify it matches what you intended
- [ ] Confirm submission receipt email
- [ ] Note the paper ID / submission number for reference

---

## Phase 5: Post-Submission

- [ ] Upload preprint to arXiv (if embargo period allows)
- [ ] Update Google Scholar profile
- [ ] Share on Twitter/X with paper link and brief description
- [ ] Archive submission code with DVC/Git tag: `git tag submission-venue-year`

---

## Resources

* [Anonymous GitHub](https://anonymous.4open.science/) — Anonymous code submission for double-blind review
* [pdffonts](https://manpages.ubuntu.com/manpages/focal/man1/pdffonts.1.html) — Check font embedding
* [NeurIPS Checklist](https://neurips.cc/public/guides/PaperChecklist) — Official reproducibility checklist
