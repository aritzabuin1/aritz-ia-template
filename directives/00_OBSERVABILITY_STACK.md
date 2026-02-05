# SOP: Advanced Observability & Tracing
**Objective**: Reduce Mean Time to Recovery (MTTR) via deep visibility.

## 1. Structured Logging
- Use JSON format for all logs.
- Required fields: `timestamp`, `level`, `message`, `request_id`, `trace_id`.

## 2. Distributed Tracing
- Propagate `correlation_id` across all internal services and LLM calls.
- Log the "Thought Trace" of the agent in `.tmp/` during development for debugging.

## 3. Health Monitoring
- Endpoint `/health` must check: Database connection, Redis connectivity, and API keys validity.