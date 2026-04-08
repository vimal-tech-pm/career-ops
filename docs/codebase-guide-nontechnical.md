# Career-Ops: Your AI-Powered Job Search Assistant — A Complete Guide

> A non-technical walkthrough of what Career-Ops does, how it works, and how to use it for your job search.

## What Is Career-Ops?

Career-Ops is a **personal job search command center** that runs inside an AI coding assistant (Claude Code). Think of it as hiring a tireless recruiter who works 24/7, except this one lives on your computer and you control everything.

It was built by someone who used it to evaluate **740+ job offers**, generate **100+ tailored resumes**, and ultimately land a Head of Applied AI role. Now it's open source so anyone can use it.

**In plain terms, Career-Ops can:**

1. **Find jobs for you** — It automatically scans 100+ company career pages and job boards, filtering for roles that match your goals.
2. **Evaluate every job offer** — It reads a job description, compares it to your resume, scores it on multiple dimensions, and tells you whether it's worth your time.
3. **Generate a custom resume for each job** — Not a generic one. A resume tailored to each specific job, with the right keywords for automated screening systems (ATS), in the right format, as a professional PDF.
4. **Help you apply** — It reads application forms and drafts personalized answers you can copy-paste.
5. **Write LinkedIn outreach messages** — Short, specific messages to hiring managers or recruiters, based on research about the company.
6. **Research companies deeply** — Before an interview, it prepares a dossier on the company's AI strategy, culture, recent moves, and competitors.
7. **Prepare you for interviews** — It generates STAR stories (Situation, Task, Action, Result) mapped to what the interviewer is likely to ask.
8. **Track everything** — A dashboard of every job you've looked at, with scores, statuses, reports, and PDFs.

---

## How It Works — The Big Picture

Imagine a funnel:

```
  DISCOVER  →  EVALUATE  →  APPLY  →  INTERVIEW  →  OFFER
   (scan)      (oferta)    (apply)     (deep)     (negotiate)
     ↓            ↓           ↓          ↓
  pipeline    reports/     tracker    story-bank
  .md         PDFs         .md         .md
```

1. **You set up your profile once** — your resume, target roles, salary expectations, and preferences.
2. **The scanner finds new jobs** — It visits career pages of companies you care about and filters by your target titles.
3. **New jobs land in your inbox** (`pipeline.md`) — a simple checklist of URLs waiting to be reviewed.
4. **Each job gets a full evaluation** — a detailed report with scores, match analysis, salary research, and interview prep.
5. **If it scores well, a custom PDF resume is generated** — optimized with the right keywords for that specific job.
6. **When you're ready to apply**, it fills out forms for you and drafts cover letters.
7. **Everything is tracked** in a master spreadsheet-like file so you always know where you stand.

---

## The Setup Process (Onboarding)

When you first start, Career-Ops walks you through setup:

### Step 1 — Your Resume (`cv.md`)

You provide your resume in any form — paste it, share a LinkedIn URL, or just describe your experience. Career-Ops converts it into a clean markdown file that becomes the "source of truth" for everything.

### Step 2 — Your Profile (`config/profile.yml`)

Basic info: name, email, location, target roles, salary range. This is used across all features so it only needs to be entered once.

### Step 3 — Job Portals (`portals.yml`)

A pre-configured list of 100+ companies to monitor (Anthropic, OpenAI, Vercel, Retool, etc.) plus search queries for job boards. You can add or remove companies anytime.

### Step 4 — Your Tracker (`data/applications.md`)

An empty table that will fill up as you evaluate offers. Think of it as your personal CRM for job hunting.

### Step 5 — Getting to Know You

The more Career-Ops knows about you — your superpowers, what excites you, deal-breakers, best achievements — the better it filters and evaluates. It's like onboarding a recruiter.

---

## The Commands (What You Can Do)

You interact with Career-Ops by running slash commands or just describing what you want in plain language:

| What you want to do | Command | What happens |
|---------------------|---------|-------------|
| See all options | `/career-ops` | Shows the main menu |
| Evaluate a job offer | Paste a job URL or description | Full A-F evaluation with score |
| Process your job inbox | `/career-ops pipeline` | Evaluates all pending URLs |
| Compare multiple offers | `/career-ops ofertas` | Side-by-side scoring matrix |
| Find new jobs | `/career-ops scan` | Scans all tracked company portals |
| Generate a tailored resume | `/career-ops pdf` | ATS-optimized PDF for a specific job |
| Get help applying | `/career-ops apply` | Drafts answers for application forms |
| Write LinkedIn message | `/career-ops contacto` | Researches contacts + drafts outreach |
| Research a company | `/career-ops deep` | Deep dive into company's AI strategy, culture, etc. |
| Check application statuses | `/career-ops tracker` | Overview of all your applications |
| Evaluate a course/cert | `/career-ops training` | Should you take this course? |
| Evaluate a portfolio project | `/career-ops project` | Should you build this project? |
| Batch process many offers | `/career-ops batch` | Parallel evaluation of multiple jobs |

---

## How Job Evaluation Works (The Scoring System)

When you paste a job URL or description, Career-Ops runs a **6-block evaluation**:

### Block A — Role Summary

A quick snapshot: what archetype (type of role) it detected, the domain, seniority level, remote policy, and team size.

### Block B — Resume Match

A line-by-line comparison: each job requirement is matched against specific lines from your resume. It identifies gaps and suggests how to address them (e.g., "you don't have this exact skill, but you have adjacent experience in X").

### Block C — Level & Strategy

Is the job at your level, above, or below? If it's slightly above, it suggests how to position yourself. If below, it advises on negotiation tactics (like asking for a 6-month review to level up).

### Block D — Compensation & Market Data

