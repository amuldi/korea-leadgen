# Korea Market Activation Platform

AI-powered lead generation for the **Springboard × SyncTalents** Korea
market-entry internship. It sources Korean companies that are actively hiring
developers, scores them against an Ideal Customer Profile (ICP), uses Claude to
qualify and draft outreach, and presents everything in one shareable app.

```
Wanted postings → enrich → ICP score → Claude qualify (fit/maybe/no) → EN/KR outreach → platform + delivery sheet
```

## My role — Sihun Jeon (전시훈)

> This repository is a fork of the team repository [dlwogud/korea-leadgen](https://github.com/dlwogud/korea-leadgen),
> built by a 5-person team during an overseas internship at Springboard Philippines (2026.06.25 – 07.17).

I led the project overall within the 5-person team, and was responsible for:

| Role | What it involved |
|---|---|
| **Overall leadership** | Coordinating the team's progress and integrating everyone's individual work into one platform |
| **Planning & support** | Shaping the pipeline structure and screen design, supporting teammates |
| **Documentation** | Writing up the pipeline flow, scoring logic, and other project explanations |
| **UI/UX** | Designing the platform's 5 screens (Overview, Pipeline, KPI Funnel, Outreach Studio, Recommendation) |
| **API integration** | Wiring up Wanted/Saramin data collection and the Anthropic Claude API |
| **Feature development** | Building and improving individual pipeline stages |
| **Presentation** | Delivered the capstone presentation in English |

**Live demo:** https://dlwogud.github.io/km-platform-demo/

## The platform

`scripts/build_platform.py` builds a single self-contained app (`data/platform.html`):

- **Overview** — KPIs, top leads, service/priority mix
- **Pipeline (CRM)** — searchable leads: priority, AI verdict, evidence, editable contacts
- **Outreach Studio** — editable Claude EN/KR drafts, searchable
- **Settings** — API keys per source + run collection (via the internal admin server)

## Quick start

```bash
pip3 install -r requirements.txt
cp .env.example .env          # add ANTHROPIC_API_KEY (for the AI steps)

# run the whole pipeline on fresh Wanted data:
bash scripts/daily_wanted.sh
# then open data/platform.html
```

## Data source

- **Primary: Wanted** (`source_wanted.py`) — where Korean tech SMEs hiring
  developers post. No API key needed. Best fit for the ICP. *Whether to run
  automated collection is a company decision — see the script header.*
- **Optional: Saramin** (`source_saramin.py` + `run_live.py`) — for teams that
  license the Saramin API. Same pipeline, different source.
- **Manual** (`import_list.py`) — import any company CSV. Fully legal fallback.

The source layer is swappable — every source writes the same schema
(`scripts/_common.py`), so scoring + AI are identical whichever feed is used.

## Automation

`scripts/daily_wanted.sh` runs the full pipeline; schedule it with cron
(12:00 daily). See `docs/AUTOMATION.md`.

## Repository layout

```
config/icp.json   The ICP (size, industries, roles, weights) — edit to retarget
scripts/          Pipeline: source_* / enrich / score / qualify / generate / platform / ...
data/             Generated outputs (gitignored)
docs/             Handover, reports, automation, ICP notes
```

## Key docs

- **`docs/HANDOVER.md`** — how the company takes this over (repo, keys, run, schedule)
- **`docs/jake-update.md`** — progress report + open decisions
- **`docs/AUTOMATION.md`** — scheduling
- **`docs/ICP.md`** — the target profile

## Status

Consolidated platform running on real Wanted data (prototype-grade). API-ready;
productionizing needs a licensed data path + company Anthropic key. Honest
limitations and open decisions are in `docs/jake-update.md`.
