# Lexi: AI news curation, from RSS ingestion to adaptive synthesis

> An AI product where trust is the feature: LLM synthesis with per-claim provenance, a resilient self-hosted ingestion stack with a real test suite, and pre-launch QA run as an engineering problem via browser automation.

**Product**: Lexi, a personalized news curation app: the user picks sources and topics, the system ingests articles continuously, and an LLM produces "Lexis", synthesized briefings with adaptive depth (deep / focused / panorama modes), plus a daily digest and a weekly report. Technical beta (installable PWA + Expo builds), not yet in app stores.

**My role**: solo design and build.

## Architecture

- **Backend**: Firebase serverless (Cloud Functions v2 + Firestore multi-database + Storage), TypeScript, esbuild.
- **Ingestion**: RSS-first, with a **self-hosted RSSHub on Cloud Run** for sources without feeds, plus templated scrapers with **144 unit tests** and homepage article detection as fallback. Ingestion has a fallback chain, so one broken source never empties a feed.
- **Synthesis**: LLM pipeline with clickable citations and per-claim provenance (every claim in a Lexis links back to the source article), quality badges, and a feedback loop that captures which matched keywords actually interested the user.
- **App**: Expo (React Native) targeting iOS, Android and Web from one codebase; onboarding with starter packs, LGPD consent flow, and a Stripe + Mercado Pago paywall (Free / Pro / Pro Plus).

## Hardening before beta

Six focused sprints: App Check + rate limits, diagnostics persistence, provenance/citations, feedback loop, audit log + metrics + SLOs, and ingestion fallbacks with an operational runbook.

Then a **functional audit driven by browser automation**: four rounds of real navigation through every page with Chrome DevTools automation, inspecting the live DOM, which surfaced 40+ real issues (broken states, dead ends, inconsistent UI) that unit tests could never catch.

## What this project demonstrates

Designing an AI product where trust is a feature (citations, provenance, quality badges), running LLM synthesis on a serverless budget, and treating pre-launch QA as an engineering problem (automated functional audits, SLOs, runbooks) rather than a checklist.
