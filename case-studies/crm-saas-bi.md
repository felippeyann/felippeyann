# BI and analytics for a B2B CRM SaaS company

**Context**: the revenue operation of a B2B SaaS company (CRM platform). I built the analytics layer used daily by SDR teams, sales managers and marketing. Company anonymized; the public demos use 100% synthetic data.

**My role**: BI engineering end to end: data pipelines, dashboard engineering, AI-assisted editorial tooling.

## Live dashboards (public demos)

Eight of the dashboards are published with synthetic data: **[live demo](https://felippeyann.github.io/sales-analytics-dashboards/)** / [repo](https://github.com/felippeyann/sales-analytics-dashboards).

Two families:

- **Apps Script web apps**: server-side dashboards on Google Apps Script reading Google Sheets in near real time, running on office TVs for the sales floor (multi-page routing served two dashboards from one project; TV-friendly responsive scaling).
- **Standalone client-side dashboards**: single-file HTML + vanilla JS + ECharts, CSV upload, cross-filtering, PNG/PDF/CSV export, optional Gemini-generated insights. Zero install, zero backend, data never leaves the browser: the right trade-off for sensitive commercial data.

## Python BI pipelines

- **PostgreSQL (AWS) to Google Sheets**: scheduled extraction of pipeline-stage monitoring data (120-day windows) with local CSV backups, execution logs and append-only history, running unattended via OS scheduler.
- **Marketing data ingestion**: scripts for BigQuery and the Meta/Google Ads APIs feeding the same reporting layer, so paid-channel spend and CRM outcomes could be read side by side.

## AI editorial system

An automated content curation and generation system for the company's editorial operation:

- RSS curation with Gemini-based scoring of candidate articles;
- original article generation grounded on trending themes plus external fact search;
- an analytics dashboard for keywords, sources and weekly performance;
- automated weekly reports delivered to the team via webhook.

## What this project demonstrates

Pragmatic BI engineering: choosing boring, operable technology (Sheets, Apps Script, single-file HTML) where it beats a heavy stack, wiring Python pipelines that run unattended for months, and putting LLMs to work inside a real editorial workflow with scoring and reporting around them.
