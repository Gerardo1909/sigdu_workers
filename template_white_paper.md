# White Paper — SIGDU: De la linterna al faro

*Sistema de Gestión Deportiva Universitaria — Business Case condensado (PRINCE2)*
*Equipo SIGDU · TP Final Integrador, Ingeniería de Software, UNSAM*

---

## 1. Resumen ejecutivo

**Problema.** El deporte universitario, ejemplificado en la Universidad Nacional de San Martín (UNSAM), se gestiona con planillas de cálculo, mensajes de WhatsApp y correos informales. El resultado es un Área Deportiva administrativamente sobrecargada, sin indicadores de participación y políticamente invisible frente a las autoridades que asignan presupuesto e infraestructura.

**Solución.** SIGDU (Sistema de Gestión Deportiva Universitaria) es una plataforma SaaS multi-tenant que reemplaza el circuito manual por un flujo digital de inscripción, asistencia y reportería. La metáfora que ordena el diseño es el pasaje *de la linterna al faro*: hoy cada coordinador ilumina su disciplina con una linterna propia y aislada; SIGDU instala una estructura fija que ilumina todo el puerto deportivo institucional, alimentada por los datos que estudiantes y coordinadores generan en cada interacción. La validación se ejecuta como piloto académico en UNSAM y escala como producto B2B para universidades privadas y clubes multiactividad de LATAM.

**Beneficios esperados.** Para la institución cliente, ahorro estimado de ~600 horas anuales de carga administrativa, trazabilidad completa de la participación deportiva y tableros de decisión para presupuesto e infraestructura. Para el modelo de negocio, un esquema de suscripción de USD 49 a 249 mensuales por institución, con un ROI proyectado superior a 10x frente a un mercado internacional cuyo pricing por usuario resulta prohibitivo en LATAM. UNSAM oficia simultáneamente de caso fundacional y de prueba de transferibilidad del modelo.

## 2. Contexto

### Situación actual

El deporte institucional en universidades argentinas opera sobre un stack informal compuesto por Excel, WhatsApp y correos electrónicos sueltos. Las inscripciones se reciben por mensaje, las asistencias se anotan en cuadernos o planillas locales, y la información muere en el dispositivo del coordinador. No existe una capa digital que consolide la actividad deportiva como un proceso institucional auditable.

UNSAM constituye el caso central de validación de este diagnóstico. Su Área Deportiva coordina múltiples disciplinas distribuidas en distintas sedes, con coordinadores que carecen de herramientas digitales unificadas y con una Secretaría de Deportes que no dispone de reportes consolidados de participación. La escala, la dispersión territorial y el modelo de gestión propios de una universidad nacional pública argentina configuran un escenario que las soluciones genéricas del mercado no contemplan: si se retira UNSAM del análisis, se pierde la especificidad operativa que justifica la propuesta.

### Datos y evidencia del problema

El relevamiento inicial identifica cuatro síntomas recurrentes: gestión administrativa íntegramente manual, asignación presupuestaria por intuición ante la ausencia de indicadores, invisibilidad del deporte en los informes de gestión institucional, y baja participación estudiantil atribuible al desconocimiento de la oferta disponible. El circuito completo —desde la inscripción por WhatsApp hasta la planilla local del coordinador que nunca llega a Secretaría— se ilustra en la **Figura 1**.

`[Figura 1 — Flujo manual actual: inscripción por WhatsApp → planilla local del coordinador → pérdida de datos antes de llegar a Secretaría]`

### Impacto de no resolverlo

Mantener el statu quo tiene tres costos concretos. Primero, el Área Deportiva permanece invisible institucionalmente: sin datos no hay argumento presupuestario. Segundo, los recursos —horas docentes, espacios físicos, materiales— se asignan sobre actividades cuya demanda real no se mide, perpetuando la sobreoferta en disciplinas marginales y la subinversión en las masivas. Tercero, se desaprovecha una palanca documentada de reputación y captación estudiantil: la oferta deportiva es un factor de elección universitaria que UNSAM no puede comunicar con evidencia.

