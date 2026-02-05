# Plan del Proyecto: [Nombre del Proyecto]

**Cliente**: [Nombre del cliente]  
**Fecha inicio**: [Fecha]  
**Estado**: 🟡 Planificación / 🟢 Desarrollo / 🔵 Revisión / ✅ Completado  
**Tipo**: 🚀 MVP (<1 semana) / 🏗️ Standard (1-4 semanas) / 🏢 Enterprise (>1 mes)

---

## 1. Resumen Ejecutivo

**¿Qué estamos construyendo?**
[Descripción en 2-3 frases del objetivo del proyecto]

**¿Para quién?**
[Cliente, usuario final, caso de uso]

**¿Por qué?**
[Problema que resuelve, valor que aporta]

**¿Cuándo?**
[Timeline esperado, hitos clave, fecha de entrega]

**Presupuesto estimado:**
- Tiempo desarrollo: [X horas/días]
- Costes APIs estimados: $[X]/mes
- Infrastructure: $[X]/mes

---

## 2. Scope del Proyecto

### In Scope (Lo que SÍ estamos construyendo)
- [ ] [Feature 1]
- [ ] [Feature 2]
- [ ] [Feature 3]

### Out of Scope (Lo que NO estamos construyendo ahora)
- ❌ [Feature X] - Razón: [por qué no]
- ❌ [Feature Y] - Razón: [por qué no]
- ❌ [Feature Z] - Razón: [por qué no]

---

## 3. Requerimientos Funcionales

### User Stories / Casos de Uso

1. **Como [X], quiero [Y], para [Z]**
   - Criterios de aceptación:
     - [ ] [Criterio 1]
     - [ ] [Criterio 2]
     - [ ] [Criterio 3]

2. **[User story 2]**
   - Criterios de aceptación: [...]

3. **[User story 3]**
   - Criterios de aceptación: [...]

---

## 4. Arquitectura Técnica

**Nota para el agente**: Por cada decisión técnica relevante tomada en esta sección (ya sea en el Stack, Infraestructura o Decisiones Clave), debes generar un documento ADR en `docs/decisions/` siguiendo la plantilla `directives/00_ADR_TEMPLATE.md`.

### Stack Tecnológico

**Backend**:
- Framework: [FastAPI / Flask / Django]
- Base de datos: [PostgreSQL / MongoDB / SQLite]
- Cache: [Redis / Memcached / None]
- Hosting: [Modal / Railway / AWS / Google Cloud]

**APIs Externas**:
- [OpenAI GPT-4 para X]
- [Anthropic Claude para Y]
- [Otras APIs necesarias]

**Infraestructura**:
- [Webhooks / Cron jobs / Workers]
- [Monitoring: logging, metrics]
- [CI/CD: GitHub Actions / None]

### Diagrama de Arquitectura

```
[Descripción textual o ASCII del flujo de datos]

User → API Gateway → [Backend Service] → Database
                   ↓
              LLM API (OpenAI/Anthropic)
                   ↓
             Cache (Redis)
```

### Decisiones Arquitectónicas Clave

1. **[Decisión 1 - ej: "PostgreSQL vs MongoDB"]**
   - **Decisión**: [Qué elegimos]
   - **Por qué**: [Razones principales]
   - **Alternativas**: [Qué descartamos y por qué]
   - **Trade-offs**: [Ventajas y desventajas]

2. **[Decisión 2]**: [...]

3. **[Decisión 3]**: [...]

---

## 5. Checklist de Requerimientos Enterprise

> **Ver `directives/00_PLANNING_CHECKLIST.md` para explicaciones detalladas de cada requerimiento.**

Marca con ✅ lo que aplica, ❌ lo que no aplica, 🔄 lo que está pendiente de decisión.

### Core Requirements (Siempre Evaluar)

| # | Requerimiento | Aplica | Prioridad | Implementación |
|---|--------------|--------|-----------|----------------|
| 1 | Error Handling | ✅/❌ | Alta/Media/Baja | [Cómo lo implementaremos] |
| 2 | Logging | ✅/❌ | Alta/Media/Baja | [Cómo lo implementaremos] |
| 3 | Secrets Management | ✅/❌ | Alta | [.env, Railway vars, etc] |
| 4 | PII Protection | ✅/❌ | Alta/N/A | [Si hay PII, cómo proteger] |
| 5 | Input Validation | ✅/❌ | Alta | [Pydantic, sanitization] |
| 6 | Testing | ✅/❌ | Media | [Pytest, coverage target] |
| 7 | Observability | ✅/❌ | Media | [/health, métricas] |
| 8 | Prompt Injection Protection | ✅/❌ | Alta/N/A | [Si user-facing LLM] |

### Advanced Requirements (Evaluar según contexto)

| # | Requerimiento | Aplica | Prioridad | Implementación |
|---|--------------|--------|-----------|----------------|
| 9 | Idempotency | ✅/❌ | [?] | [Si aplica: webhooks, payments] |
| 10 | Prompt Caching | ✅/❌ | [?] | [Redis, TTL, hit rate target] |
| 11 | Rate Limiting | ✅/❌ | [?] | [Límite: X req/min] |
| 12 | Retries & Backoff | ✅/❌ | [?] | [Max 3 retries, exponential] |
| 13 | Timeouts | ✅/❌ | [?] | [API: 30s, DB: 5s] |
| 14 | Fallbacks | ✅/❌ | [?] | [OpenAI → Claude, o degrade] |
| 15 | Cost Monitoring | ✅/❌ | [?] | [Track tokens, alert $X/día] |
| 16 | Disaster Recovery | ✅/❌ | [?] | [Backups diarios] |
| 17 | Audit Trails | ✅/❌ | [?] | [Log admin actions] |
| 18 | GDPR/CCPA | ✅/❌ | [?] | [Export/delete endpoints] |
| 19 | Health Checks | ✅/❌ | Alta | [Recomendado siempre] |
| 20 | Prompt Versioning | ✅/❌ | [?] | [Version hash con LLM calls] |

