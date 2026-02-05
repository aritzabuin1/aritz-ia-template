# [Nombre del Proyecto]

> [Descripción breve de 1-2 líneas de qué hace este proyecto]

**Estado**: 🚧 En desarrollo / ✅ Producción  
**Cliente**: [Nombre cliente o "Interno"]  
**Stack**: Python 3.11+ | FastAPI | PostgreSQL | Redis

---

## Descripción

[Descripción detallada del proyecto: qué problema resuelve, para quién, cómo funciona]

---

## Características Principales

- ✨ [Feature 1]
- ✨ [Feature 2]
- ✨ [Feature 3]
- 🔐 Seguridad enterprise-level (secrets management, input validation, PII protection)
- 📊 Monitoring y observabilidad
- ⚡ Optimizado para performance ([métricas si aplica])

---

## Arquitectura

```
[Diagrama ASCII o descripción de arquitectura]

User → API → Backend Service → Database
            ↓
        LLM API
            ↓
        Cache
```

**Stack técnico**:
- **Backend**: [FastAPI / Flask / Django]
- **Database**: [PostgreSQL / MongoDB / SQLite]
- **Cache**: [Redis / None]
- **APIs externas**: [OpenAI, Anthropic, etc]
- **Hosting**: [Modal / Railway / AWS / GCP]

**Decisiones arquitectónicas clave**: Ver `docs/architecture.md` o `PLAN.md` sección 4.

---

## Setup y Configuración

### Prerrequisitos

- Python 3.11+
- [PostgreSQL / MongoDB / None si es SQLite]
- [Redis si aplica]
- Cuentas API: [OpenAI / Anthropic / etc]

### Instalación

1. **Clonar repositorio**:
```bash
git clone [URL]
cd [project-name]
```

2. **Crear virtual environment**:
```bash
python -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate
```

3. **Instalar dependencias**:
```bash
pip install -r requirements.txt
```

4. **Configurar variables de entorno**:
```bash
cp .env.example .env
# Editar .env con tus credenciales
```

Variables requeridas (ver `.env.example`):
- `OPENAI_API_KEY` - API key de OpenAI
- `DATABASE_URL` - Connection string de database
- [Otras variables necesarias]

5. **Inicializar database** (si aplica):
```bash
python -m execution.init_db
# O el comando específico de tu proyecto
```

6. **Ejecutar tests** (para verificar setup):
```bash
pytest
```

---

## Uso

### Desarrollo Local

```bash
# Ejecutar servidor
uvicorn main:app --reload --port 8000

# O si tienes script específico:
python main.py
```

API disponible en: `http://localhost:8000`  
Docs interactivos: `http://localhost:8000/docs`

### Producción

[Instrucciones específicas de deployment - Modal / Railway / Docker / etc]

---

## Testing

```bash
# Todos los tests
pytest

# Con coverage
pytest --cov=. --cov-report=html

# Solo unit tests
pytest tests/unit/

# Solo integration tests
pytest tests/integration/
```

---

## Estructura del Proyecto

```
/
├── CLAUDE.md                      # Instrucciones para agente IA
├── PLAN.md                        # Plan detallado del proyecto
├── ARITZ.md                       # Knowledge base acumulativo
├── README.md                      # Este archivo
│
├── agents/                         # Specialized agent system prompts
|
├── directives/                    # SOPs y procedimientos
│   ├── 00_PLANNING_CHECKLIST.md
│   ├── 00_ARITZ_DOCUMENTATION.md
	...
│   └── [task-specific].md
│
├── execution/                     # Scripts Python
│   ├── __init__.py
│   ├── [main_scripts].py
│   └── utils/
|
├── tests/                         # Tests
│   ├── unit/
│   └── integration/
│
├── docs/                          # Documentación
│   ├── architecture.md
│   └── decisions/
│
├── monitoring/                    # Logs y métricas
│   ├── logs/
│   └── metrics/
│
├── config/                        # Configuraciones por entorno
│   ├── dev/
│   ├── staging/
│   └── prod/
│
├── .tmp/                          # Archivos temporales (no commit)
│
├── .env                           # Variables entorno (NO COMMIT)
├── .env.example                   # Template de variables
├── .gitignore
├── requirements.txt               # Dependencias Python
└── [main.py / app.py]            # Entry point
```

---

## Directivas y Automatización

Este proyecto usa arquitectura de 3 capas con Claude Code:

- **Directivas** (`directives/`): SOPs en Markdown describiendo qué hacer
- **Orquestación**: Claude Code toma decisiones basado en directivas
- **Ejecución** (`execution/`): Scripts Python deterministas

Ver `CLAUDE.md` para más detalles sobre el workflow de desarrollo.

---

## Seguridad

⚠️ **IMPORTANTE**: Este proyecto maneja [datos sensibles / APIs de pago / PII / etc].

**Prácticas de seguridad implementadas**:
- ✅ Secrets en variables de entorno (nunca en código)
- ✅ Input validation con Pydantic
- ✅ Logging sin secrets ni PII
- ✅ [Rate limiting / Prompt injection protection / etc]

**Antes de deploy**:
- [ ] Verificar `.env` no está commiteado
- [ ] Revisar logs no exponen secrets
- [ ] Ejecutar tests de seguridad
- [ ] Validar protección de endpoints

---

## Monitoring y Observabilidad

**Health check**: `GET /health`

**Métricas trackeadas**:
- [Latencia de requests]
- [Error rate]
- [Costes de API]
- [Otras métricas específicas]

**Logging**:
- Nivel: [INFO / DEBUG] en dev, [INFO / WARNING] en prod
- Formato: JSON estructurado
- Storage: [donde se guardan logs]

**Alertas**:
- [Qué condiciones triggerean alertas]
- [A quién se notifica]

---

## Contribuir

[Si es proyecto interno de equipo, instrucciones de cómo contribuir]

1. Crear branch desde `main`
2. Implementar cambios
3. Ejecutar tests: `pytest`
4. Crear PR con descripción clara
5. [Proceso de code review si aplica]

---

## Mantenimiento

**Actualizar dependencias**:
```bash
pip install --upgrade -r requirements.txt
pip freeze > requirements.txt
```

**Backups** (si aplica):
[Cómo y cuándo se hacen backups]

**Monitoring de costes**:
[Cómo trackear gastos de APIs]

---

## Licencia

[MIT / Proprietary / etc]

---

## Contacto

**Desarrollador**: Aritz  
**Cliente**: [Nombre cliente]  
**Soporte**: [Email / Slack / etc]

---

## Changelog

Ver `PLAN.md` sección "Log de Cambios" para historial detallado de cambios al proyecto.

**Última actualización**: [Fecha]
