# SOP: Dependency Management & Security
**Objective**: Prevent supply chain attacks and ensure license compliance.

## 1. Version Pinning
- All dependencies in `requirements.txt` must have exact versions (e.g., `fastapi==0.109.0`).
- No floating versions allowed for production.

## 2. Security Scanning (CVEs)
- Use `Snyk` or `pip-audit` to check for known vulnerabilities before any major merge.
- High/Critical vulnerabilities must be patched immediately.

## 3. License Compliance
- Only MIT, Apache 2.0, or BSD licenses allowed.
- Any GPL/AGPL dependency requires explicit approval from Aritz.