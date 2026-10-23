# CPIA — Clinical Protocol Intelligence Assistant

![CPIA architecture](assets/architecture-overview.jpg)

**Release:** v1.0.0 (documented baseline) · **Environment:** demo only · **Status:** research/educational prototype, **not clinically validated**.

> ⚠️ **UNVERIFIED DOCUMENTS — DEMO ONLY — NOT FOR CLINICAL DECISIONS.** CPIA is a demonstration of document-grounded question answering, not a medical device, clinical decision support system, or substitute for validated guidelines and clinician judgment. Do not enter identifiable patient data.

## Overview

CPIA is an n8n-based proof of concept for asking questions about a private collection of PDF medical documents through Telegram. A retrieval-augmented generation (RAG) pipeline stores extracted PDF text in Chroma Cloud and makes retrieved context available to an AI agent. Input validation and response-formatting steps aim to limit unsafe or unsupported output.

The uploaded workflow contains **30 nodes** arranged in two functional paths within **one exported n8n workflow**. Screenshots show example demo responses and a blocked prompt-injection attempt; these examples do **not** establish systematic safety or clinical accuracy.

## Architecture

![n8n workflow](assets/workflow-overview.png)

### A. Document ingestion

`Manual Trigger → Google Drive Search → Download file → Extract from File (PDF) → Prepare Document Metadata → Loop Over Items → ChromaDB Insert Documents`

Chroma ingestion uses **Gemini embeddings**, **Default Data Loader**, and **Recursive Character Text Splitter** (chunk size **500**, overlap **50**). The configured Chroma collection is `cpia_protocols_demo`. Metadata includes document ID/title, source URL, approval status `demo_only`, and document version `unverified`.

### B. Telegram question answering

`Telegram Trigger → Prepare Telegram Input → Blacklist Check → Blacklist Gate → OpenRouter LLM Input Validator → Validate Output → Validation Gate → AI Agent + Chroma Retrieval + Cohere Reranker → Prepare Agent Output → Output Safety Gate → Format Final Response → Telegram reply`

Blocked paths return a security message. The AI agent is instructed to retrieve evidence first, avoid inventing references, distinguish guidelines from case notes, and declare the demo status. The retrieval tool uses Chroma Cloud, Gemini embeddings and Cohere reranking (`topN: 6`). A Simple Memory node is configured with context window 20.

## Technology stack

- **Orchestration:** n8n (workflow JSON included)
- **User interface:** Telegram Bot
- **Document source:** Google Drive PDF folder
- **Extraction/chunking:** n8n Extract From File, Default Data Loader, Recursive Character Text Splitter
- **Embeddings:** Google Gemini
- **Vector store:** Chroma Cloud
- **Reranking:** Cohere
- **LLM / validator:** OpenRouter chat model (`openrouter/free` in exported configuration)

## Repository structure

```text
CPIA-GitHub-v1.0/
├── README.md
├── workflows/CPIA_v1.0_SANITIZED.json
├── docs/ARCHITECTURE.md
├── docs/SETUP.md
├── docs/LIMITATIONS_AND_RISKS.md
├── tests/TEST_PLAN.md
├── config/.env.example
├── assets/
├── SECURITY.md
├── CHANGELOG.md
└── .gitignore
```

## Setup (n8n Cloud)

1. Create a **private** repository or download this repository locally.
2. In n8n, use **Import from File** and select `workflows/CPIA_v1.0_SANITIZED.json`.
3. Reconnect the **Google Drive, Chroma Cloud, Telegram, OpenRouter, Gemini, and Cohere** credentials. Credentials are intentionally omitted from this repository.
4. Replace `REPLACE_WITH_GOOGLE_DRIVE_FOLDER_ID` in the Google Drive search query with your folder ID **inside your private n8n instance**.
5. Confirm the Chroma collection `cpia_protocols_demo` exists and that insertion and retrieval use compatible embedding models.
6. Ingest **only synthetic or irreversibly de-identified, authorized demonstration PDFs**. Keep original PDFs outside this repository.
7. Verify document-to-metadata association, then test ingestion, retrieval, input blocking, refusal on absent evidence, citations and Telegram formatting before enabling the workflow.
8. Enable only after reviewing all configured node settings and confirming the Telegram bot is restricted to authorized demo users.

See [setup details](docs/SETUP.md) and [known limitations](docs/LIMITATIONS_AND_RISKS.md).

## Evidence and test status

Provided screenshots demonstrate Telegram responses to questions on diabetes-related uploaded PDFs and a rejected prompt-injection attempt. These are **illustrative manual observations**; no quantified retrieval precision, citation accuracy, clinical accuracy, red-team coverage, or production readiness has been established.

See [test plan](tests/TEST_PLAN.md).

## Known issues / next priorities

- The source URL is blank in some observed Telegram outputs, and some source-reference text is malformed.
- The metadata-preparation code pairs files with extracted text **by array index**; item linking should replace this before relying on provenance.
- Current output validation checks for nonempty text; it is **not** independent factual or citation verification.
- Exported workflow status is `active: false`.
- The exported workflow contains no enforced Telegram user allowlist; secure deployment needs explicit access controls.
- Some example inputs are clinical case notes/referrals rather than approved protocols; source-type separation and privacy checks are required.
- Manual demo observations do not establish safety, accuracy, or clinical validity.

## Data governance

No patient records, clinical PDF corpus, credentials, or live service identifiers are distributed in this repository. The `approval_status: demo_only` and `document_version: unverified` fields are descriptive flags, **not approval or validation evidence**.

## Versioning

- `v1.0.0` — sanitized snapshot of uploaded n8n workflow and observed demo behavior.
- `v2.0` — planned; not represented as implemented here.

## License

No open-source license is granted by this repository unless the owner explicitly adds one. All rights reserved by default.
