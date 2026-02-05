# Cómo Mantener ARITZ.md - Guía para el Agente

**ESTE ARCHIVO ES PARA TI, EL AGENTE. LEE ESTO CUANDO NECESITES ACTUALIZAR ARITZ.md**

---

## Propósito de ARITZ.md

ARITZ.md es el **knowledge base acumulativo** de Aritz. Documentas TODO lo aprendido en cada proyecto para que Aritz se convierta en un arquitecto de soluciones IA cada vez más profesional y valioso.

**Piensa en ello como**: El diario técnico de un ingeniero senior que documenta patrones, decisiones, y lecciones aprendidas.

---

## Cuándo Actualizar ARITZ.md

### Frecuencia y Triggers

**Actualiza INMEDIATAMENTE después de**:

1. ✅ **Completar una feature significativa** (>2 horas trabajo)
   - No esperes al final del proyecto
   - Documenta mientras está fresco en memoria

2. ✅ **Resolver un problema complejo** (debugging >30 min)
   - Causa raíz + solución + prevención

3. ✅ **Tomar decisión arquitectónica importante**
   - Antes de implementar, documenta el razonamiento

4. ✅ **Descubrir gotcha de herramienta/API**
   - Límites, comportamientos inesperados, workarounds

5. ✅ **Implementar patrón enterprise-level nuevo**
   - Primera vez que Aritz usa esta técnica

6. ✅ **Al final de cada sesión de trabajo significativa**
   - Resumen de qué se logró, decisiones tomadas

**NO actualices por**:
- Cambios triviales (fix typo, ajuste CSS)
- Experimentos fallidos sin aprendizaje
- Refactors que no cambian arquitectura

---

## Qué NO Documentar en ARITZ.md

❌ **Evita documentar**:

1. **Información obvia**
   - "Usamos Python" → esto no aporta
   - "Instalamos dependencias con pip" → obvio

2. **Experimentos fallidos sin lección**
   - Probaste 3 librerías y ninguna sirvió → no documentes
   - Pero si aprendiste "X librería no soporta Y" → SÍ documenta

3. **Detalles de implementación rutinarios**
   - No documentes cada función que escribiste
   - Solo patrones reutilizables o decisiones no-obvias

4. **Código completo**
   - ARITZ.md no es repositorio de código
   - Snippets clave SÍ, archivos completos NO

5. **Problemas temporales externos**
   - "API de OpenAI estuvo caída 2 horas" → irrelevante
   - "API de OpenAI falla con inputs >10K tokens" → relevante

6. **Opiniones personales sin fundamento técnico**
   - "No me gusta MongoDB" → no aporta
   - "Elegimos PostgreSQL sobre MongoDB porque necesitábamos JOINs complejos" → SÍ documenta

---

## Estructura Requerida de ARITZ.md

ARITZ.md se organiza por **proyecto**, dentro de cada proyecto por **categoría**.

### Template de Proyecto:

```markdown
## Proyecto: [Nombre Cliente] - [Mes Año]

### Resumen del Proyecto
[1-2 párrafos: qué construiste, para quién, contexto de negocio]

### 1. QUÉ HICIMOS - Implementaciones Clave

#### [Nombre implementación]
**Qué**: [Descripción técnica concreta]
**Por qué**: [Problema, objetivo negocio]
**Cómo**: [Pasos técnicos, tecnologías, decisiones]
**Resultado**: [Métricas, impacto cuantificado]

### 2. DECISIONES ARQUITECTÓNICAS

#### [Nombre decisión]
**Decisión**: [Qué decidimos]
**Alternativas consideradas**: [Otras opciones evaluadas]
**Razones**: [Por qué elegimos esta]
**Trade-offs**: [Ventajas/desventajas]
**Resultado**: [Cómo funcionó en práctica]

### 3. PROBLEMAS RESUELTOS

#### [Descripción problema]
**Problema**: [Qué estaba roto]
**Causa raíz**: [Por qué ocurría]
**Solución**: [Cómo arreglamos paso a paso]
**Prevención futura**: [Cómo evitar que vuelva a pasar]

### 4. OPTIMIZACIONES Y MEJORAS

#### [Nombre optimización]
**Situación inicial**: [Estado before, métricas]
**Cambio realizado**: [Qué modificamos]
**Impacto**: [Mejora cuantificada]
**Aplicabilidad**: [Otros proyectos donde usar esto]

### 5. LECCIONES APRENDIDAS

- **[Lección]**: [Explicación detallada de qué aprendiste y por qué importa]

### 6. HERRAMIENTAS Y CONFIGURACIONES ÚTILES

#### [Nombre herramienta/config]
**Para qué sirve**: [Caso de uso]
**Cómo configurar**: [Pasos específicos o código]
**Gotchas/Trampas**: [Problemas comunes a evitar]

---
```

---

## Reglas de Escritura CRÍTICAS

### 1. SIEMPRE en castellano de España
- Todo contenido en castellano
- Términos técnicos en inglés cuando sea estándar (ej: "prompt caching" NO "cacheo de prompts")
- Explicaciones en castellano claro

### 2. Sé ESPECÍFICO y TÉCNICO

