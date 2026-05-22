# SIGDU Workers

Sistema de workers especializados para iterar sobre el TP final de Ingeniería de Software (UNSAM) — proyecto **SIGDU (Sistema de Inteligencia y Gestión Deportiva Universitaria)**, metodología PRINCE2.

Cada worker es un prompt XML que se carga al inicio de un chat independiente. Los workers comparten contexto a través de los archivos de `guidelines/` (fundamentos, rúbrica, fuentes Notion, aprendizajes) y se pasan resultados a través de archivos en `tasks/`.

## Estructura del repo

```
sigdu_workers/
├── README.md                  Este archivo
├── rubrica_evaluacion.md      Rúbrica cruda de la cátedra
├── template_white_paper.md    Template oficial del WP
├── guidelines/
│   ├── fundamentos.xml        Identidad SIGDU, contexto PRINCE2, reglas comunes
│   ├── rubrica.xml            Rúbrica estructurada (consumida por revisor)
│   ├── notion_sources.xml     Mapa de URLs canónicas del Notion
│   └── aprendizajes.xml       Lecciones acumuladas entre sesiones
├── roles/
│   ├── workflow.xml           Orden de invocación y flujos abreviados
│   ├── project_manager.xml    PM PRINCE2 — puerta de entrada
│   ├── business_analyst.xml   Evidencia, beneficios, KPIs UNSAM
│   ├── arquitecto.xml         Diseño técnico, estilos, MVP
│   ├── disenador_ux.xml       Personas, USM, mockups
│   ├── analista_riesgos.xml   Riesgos, costos, VAN/TIR
│   ├── redactor.xml           Integra todo en el WP final
│   ├── revisor.xml            Evalúa contra la rúbrica
│   ├── sincronizador_notion.xml  Push controlado a Notion (on-demand)
│   └── presentador.xml        Defensa oral (on-demand)
└── tasks/                     Outputs de cada sesión de worker
```

## Cómo usar

1. **Abrir un chat nuevo** para el rol que necesites.
2. **Pegar el contenido completo del XML del rol** como prompt inicial.
3. **Adjuntar el handoff** del rol anterior (XML producido en `tasks/`) o el pedido original del usuario.
4. **El worker hace preguntas clarificadoras**, propone enfoques, produce un archivo en `tasks/` y te indica el próximo rol a invocar.
5. **Cerrás el chat** cuando el worker registra los aprendizajes nuevos en `guidelines/aprendizajes.xml`.

## Tabla de roles

| Rol | Cuándo invocarlo | Output |
|-----|------------------|--------|
| `project_manager` | Primero, cuando llega un pedido nuevo del usuario | `tasks/YYYY-MM-DD-pm-*.xml` |
| `business_analyst` | Tras el PM; para evidencia, beneficios, validación UNSAM | `tasks/YYYY-MM-DD-ba-*.xml` |
| `arquitecto` | Tras el BA (paralelo con UX); diseño técnico | `tasks/YYYY-MM-DD-arq-*.xml` |
| `disenador_ux` | Tras el BA (paralelo con Arq); personas, USM, mockups | `tasks/YYYY-MM-DD-ux-*.xml` |
| `analista_riesgos` | Tras Arq + UX; riesgos, costos, evaluación financiera | `tasks/YYYY-MM-DD-rf-*.xml` |
| `redactor` | Tras los 5 anteriores; integra en el WP final | `tasks/YYYY-MM-DD-wp-vN.md` |
| `revisor` | Sobre cualquier artefacto; scorecard 0-3 por eje | `tasks/YYYY-MM-DD-rev-*.xml` |
| `sincronizador_notion` | On-demand, cuando subís un output a Notion | `tasks/YYYY-MM-DD-sync-*.xml` |
| `presentador` | On-demand, cerca de la defensa oral | `tasks/YYYY-MM-DD-pres-*.md` |

Detalle completo del flujo en [`roles/workflow.xml`](roles/workflow.xml).

## Flujos abreviados (atajos para tareas focalizadas)

- **fix de redacción**: `redactor → revisor`
- **ajustar riesgos**: `analista_riesgos → redactor → revisor`
- **subir score UNSAM**: `business_analyst → redactor → revisor`
- **alternativa arquitectónica**: `arquitecto → analista_riesgos → redactor → revisor`
- **solo revisión**: `revisor`
- **publicar a Notion**: `sincronizador_notion`

## Convención de archivos en `tasks/`

Formato: `tasks/YYYY-MM-DD-{rol}-{titulo-kebab}.{xml|md}`

Ejemplos:
- `tasks/2026-05-22-pm-razones-business-case.xml`
- `tasks/2026-05-22-arq-mvp-fase-1.xml`
- `tasks/2026-05-22-wp-v3.md`
- `tasks/2026-05-22-rev-wp-v3.xml`

## Aprendizajes entre sesiones

Cada worker DEBE leer `guidelines/aprendizajes.xml` al inicio y registrar lecciones nuevas al cierre (correcciones del usuario, gotchas, patrones útiles). Esto es lo que evita que pierdas contexto entre sesiones.

Categorías válidas: `bug`, `patron`, `gotcha`, `optimizacion`, `seguridad`, `integracion`, `flujo_proyecto`.

## Reglas de oro

1. **Un chat = un rol.** Nunca mezclar roles en la misma sesión.
2. **Lectura obligatoria al inicio.** `fundamentos.xml`, `rubrica.xml`, `notion_sources.xml`, `aprendizajes.xml`.
3. **Auto-evaluación antes de entregar.** Cada worker simula el scorecard del revisor sobre su propio output.
4. **Output local, Notion controlado.** Workers escriben en `tasks/`; a Notion solo el sincronizador con confirmación del usuario.
5. **Iterar hasta excelencia.** El revisor no aprueba con score &lt; 2 en algún eje. La meta es 3/3 en los 4 ejes.
6. **Citá fuente o marcá [TBD].** Nunca inventes datos.

## Fuente de verdad

- **Notion (canónico)**: https://www.notion.so/322f263368668100b6a9fbd346cb8f09
- **Drive del proyecto**: https://drive.google.com/drive/u/2/folders/1jWNgBcwMktK7NDk-WQp7QELQnMX3TsPH

Mapa completo de URLs en [`guidelines/notion_sources.xml`](guidelines/notion_sources.xml).
