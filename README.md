# SIGDU Workers

Harness de workers especializados para el TP final de Ingeniería de Software (UNSAM) — proyecto
**SIGDU (Sistema de Inteligencia y Gestión Deportiva Universitaria)**, metodología PRINCE2.

Cada worker es un prompt XML que se carga al inicio de un chat independiente. Hay **dos tracks**:

- **Track DOC** — produce y refina los entregables del TP (Business Case, Canvas, USM,
  Arquitectura, White Paper). Se ejecutan por copy-paste; se evalúan contra la rúbrica de la cátedra.
- **Track DEV** — construye el **MVP funcional** (repo de código separado: FastAPI + Next.js 14).
  Corren con acceso al repo de código (leen/escriben, corren ruff/mypy/pytest/typecheck); se
  evalúan contra un Definition of Done técnico, no contra la rúbrica.

Los workers comparten contexto vía `guidelines/` (núcleo + memoria sharded) y se pasan resultados
por archivos en `tasks/`.

## Estructura del repo

```
sigdu_workers/
├── README.md
├── rubrica_evaluacion.md      Rúbrica cruda de la cátedra
├── template_white_paper.md    Template oficial del WP
├── guidelines/
│   ├── core.xml               NÚCLEO invariante — se pega SIEMPRE (mínimo contexto)
│   ├── fundamentos.xml        Detalle PRINCE2 + criterios UNSAM + secciones BC (on-demand, roles doc)
│   ├── rubrica.xml            Rúbrica estructurada (on-demand, revisor + auto-eval)
│   ├── notion_sources.xml     Mapa de URLs canónicas del Notion (on-demand)
│   ├── aprendizajes.xml       MIGRADO -> guidelines/memory/ (stub de redirección)
│   └── memory/                Memoria sharded por tipo de tarea
│       ├── INDEX.xml          Índice liviano (1 línea/lección) — se lee SIEMPRE
│       ├── cross.xml          Lecciones transversales (stack canónico, consistencia) — se lee SIEMPRE
│       ├── doc-process.xml    Lecciones de roles documentales
│       ├── backend.xml        Lecciones de backend_dev
│       ├── frontend.xml       Lecciones de frontend_dev
│       └── qa.xml             Lecciones de qa_testing / code_reviewer
├── roles/
│   ├── workflow.xml           Tracks doc/dev, routing de memoria, progreso, destilación
│   │   ── Track DOC ──
│   ├── project_manager.xml · business_analyst.xml · arquitecto.xml · disenador_ux.xml
│   ├── analista_riesgos.xml · redactor.xml · revisor.xml
│   │   ── Track DEV ──
│   ├── backend_dev.xml · frontend_dev.xml · qa_testing.xml · code_reviewer.xml
│   │   ── On-demand ──
│   ├── sincronizador_notion.xml · presentador.xml · curador_memoria.xml
└── tasks/                     Outputs de cada sesión + PROGRESO.md (tablero vivo)
```

## Cómo usar (track doc, copy-paste)

1. **Abrí un chat nuevo** para el rol que necesites.
2. **Pegá el XML del rol** + lo que indique su bloque `<contexto_minimo>` (no todo `guidelines/`:
   solo `core.xml`, `memory/INDEX.xml`, `memory/cross.xml`, el shard del rol y lo on-demand que
   liste). Esto minimiza la ventana de contexto.
3. **Adjuntá el handoff** del rol anterior (XML en `tasks/`) o el pedido original.
4. El worker hace preguntas, produce un archivo en `tasks/`, actualiza `tasks/PROGRESO.md`,
   registra lecciones nuevas en su shard de `memory/` e indica el próximo rol.

## Cómo usar (track dev)

Los roles dev (`backend_dev`, `frontend_dev`, `qa_testing`, `code_reviewer`) **no se ejecutan por
copy-paste puro** (no se construye una app pegando texto). Se usan con acceso al **repo de código
separado** `sigdu`, ubicado en un directorio **adyacente** a este harness (ruta típica `../sigdu`)
y operado por terminal Ubuntu. Pegás el rol + su `<contexto_minimo>`, y el worker lee, escribe y
corre las herramientas (ruff/mypy/pytest/typecheck) en ese repo. El handoff queda en `tasks/` y la
memoria/progreso siguen viviendo en este harness.

## Tabla de roles

