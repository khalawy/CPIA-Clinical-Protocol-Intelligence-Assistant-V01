# Setup and import

## Prerequisites
- n8n instance with relevant Google Drive, Telegram, Chroma Cloud, Gemini, OpenRouter and Cohere integrations.
- Access to a private Google Drive folder containing **synthetic/de-identified authorized** PDFs.
- A Chroma Cloud collection `cpia_protocols_demo` and compatible Gemini embedding configuration.
- A Telegram bot dedicated to nonclinical demonstration.

## Import
1. In n8n select **Import from File** and import `workflows/CPIA_v1.0_SANITIZED.json`.
2. Reconnect every credential; no working credentials ship with this repository.
3. Replace the folder placeholder in node `02_GDrive_Search_Protocols`.
4. Inspect all nodes for compatibility with your n8n version; check every expression and model configuration.
5. Confirm input metadata maps to the correct source PDF before indexing. The current index-based mapping is fragile.
6. Run the manual ingestion path on synthetic PDFs only. Check number of processed items and Chroma collection content.
7. Test Telegram query and blocking paths; inspect missing/incorrect citations.
8. Add explicit Telegram user/chat allowlisting before any shared demonstration.
9. Do not activate until privacy, provenance, security and safety review is complete.

## Important
- `active: false` is intentionally retained in the sanitized export.
- Never commit original exports, service credentials, bot tokens, private Google Drive IDs, patient PDFs, n8n execution logs, or real chat transcripts.
- The n8n workflow export is a configuration snapshot; it is not a self-contained application deployment.
