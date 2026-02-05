# Estándares de Calidad y Testing

**Objetivo**: Cero errores críticos en producción y alta mantenibilidad.

## 1. Definición de "Hecho" (Definition of Done)
Un componente solo está terminado si:
1. **Unit Tests**: Cobertura >80% en lógica de negocio.
2. **Integration Tests**: Los flujos de punta a punta (ej. API -> LLM -> DB) funcionan.
3. **LLM Evals**: Si hay prompts complejos, se han testeado con al menos 5 casos de entrada diferentes para evitar alucinaciones.

## 2. Herramientas
- Usar `pytest` para lógica Python.
- Usar scripts en `execution/` para validación de prompts (Evals).
- Resultados de tests críticos deben quedar documentados en el log del proyecto.