| Rol | Track | Cuándo invocarlo | Output |
|-----|-------|------------------|--------|
| `project_manager` | doc | Primero, ante un pedido nuevo | `tasks/*-pm-*.xml` |
| `business_analyst` | doc | Tras el PM; evidencia, beneficios, validación UNSAM | `tasks/*-ba-*.xml` |
| `arquitecto` | doc+dev (bisagra) | Tras el BA; diseño técnico, stack, MVP scope | `tasks/*-arq-*.xml` |
| `disenador_ux` | doc | Tras el BA (paralelo); personas, USM, mockups | `tasks/*-ux-*.xml` |
| `analista_riesgos` | doc | Tras Arq + UX; riesgos, costos, finanzas | `tasks/*-rf-*.xml` |
| `redactor` | doc | Tras los 5 anteriores; integra el WP | `tasks/*-wp-vN.md` |
| `revisor` | doc | Sobre cualquier artefacto; scorecard 0-3 por eje | `tasks/*-rev-*.xml` |
| `backend_dev` | dev | Construir backend del MVP (FastAPI) | repo + `tasks/*-be-*.md` |
| `frontend_dev` | dev | Construir frontend del MVP (Next.js 14) | repo + `tasks/*-fe-*.md` |
| `qa_testing` | dev | Tests + cobertura vs USM | repo + `tasks/*-qa-*.md` |
| `code_reviewer` | dev | Review técnico del diff (no rúbrica) | `tasks/*-cr-*.md` |
| `sincronizador_notion` | on-demand | Subir un output a Notion (con confirmación) | `tasks/*-sync-*.xml` |
| `presentador` | on-demand | Cerca de la defensa oral | `tasks/*-pres-*.md` |
| `curador_memoria` | on-demand | Destilar lecciones recurrentes a los prompts | `tasks/*-cur-*.md` |

Detalle completo de flujos en [`roles/workflow.xml`](roles/workflow.xml).

## Sistema de memoria (sharded + destilación)

La memoria ya no es un único `aprendizajes.xml`: vive en `guidelines/memory/` partida por tipo de
tarea, con un `INDEX.xml` liviano. **Cada worker lee solo lo suyo**: `INDEX` + `cross` + el shard
de su rol (ver `<memoria_relevante>` de cada rol y la tabla `routing_de_memoria` en `workflow.xml`).

Formato de lección: frontmatter con `id`, `worker`, `categoria`, `tags`, `recurrencia`, `estado`
(`activo` | `distilado`) + cuerpo `contexto`/`descubrimiento`/`regla`.

**Loop de destilación (mejora progresiva de los prompts):** cuando una lección llega a
`recurrencia >= 3` (o se marca estable), el rol `curador_memoria` pliega su regla dentro del prompt
del rol correspondiente (o de `core.xml` si es universal) y la marca `distilado` (se conserva para
auditoría, deja de leerse en runtime). Así los prompts mejoran solos y la memoria activa se
mantiene chica.

Categorías válidas: `bug`, `patron`, `gotcha`, `optimizacion`, `seguridad`, `integracion`,
`flujo_proyecto`, `flujo_git`.

## Progreso

`tasks/PROGRESO.md` es el tablero vivo (una fila por work item, estados
`pendiente/en_progreso/bloqueado/en_revision/aprobado`). Cada worker actualiza su fila al abrir y
al cerrar. Protocolo en `workflow.xml` → `<protocolo_progreso>`.

## Stack técnico del MVP (canónico)

Declarado una sola vez (este README + Notion "Implementación MVP — Guía técnica"); el resto lo
referencia (regla de oro #3).

- **Frontend**: Next.js 14 · TypeScript · Vercel
- **Backend**: Python 3.12+ · FastAPI · SQLAlchemy 2.0 (async) · Alembic · Pydantic v2
- **Base de datos**: PostgreSQL 16 · schema-per-tenant (NO BaaS — ver lección L003)
- **Testing**: pytest · pytest-asyncio · httpx · factory-boy
- **Infraestructura**: Docker · Docker Compose · GitHub Actions
- **Calidad**: ruff (lint + formatter) · mypy · pre-commit hooks

## Reglas de oro

1. **Un chat = un rol.** Nunca mezclar roles en la misma sesión.
2. **Lectura mínima al inicio.** `core.xml` + `memory/INDEX.xml` + `memory/cross.xml` + el shard de
   tu rol. El resto (`fundamentos`, `rubrica`, `notion_sources`) solo si tu `<contexto_minimo>` lo pide.
3. **Auto-evaluación (doc) / Definition of Done (dev)** antes de entregar.
4. **Output local, Notion controlado.** Workers escriben en `tasks/` y `memory/`; a Notion solo el
   sincronizador con confirmación del usuario.
5. **Iterar hasta excelencia.** Doc: revisor 3/3 en los 4 ejes. Dev: code_reviewer sin ejes en
   rojo y tests verdes.
6. **Citá fuente o marcá [TBD].** Nunca inventes datos. El stack tiene un solo lugar canónico.

## Fuente de verdad

- **Notion (canónico)**: https://www.notion.so/322f263368668100b6a9fbd346cb8f09
- **Drive del proyecto**: https://drive.google.com/drive/u/2/folders/1jWNgBcwMktK7NDk-WQp7QELQnMX3TsPH

Mapa completo de URLs en [`guidelines/notion_sources.xml`](guidelines/notion_sources.xml).
