# Architecture and data flow

## Ingestion
1. Manual trigger starts folder-wide Google Drive PDF search.
2. Download and Extract From File produce text items.
3. Code node attaches title, source URL and demo-only flags.
4. Loop Over Items sends each document to Chroma Insert Documents.
5. Default Data Loader loads `$json.text` with metadata and Recursive Character Text Splitter chunks text into 500 characters with 50 overlap.
6. Gemini embeddings create vectors stored in Chroma collection `cpia_protocols_demo`.

## Question answering
1. Telegram receives a message and derives text/session ID.
2. Regex blacklist screens prompt-override patterns; an OpenRouter LLM classifies allowed vs blocked input.
3. Allowed input reaches an AI Agent instructed to use Chroma retrieval first.
4. Chroma tool uses Gemini embeddings and Cohere reranking (`topN: 6`).
5. Response passes a nonempty check and formatting, which extracts apparent PDF names from generated text and appends a demo disclaimer.
6. Telegram sends the response; blocked requests receive a security alert.

## Trust boundaries
- Telegram user text, Google Drive PDF content, and retrieved chunks are untrusted.
- LLM validation and regex patterns are defense-in-depth, not guarantees.
- Source references inferred from model output are not verified provenance.
- Telegram bot access control is not enforced by the exported flow.
- Uploaded documents may include case notes/referrals; never treat them as validated protocols.

## Deployment architecture
Google Drive (private) → n8n (orchestration) → Gemini embeddings → Chroma Cloud; Telegram → n8n guardrails → OpenRouter agent ↔ Chroma Cloud + Cohere → Telegram.
