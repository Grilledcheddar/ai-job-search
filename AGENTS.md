# AI Job Search OpenCode Rules

This repository is a job application workspace. OpenCode should treat the existing Claude Code workflow files as the source of truth and use the OpenCode wrapper commands in `.opencode/commands/` to invoke those workflows.

## Primary workflow

- Use `CLAUDE.md` as the main candidate profile and workflow contract.
- Use `.claude/commands/*.md` as the canonical slash-command definitions.
- Use `.claude/skills/job-application-assistant/` for fit evaluation, CV tailoring, cover letters, and interview preparation.
- Use `.claude/skills/job-scraper/` and `.agents/skills/*/cli` for portal search workflows.
- Use `.claude/skills/upskill/` for gap analysis.

## Safety and accuracy rules

- Never fabricate candidate skills, job history, certifications, dates, metrics, or company facts.
- Always evaluate fit before drafting application documents.
- Always preserve stated deal-breakers and schedule/location constraints.
- For company-specific claims, verify with current web research before using them in CVs, cover letters, or interview prep.
- Generated CVs and cover letters must compile successfully before being treated as final.

## Local tooling

- Job search CLIs live under `.agents/skills/<tool>/cli` and use Bun.
- CV compilation uses `lualatex`.
- Cover-letter compilation uses `xelatex`.
- Optional ATS text-layer checks use `pdftotext` from Poppler.

## Expected command style

Prefer the OpenCode commands:

- `/setup` for profile onboarding
- `/scrape` for job search
- `/rank` for scoring scraped postings
- `/apply <url-or-job-text>` for fit evaluation and application drafting
- `/interview` for interview prep
- `/outcome` for recording application outcomes
- `/upskill` for skill-gap analysis

When uncertain, read the matching file under `.claude/commands/` and follow it exactly.