### Cómo el proyecto mejora la situación

SIGDU convierte cada inscripción y cada toma de asistencia en un dato estructurado que alimenta tableros de gestión. El coordinador deja de operar como nodo único de información; la Secretaría de Deportes accede a métricas de participación por disciplina, sede y período; las autoridades incorporan el deporte a su agenda de decisión con evidencia. La propuesta no agrega una herramienta más al ecosistema fragmentado: lo reemplaza por una infraestructura común. Validado el piloto en UNSAM, el mismo núcleo se ofrece bajo modelo SaaS a universidades privadas y clubes multiactividad de la región, atacando un mercado que las plataformas estadounidenses dejaron desatendido.

## 3. Propuesta de Solución — Descripción del proyecto

### Propósito del proyecto

SIGDU instala el faro institucional que el deporte universitario nunca tuvo: una estructura digital fija que convierte la energía dispersa de inscripciones, clases y asistencias en un mapa permanente de lo que ocurre en el área deportiva. Hoy cada coordinador opera con su propia linterna —ilumina solo donde apunta y se apaga cuando él se va—; SIGDU reemplaza ese patrón por una infraestructura común. La UNSAM, donde el seguimiento de actividades vive en Excel y grupos de WhatsApp, opera como caso central de validación: el sistema se diseña contra sus restricciones reales (multi-sede, presupuesto público acotado, ausencia de integración con SIU-Guaraní en R1-R2) y no como un genérico exportable.

### Funcionalidades principales

El MVP cubre 25 historias de usuario agrupadas en siete actividades del journey institucional:

- **A0 — Onboarding institucional**: arquitectura multi-tenant con *schema-per-tenant* que aísla datos por institución desde el día uno.
- **A1 — Acceso**: registro con email institucional, validación de dominio y autenticación JWT.
- **A2 — Descubrir oferta**: catálogo público sin login, filtrable por disciplina y sede.
- **A3 — Participar**: inscripción con control de cupo concurrente y confirmación por email.
- **A4 — Operar clases**: toma de asistencia por QR, *mobile-first* para profesores en cancha.
- **A5 — Gestionar institución**: RBAC con cinco roles (Alumno, Profesor, Admin, Directivo, Super-admin SIGDU) y alta de disciplinas y sedes.
- **A6 — Analizar**: dashboard ejecutivo de KPIs y exportación a Excel para reportes de gestión.

La propuesta de valor única que articula estas siete actividades se enuncia así: *SIGDU transforma la gestión deportiva institucional de planillas manuales a inteligencia accionable, a una fracción del costo de las alternativas del mercado.* El journey completo desde la inscripción hasta el reporte ejecutivo se diagrama en la **Figura 2**.

`[Figura 2 — Mapa funcional USM A0-A6: journey de inscripción, asistencia y reporte]`

### Público objetivo

Los **usuarios finales** se distribuyen en los cinco roles RBAC ya mencionados, con foco operativo en estudiantes, profesores y coordinadores. Las **áreas afectadas** dentro de la institución son la Secretaría o Área Deportiva, las autoridades académicas que reciben reportes y el alumnado como destinatario del servicio.

El **cliente piloto** es la UNSAM, bajo convenio sin monetización directa: acceso a la plataforma, revisiones trimestrales con el área deportiva y autorización para citar resultados en materiales comerciales. Sobre esa base de validación se priorizan tres **segmentos comerciales**:

1. Universidades privadas de LATAM, con disposición a pagar entre USD 149 y 249 mensuales.
2. Clubes multiactividad, entre USD 99 y 199.
3. Instituciones educativas premium (colegios bilingües, terciarios), entre USD 49 y 99.

Las universidades públicas no integran el mercado comercial: sus ciclos presupuestarios burocráticos los hacen inviables como pagadores, aunque sí son aliados estratégicos como casos de referencia. Los **socios clave** son la propia UNSAM como caso de éxito documentado, integradores del ecosistema Google Workspace y calendarios institucionales, y clubes deportivos universitarios como vía natural de expansión.

