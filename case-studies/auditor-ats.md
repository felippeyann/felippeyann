# Auditor ATS: a solo-built AI SaaS in production

**Product**: [cvaudit.com.br](https://cvaudit.com.br). AI resume analysis for the Brazilian job market: the user uploads a CV (PDF) and a job description, the system scores ATS compatibility (0-100), finds missing keywords and delivers a full HTML report by email. Micro-ticket pricing with four tiers and prepaid credit packages.

**My role**: everything. Product, architecture, backend, frontend, prompts, payments, SEO, incident response.

## Architecture

- **Fully serverless Firebase**: Hosting + Cloud Functions v2 (Node 22, 49 functions) + Firestore + Storage. No servers to manage; cost scales with usage, which is what a micro-ticket business needs.
- **AI pipeline**: Claude (Sonnet) with structured JSON output renders into a templated HTML report. Heavy tiers generate ~11k output tokens; a real incident taught me to size `max_tokens` generously and check `stop_reason` on every structured-output call.
- **Payments**: Stripe + Mercado Pago webhooks feeding a durable fulfillment queue (`fulfillmentJobs`), so a webhook hiccup never loses a paid audit. Prepaid credits live in a wallet collection keyed by email.
- **Data model**: audits, contacts (lightweight CRM), anonymized analytics, and event tracking (GA4 + Pixel + Firestore hybrid).

## Privacy by design (LGPD)

- CV PDFs are deleted right after processing.
- Extracted CV text is kept for 7 days (upgrade window), then purged by a scheduled function.
- IPs are stored only as truncated SHA-256 hashes.
- The analytics base keeps only anonymized CV metadata, which powers original published studies about the Brazilian job market instead of sitting idle.

## Engineering discipline that came from real incidents

- A stale clone on another machine once deployed and wiped 28 blog posts from production (Firebase Hosting mirrors the local folder). The answer was a **predeploy guard script** that blocks any deploy when the branch, working tree or remote sync state is wrong, plus a documented multi-machine workflow.
- CI runs lint + 187 backend tests + a check that compiled CSS is committed, on every push.
- Every production incident gets a written post-mortem in the repo, and the fixes become guards or tests, not memory.

## Distribution

- **SEO/GEO**: 87-post blog with programmatically generated covers (Pillow), `llms.txt` + `llms-full.txt` for AI crawlers, sitemap automation and IndexNow submission. Content pipeline is scripted end to end (single-source catalog generates index, llms files and covers).
- Paid acquisition experiments (Google Ads) instrumented so that channel attribution lands in the product's own admin dashboard, not just in the ads console.

## What this project demonstrates

Shipping and operating a real product alone: architecture choices tied to unit economics, privacy as a design constraint, incident-driven hardening, and a data strategy where every completed audit compounds into a proprietary, anonymized dataset.
