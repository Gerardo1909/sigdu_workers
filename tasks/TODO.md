Veredicto: APROBAR — 11/12

  Verifiqué el trabajo iterado del BA (v2) y del UX (v2) contra el scorecard previo. Todos los
  pendientes alta y media severidad están cerrados:

  - C4 (alta, BA): §1.2 re-mapeado a taxonomía USM A0–A6, con tabla §1.2-bis de equivalencia. Conteo
   verificado: 4+4+3+3+3+5+3 = 25 ✓
  - C4-operativo (alta, BA): SEC-2 cubre E5.0 explícito; SEC-3 cubre E2.1 explícito; sin gaps en USM
   A2/A5.
  - C9 (media, BA): NOCIÓN→NOTION corregido (etiqueta interna, no llega a Notion).
  - C8/C2 (UX): handoff aclara que el cambio en fila A2 es nota al pie (no diff literal) y que
  "55→71" proviene del XML del UX. Doble cobertura con Arq Entregable 4, sin contradicción.

  Pendientes remanentes (NO bloquean Notion): confirmar [TBD] del Área Deportiva con el PM y
  narrativa memorable para defensa oral (sube originalidad de 2 a 3).

  Revisión completa en tasks/2026-05-25-rev-output-scorecard-final-v2.xml.

  ---
  Archivos a pasarle al sincronizador_notion (7, en orden)

  Las v2 son patches sobre las v1, no reemplazos. El sincronizador necesita ambas:

  1. tasks/2026-05-25-pm-auditoria-y-alineacion-general.xml — contexto de decisiones.
  2. tasks/2026-05-25-ux-output-usm-corregido.xml — USM base (conteo 71, ACs, reasignación E5.0).
  3. tasks/2026-05-25-ux-output-usm-corregido-v2.xml — override: handoff con pasos 1-7 (usar como
  guía operativa del sync).
  4. tasks/2026-05-25-ux-usm-planning-poker.xml — estimaciones por historia.
  5. tasks/2026-05-25-arq-output-adr002-expandido.xml — ADR-002 v2.0, stack, frontend, aclaraciones
  ARQ-SYNC-1/2 (usar texto de <nota_al_pie_propuesta> para fila A2).
  6. tasks/2026-05-25-ba-output-bc-corregido-y-plan-pruebas.xml — BA base (E1-E5 del BC + Plan de
  Pruebas §1.1, §1.3, §2-4, §5.1-3, §5.5, §7).
  7. tasks/2026-05-25-ba-output-bc-corregido-y-plan-pruebas-v2.xml — override: reemplaza §1.2,
  agrega §1.2-bis, reemplaza §5.4 del Plan de Pruebas.

  Opcionalmente, el scorecard v2 (tasks/2026-05-25-rev-output-scorecard-final-v2.xml) como audit
  trail — no se publica.

  El Entregable 6 del archivo de revisión incluye el orden sugerido de aplicación página por página
  en Notion (9 pasos).
