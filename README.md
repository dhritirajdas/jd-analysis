# Job Analysis — Claude Code Agent

A Claude Code-powered job evaluation system. Paste any job description and get a detailed, scored report telling you whether to apply — based on your resume, preferences, and salary expectations.

---

## Getting Started

### 1. Prerequisites
- [Claude Code](https://claude.ai/code) installed and authenticated

### 2. Clone the repo
```bash
git clone https://github.com/dhritirajdas/jd-analysis.git
cd jd-analysis
```

### 3. Open in Claude Code
Open the `jd-analysis` folder in Claude Code.

### 4. Just say hi
Type anything — even just `hi` — in the chat. The agent will take it from there.

It will walk you through setting up your resume and preferences step by step. Once that's done, paste any job description and it will evaluate it for you.

---

## What It Does

When you paste a JD, the agent:

1. Checks for hard blockers (visa, mandatory degree, salary mismatch)
2. Detects the role archetype (Engineering, Product, Data, Sales, etc.)
3. Scores the JD across 5 weighted parameters against your resume
4. Cross-checks your preferences and deal-breakers
5. Prints a full evaluation report with a verdict and a 2-week improvement plan

---

## Sample Output

```
## Evaluation

**Archetype:** Data / Analytics
**Blocker Notes:** None

**Scores:**

| Parameter         | Score | Notes                                        |
|-------------------|-------|----------------------------------------------|
| Skills match      | 4/5   | Strong SQL/Python match; Spark not in resume |
| Experience fit    | 3/5   | Domain mismatch: FMCG → SaaS                |
| Role-level fit    | 4/5   | Mid-level alignment confirmed                |
| Education / Certs | 5/5   | BITS Pilani, relevant degree                 |
| Salary fit        | 4/5   | Within 8% of target                          |

**Weighted Final Score: 3.85 — Apply with Caution**

**Flags:**
- Work mode: JD requires on-site (Bangalore); your preference is Remote

---

### Detailed Analysis

**Skills match (4/5)**
The JD requires SQL, Python, and Tableau — all present in the resume with hands-on project evidence.
Spark is listed as preferred but absent from the resume; this is a gap worth addressing before applying.

**Experience fit (3/5)**
The JD asks for 3–5 years in a SaaS or B2B analytics environment. Resume shows 2.5 years in FMCG.
Domain mismatch is the main drag — the analytical skills transfer, but the industry context does not.

**Role-level fit (4/5)**
The role is a mid-level individual contributor with no direct reports. Resume scope aligns well.
No leadership gap; current titles and responsibilities match the seniority expected.

**Education / Certs (5/5)**
BITS Pilani (B.E. Computer Science) is a Tier 1 institution and directly relevant.
No certifications required by the JD; none are missing.

**Salary fit (4/5)**
JD states ₹18–22 LPA. Company is a mid-tier SaaS firm (no adjustment).
Candidate leverage: BITS Pilani (+0.5), strong Python/SQL (+0.5) → effective expected: ~₹20.5 LPA.
User target is ₹22 LPA — within 8%, scoring 4/5.

---

### 2-Week Improvement Plan

| Parameter         | Score | Action                                                                 |
|-------------------|-------|------------------------------------------------------------------------|
| Skills match      | 4/5   | Complete Databricks Spark fundamentals course (~6 hrs) on Coursera     |
| Experience fit    | 3/5   | Reframe FMCG metrics work as customer/revenue analytics in resume      |
| Role-level fit    | 4/5   | No action needed                                                       |
| Education / Certs | 5/5   | No action needed                                                       |
| Salary fit        | 4/5   | Research SaaS analyst comp bands; prepare negotiation range ₹21–24 LPA|
```

---

## Reset

To clear your resume and preferences and start fresh, type `reset` in the chat.

---

## Notes

- The agent does a live web search for market salary data when a JD doesn't list compensation
- All scoring criteria are defined in `CLAUDE.md` — you can edit them to adjust weights or verdict thresholds
- This project is designed for the Indian job market by default but works globally
