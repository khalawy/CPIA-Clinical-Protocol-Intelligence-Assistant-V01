# Security policy

CPIA v1.0 is a demonstration prototype. Do not submit real patient data or use it for clinical decisions.

## Reporting
Report vulnerabilities privately to the repository owner. Do not post tokens, personal health information, private Google Drive links or exploits in public issues.

## Secrets and privacy
The public export removes n8n credential references, webhook IDs, instance metadata, and the configured Google Drive folder ID. It is **not** a substitute for secret scanning and human review. Rotate any secrets if they were ever exposed. Never commit original n8n exports, executions, patient documents, or identifiable Telegram transcripts.

## Hardening before public demo
Add Telegram allowlisting; verify document de-identification; preserve trustworthy chunk provenance; test injection from both user and documents; independently check citations; rate-limit requests; implement logging and retention controls.
