# Limitations and risks (v1.0)

| Area | Observation | Priority |
|---|---|---|
| Privacy | Sample corpus appears to include case notes/referrals; synthetic/de-identified-only requirement | Critical |
| Provenance | `05_Prepare_Document_Metadata` matches extracted items to Drive results by index | High |
| Citation integrity | Screenshots show empty source URLs and malformed source-reference names | High |
| Factual validation | Output gate tests nonempty response, not claim-to-source entailment | High |
| Access control | Exported Telegram path has no explicit allowlist | High |
| Source status | Demo-only and unverified fields are labels, not document validation | High |
| LLM security | Regex and LLM classification can miss prompt injections | High |
| Output formatting | PDF names are regex-extracted from generated answer rather than trusted retrieval metadata | Medium |
| Availability | `openrouter/free` model availability and behavior can vary | Medium |
| Export state | Workflow exported with `active: false` | Informational |

## Scope
The repository documents a proof of concept, not a medical device, validated clinical guideline repository, or production-ready service. Do not rely on medical examples as treatment guidance. Validate against authoritative current guidelines and qualified clinicians outside this demo.