### Canales

La distribución combina presencia digital y venta consultiva. El **portal web (SPA)** y la **landing pública con catálogo de actividades** funcionan como vidriera institucional; la **PWA mobile** sostiene la operación cotidiana de profesores. La adquisición B2B se apoya en contacto directo a directores deportivos por email y LinkedIn, referencias surgidas del caso UNSAM, y presencia segmentada en redes (LinkedIn para decisores, Instagram para comunidad).

## 4. Impacto esperado y beneficios

### Comparación con soluciones existentes

El mercado global está dominado por TeamSnap, SportsEngine y LeagueApps, productos US-céntricos diseñados para ligas amateur y con pricing por usuario (USD 15-30 por usuario/mes). Trasladado a una universidad de 200 estudiantes activos, ese modelo implica ~USD 3.000 mensuales, cifra inviable para el presupuesto deportivo de una institución LATAM. El análisis Build vs Buy resulta concluyente: no existe producto específico para gestión deportiva universitaria en la región, y forzar una solución pensada para coaches de béisbol juvenil resuelve mal el problema institucional.

SIGDU se diferencia en cinco vectores: pricing institucional fijo en lugar de por usuario, soporte y producto en español, enfoque en la institución y no en la liga, reportes orientados a directivos —no solo a coaches operativos— y una hoja de ruta que contempla integración con SIU-Guaraní en R3, barrera que los incumbentes globales no pueden replicar con rapidez.

### Beneficios cualitativos y cuantitativos

Las estimaciones cuantitativas que siguen son cálculos propios del equipo basados en supuestos del Business Case (ver Anexo A); los benchmarks SaaS (LTV:CAC, churn) toman como referencia *SaaS Metrics 2.0* (Skok). Los valores se validan empíricamente durante el piloto UNSAM.

- **Carga administrativa**: ~3 horas semanales por coordinador × 5 coordinadores × 40 semanas equivalen a ~600 horas/año liberadas por institución.
- **Optimización presupuestaria**: ~15 % del presupuesto deportivo se vuelve reasignable al detectar disciplinas sin demanda real.
- **ROI cliente**: superior a 10× el costo de la suscripción anual.
- **Break-even del negocio**: dos clientes pagos cubren los costos operativos del orden de USD 50-65 mensuales hacia los meses 9-12.

En el plano cualitativo, SIGDU otorga visibilidad institucional mediante reportes de gestión con métricas deportivas que hoy no existen, instala una cultura de datos que sirve de precedente para otras áreas, refuerza la reputación de la institución como organización moderna y consolida la identidad comunitaria a través de la actividad física registrada y compartida.

### Indicadores de éxito esperados

El piloto en la UNSAM se evalúa contra criterios definidos en el H2 del cronograma:

- Adopción ≥ 60 % del alumnado activo en disciplinas piloto.
- Al menos una disciplina operando 100 % digital de punta a punta.
- ≥ 80 % de las clases con asistencia digital registrada.
- NPS ≥ 7/10 entre profesores y coordinadores.
- Cero incidentes de severidad P1 durante el período.

A nivel de negocio, los KPIs objetivo son LTV:CAC > 3:1 —estimación interna 11,9:1 con churn mensual del 8,3 % y CAC bajo USD 150—, *payback* cercano a un mes y MRR ≥ USD 200 al cierre del Mes 12. `[ANEXO A — modelo financiero detallado: cohortes, churn, CAC por canal y proyección a 24 meses]`

## 5. Arquitectura y diseño inicial

### 5.1 Diseño de alto nivel

