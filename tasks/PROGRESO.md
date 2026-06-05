# PROGRESO — Tablero de trabajo SIGDU

Tablero vivo. Una fila por work item. Cada worker actualiza su fila al abrir (`en_progreso`) y al
cerrar (`en_revision` / `aprobado` / `bloqueado`), con link al output en `tasks/`.

**Estados:** `pendiente` · `en_progreso` · `bloqueado` · `en_revision` · `aprobado`
**Protocolo:** ver `roles/workflow.xml` → `<protocolo_progreso>`.

| id | título | track | estado | rol_actual | proximo_rol | output | fecha |
|----|--------|-------|--------|-----------|-------------|--------|-------|
| W001 | Construir MVP — backend (FastAPI, schema-per-tenant) | dev | pendiente | — | arquitecto → backend_dev | — | — |
| W002 | Construir MVP — frontend (Next.js 14) | dev | pendiente | — | backend_dev → frontend_dev | — | — |

## Notas
- El track doc del TP ya tuvo una primera ronda completa (Business Case, USM, Arquitectura, Plan de
  Pruebas) cuyos outputs viven en Notion; los XML de esas sesiones se limpiaron de `tasks/`.
- El track dev arranca por el arquitecto: definir contrato de API + modelo de datos multi-tenant
  para el repo de código adyacente `sigdu` (../sigdu).