Real salary research using Glassdoor, Levels.fyi, and other sources. How does this company pay? Is the role in demand?

### Block E — Personalization Plan

The top 5 changes to make to your resume and LinkedIn profile to maximize your match for this specific job.

### Block F — Interview Prep

6-10 STAR stories (Situation, Task, Action, Result) tailored to what this interviewer is likely to ask, plus red-flag questions and how to handle them.

### The Score

A weighted average across:

- **Resume match** — How well your experience fits
- **North Star alignment** — How close this is to your ideal role
- **Compensation** — How the pay compares to market
- **Cultural signals** — Remote policy, growth opportunities, company culture
- **Red flags** — Anything concerning (vague JD, unrealistic expectations, etc.)

Scores range from **1 to 5**. Generally:

- **4.0+** = Strong match, apply
- **3.0–3.9** = Decent match, consider it
- **Below 3.0** = Probably not worth your time

---

## How Resume Generation Works

Career-Ops doesn't just send the same resume everywhere. For each job:

1. It reads the job description and extracts **15-20 keywords** that automated screening systems look for.
2. It detects what **type of role** it is and adjusts how your experience is framed.
3. It **rewrites your professional summary** to highlight what matters for this specific job.
4. It **reorders your experience bullets** so the most relevant ones come first.
5. It **injects keywords naturally** — not by inventing skills you don't have, but by rephrasing your real experience using the exact vocabulary from the job description.
   - Example: If you wrote "LLM workflows with retrieval" and the job says "RAG pipelines," it becomes "RAG pipeline design and LLM orchestration workflows."
6. It generates a **clean, professional PDF** designed to pass automated screening systems (single column, standard headings, selectable text).

The result: a resume that is truthful, but speaks the language of each specific job.

---

## How the Job Scanner Works

The scanner has three levels of discovery:

1. **Direct scraping** — Visits the actual career pages of 100+ tracked companies and reads all open positions.
2. **API queries** — For companies using certain hiring platforms (like Greenhouse), it fetches job listings directly from their data feed.
3. **Web search** — Broader searches across job boards to discover new companies you're not tracking yet.

After scanning, it:

- Filters results by your target titles (includes roles with keywords like "AI," "ML," "Platform," etc.; excludes things like "Junior," "Java," "Blockchain")
- Removes duplicates against jobs you've already seen
- Checks if the listing is still active
- Adds new finds to your inbox (`pipeline.md`)

You can set it to scan automatically on a schedule (e.g., every 3 days) so you never miss a new opening.

---

## How Tracking Works

Every job you evaluate gets tracked in `data/applications.md` — a table with:

| Column | What it shows |
|--------|--------------|
| # | Sequential number |
| Date | When you evaluated it |
| Company | Company name |
| Role | Job title |
| Score | The evaluation score (out of 5) |
| Status | Where you are in the process |
| PDF | Whether a tailored resume was generated |
| Report | Link to the full evaluation report |
| Notes | Any additional context |

**Statuses progress through a lifecycle:**

- **Evaluated** → You reviewed it, haven't decided yet
- **Applied** → You submitted an application
- **Responded** → The company got back to you
- **Interview** → You're in the interview process
- **Offer** → You received an offer
- **Rejected** → They said no
- **Discarded** → You decided to pass, or the job closed
- **SKIP** → Doesn't fit, not applying

---

## Your Files — What's Yours vs. What's System

Career-Ops has a strict rule: **your personal data is never overwritten by system updates.**

**Your files (safe forever):**

- `cv.md` — Your resume
- `config/profile.yml` — Your personal info and preferences
- `modes/_profile.md` — Your custom scoring and negotiation scripts
- `portals.yml` — Your tracked companies
- `data/` — Your application tracker and pipeline
- `reports/` — Your evaluation reports
- `output/` — Your generated PDFs

**System files (can be updated):**

- `modes/` (except `_profile.md`) — Evaluation logic
- Scripts (`.mjs` files) — PDF generation, tracking, etc.
- Templates — Resume template, portal examples
- `CLAUDE.md` — System instructions

When an update is available, Career-Ops tells you and asks permission. Your data stays untouched.

---

## Multi-Language Support

Career-Ops works in **English** by default, but also has full support for:

- **German** — For the DACH market (Germany, Austria, Switzerland), with local terminology like Probezeit, 13. Monatsgehalt, Tarifvertrag
- **French** — For France, Belgium, Switzerland, Luxembourg, with terms like CDI/CDD, convention collective SYNTEC, RTT, mutuelle

It auto-detects the language of a job description and generates content in that language. You can also set a default language in your profile.

---

## The Philosophy

Career-Ops is built around a core principle: **quality over quantity.**

- It **strongly discourages** applying to jobs that score below 4.0/5. Your time and the recruiter's time are both valuable.
- It **never submits an application without your approval.** It fills out forms and drafts everything, but you always click "Submit."
- It aims for **fewer, better applications** rather than mass-blasting companies with generic resumes.
- It **learns from your feedback.** If you say "that score was too high" or "you missed that I know X," it adjusts for next time.

---

## Quick Start Summary

1. **Start Career-Ops** → it walks you through setup (resume, profile, portals)
2. **Scan for jobs** → `/career-ops scan` finds open roles at tracked companies
3. **Review your inbox** → `/career-ops pipeline` evaluates pending jobs
4. **Or paste any job URL** → get an instant evaluation + tailored resume
5. **Apply with help** → `/career-ops apply` drafts your application answers
6. **Track everything** → `/career-ops tracker` shows where you stand
7. **Customize anytime** → just ask to change archetypes, companies, language, scoring, etc.

Everything is customizable. If the default role types don't match your career, just say "change the archetypes to data engineering roles" and it's done. The system is designed to be shaped to your specific job search.