SIGDU adopta una arquitectura en capas con dominios modulares (modular monolith) organizada en cinco niveles. La capa de **presentación** integra una SPA web para personal administrativo, una landing pública para captación comercial y una PWA orientada a profesores en pista. La capa de **API y seguridad** expone un API Gateway que resuelve el tenant por subdominio o header dedicado, delega la autenticación a un Auth Service basado en JWT stateless con validación de dominio de email, y aplica RBAC sobre cinco roles jerárquicos: Alumno, Profesor, Administrativo Institucional, Directivo y Super-admin SIGDU. La capa de **dominios de negocio** se descompone en siete bounded contexts DDD alineados uno a uno con las actividades del modelo USM (A0 a A6) más un contexto transversal de Plataforma: Tenant Management, Catálogo, Inscripciones, Operación de Clases, Gestión Institucional, Analytics y Plataforma. La capa de **servicios transversales** agrupa Notificaciones por email, generación y validación de QR, exportación a Excel y PDF, y File Storage. La capa de **datos** combina PostgreSQL 16 con estrategia schema-per-tenant, Object Storage para archivos y un motor NoSQL incorporado en R3 para logs y telemetría.

El diagrama completo de las cinco capas, los siete bounded contexts y la estrategia schema-per-tenant se presenta en la **Figura 3**.

`[Figura 3 — Arquitectura SIGDU: 5 capas, schema-per-tenant y bounded contexts A0-A6]`

La decisión arquitectónica central (ADR-002) es el aislamiento por schema. La alternativa row-level con `tenant_id` reduce costos de infraestructura pero introduce un riesgo de fuga de datos inaceptable bajo la Ley 25.326: un único `WHERE` omitido compromete a toda la base instalada. La alternativa database-per-tenant ofrece aislamiento físico pero multiplica costos operativos por encima del pricing target (USD 49 a 249 mensuales). El schema-per-tenant otorga aislamiento lógico fuerte mediante `search_path` por transacción, mantiene un único backup unificado vía `pg_dump` y escala hasta ~1000 schemas en PostgreSQL 16 con degradación inferior al 2 %. La decisión complementaria (ADR-001) descarta microservicios: con un equipo de cuatro integrantes y una fase de validación abierta, el overhead operativo es desproporcionado. Los bounded contexts quedan definidos con la rigurosidad necesaria para habilitar extracciones futuras —Analytics es el candidato natural— sin refactor disruptivo.

### 5.2 Tecnologías sugeridas

El frontend se construye con Next.js 14 (App Router) y TypeScript sobre Vercel Hobby; el SSR mejora el time-to-first-content de los dashboards. El backend utiliza Python 3.12+ con FastAPI por su soporte async nativo, Pydantic v2 y OpenAPI auto-generado, junto a SQLAlchemy 2.0 async, Alembic para migraciones y `uv` como gestor de dependencias. La base de datos es PostgreSQL 16 autogestionado en Railway o Render; se descartó Supabase porque el pooler Supavisor no garantiza control transaccional del `search_path`, requisito ineludible del aislamiento por schema. El stack de calidad combina `ruff`, `mypy`, `pytest`, `pytest-asyncio` y `factory-boy`, con CI en GitHub Actions.

### 5.3 Atributos de calidad

La **seguridad y aislamiento** combina schema-per-tenant, RBAC, RLS defensivo, audit log inmutable y cumplimiento de Ley 25.326; la validación de dominio de email opera como primer filtro de pertenencia institucional. La **disponibilidad** se apoya en deploys stateless habilitados por JWT, lo que permite escala horizontal sin afinidad de sesión, y en un backup unificado por instancia. El **rendimiento** fija objetivos medibles: dashboard A6 ≤ 3 s, exportación Excel ≤ 30 s y latencia p95 < 500 ms bajo 50 usuarios concurrentes en R3. La **mantenibilidad** aplica Clean Architecture por módulo —el dominio no depende del framework— con cobertura ≥ 80 % en A0+A1, A3 y A4, y ≥ 60 % en A6. La **escalabilidad** se materializa por release: extracción del contexto Analytics, incorporación de cache Redis en R2 y motor NoSQL para logs en R3.

### 5.4 Estrategia de implementación

