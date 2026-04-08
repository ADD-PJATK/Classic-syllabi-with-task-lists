# Semester project (classic / stationary track)

**Course:** Analysis of Large Data Sets (ADD) — *Analiza dużych zbiorów danych*  
**Source:** Official syllabus (*stacjonarne* programme excerpt): [syllabus-reference/ADD_stacjonarne.pdf](syllabus-reference/ADD_stacjonarne.pdf).

---

## 1. Role in assessment

Per the syllabus, laboratory classes receive a **graded credit (“zaliczenie z oceną”)**. Assessment components typically include:

- **Written quiz / colloquium** (in-class or online—per current course rules).
- **Team semester project**, consisting of:
  - **Software demonstration** (working pipeline, tool workflows, or reproducible analysis).
  - **Final written report** documenting teamwork, data, methods, and results.

The exact weights are defined by the **current** *Course Rules and Assessment Policy* document for your academic year; treat this file as **content expectations**, not a legal override of faculty announcements.

---

## 2. Learning goals

The project should show that a team can:

1. **Pose** a non-trivial question on a **large or realistic** dataset (transactional, text, graph, stream snippet, or high-dimensional vectors).
2. **Prepare** data responsibly (cleaning, train/test or stream protocol, no bulk sensitive leakage).
3. **Apply** at least **two** distinct techniques from the **syllabus themes**, for example:
   - association rule mining,  
   - collaborative or content-based recommendations,  
   - dimensionality reduction / PCA-style workflows,  
   - scalable clustering or stream-oriented evaluation,  
   - large-data decision trees or incremental models,  
   - graph metrics (PageRank / HITS / communities).
4. **Evaluate** results with **quantitative** metrics and **baselines**.
5. **Communicate** limitations and engineering trade-offs.

---

## 3. Team and supervision

- **Team size:** follow the instructor’s announcement (often **2–4** students on the classic track).
- **Topic approval:** baseline alignment with syllabus topics; **Session 6** lab is reserved for **project topic discussion**—teams should arrive with **2–3** candidate ideas.
- **Check-in:** **Session 9** includes **partial results** presentations; teams should expect feedback and possible scope adjustments.

---

## 4. Deliverables

### 4.1 Code / reproducible workflow

- Version-controlled repository **or** zipped artifact with clear structure (`data/README`, `src/`, notebooks).
- Environment specification: `requirements.txt`, Conda env, or RapidMiner project export + version note.
- **Small** samples in Git; **large** data linked with download instructions or generation script.

### 4.2 Demonstration

- Live or recorded demo acceptable if the instructor agrees.
- Show **end-to-end** flow: raw → processed → model → metric → one figure the audience understands in **< 1 minute**.

### 4.3 Final report (PDF, English or Polish per course policy)

Recommended structure (page count per instructor—typical **10–20** pages excl. appendices):

1. **Title, authors, affiliations, index numbers**  
2. **Abstract**  
3. **Introduction** — problem, motivation, contributions  
4. **Related work / baselines** (short)  
5. **Data** — source, license, schema, descriptive statistics  
6. **Methods** — algorithms, parameters, why they fit the problem  
7. **Experiments** — protocol, tables, figures  
8. **Discussion** — failures, bias, scalability  
9. **Conclusion**  
10. **References**  
11. **Appendix** — extra plots, pseudo-code, team log (optional)  
12. **Individual contributions** — explicit paragraph or table per person  

---

## 5. Mapping to syllabus lab weeks (high level)

| Week (approx.) | Syllabus lab focus | Project expectation |
|----------------|-------------------|---------------------|
| 6 | RapidMiner ↔ Hadoop intro; **topics discussed** | Submit **draft topic + roles** (see Session 6 task list). |
| 9 | **Partial presentations** | **Milestone:** data + first result + risks. |
| 13 | **Project completion** | **Final demo + report** |

---

## 6. Ethical use of data

- Cite **sources**; respect **terms of use** and **personal data** rules.  
- Do not publish secrets (API keys, student PII). Use `.gitignore` and environment variables.

---

## 7. File naming (suggestion)

- Team report: `ADD_ProjectReport_TeamNameOrID.pdf`  
- Slides: `ADD_ProjectSlides_TeamNameOrID.pdf`

---

*This document paraphrases the official Polish syllabus objectives. If anything conflicts with the live course page, announcement, or instructor email, follow those sources.*
