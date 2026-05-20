# WoodClaw

WoodClaw is a configurable B2B commercial intelligence platform that turns
company research into structured, source-cited decision briefs.

Four specialised AI agents — Sales, Marketing, Procurement, and General
Research — analyse the same target company from four different business
perspectives. Industry knowledge is plug-in via presets; the default
**Generic B2B** preset works across sectors, and authored presets like
the **Wood / Furniture Showcase** demonstrate how deep a vertical can go.

Built with Python, Streamlit, Anthropic Claude (Agent SDK), web search,
Markdown-based configuration files, and SQLite.

- **Demo available on Hugging Face Spaces:** [huggingface.co/spaces/taymazm/woodclaw](https://huggingface.co/spaces/taymazm/woodclaw) *(password-protected — request access from murattaymaz@gmail.com)*
- **Marketing site:** [woodclaw.io](https://woodclaw.io)
- **Built in Berlin by:** Murat Taymaz

---

## Features

- **Four AI agents.** Sales · Marketing · Procurement · General Research — each with its own lens, prompt, and scoring logic.
- **Five workflows.** Quick Lookup · Research Brief · Fit Assessment · Outreach Draft · RFQ Draft. The user picks the deliverable shape.
- **Industry presets.** Generic B2B by default; sector-specific presets (Wood / Furniture Showcase today, more on roadmap) for vertical depth.
- **No accidental outreach.** Three-layer guard ensures the agent never drafts emails, RFQs, LinkedIn messages, or follow-ups without explicit opt-in.
- **Source-aware.** Key factual claims are supported with source URLs where available. Unknowns are written as `unknown`, never guessed. Each brief reports overall source quality so the reader knows how much weight to put on its claims.
- **Pipeline tracking.** Every run is saved to SQLite. Generic Kanban statuses (New → Researching → Qualified → Contacted → Negotiating → Won / Lost / Archived). CSV export.

## How it works

A run is fully specified by three orthogonal axes:

| Axis | Options | Example |
|---|---|---|
| **agent_role** | sales · marketing · procurement · general_research | sales |
| **workflow_type** | quick-lookup · research-brief · fit-assessment · outreach-draft · rfq-draft | fit-assessment |
| **industry_preset** | generic_b2b *(default)* · wood_furniture *(showcase)* · (more on roadmap) | generic_b2b |

The system prompt is composed at runtime from the agent's lens + the workflow's output spec + the industry preset's domain knowledge + a depth cap + an outreach guard. The agent never invents facts; web sources are cited inline.

## Agent roles

- **Sales Agent.** Evaluates the target as a buyer / lead / distributor / commercial opportunity. Buyer-fit signals; named procurement contacts; recommended next step. Does not analyse marketing positioning or evaluate suppliers.
- **Marketing Agent.** Analyses outward positioning — claims, messaging tone, audience, competitive signals. Does not score for buyer or supplier fit; does not draft sales emails or RFQs.
- **Procurement Agent.** Evaluates the target as a supplier. Production capability, certifications, lead times, reliability signals, regulatory readiness. Does not score as a customer; does not analyse marketing positioning.
- **General Research Agent.** Produces neutral factual summaries with explicit unknowns and cited sources. Stays role-agnostic. Does not score, recommend, or draft outreach.

## Industry presets

Each preset is a directory of four Markdown files:
`company.md` · `products.md` · `rubric.md` · `outreach_template.md`.

To add a new sector:
1. Create `presets/{your_sector}/`.
2. Fill in the four files (use `presets/generic_b2b/` as a template).
3. Restart the app — the new preset appears in the sidebar.

The code is industry-agnostic; all sector knowledge lives in preset files.

## Example runs (same target, four agents)

**Target:** Siemens Energy (Generic B2B preset)

| Agent | Workflow | What you get |
|---|---|---|
| Sales | Fit Assessment | Buyer-fit score + tier + recommended next step. |
| Marketing | Research Brief | Positioning, messaging, audience, competitive signals. |
| Procurement | Fit Assessment | Supplier-fit score + tier + risk note. |
| General Research | Quick Lookup | 150-word neutral factual summary + sources. |

**Wood / Furniture Showcase** *(optional vertical example)*: Juodeliai with the Procurement Agent + Fit Assessment demonstrates how deep an authored sector preset can go (FSC + EUDR + capacity signals + named contacts).

## Limitations

- **Public-source research only.** No paywalled databases, proprietary CRMs, or private supplier directories.
- **Anthropic Claude API costs** ~€0.10–0.20 per run; the demo has a monthly spend cap.
- **Early-stage MVP focused on public-source company research and first-pass decision support. Not intended to replace professional due diligence or human business judgment.**
- **Authored presets are limited at MVP** — only Generic B2B and Wood / Furniture Showcase ship; others are roadmap stubs.

## Source policy

WoodClaw prioritizes primary sources — company websites, annual reports, investor releases, supplier portals, government registries, and certification bodies. Reputable secondary databases and trade press are used cautiously, and weak sources such as Wikipedia, SEO company-profile pages, and scraped directories are not used for critical claims unless verified elsewhere. For private companies, revenue / employee / ownership figures pulled from third-party databases are presented as estimates, not facts. Each brief includes a source-quality summary so the reader knows how much weight to put on its claims.

## Roadmap

**Phase 2:** Pipeline filters, output audit UI, better extraction.

**Phase 3:** New workflows (Supplier Search, Competitor Scan, Company Comparison, Full Workflow), new presets (Packaging, Machinery, Food Export, Textiles, Construction Materials), optional paid Pro plan.

**Out of scope:** Enterprise multi-tenant, CRM sync, mobile app.

---

## About this repository

This repository contains the static **marketing site** for WoodClaw,
hosted on GitHub Pages at [woodclaw.io](https://woodclaw.io).

The actual agent code (Python + Streamlit) is deployed as a
Hugging Face Space at
[huggingface.co/spaces/taymazm/woodclaw](https://huggingface.co/spaces/taymazm/woodclaw).
Source for the agent itself is maintained privately at this time.

### Files in this repo

- `index.html` — the entire landing page (inline CSS, no build step, no JS).
- `CNAME` — tells GitHub Pages that the custom domain is `woodclaw.io`.
- `README.md` — this file.

### Local preview of the landing site

```bash
open index.html
# or, for relative paths:
python3 -m http.server 8000
# then open http://localhost:8000
```

### Maintenance notes (GitHub Pages + custom domain)

The custom domain `woodclaw.io` is wired via:
- `CNAME` file in this repo (tells GitHub Pages the domain).
- A records at the domain registrar pointing the apex to GitHub Pages
  IPs (`185.199.108.153`, `109.153`, `110.153`, `111.153`).
- A `www` CNAME pointing to `taymazm.github.io`.
- "Enforce HTTPS" enabled in **Settings → Pages**.

To update the landing page: edit `index.html`, commit, push. GitHub
Pages redeploys automatically in ~30 seconds.
