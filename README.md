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

---

## New User Flow

When you open the project for the first time, the agent detects that your `resume.md` and `user.md` are blank and shows a welcome message. It then walks you through setup one step at a time:

1. **Resume** — Paste or upload your resume directly in the chat. The agent writes it to `resume.md`.
2. **Target salary** — Monthly or annual, your call.
3. **Preferred location(s)** — Cities or regions you're open to.
4. **Work mode** — Remote, hybrid, or on-site.
5. **Deal-breakers** — Anything you won't accept (e.g. no bond agreements, no night shifts).

Each answer is saved to `user.md` immediately. Once all four preferences are collected, the agent confirms you're set up and prompts you to paste a JD.

---

## Pasting a JD — The Evaluation Pipeline

Paste any job description into the chat. The agent runs a four-step pipeline silently, then prints the full report.

### Step 1 — Hard Blocker Check

Before scoring, the agent scans the JD for absolute requirements you clearly cannot meet:

- Visa / work authorisation (e.g. "must be legally authorised to work in X")
- Mandatory degree (e.g. "Bachelor's in CS required", not preferred)
- Location with no exceptions (e.g. "on-site in X city only")
- Certifications requiring years of hands-on work (e.g. CA, CPA, PMP required)

**If a hard blocker is found:** the pipeline stops immediately. The only output is one line — `"Hard blocker: [reason]. Verdict: Do Not Apply."` No score is shown.

**If a requirement says "preferred" or "nice to have":** it is flagged as a caution note and the pipeline continues.

---

### Step 2 — Archetype Detection

The agent classifies the role into one of eight archetypes:

| Archetype | Examples |
|-----------|----------|
| Engineering | SWE, DevOps, Data Engineer, ML Engineer |
| Solutions / Consulting | Solutions Architect, Pre-Sales, Implementation |
| Product Management | PM, APM, Product Owner |
| Sales / BD / Account Mgmt | AE, BDR, KAM, CSM |
| Data / Analytics | Analyst, Data Scientist, BI |
| Operations / Program Mgmt | Ops, Program Manager, Chief of Staff |
| People / Leadership | HR, L&D, EM, Director+ |
| Creative / Design | UX, Content, Brand |

The archetype determines what counts as a strong skills match and experience fit for that role. If the role title is unusual (e.g. "Bancassurance", "RevOps", "Relationship Manager — HNI"), the agent does a web search to map JD terminology to resume equivalents before scoring.

---

### Step 3 — Weighted Scoring

Each of the five parameters is rated 1–5 based on your resume vs. the JD requirements. The scores are combined into a weighted final score.

| Parameter | Weight | What is assessed |
|-----------|--------|-----------------|
| Skills match | 30% | Must-haves vs nice-to-haves from the JD, each mapped to a specific resume line or flagged as missing |
| Experience fit | 25% | Years required vs actual, domain/industry match, and role type (built vs managed vs consulted) |
| Role-level fit | 20% | JD seniority vs your current level — penalises both underqualified and overqualified |
| Education / Certs | 15% | Degree relevance and institution tier (verified via NIRF web search); cert adjustments applied |
| Salary fit | 10% | Salary anchor → company tier adjustment → candidate leverage modifiers → vs your target |

**Rating scale:**
- **5** — Exact match, no gap
- **4** — Strong match, minor gap
- **3** — Partial match, noticeable gap
- **2** — Weak match, significant gap
- **1** — Little to no match

**Final score** = (Skills × 0.30) + (Experience × 0.25) + (Role-level × 0.20) + (Education × 0.15) + (Salary × 0.10)

#### How Skills match is scored
The agent identifies must-have skills from the JD (using the archetype table), then cites the exact resume line that demonstrates each one — or explicitly marks it as missing. Gaps are classified as hard blockers, learnable gaps, or minor weaknesses.

#### How Experience fit is scored
Three signals are extracted: required years, domain/industry, and role type. Each is compared against the resume directly. Domain mismatches (e.g. FMCG → SaaS) are flagged explicitly. The agent notes whether any gap is bridgeable within two weeks.

