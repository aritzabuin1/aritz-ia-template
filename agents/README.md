# Specialized Agent Prompts

Esta carpeta contiene system prompts para sub-agentes especializados.

---

## ¿Qué son los Sub-Agentes?

Los sub-agentes son instancias de Claude con prompts especializados para tareas específicas. En lugar de que el agente principal intente ser experto en todo, puede delegar trabajo a agentes especializados.

**Ejemplo de delegación**:
```
Agente Principal (orquestador)
    ↓
Delega revisión de seguridad
    ↓
Sub-Agente: Security Reviewer
    ↓
Retorna lista de vulnerabilidades
    ↓
Agente Principal integra feedback
```

---

## Cuándo Crear un Sub-Agente

Crea un sub-agente cuando:
- ✅ La tarea requiere expertise muy específico (security, performance, cloud architecture)
- ✅ La tarea es aislable (puede hacerse independientemente)
- ✅ Quieres resultados más consistentes en esa área específica
- ✅ El contexto del agente principal está saturado (>100K tokens)

NO creas sub-agente si:
- ❌ La tarea es simple y el agente principal puede hacerla bien
- ❌ Requiere demasiado contexto compartido (mejor hacer en un solo agente)
- ❌ Es una tarea única que no se repetirá

---

## Cómo Usar

### 1. Crear el Prompt del Agente

Crea un archivo `agents/nombre-rol.md`:

```markdown
# [Nombre del Rol] Agent

You are a [specialized role] with deep expertise in [domain].

## Your responsibilities:
- [Responsabilidad 1]
- [Responsabilidad 2]
- [Responsabilidad 3]

## Your constraints:
- [Qué NO debe hacer]
- [Limitaciones de scope]

## Your output format:
[Descripción del formato esperado]

## Your tone:
[Tono y estilo de comunicación]

Be [adjetivos que describen el comportamiento deseado].
```

### 2. Invocar desde el Agente Principal

En Claude Code, el agente principal puede:

```markdown
"Necesito revisar este código para vulnerabilidades de seguridad.
Lee el prompt en `agents/security-reviewer.md` y asume ese rol.
Aquí está el código: [código]"
```

O usando el sistema de sub-agentes (ver `directives/00_SUBAGENT_ORCHESTRATION.md`).

---

## Ejemplos de Sub-Agentes Útiles

### Security Reviewer
**Cuándo**: Antes de deploy, revisión de código sensible
**Qué hace**: Encuentra vulnerabilidades, valida secrets management, input sanitization
**Output**: Lista priorizada de issues con severity

### Performance Optimizer
**Cuándo**: App lenta, queries N+1, memory leaks
**Qué hace**: Analiza bottlenecks, sugiere optimizaciones
**Output**: Análisis de performance + recomendaciones específicas

### Cloud Architect
**Cuándo**: Diseñar infraestructura, elegir servicios cloud
**Qué hace**: Propone arquitectura, compara opciones, calcula costes
**Output**: Diagrama de arquitectura + trade-offs

### Data Privacy Officer
**Cuándo**: Proyecto con PII, compliance GDPR/CCPA
**Qué hace**: Valida cumplimiento, revisa flujos de datos
**Output**: Checklist de compliance + gaps

### Testing Specialist
**Cuándo**: Coverage bajo, bugs frecuentes
**Qué hace**: Diseña estrategia de testing, identifica edge cases
**Output**: Plan de testing + casos de test específicos

---

## Template de Sub-Agente

```markdown
# [Role Name] Agent

You are a [role description] with [X years] of experience in [domain].

## Core Expertise
- [Skill 1]
- [Skill 2]
- [Skill 3]

## Your Mission
[What you're optimizing for: security, performance, cost, reliability, etc.]

## What You Review
- [Type of artifact 1: code, configs, designs, etc.]
- [Type of artifact 2]

## What You Check For
1. **[Category 1]**
   - [Specific check]
   - [Specific check]

2. **[Category 2]**
   - [Specific check]
   - [Specific check]

## Your Output Format
```
## [SEVERITY LEVEL] Issues

### [Issue Title]
**Location**: [File:Line or component]
**Impact**: [What breaks or degrades]
**Fix**: [Specific remediation steps]

[Repeat for each issue]
```

## Your Constraints
- DO NOT: [Things to avoid]
- FOCUS ON: [What matters most]
- IGNORE: [What's acceptable/not your concern]

## Your Tone
[Tone description: Technical but clear, direct but constructive, etc.]

Remember: You are an expert advisor, not a gatekeeper. Provide actionable feedback that helps the team improve.
```

---

## Best Practices

1. **Prompt Específico**: El prompt del sub-agente debe ser más específico que el del agente principal
2. **Output Estructurado**: Define formato claro (JSON, Markdown con headers, lista numerada)
3. **Scope Limitado**: El sub-agente solo hace UNA cosa muy bien
4. **Tone Consistente**: Todos tus sub-agentes deben tener tone profesional y constructivo
5. **Versionado**: Si cambias un prompt de sub-agente, documenta qué cambió

---

## Crear Tu Primer Sub-Agente

**Quick Start**:

1. Identifica una tarea repetitiva que requiere expertise específico
2. Copia el template de arriba
3. Personaliza para tu caso de uso
4. Prueba con el agente principal
5. Itera el prompt hasta que los resultados sean consistentes

**Ejemplo - Security Reviewer**:

Crea `agents/security-reviewer.md` con el template, personaliza, y prueba:

```
Agente Principal: "Actúa como Security Reviewer (lee agents/security-reviewer.md).
Revisa este código: [código]"

Sub-Agente: [Analiza y retorna issues estructurados]
```

---

## Cuándo Actualizar el Template Base

Si creas un sub-agente que es:
- ✅ Genérico (no específico del proyecto)
- ✅ Reutilizable (útil en futuros proyectos)
- ✅ Probado (funciona bien)

→ Síncronízalo al template base usando `sync-improvements.sh`

---

**Documentación relacionada**:
- `directives/00_SUBAGENT_ORCHESTRATION.md` - Cómo delegar trabajo a sub-agentes
- `CLAUDE.md` - Referencias a sub-agentes en Operating Principles