❌ **MAL**: "Mejoramos el rendimiento"  
✅ **BIEN**: "Redujimos tiempo de respuesta de 8s a 1.2s implementando prompt caching con Redis (TTL: 1h) y moviendo validación Pydantic fuera del hot path"

❌ **MAL**: "Añadimos seguridad"  
✅ **BIEN**: "Implementamos rate limiting con sliding window (100 req/min por IP) usando Redis, añadimos validación Pydantic para prevenir prompt injection, configuramos CORS restrictivo (solo dominios whitelisted)"

### 3. Incluye MÉTRICAS cuando sea posible
- Costes: "$450/mes → $45/mes en OpenAI"
- Performance: "Latencia p95: 12s → 3.2s"
- Código: "Coverage: 45% → 87%"
- Negocio: "Procesa 10K webhooks/día sin fallos"

### 4. Documenta el PORQUÉ tanto como el CÓMO

No asumas contexto. Explica:
- Por qué era importante resolver esto
- Qué alternativas consideraste y por qué descartaste
- Qué trade-offs aceptaste
- Qué aprendiste que antes no sabías

### 5. Hazlo ACCIONABLE

Cada entrada debe responder: **"Si me encuentro con esto otra vez, ¿qué haría?"**

❌ **MAL**: "Usamos FastAPI"  
✅ **BIEN**: "Elegimos FastAPI sobre Flask porque necesitábamos async/await nativo para múltiples llamadas concurrentes a APIs externas. Setup: `pip install fastapi[all] uvicorn`, estructura con routers por dominio, Pydantic models para validación. Gotcha: uvicorn con --reload solo en dev, en prod usar gunicorn con uvicorn workers"

---

## Ejemplo Completo: Entrada BUENA

```markdown
#### Prompt Caching para Reducción Costes 90%

**Qué**: Implementamos caching de respuestas LLM usando hash de (prompt + document_chunk) como key en Redis.

**Por qué**: Muchos documentos tienen secciones similares (legal boilerplate, métricas estándar). Gastábamos $450/mes en llamadas repetidas a GPT-4. Cliente tiene presupuesto ajustado, necesitábamos optimizar.

**Cómo**:
1. Setup Redis en Railway ($5/mes)
2. Función `cached_llm_call(prompt, content, model, ttl=3600)`:
   - Cache key = `sha256(prompt + content + model)`
   - Check Redis: `redis.get(cache_key)`
   - Si hit: return cached response (log hit)
   - Si miss: call OpenAI, store en Redis con TTL
3. TTL de 1 hora (docs raramente reanalizados mismo día)
4. Logging de cache hit rate para monitoreo

**Código relevante**:
```python
import redis, hashlib, json

redis_client = redis.from_url(os.getenv('REDIS_URL'))

def cached_llm_call(prompt, content, model="gpt-4", ttl=3600):
    cache_key = hashlib.sha256(f"{prompt}{content}{model}".encode()).hexdigest()
    
    cached = redis_client.get(cache_key)
    if cached:
        logger.info(f"Cache HIT: {cache_key[:8]}")
        return json.loads(cached)
    
    logger.info(f"Cache MISS: {cache_key[:8]}")
    response = openai_call(prompt, content, model)
    redis_client.setex(cache_key, ttl, json.dumps(response))
    return response
```

**Resultado**: 
- Cache hit rate: 73% tras primera semana
- Coste mensual: $450 → $120 (73% reducción)
- Latencia mejorada: 8s → 1.2s en cache hits
- Cliente muy contento con optimización

**Aplicabilidad futura**: Aplicar a cualquier proyecto con análisis repetitivo de contenido. Considerar aumentar TTL si contenido es más estático. Para contenido que cambia (ej: noticias), reducir TTL a 15-30 min.
```

---

## Proceso Paso a Paso para Actualizar

Cada vez que termines implementación significativa:

1. **Abre ARITZ.md**
2. **Identifica sección de proyecto** (o crea nueva si proyecto nuevo)
3. **Determina categoría** (QUÉ HICIMOS / DECISIONES / PROBLEMAS / OPTIMIZACIONES / LECCIONES / HERRAMIENTAS)
4. **Escribe entrada** siguiendo estructura requerida
5. **Verifica incluyes**: QUÉ + POR QUÉ + CÓMO, con métricas, técnicamente específico
6. **Actualiza fecha** al inicio archivo
7. **Guarda archivo**

---

## Checklist de Calidad

Antes de guardar entrada en ARITZ.md, verifica:

- [ ] Está en castellano (excepto términos técnicos estándar)
- [ ] Incluye QUÉ, POR QUÉ, CÓMO
- [ ] Es técnicamente específico (no vago)
- [ ] Incluye métricas o cuantificación impacto
- [ ] Explica decisiones y trade-offs
- [ ] Es accionable (puede reusarse en futuro)
- [ ] Fecha actualizada al inicio archivo
- [ ] No documenta cosas obvias o triviales
- [ ] Aporta valor real para futuros proyectos

---

**Recuerda**: ARITZ.md es la herramienta más valiosa de Aritz para crecer profesionalmente. Trátalo con cuidado que merece.

**Frecuencia ideal**: Actualizar al menos 2-3 veces por sesión de trabajo significativa. Mejor muchas actualizaciones pequeñas que una gigante al final.