**Decisiones tomadas**:
- [Resumen de qué requirements aplican y por qué]
- [Qué decidimos NO implementar y razones]

---

## 6. Directivas y Herramientas

### Directivas Nuevas a Crear

1. **`directives/[nombre_tarea].md`**
   - Propósito: [Qué hace esta directiva]
   - Herramienta ejecutora: `execution/[script].py`

2. **[Otra directiva]**: [...]

### Directivas Existentes a Modificar

1. **`directives/existing.md`**
   - Cambios necesarios: [Qué actualizaremos y por qué]

### Scripts de Ejecución a Crear

1. **`execution/[nombre_script].py`**
   - Input: [Qué recibe]
   - Output: [Qué produce]
   - Dependencias: [APIs, librerías necesarias]
   - Estimación: [X horas]

2. **[Otro script]**: [...]

### Sub-agentes Necesarios
- [ ] **[Nombre Agente]**: Propósito y prompt en `agents/[nombre].md`

---

## 7. Fases de Implementación

### Fase 1: Setup y Fundamentos
**Duración estimada**: [X días]

- [ ] Estructura de proyecto (carpetas, .gitignore)
- [ ] Setup .env y gestión de secretos
- [ ] Logging básico configurado
- [ ] Health check endpoint
- [ ] Testing framework setup

**Entregable**: Proyecto base funcional

---

### Fase 2: Core Functionality
**Duración estimada**: [X días]

- [ ] Implementar [feature principal 1]
- [ ] Implementar [feature principal 2]
- [ ] Validación de inputs (Pydantic)
- [ ] Error handling básico
- [ ] Tests unitarios para core

**Entregable**: MVP funcional

---

### Fase 3: Optimización y Seguridad
**Duración estimada**: [X días]

- [ ] Prompt caching (si aplica)
- [ ] Rate limiting (si aplica)
- [ ] Guardrails de seguridad
- [ ] Tests de integración
- [ ] Performance tuning

**Entregable**: Sistema optimizado y seguro

---

### Fase 4: Monitoring y Deploy
**Duración estimada**: [X días]

- [ ] Monitoring configurado
- [ ] Alertas setup
- [ ] Documentación completa (README, architecture)
- [ ] Deploy a producción
- [ ] Handoff al cliente

**Entregable**: Sistema en producción con monitoring

---

## 8. Riesgos y Mitigaciones

| Riesgo | Probabilidad | Impacto | Mitigación |
|--------|--------------|---------|------------|
| [Riesgo 1 - ej: API externa down] | Media | Alto | Implementar fallback + retry logic |
| [Riesgo 2 - ej: Budget API excedido] | Baja | Alto | Cost monitoring + alertas + límites |
| [Riesgo 3 - ej: Datos PII leaked] | Baja | Crítico | Validación inputs + no logging PII |
| [Riesgo 4] | [?] | [?] | [Cómo mitigamos] |

---

## 9. Criterios de Éxito

### Técnicos
- [ ] Sistema procesa X requests/día sin errores críticos
- [ ] Latencia p95 < X segundos
- [ ] Coverage de tests > X%
- [ ] 0 secrets expuestos en repo
- [ ] 0 incidents críticos en primera semana producción
- [ ] Todos los requirements enterprise marcados ✅ están implementados

### Negocio
- [ ] Cliente puede [hacer X] sin intervención manual
- [ ] Coste operacional < $X/mes
- [ ] Tiempo de procesamiento reducido en Y%
- [ ] [Otros KPIs específicos del cliente]

---

## 10. Plan de Testing

### Unit Tests
- [ ] [Componente A] - Coverage target: X%
- [ ] [Componente B] - Coverage target: Y%

### Integration Tests
- [ ] [Workflow completo 1]
- [ ] [Workflow completo 2]

### Manual Testing
- [ ] [Scenario 1 - happy path]
- [ ] [Scenario 2 - error cases]
- [ ] [Scenario 3 - edge cases]

### LLM Quality Evals (Systematic)
- [ ] Crear Golden Dataset en `tests/evals/` según `directives/00_EVALUATION_PROTOCOLS.md`
- [ ] Ejecutar suite de evaluación vía `execution/run_evals.py`
- [ ] Objetivo de Quality Score: >85%
---

## 11. Próximos Pasos

**ANTES de empezar implementación:**
1. ✋ **[Aritz] Revisar y aprobar este plan**
   - Comentarios/cambios requeridos: [...]
   - Aprobación: ❌ Pendiente / ✅ Aprobado

**Una vez aprobado:**
2. **[Agente] Empezar Fase 1**
3. **[Agente] Actualizar ARITZ.md con decisiones tomadas**
4. **[Agente] Actualizar este PLAN.md conforme avanzamos** (marcar ✅ completados)

---

## 12. Log de Cambios al Plan

| Fecha | Cambio | Razón |
|-------|--------|-------|
| [Fecha inicial] | Plan creado | - |
| [Fecha] | [Qué cambió] | [Por qué cambió] |

---

**Notas finales**:
- Este plan es un documento vivo. Se actualiza durante el proyecto.
- Cambios significativos requieren aprobación de Aritz.
- Todas las implementaciones deben documentarse en ARITZ.md.
- Ver `directives/00_PLANNING_CHECKLIST.md` para más detalles sobre cada requirement.