La estrategia es incremental y anclada en el piloto UNSAM. El Release R1 (110 story points, 25 HU) constituye un MVP desplegable al cierre de la Fase 1. La Fase 2 lo somete a operación real en UNSAM y la Fase 3 incorpora un subset prioritario de R2 (~26 pts) con feedback medido. La evolución infraestructural es deliberada: R1 corre como monolito modular en un único servidor; R2 introduce escala horizontal, PgBouncer y CDN; R3 extrae Analytics, agrega cache layer y NoSQL. Cada salto se justifica por carga observada, no por anticipación especulativa.

## 6. Plan de desarrollo y cronograma tentativo

### 6.1 Fases, ciclo de vida y entregables

El proyecto se gobierna mediante PRINCE2 con justificación continua de negocio y gestión por excepción, complementado por ciclos iterativos de 1 a 2 semanas internos a cada fase. Se definen cuatro hitos formales que actúan como puertas de control:

- **H0 — Diseño aprobado (Sem. 4)**: arquitectura multi-tenant validada, ≥ 20/25 HU de R1 con criterios de aceptación cerrados y entorno local ejecutable.
- **H1 — MVP desplegado (Sem. 12)**: 25 HU verdes en staging, cobertura ≥ 70 % en módulos críticos y cero bugs P1 abiertos.
- **H2 — Piloto UNSAM validado (Sem. 16)**: adopción ≥ 60 %, NPS ≥ 7, al menos una disciplina operada íntegramente en digital y cero incidentes P1.
- **H3 — Producto estabilizado (Sem. 20)**: suite E2E verde y deuda técnica P1/P2 saldada.

### 6.2 Cronograma

El cronograma se expresa en semanas relativas al kick-off, sin fechas absolutas, para preservar flexibilidad ante el calendario académico UNSAM.

| Fase | Semanas | Foco | Salida |
|------|---------|------|--------|
| 0 — Diseño | 1–4 | Arquitectura multi-tenant, refinamiento USM, CI/CD base, wireframes | H0 |
| 1 — MVP | 5–12 | Construcción R1: A0 (29 pts) → A1 (18) → A2 (9) → A3 (15) → A4 (10) → A5 (13) → A6 (16) | H1 |
| 2 — Piloto UNSAM | 13–16 | Onboarding del tenant, taller con Área Deportiva, operación supervisada, medición de KPIs | H2 |
| 3 — Iteración | 17–20 | Subset prioritario R2 (~26 pts), suite E2E, cierre de deuda | H3 |

La Fase 1 trabaja con una velocity objetivo de 13,75 pts/semana (~14 pts operativos). La épica E0.3 de multi-tenancy (13 pts) concentra el mayor riesgo técnico y se ataca como primera prioridad absoluta en Sem. 5. La Fase 2 es el corazón del proyecto: UNSAM no es un cliente cualquiera sino el caso ancla que valida tracción, completitud funcional y aislamiento real entre tenants antes de cualquier movimiento comercial.

### 6.3 Costos

El desarrollo del MVP no genera costo monetario directo por tratarse de trabajo académico. La operación mensual escalada se ubica entre **USD 20 y 65** (Vercel + Railway PostgreSQL + dominio + email transaccional + comisión Stripe por transacción). El CAC objetivo es inferior a **USD 150** apoyado en un modelo founder-led con referidos desde la red UNSAM. El break-even sustentable se alcanza con 3 a 5 clientes pagos en plan Pro (USD 149 mensuales).

### 6.4 Recursos

El equipo se compone de cuatro integrantes con dedicación aproximada de 12 horas semanales cada uno, distribuidos en los roles de Backend Developer, Frontend Developer, QA + DevOps y Business Analyst con funciones de Project Manager. El stack tecnológico es íntegramente open source y opera sobre tiers gratuitos o económicos durante el MVP, lo que mantiene la huella de costos compatible con el modelo de negocio.

### 6.5 Riesgos

