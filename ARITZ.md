# Knowledge Base - Aritz

Última actualización: Febrero 2025

---

## Introducción

Este documento es mi base de conocimiento acumulativa. Aquí documento TODO lo que aprendo en cada proyecto: decisiones técnicas, problemas resueltos, optimizaciones implementadas, y lecciones aprendidas.

**Objetivo**: Convertirme en un arquitecto de soluciones IA cada vez más profesional, valioso y eficiente con cada proyecto.

**Cómo usar este documento**:
- Revisar antes de empezar proyectos similares
- Consultar cuando enfrento problemas conocidos
- Identificar patrones reutilizables
- Medir mi progreso técnico a lo largo del tiempo

---

## Índice de Proyectos

1. [Proyecto Ejemplo: Automatización Due Diligence](#proyecto-ejemplo-automatización-due-diligence---enero-2025) ← Ejemplo ilustrativo
2. [Próximos proyectos se añadirán aquí...]

---

## Proyecto Ejemplo: Automatización Due Diligence - Enero 2025

> **NOTA**: Este es un proyecto de EJEMPLO para ilustrar cómo documentar. Los proyectos reales se documentarán siguiendo esta misma estructura.

### Resumen del Proyecto

Sistema que analiza memorandos de inversión (documentos PDF de 50-200 páginas) extrayendo automáticamente riesgos, oportunidades, y métricas financieras clave. Genera informes ejecutivos de 2 páginas para el equipo de inversión. Procesa aproximadamente 30 documentos por semana. Cliente: Fondo de inversión mid-market con equipo de 5 analistas que gastaban 4-6 horas por documento en análisis inicial.

**Resultado final**: Reducción del tiempo de análisis inicial de 5 horas a 45 minutos por documento, ahorro mensual en costes de personal ~$15K.

### 1. QUÉ HICIMOS - Implementaciones Clave

#### Prompt Caching para Reducción de Costes 90%

**Qué**: Implementamos caching de respuestas LLM usando hash de (prompt + document_chunk) como key en Redis.

**Por qué**: Muchos documentos de inversión tienen secciones similares (legal boilerplate, términos estándar, métricas comunes). Estábamos gastando $450/mes en llamadas repetidas a GPT-4 analizando contenido idéntico. El cliente tiene presupuesto ajustado y necesitábamos optimizar costes sin sacrificar calidad.

**Cómo**:
1. Setup de Redis en Railway ($5/mes, tier básico suficiente)
2. Creamos función `cached_llm_call(prompt, content, model, ttl=3600)`:
   - Genera cache_key = `sha256(prompt + content + model)`
   - Check Redis: `redis.get(cache_key)`
   - Si cache hit: return cached response inmediatamente (log hit)
   - Si cache miss: call OpenAI API, store resultado en Redis con TTL, return response
3. TTL configurado en 1 hora (documentos analizados raramente se reanalizan mismo día, pero puede haber revisiones)
4. Logging de cache hit rate para monitoreo y optimización

**Código relevante**:
```python
import redis
import hashlib
import json
import os

redis_client = redis.from_url(os.getenv('REDIS_URL'))

def cached_llm_call(prompt, content, model="gpt-4", ttl=3600):
    # Generar key única basada en inputs
    cache_key = hashlib.sha256(
        f"{prompt}{content}{model}".encode()
    ).hexdigest()
    
    # Intentar recuperar de cache
    cached = redis_client.get(cache_key)
    if cached:
        logger.info(f"Cache HIT: {cache_key[:8]}")
        return json.loads(cached)
    
    # Cache miss - llamar a API
    logger.info(f"Cache MISS: {cache_key[:8]}")
    response = openai_client.chat.completions.create(
        model=model,
        messages=[{"role": "user", "content": f"{prompt}\n\n{content}"}]
    )
    
    # Guardar en cache
    redis_client.setex(
        cache_key, 
        ttl, 
        json.dumps(response.choices[0].message.content)
    )
    return response.choices[0].message.content
```

**Resultado**: 
- Cache hit rate: 73% tras primera semana de operación
- Coste mensual OpenAI: $450 → $122 (reducción 73%)
- Latencia mejorada: 8.2s promedio → 1.4s en cache hits
- ROI positivo desde día 2 (costes Redis $5/mes vs ahorro $328/mes)
- Cliente muy satisfecho con optimización

**Aplicabilidad futura**: Aplicar a cualquier proyecto con análisis repetitivo de contenido. Considerar aumentar TTL a 24-48h si contenido es completamente estático. Para contenido que cambia frecuentemente (ej: análisis de noticias en tiempo real), reducir TTL a 15-30 min o implementar invalidación selectiva.

#### Sistema de Guardrails para Prompt Injection

**Qué**: Implementamos validación multicapa para prevenir prompt injection en endpoints donde analistas pueden enviar queries personalizadas al sistema.

**Por qué**: Aunque sistema es interno, analistas pueden (intencionalmente o no) formular queries que extraigan el system prompt, accedan a datos de otros documentos, o modifiquen comportamiento del análisis. Riesgo de contaminación de resultados y potencial leak de datos sensibles.

**Cómo**:
1. **Capa 1 - Detección de Patrones**: Regex para detectar intentos obvios
2. **Capa 2 - Validación Pydantic**: Type checking + content validation
3. **Capa 3 - System Prompt Hardening**: Delimiters claros entre system y user input
4. **Capa 4 - Output Filtering**: Verificar responses no contengan fragments de system prompt
5. **Logging de Intentos**: Track todos los intentos bloqueados para análisis

**Resultado**:
- 0 incidentes en 3 meses de producción
- 12 intentos bloqueados (mayormente queries mal formadas, no maliciosas)
- Patrón documentado y reutilizable para futuros proyectos

**Aplicabilidad futura**: Usar en CUALQUIER interfaz LLM donde users puedan influir en prompts.

### 2. DECISIONES ARQUITECTÓNICAS

#### Decisión: FastAPI + Pydantic para Backend

**Decisión**: Usar FastAPI con Pydantic models en lugar de Flask.

**Alternativas consideradas**: 
- Flask + marshmallow (más maduro, team familiar)
- Django REST (overkill para este proyecto)

**Razones**: 
- Necesitábamos async/await para llamadas concurrentes a OpenAI (procesar múltiples secciones de documento en paralelo)
- Pydantic validation out-of-the-box crítico para enterprise-level input validation
- FastAPI's automatic OpenAPI docs útil para handoff al cliente
- Type hints = menos bugs, mejor DX

**Trade-offs**:
- Ventajas: Performance 2-3x mejor que Flask sync, validación robusta, docs automáticas
- Desventajas: Curva aprendizaje para async/await, menos ecosystem maduro que Flask

**Resultado**: Excelente decisión. Async permitió procesar documentos 60% más rápido. Pydantic catcheó múltiples edge cases en validación que hubieran llegado a producción.

#### Decisión: PostgreSQL para Almacenamiento vs MongoDB

**Decisión**: PostgreSQL en Railway.

**Alternativas consideradas**:
- MongoDB Atlas (document-oriented parecía natural para PDFs)
- SQLite (más simple)

**Razones**:
- Necesitábamos relaciones complejas (Documents ↔ Analyses ↔ Findings)
- ACID properties críticas (análisis financiero = datos sensibles)
- Team más familiar con SQL
- pgvector extension útil para futura semantic search

**Trade-offs**:
- Ventajas: Integridad referencial, ACID, queries complejas fáciles
- Desventajas: Schema migrations más rígidas, no tan natural para JSON nested

**Resultado**: Correcta. Relaciones entre entities resultaron más complejas de lo anticipado. PostgreSQL manejó esto excelentemente.

### 3. PROBLEMAS RESUELTOS

#### Problema: Timeout en Documentos Largos (>150 páginas)

**Problema**: Documentos de 150+ páginas causaban timeouts (30s límite de OpenAI) y errores en producción. Cliente reportó 3 fallos en primera semana.

**Causa raíz**: Intentábamos analizar documento completo en una sola llamada a API. Token limit de GPT-4 (8K en ese momento) insuficiente para docs largos.

**Solución**:
1. Implementamos chunking inteligente: 
   - Split por secciones (usando PDF structure/headers)
   - Max 3K tokens por chunk (margen de seguridad)
2. Procesamiento paralelo con asyncio (max 3 requests concurrentes para no hit rate limits)
3. Agregación de resultados con LLM final pass (meta-análisis)
4. Implementamos retry logic con exponential backoff para llamadas individuales

**Código relevante**:
```python
async def analyze_large_document(doc_path, max_concurrent=3):
    chunks = intelligent_chunk(doc_path, max_tokens=3000)
    
    # Procesar chunks en paralelo con semaphore
    sem = asyncio.Semaphore(max_concurrent)
    async def process_chunk(chunk):
        async with sem:
            return await analyze_chunk(chunk)
    
    chunk_results = await asyncio.gather(*[
        process_chunk(c) for c in chunks
    ])
    
    # Meta-análisis para consolidar
    final_analysis = await consolidate_findings(chunk_results)
    return final_analysis
```

**Prevención futura**: 
- Añadimos test con doc sintético de 200 páginas en test suite
- Documentamos límites claros en PLAN.md de futuros proyectos (max tokens, rate limits)
- Creamos directiva `directives/process_large_documents.md` con este patrón

**Resultado**: 0 timeouts en 2 meses siguientes. Performance mejoró (parallelization).

### 4. OPTIMIZACIONES Y MEJORAS

#### Optimización: Prompt Engineering para Reducir Tokens 40%

**Situación inicial**: Prompts iniciales muy verbosos (~1200 tokens por análisis). Coste alto, latencia alta.

**Cambio realizado**: 
1. Condensamos instrucciones de 1200 tokens a 720 tokens
2. Usamos examples más concisos (few-shot learning)
3. Movimos formatting instructions a post-processing (no en prompt)

**Implementación**:
- Before: "Please analyze this document thoroughly, paying attention to all financial metrics, risk factors, market opportunities..." (verbose)
- After: "Extract: financial metrics, risks, opportunities. Format: JSON." (conciso + examples)

**Impacto**: 
- Tokens por request: 1200 → 720 (40% reducción)
- Coste mensual: $122 → $73 (reducción adicional 40% post-caching)
- Latencia: 1.4s → 0.9s promedio
- **Calidad mantenida** (validated con test set de 50 docs)

**Aplicabilidad**: Aplicar aggressive prompt optimization a todos proyectos con volumen alto de llamadas LLM. Trade-off: requiere más testing para validar calidad no degrada.

### 5. LECCIONES APRENDIDAS

- **Async/await vale la pena**: 60% mejora en performance para I/O-bound tasks. La curva de aprendizaje inicial se amortiza rápido en proyectos con múltiples API calls.

- **Prompt caching es low-hanging fruit**: En proyectos con contenido repetitivo, implementar caching PRIMERO antes de optimizar prompts. ROI inmediato.

- **Testing con documentos reales es crítico**: Tests sintéticos no capturaron edge cases. Pedir al cliente 20-30 docs reales para test suite vale el esfuerzo.

- **Validación de inputs previene 80% de bugs**: Pydantic models capturaron innumerables edge cases que hubieran llegado a producción. Invertir tiempo en schemas robustos.

- **Chunking inteligente > chunking fijo**: Dividir por estructura semántica (secciones, headers) produce mejores resultados que dividir cada N tokens.

### 6. HERRAMIENTAS Y CONFIGURACIONES ÚTILES

#### Redis en Railway

**Para qué sirve**: Caching de respuestas LLM, session storage, rate limiting.

**Cómo configurar**:
1. Railway dashboard → New → Redis
2. Copy connection string
3. `.env`: `REDIS_URL=redis://...`
4. Python: `pip install redis`
5. Connect: `redis.from_url(os.getenv('REDIS_URL'))`

**Gotchas**:
- Railway Redis free tier: 100MB. Suficiente para caching moderado.
- TTL default es None (data persiste forever) - SIEMPRE especificar TTL
- Keys no expiran automáticamente bajo memory pressure - implementar LRU manualmente si necesario

#### FastAPI + Pydantic Validation Pattern

**Para qué sirve**: Type-safe API development con validación automática.

**Cómo configurar**:
```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel, validator

class DocumentAnalysisRequest(BaseModel):
    document_url: str
    analysis_type: str
    
    @validator('analysis_type')
    def validate_analysis_type(cls, v):
        allowed = ['financial', 'risk', 'full']
        if v not in allowed:
            raise ValueError(f'Must be one of {allowed}')
        return v

app = FastAPI()

@app.post("/analyze")
async def analyze(request: DocumentAnalysisRequest):
    # Pydantic ya validó inputs aquí
    result = await process_document(request)
    return result
```

**Gotchas**:
- Pydantic v2 (2024+) tiene breaking changes vs v1
- Validators ejecutan en orden definición - orden importa
- async validators requieren Pydantic v2+

---

## Patrones y Best Practices Acumulados

### Gestión de Costes LLM

**Patrón**: Caching + Prompt optimization + Monitoring

**Cuándo aplicar**: SIEMPRE en proyectos production con LLMs

**Implementación típica**:
1. Setup Redis caching (ver herramientas arriba)
2. Optimizar prompts (medir tokens antes/después)
3. Implement cost monitoring:
```python
def track_llm_cost(tokens_used, model="gpt-4"):
    cost_per_1k = 0.03  # $0.03 per 1K tokens for GPT-4
    cost = (tokens_used / 1000) * cost_per_1k
    # Log to metrics system
    metrics.increment('llm_cost_usd', cost)
    return cost
```

**Proyectos donde se usó**: Due Diligence Automation

---

## Herramientas y Stack Técnico Preferido

### APIs de IA

**OpenAI**:
- Usado en: Due Diligence project
- Casos de uso: Document analysis, text generation
- Gotchas: Rate limits agresivos en tier básico (3 req/min), batch API tarda 24h, GPT-4 límite 8K tokens (considerar GPT-4-32K para docs largos)
- Best practice: Siempre implementar retries con exponential backoff

**Anthropic (Claude)**:
- Usado en: (Próximos proyectos)
- Ventajas conocidas: Context window más largo (100K+), mejor en análisis de docs largos
- A explorar: Prompt caching nativo, pricing vs OpenAI

### Infrastructure

**Railway**:
- Para qué: Hosting de APIs, PostgreSQL, Redis
- Pros: Setup rápido, free tier generoso, logs integrados
- Contras: Pricing puede escalar rápido con traffic, cold starts ocasionales
- Setup típico: Web service (FastAPI) + PostgreSQL + Redis, todo en mismo proyecto

**Redis**:
- Para qué: Caching LLM, rate limiting, session storage
- Setup típico: Railway deployment, connection via `redis-py`
- Best practice: SIEMPRE usar TTL, monitorear memory usage

### Frameworks y Librerías

**FastAPI**:
- Por qué preferimos: Async support, Pydantic validation, auto-docs, type safety
- Patrones comunes: Router-based structure, dependency injection, middleware para logging/auth
- Gotchas: Async/await learning curve, menos middleware ecosystem que Flask

**Pydantic**:
- Para qué: Input/output validation, config management
- Best practice: Define models para TODO lo que cruza API boundary
- Gotchas: v2 breaking changes, validators pueden ser confusos al inicio

**PyPDF2 / pdfplumber**:
- Para qué: PDF parsing y text extraction
- Experiencia: PyPDF2 fallaba con PDFs complejos, pdfplumber más robusto pero más lento
- Recomendación: pdfplumber para producción, PyPDF2 solo para PDFs simples

---

## Métricas y KPIs a Mejorar

**Trackeados desde proyecto Due Diligence**:

- **Tiempo medio de implementación**: 
  - Actual: ~3 semanas para MVP cliente-facing
  - Objetivo: Reducir a 2 semanas con mejor reutilización de patrones

- **Tasa de bugs en producción**: 
  - Actual: 2-3 bugs menores por proyecto en primeras 2 semanas
  - Objetivo: <1 bug por proyecto (mejor testing + validación)

- **Coste medio de operación LLM**: 
  - Actual: $73/mes post-optimización (Due Diligence)
  - Objetivo: Mantener <$100/mes en proyectos similares

- **Coverage de tests**: 
  - Actual: 67% (Due Diligence)
  - Objetivo: >80% para código crítico

- **Satisfacción cliente**: 
  - Actual: 9/10 (Due Diligence)
  - Objetivo: Mantener >8/10

---

**Nota para el agente**: Este documento DEBE actualizarse después de cada implementación significativa. Ver `directives/00_ARITZ_DOCUMENTATION.md` para instrucciones detalladas de cómo mantener este archivo. El proyecto de ejemplo arriba ilustra el nivel de detalle esperado para proyectos reales.