#### How Role-level fit is scored
The JD's expected seniority level is compared to the candidate's current level. If underqualified, the gap is assessed as bridgeable or permanent. If overqualified, the user is told explicitly: recruiters may see them as a flight risk.

#### How Education / Certs is scored
The agent web searches the institution name against NIRF rankings to classify it:

| Tier | Criteria |
|------|----------|
| Tier 1 | Centrally funded + highly selective entrance (JEE Advanced, CAT, GATE, BITSAT) + NIRF top 50 or globally recognised |
| Tier 2 | State-funded or reputed private + entrance-based admission + NIRF 51–200 |
| Tier 3 | Private college with management quota or limited recognition |

Base score follows from tier (Tier 1 = 5, Tier 2 = 4, Tier 3 = 3). If the JD lists a preferred cert and you have it, score is bumped +1 (max 5).

#### How Salary fit is scored
1. **Anchor** — JD-stated range (midpoint used) or market rate from web search
2. **Company tier adjustment** — researched via web search each time:
   - Tier 1 (Fortune 500, Big Tech, unicorn, bulge-bracket bank): × 1.5
   - Tier 2 (established regional brand, Series B+, mid-size product co): × 1.0
   - Tier 3 (pre-Series B, unknown brand, unverifiable): × 0.75
3. **Candidate leverage** — applied on top of the tier adjustment:
   - Tier 1 college: +10%
   - Strong in-demand skill (LLMs, cloud infra, Kubernetes, etc.): +5% per skill, capped at +20%
   - Experience ≥2 years above JD minimum: +5%
   - Domain mismatch (if flagged in Experience fit): −10%
4. **Score** — effective expected salary vs your target from `user.md`

---

### Step 4 — Verdict

Before showing the score, the agent cross-checks `user.md` for deal-breakers, location preference, and work mode. Any mismatches are flagged at the top of the output.

| Weighted Final Score | Verdict |
|----------------------|---------|
| 4.0 – 5.0 | Apply |
| 3.0 – 3.9 | Apply with Caution |
| Below 3.0 | Do Not Apply |

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
JD states ₹18–22 LPA. Company is a mid-tier SaaS firm (Tier 2, no adjustment).
Candidate leverage: BITS Pilani (+10%), strong Python/SQL (+10%) → effective expected: ~₹20.5 LPA.
User target is ₹22 LPA — within 8%, scoring 4/5.

---

### 2-Week Improvement Plan

| Parameter         | Score | Priority | Action                                                              | Resource / Tool                        | Time Estimate |
|-------------------|-------|----------|---------------------------------------------------------------------|----------------------------------------|---------------|
| Skills match      | 4/5   | High     | Complete Databricks Spark fundamentals course (~6 hrs) on Coursera | Coursera — Databricks Spark course     | 2 days        |
| Experience fit    | 3/5   | High     | Reframe FMCG metrics work as customer/revenue analytics in resume  | Resume — Work Experience section       | 1 day         |
| Role-level fit    | 4/5   | Low      | No action needed                                                    | —                                      | —             |
| Education / Certs | 5/5   | Low      | No action needed                                                    | —                                      | —             |
| Salary fit        | 4/5   | Med      | Research SaaS analyst comp bands; prepare negotiation range ₹21–24 LPA | Glassdoor, Levels.fyi, AmbitionBox | 2 hours       |

**Week-by-week breakdown:**
- **Days 1–7:** Complete Spark course; reframe FMCG bullet points in resume Experience section
- **Days 8–14:** Research comp bands; prepare negotiation BATNA; finalise resume edits
```

---

## Reset

To clear your resume and preferences and start fresh, type `reset` in the chat. The agent will ask for confirmation before erasing anything.

---

## Notes

- The agent does a live web search for market salary data when a JD does not list compensation
- Company tier is researched fresh for each JD — not inferred from the company name
- Institution tier is verified via NIRF web search before scoring Education / Certs
- The ATS & Resume Checklist is generated silently during evaluation but is not saved to a file (feature in progress)
- All scoring criteria, weights, and verdict thresholds are defined in `CLAUDE.md` — you can edit them directly
- This project is designed for the Indian job market by default but works globally
