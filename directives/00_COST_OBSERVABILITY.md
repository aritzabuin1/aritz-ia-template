# SOP: Monitorización de Costes y Observabilidad

**Objetivo**: Garantizar que el cliente nunca reciba una factura sorpresa y que la solución sea financieramente sostenible.

## 1. Acciones Obligatorias por el Agente
1. **Estimación Inicial**: En cada `PLAN.md`, el agente DEBE estimar el coste de tokens basado en el volumen de datos previsto.
2. **Log de Consumo**: Si se implementa un script en `execution/`, este debe incluir una función para contar tokens y loguear el coste aproximado en `monitoring/COSTS.md`.
3. **Alertas**: Si una tarea de procesamiento masivo supera los $10 de coste estimado, el agente DEBE detenerse y pedir confirmación explícita.

## 2. Estructura de monitoring/COSTS.md
- **Fecha** | **Tarea** | **Modelo** | **Tokens/Units** | **Coste Est.**
- 2026-02-05 | Test de Ingesta | Claude 3.5 Sonnet | 150k | $0.45