Cuatro riesgos ocupan zona roja en la matriz P×I y tienen mitigación documentada. **R01 — Multi-tenancy bloqueante (Sev. 20)**: se aborda con un spike arquitectónico en Sem. 5 y contingencia de +1 semana si E0.3 no cierra en Sem. 6. **R03 — No conversión a SaaS (Sev. 15)**: KPIs acordados con UNSAM previo al piloto y pricing flexible desde USD 49. **R02 — Baja adopción UNSAM (Sev. 12)**: el referente del Área Deportiva se incorpora como co-diseñador desde Fase 0, complementado por un taller presencial pre-piloto. **R09 — Deuda técnica (Sev. 12)**: code reviews obligatorios, cobertura mínima como condición de avance entre sprints y reserva explícita de Sem. 19–20 para saneamiento.

`[ANEXO B — Gantt detallado por sprint y registro de riesgos completo con matriz P×I]`

## 7. Conclusiones y Recomendaciones

SIGDU responde a una necesidad documentada y subestimada: el deporte universitario carece de la capa digital que ya tienen sus pares administrativos (alumnos, finanzas, recursos humanos). La propuesta no se limita a digitalizar planillas; instala el faro que la comunidad UNSAM —coordinadores, estudiantes, Área Deportiva y autoridades— necesita para que la energía de cada inscripción y cada asistencia ilumine las decisiones de toda la institución. El piloto en UNSAM ancla la solución en un caso real y, al mismo tiempo, prueba la transferibilidad del modelo a un mercado regional desatendido por los actores internacionales.

**Próximos pasos sugeridos:**

1. Cerrar el Hito 0 con el diseño aprobado por el comité académico y el sponsor institucional.
2. Ejecutar la Fase 1 de construcción del MVP hasta el Hito 1 (deploy productivo).
3. Desplegar el piloto en UNSAM (Hito 2) con dos a tres disciplinas seleccionadas y métricas de adopción definidas.
4. Evaluar resultados del piloto y, alcanzado el Hito 3, activar la estrategia comercial B2B hacia universidades privadas y clubes multiactividad de LATAM.

---

## 8. Anexos

*Los anexos no computan en la extensión del cuerpo del White Paper (5-7 páginas).*

- **Anexo A — Modelo financiero detallado** *(placeholder)*: escenarios conservador / moderado / optimista, cohortes, churn 8,3 % mensual, CAC por canal, proyección de MRR/ARR a 24 meses, análisis de sensibilidad.
- **Anexo B — Gantt detallado por sprint y registro de riesgos** *(placeholder)*: cronograma semanal con asignación por rol, matriz P×I completa con planes de mitigación y contingencia para los 12 riesgos del registro.
- **Anexo C — User Story Map completo** *(placeholder)*: 71 historias de usuario distribuidas en R1 (110 pts) / R2 (127 pts) / R3 (117 pts), con criterios de aceptación.
- **Anexo D — Plan de Pruebas de Software** *(placeholder)*: estrategia unit / integration / E2E / seguridad, umbrales de cobertura por módulo, pruebas de aislamiento cross-tenant, criterios de aprobación por release.
- **Anexo E — Modelo Canvas SIGDU SaaS** *(placeholder)*: lienzo completo con socios clave, propuesta de valor, segmentos, canales, recursos y estructura de costos.
- **Anexo F — Figuras referenciadas en el cuerpo** *(placeholder)*: Figura 1 (flujo manual actual), Figura 2 (mapa funcional USM A0-A6), Figura 3 (diagrama de arquitectura).
- **Referencias bibliográficas** *(placeholder)*: PRINCE2 Manual; Ley 25.326 de Protección de Datos Personales; SaaS Metrics 2.0 (David Skok); The Lean Startup (Eric Ries); benchmarks Citus / Depesz sobre escalabilidad de schemas en PostgreSQL.

---

## Consideraciones de formato (cátedra)

- **Fuente:** Arial 12, interlineado 1.15.
- **Extensión objetivo:** 5-7 páginas de contenido (secciones 1 a 7). Anexos, carátula, índice y bibliografía no computan.
