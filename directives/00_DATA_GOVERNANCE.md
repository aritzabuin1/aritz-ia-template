# SOP: Data Governance & Privacy (Enterprise Grade)

**Objective**: Ensure compliance with GDPR, CCPA, and strict corporate data policies (Apple/Nike level).

## 1. Data Lineage & Provenance
- **Traceability**: Every output must be traceable to its source document in `docs/` or an external API response.
- **Labeling**: Maintain a clear map of where data comes from and where it is being sent.

## 2. PII & Sensitive Information Handling
- **Sanitization**: BEFORE sending any data to an external LLM (OpenAI/Anthropic), the agent MUST sanitize PII (Names, Emails, IDs, Phone numbers).
- **Redaction**: Use placeholders like `[USER_EMAIL_REDACTED]` for logs and intermediate processing.

## 3. Data Retention & Cleanup
- **Ephemeral Storage**: Files in `.tmp/` must be deleted after the execution phase is completed.
- **Log Privacy**: Never log raw API responses that contain sensitive user data. Log only metadata and status.

## 4. Consent & Access Control
- **Authorization Check**: Before performing "Write" operations on external systems (e.g., Google Drive, Databases), verify the action aligns with the project scope in `PLAN.md`.
