# Job Analysis — Claude Code Agent

A Claude Code-powered job evaluation system. Paste any job description and get a detailed, scored report telling you whether to apply — based on your resume, preferences, and salary expectations.

---

## What It Does

When you paste a JD, the agent:

1. Checks for hard blockers (visa, mandatory degree, salary mismatch)
2. Detects the role archetype (Engineering, Product, Data, Sales, etc.)
3. Scores the JD across 5 weighted parameters against your resume
4. Cross-checks your preferences and deal-breakers
5. Prints a full evaluation report with a verdict and a 2-week improvement plan

---

## Setup

### Prerequisites
- [Claude Code](https://claude.ai/code) installed and authenticated

### Steps

1. Clone this repo
2. Open the folder in Claude Code
3. Set up your resume and preferences (see below)

---

## Setting Up Your Profile

The agent needs two files to evaluate JDs:

### `resume.md` — Your Resume
- On first use, just paste a JD — the agent will detect that `resume.md` is empty and ask for your resume
- You can paste your existing resume or let the agent collect it section by section
- See [`resume_template.md`](resume_template.md) for a fully populated example

### `user.md` — Your Job Preferences
- Stores your salary target, work mode preference, location, deal-breakers, etc.
- The agent updates this file whenever you state a new preference in chat
- See [`user_template.md`](user_template.md) for a fully populated example

> **Note:** Never manually edit `resume.md` or `user.md` in ways that break the structure — the agent parses these files during evaluation.

---

## Usage

1. Open the project in Claude Code
2. Paste a job description into the chat
3. The agent runs the full pipeline silently and prints the evaluation report

### Sample Output

```
## Evaluation

Archetype: Data / Analytics
Blocker Notes: None

Scores:

| Parameter        | Score | Notes                          |
|------------------|-------|--------------------------------|
| Skills match     | 4/5   | Strong SQL/Python match ...    |
| Experience fit   | 3/5   | Domain mismatch: FMCG → SaaS  |
| Role-level fit   | 4/5   | Mid-level alignment confirmed  |
| Education / Certs| 5/5   | BITS Pilani, relevant degree   |
| Salary fit       | 4/5   | Within 8% of target            |

Weighted Final Score: 3.85 — Apply with Caution
```

---

## File Overview

| File | Purpose |
|------|---------|
| `CLAUDE.md` | Agent instructions and evaluation pipeline |
| `resume.md` | Your resume (auto-created on first use) |
| `user.md` | Your job preferences (auto-created on first use) |
| `resume_template.md` | Example of a populated resume |
| `user_template.md` | Example of populated user preferences |

---

## Reset

To clear your resume and preferences and start fresh, type `reset` in Claude Code chat.

---

## Notes

- The agent does a live web search for market salary data when a JD doesn't list compensation
- All scoring criteria are defined in `CLAUDE.md` — you can edit them to adjust weights or verdict thresholds
- This project is designed for the Indian job market by default but works globally
