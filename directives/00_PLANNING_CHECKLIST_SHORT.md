# Enterprise Planning Checklist - Quick Reference

**Use this for quick evaluation. For detailed explanations, see `00_PLANNING_CHECKLIST.md`**

---

## Quick Decision Matrix

| Project Type | Core Requirements | Advanced Requirements |
|--------------|-------------------|----------------------|
| **Internal MVP** | 1, 2, 3, 5, 6, 19 | None |
| **Client-Facing** | 1-8, 11, 13, 19 | 9, 10, 12, 14, 20 |
| **Enterprise** | ALL (1-8) | ALL (9-20) |

---

## Core Requirements (Always Evaluate)

| # | Requirement | When to Apply | When to Skip |
|---|------------|---------------|--------------|
| 1 | **Error Handling** | ALWAYS | Never |
| 2 | **Logging** | ALWAYS | Never |
| 3 | **Secrets Management** | ALWAYS | Never |
| 4 | **PII Protection** | If handling user data | Mandatory per 00_DATA_GOVERNANCE.md |
| 5 | **Input Validation** | ALWAYS | Never |
| 6 | **Testing** | ALWAYS (at least critical paths) | Unit tests + 00_EVALUATION_PROTOCOLS.md |
| 7 | **Observability** | Production systems | MVPs, prototypes |
| 8 | **Prompt Injection Protection** | User-facing LLM | Internal, trusted users only |

---

## Advanced Requirements (Case-by-Case)

| # | Requirement | When to Apply | When to Skip |
|---|------------|---------------|--------------|
| 9 | **Idempotency** | Webhooks, payments, writes | Read-only, stateless |
| 10 | **Prompt Caching** | Repeated queries, high volume | Real-time, personalized |
| 11 | **Rate Limiting** | Public APIs, resource-heavy | Internal tools, trusted users |
| 12 | **Retries & Backoff** | External APIs, network ops | Non-idempotent writes |
| 13 | **Timeouts** | ALWAYS (all external calls) | Internal, synchronous |
| 14 | **Fallbacks** | Critical features | Non-critical, single provider |
| 15 | **Cost Monitoring** | ANY paid APIs / Production / LLM projects | Track tokens/logs per 00_COST_OBSERVABILITY.md |
| 16 | **Disaster Recovery** | Important data, production DB | Ephemeral, reproducible |
| 17 | **Audit Trails** | Compliance, financial, admin | Simple tools, no compliance |
| 18 | **GDPR/CCPA** | EU/CA users, ANY PII | No user data, internal |
| 19 | **Health Checks** | ALL production services | Dev only, not deployed |
| 20 | **Prompt Versioning** | Production LLM, A/B testing | Prototypes, stable prompts |

---

## Quick Evaluation Process

**When starting a project:**

1. **Identify project type** (Internal MVP / Client-Facing / Enterprise)
2. **Check matrix** → start with suggested requirements
3. **For each requirement:**
   - Does it apply? → Mark ✅ in PLAN.md
   - If yes, how implement? → Document approach
   - If no, why not? → Document reason
4. **Review with Aritz** → get approval
5. **Implement** → follow plan

**When user asks "¿nos falta algo?":**

1. **Re-read this checklist**
2. **Compare vs current implementation**
3. **Identify gaps** (marked as ✅ but not implemented, or should be ✅ but isn't)
4. **Propose additions** if project evolved
5. **Update PLAN.md** with gaps + remediation

---

## 3 Key Questions for Each Requirement

1. **Does this apply to our project?**
   - Consider: user data, public vs internal, compliance needs, risk level

2. **What's the priority?**
   - High (blocking, must-have)
   - Medium (important, should-have)
   - Low (nice-to-have)

3. **How will we implement it?**
   - Specific approach, tools, timeline
   - Document in PLAN.md

---

## Common Patterns

**High-Volume LLM System** (e.g., document analysis):
- ✅ MUST: 1, 2, 3, 5, 6, 10, 13, 15, 19
- ⚠️ CONSIDER: 11, 12, 14, 20

**User-Facing Chat/Assistant**:
- ✅ MUST: 1, 2, 3, 4, 5, 6, 7, 8, 11, 19
- ⚠️ CONSIDER: 10, 12, 13, 14, 18, 20

**Webhook/Event Processing**:
- ✅ MUST: 1, 2, 3, 5, 6, 9, 12, 13, 19
- ⚠️ CONSIDER: 11, 15, 17

**Internal Automation Tool**:
- ✅ MUST: 1, 2, 3, 5, 6, 19
- ⚠️ CONSIDER: 12, 13, 15

---

## Red Flags - Don't Skip These

🚨 **NEVER skip** for ANY production system:
- Error Handling (1)
- Logging (2)
- Secrets Management (3)
- Input Validation (5)
- Health Checks (19)

🚨 **NEVER skip** if handling user data:
- PII Protection (4)
- GDPR/CCPA if applicable (18)

🚨 **NEVER skip** if user-facing LLM:
- Prompt Injection Protection (8)
- Rate Limiting (11)

---

## Next Steps

**For detailed explanations:**
→ Read `00_PLANNING_CHECKLIST.md`

**When planning:**
1. Use this checklist for quick evaluation
2. Mark applicable requirements in PLAN.md table
3. Consult full checklist for implementation details
4. Get Aritz's approval

**When reviewing:**
1. Compare current state vs this checklist
2. Identify gaps
3. Prioritize fixes
4. Update PLAN.md

---

**Last updated**: February 2025  
**See also**: `00_PLANNING_CHECKLIST.md` (full version with detailed explanations)
