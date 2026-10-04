# Rare Disease Atlas

**Find → Challenge → Act**

Rare Disease Atlas is a hackathon prototype for turning fragmented rare-disease knowledge into **source-backed research decisions**.

Instead of stopping at “these diseases look related,” the Atlas asks a harder question:

> **Would this connection survive scrutiny — and what is the smallest useful next step if it does?**

Built for the Hack-Nation × OpenAI × Buffalo Initiative rare-disease challenge.

## The problem

Rare-disease communities often face a fragmented landscape of disease names, genes and variants, phenotypes, papers, registries, studies, researchers, patient groups, and research assets. Finding a promising connection is only part of the work. A patient organization also needs to understand:

- Why the connection exists
- What evidence supports it
- What evidence argues against it
- What is still unknown
- Whether an existing asset can be reused
- Who could help validate it
- What concrete action should happen next

## Our wedge: not another biomedical chatbot

Many AI prototypes can retrieve papers or summarize a knowledge graph. Rare Disease Atlas is designed as a **collaboration decision engine**.

The core loop is:

### 1. Find

Surface non-obvious disease neighbors through mechanism, gene/variant biology, phenotype, research assets, and network overlap — not disease-name similarity alone.

### 2. Challenge

Every proposed connection can be challenged. The product exposes:

- supporting evidence
- counter-evidence
- unresolved assumptions
- missing evidence
- a falsifiable next experiment

A valid outcome is **“not supported yet.”**

### 3. Act

Connections that survive the challenge can become an actionable research handoff: a reusable asset, a potential collaborator, a validation experiment, and a trusted route to contact.

## Family-friendly collaboration

The prototype deliberately avoids open patient-to-patient DMs.

Instead, **Mechanism Constellations** organize verified patient groups, researchers, registries, studies, and other research actors around a shared biological question.

Users can:

- **Join constellation**
- **Request introduction**
- see the shared research objective before contacting anyone

The aim is coordinated research rather than building another social network.

## Prototype demo

Open `index.html` in a browser.

The current demo uses Gaucher disease as an illustrative starting point and demonstrates:

1. selecting an unexpected research neighbor
2. clicking **Challenge this connection**
3. reviewing support, counter-evidence, and missing evidence
4. seeing a proposed decisive experiment
5. finding a mechanism constellation
6. joining it or requesting a trusted introduction

No build step or external dependency is required.

## Evidence architecture

The intended production graph uses stable entities, synonyms, provenance, confidence, and evidence attached to every edge.

Planned source categories from the challenge brief include:

| Layer | Sources |
| --- | --- |
| Disease & phenotype normalization | MONDO, HPO |
| Genes & variants | OMIM, ClinVar |
| Literature & investigators | PubMed, PMC |
| Studies & reusable assets | ClinicalTrials.gov, NIH RePORTER |
| Patient communities | NORD, Global Genes, Orphanet, EURORDIS, Rare Disease UK, Genetic Alliance |
| Stretch sources | Jackson Laboratory, RareConnect, bioRxiv, medRxiv |

OpenAI models can support entity/claim extraction, name reconciliation, evidence synthesis, and plain-language path explanations. The model should not be the source of truth: graph claims must resolve back to evidence.

## Evidence integrity principles

A research connection should carry:

- **Claim** — what the edge asserts
- **Source** — where the evidence came from
- **Evidence type** — paper, variant record, trial, registry, etc.
- **Date/version**
- **Confidence**
- **Supporting evidence**
- **Contradictory evidence**
- **Unknowns / evidence debt**

The UI should make uncertainty visible rather than hiding it behind a single AI confidence score.

## 10× thesis

The prototype does **not** claim to make drug development 10× faster.

The measurable milestone is narrower and testable:

> Reduce the time from an isolated diagnosis/community question to a **qualified, source-backed collaboration candidate and validation plan** from weeks of disconnected searching and outreach to a brief that can be expert-reviewed in days.

That baseline should be validated with patient organizations and rare-disease researchers.

## Architecture direction

```
source ingestion
      ↓
entity extraction + normalization
      ↓
evidence graph
      ↓
connection ranking
      ↓
adversarial evidence challenge
      ↓
action / collaboration handoff
```

A production implementation would keep graph reasoning constrained to retrieved evidence and record the provenance behind every generated explanation.

## Why this could stand out

The memorable interaction is not “ask AI a rare-disease question.”

It is:

**Find a surprising connection → try to disprove it → reveal what evidence is missing → identify the smallest experiment → connect the people/assets capable of running it.**

This turns the graph from a visualization into a research decision tool.

## Run the live prototype

Requires Node.js 18+ and an OpenAI API key.

```bash
npm install
cp .env.example .env
# add your OPENAI_API_KEY to .env
set -a && source .env && set +a
npm start
```

Then open `http://localhost:3000`.

The live challenge flow calls the server, retrieves current PubMed and ClinVar metadata through NCBI E-utilities, then sends only the retrieved evidence to OpenAI for an adversarial review. The API key never belongs in browser code.

### API routes

- `GET /api/health` — backend/OpenAI configuration status
- `GET /api/evidence?disease=...&neighbor=...` — live PubMed + ClinVar evidence slice
- `POST /api/challenge` — evidence-constrained OpenAI challenge

## Deployment

This version requires a Node-capable host and the `OPENAI_API_KEY` environment variable. A static-only host can display `index.html`, but it cannot execute the live evidence/OpenAI endpoints.

## Current status

This repository now contains a small full-stack prototype. **Challenge mode retrieves live PubMed and ClinVar metadata and uses OpenAI to evaluate only that retrieved evidence.** The discovery-neighbor graph itself remains a curated demonstration, and generated conclusions are research leads requiring expert review. Authentication, verified organization identities, broader source ingestion, full-text evidence extraction, and production safety/privacy controls remain future work.

## Safety

Rare Disease Atlas is a research-navigation concept, **not medical advice**, a diagnostic system, or a treatment recommendation tool. Proposed cross-disease connections require expert review and source validation before use in research or clinical decision-making.

## Repository

- `index.html` — interactive Find → Challenge → Act UI
- `server.js` — live NCBI retrieval + server-side OpenAI challenge API
- `package.json` — Node dependencies and run scripts
- `.env.example` — environment variable template (never commit a real key)
- `README.md` — product thesis, evidence approach, demo flow, and architecture
