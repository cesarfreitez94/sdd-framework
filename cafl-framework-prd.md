# PRD — cafl-framework
## Flujo de desarrollo guiado por fases, comandos y evidencias

**Versión:** 1.3
**Estado:** APPROVED WITH MINOR FIXES — Listo para implementación MVP
**Fecha:** 2026-06-07

---

## 1. Resumen ejecutivo

cafl-framework es un framework de desarrollo de software guiado por fases, comandos y evidencias. Cubre el ciclo completo desde Discovery hasta Closure, integrando OpenSpec como mecanismo principal de la fase de Specification, y comandos complementarios propios para las fases no cubiertas por OpenSpec.

El MVP es operable con markdown + comandos OpenCode + OpenSpec perfil core. No requiere automatización, agentes complejos ni ceremonias. Es una base clara, repetible y mejorable progresivamente.

---

## 2. Problema

Los flujos de desarrollo con agentes IA tienden a operar sin estructura, perdiendo contexto entre sesiones, duplicando trabajo y tomando decisiones ad hoc. Esto produce:

- Pérdida de contexto entre sesiones de agentes.
- Ausencia de evidencias claras por fase.
- Confusión entre lo que cubre OpenSpec y lo que debe resolverse fuera.
- Falta de trazabilidad desde el requerimiento hasta la verificación.
- Dificultad para aplicar TDD y CDD de forma ordenada.
- Incapacidad de retomar trabajo pausado sin perder estado.

---

## 3. Objetivo del producto

Construir un framework mínimo viable que:

- Cubra todas las fases del ciclo con comandos claros.
- Integre OpenSpec sin duplicar su funcionalidad.
- Produzca evidencias mínimas por fase en markdown.
- Permita retomar trabajo entre sesiones sin perder contexto.
- Sea mejorable progresivamente sin rehacerse.

---

## 4. Usuarios objetivo

| Rol | Tipo | Responsabilidad principal |
|---|---|---|
| Product Owner | Humano | Define qué se construye, valida prioridades, aprueba entregas |
| Business Analyst Agent | Agente IA | Elicitación, PRD, backlog, criterios de aceptación |
| Discovery Agent | Agente IA | Exploración de repo, arquitectura, deuda técnica, riesgos |
| Specification Agent | Agente IA | OpenSpec: explore, propose, apply, archive |
| Developer Agent | Agente IA | Implementación siguiendo tasks.md |
| QA Agent | Agente IA | Plan de pruebas, ejecución, evidencias |
| Tech Lead Agent | Agente IA | Review técnico, arquitectura, calidad |

> Scrum completo con ceremonias (daily, refinement, sprint planning, métricas) queda para v1.1. El MVP no requiere estructura Scrum.

---

## 5. Mapa maestro del flujo

Trazabilidad directa desde los 15 items originales del PRD base hacia los comandos MVP.

| Fase | Item | Cobertura OpenSpec | Comando MVP | Salida |
|---|---|---|---|---|
| Discovery | Explore repo | No cubierta | `/project:discover-repo` | `docs/repo-assessment.md` |
| Discovery | Elicitación BABOK | No cubierta | `/project:elicit` | `docs/elicitation-notes.md` |
| Definition | PRD | No cubierta | `/project:prd` | `docs/prd.md` |
| Definition | Backlog | No cubierta | `/project:backlog` | `docs/backlog.md` |
| Definition | Priorización | No cubierta | `/project:prioritize` | `docs/prioritization.md` |
| Specification | OpenSpec / SDD | Principal | `/opsx:explore` + `/opsx:propose` | proposal, specs, design, tasks |
| Specification | Criterios de aceptación | Parcial (estructura en specs) | Bloque de aceptación/contrato en specs | Dentro de `openspec/changes/<change>/specs/` |
| Specification | Plan de pruebas | No cubierta | `/project:test-plan` | `docs/test-plan.md` |
| Delivery | Apply | Cubierta | `/opsx:apply` | Código implementado |
| Delivery | Test | No cubierta | `/project:test` | `docs/test-results/<change>.md` |
| Delivery | Documentation | Fuera del MVP | — | v1.2 |
| Delivery | CI/CD | Fuera del MVP | — | v1.2 |
| Closure | Review | No cubierta | `/project:review` | `docs/review-report.md` |
| Closure | Release notes | Fuera del MVP | Cubierto temporalmente en `/project:review` o `/project:retro` | v1.1 |
| Closure | Retro / lessons learned | No cubierta | `/project:retro` | `docs/retro.md`, `docs/lessons-learned.md` |

---

## 6. Fases del flujo

### 6.1 Discovery

**Objetivo:** Entender el contexto real antes de definir qué se construye.
**Agente responsable:** Discovery Agent
**Cobertura OpenSpec:** No cubierta

| Item | Comando | Salida |
|---|---|---|
| Explore repo | `/project:discover-repo` | `docs/repo-assessment.md` |
| Elicitación BABOK | `/project:elicit` | `docs/elicitation-notes.md`, `docs/business-context.md`, `docs/risks-assumptions-constraints.md` |

**Nota:** `discover-repo` es obligatorio incluso en repos existentes. No se salta.

---

### 6.2 Definition

**Objetivo:** Transformar el entendimiento del problema en definición de producto y backlog accionable.
**Agente responsable:** Business Analyst Agent
**Cobertura OpenSpec:** No cubierta

| Item | Comando | Salida |
|---|---|---|
| PRD | `/project:prd` | `docs/prd.md` |
| Backlog | `/project:backlog` | `docs/backlog.md` |
| Priorización | `/project:prioritize` | `docs/prioritization.md` |

**Estructura mínima del backlog:**

```markdown
## Product Backlog

| ID | Historia de usuario | Criterios de aceptación | Prioridad | Estado |
|---|---|---|---|---|
| US-001 | Como [rol], quiero [acción] para [beneficio] | CA-001, CA-002 | Alta | Pendiente |
```

**Priorización:** MoSCoW (Must, Should, Could, Won't). Aplicada por BA Agent, validada por PO humano.

**Persistencia MVP:** El backlog actúa como fuente de verdad del estado entre sesiones. Cada item incluye columna `Estado` (Pendiente / En progreso / Bloqueado / Completado) y columna `Fase actual`. Simple, sin archivos adicionales.

---

### 6.3 Specification

**Objetivo:** Convertir la definición en especificación técnica y criterios verificables.
**Agente responsable:** Specification Agent
**Cobertura OpenSpec:** Principal. Los criterios de aceptación, contratos CDD y plan de pruebas dependen también de PRD, backlog y QA — OpenSpec los estructura pero no los resuelve solo.

#### Ruta rápida — cambios simples o claros

```
/opsx:explore   → investigar antes de comprometerse (opcional pero recomendado)
/opsx:propose   → genera proposal + specs + design + tasks en un paso
```

#### Ruta completa — proyectos grandes o cambios complejos (v1.1)

```
/opsx:explore
/opsx:new       → inicializa el cambio formalmente
/opsx:continue  → construye artefactos de forma incremental, permite pausar y retomar
/opsx:ff        → fast-forward cuando hay claridad, genera todos los artefactos en batch
```

> Los comandos `/opsx:new`, `/opsx:continue`, `/opsx:ff` son del perfil expandido de OpenSpec. Están documentados pero no integrados en el MVP. Se incorporan en v1.1.

**Artefactos generados por OpenSpec tras propose:**

| Artefacto | Archivo | Contenido |
|---|---|---|
| Proposal | `openspec/changes/<change>/proposal.md` | Por qué, qué cambia, scope |
| Specs | `openspec/changes/<change>/specs/` | Requerimientos y escenarios |
| Design | `openspec/changes/<change>/design.md` | Enfoque técnico |
| Tasks | `openspec/changes/<change>/tasks.md` | Checklist de implementación |

**Plan de pruebas:** Comando complementario ejecutado sobre la spec aprobada.

| Item | Comando | Salida |
|---|---|---|
| Plan de pruebas | `/project:test-plan` | `docs/test-plan.md` |

---

### 6.4 Delivery

**Objetivo:** Implementar, probar y documentar el cambio especificado.
**Agentes responsables:** Developer Agent, QA Agent
**Cobertura OpenSpec:** Parcial

| Item | Comando | Cobertura | Salida |
|---|---|---|---|
| Apply | `/opsx:apply` | OpenSpec core | Código implementado |
| Sync (opcional) | `/opsx:sync` | OpenSpec core | Delta specs sincronizadas sin archivar |
| Test | `/project:test` | cafl complementario | `docs/test-results/<change>.md` |

> `/opsx:verify` pertenece al perfil expandido de OpenSpec. No se incluye en el happy path MVP. Queda para v1.1.

> CI/CD queda fuera del MVP. Se documenta como checklist manual si aplica.

---

### 6.5 Closure

**Objetivo:** Revisar, cerrar, documentar la entrega y capturar aprendizajes.
**Agentes responsables:** Tech Lead Agent, BA Agent
**Cobertura OpenSpec:** Parcial

| Item | Comando | Cobertura | Salida |
|---|---|---|---|
| Archive | `/opsx:archive` | OpenSpec core | Specs archivadas, cambio cerrado |
| Review | `/project:review` | cafl complementario | `docs/review-report.md` |
| Release notes | — | Fuera del MVP | Cubierto temporalmente dentro de `/project:review` o `/project:retro` si aplica. Comando formal `/project:release-notes` disponible en v1.1. |
| Retro | `/project:retro` | cafl complementario | `docs/retro.md`, `docs/lessons-learned.md` |

---

## 7. Happy path MVP

```
/project:discover-repo
/project:elicit
/project:prd
/project:backlog
/project:prioritize
/opsx:explore
/opsx:propose
/project:test-plan
/opsx:apply
/project:test
/opsx:archive
/project:review
/project:retro
```

13 pasos. Los comandos OpenSpec usados en el happy path son caja negra ya existente. `/opsx:sync` está disponible como comando opcional durante Delivery pero no forma parte del happy path base. Los 8 comandos cafl son los que se construyen.

---

## 8. Persistencia de estado entre sesiones

### 8.1 El problema

Los agentes IA no tienen memoria entre sesiones. Al retomar, necesitan saber:

- En qué fase está el proyecto.
- Qué items del backlog están en progreso, bloqueados o completados.
- Qué artefactos OpenSpec existen y en qué estado.
- Qué decisiones ya se tomaron y qué queda pendiente.

OpenSpec resuelve parcialmente la persistencia de Specification mediante sus artefactos versionados en git (proposal, specs, design, tasks). En v1.1 se evaluará `/opsx:continue` para retomar cambios de forma incremental entre sesiones.

### 8.2 Estrategia MVP — Backlog como estado

El backlog (`docs/backlog.md`) actúa como fuente de verdad del estado entre sesiones. Sin archivos adicionales.

```markdown
| ID | Historia | Prioridad | Estado | Fase actual | Último comando | Próximo |
|---|---|---|---|---|---|---|
| US-001 | ... | Alta | En progreso | Specification | /opsx:explore | /opsx:propose |
| US-002 | ... | Media | Pendiente | — | — | /project:discover-repo |
```

Un agente que retoma lee el backlog y sabe exactamente dónde continuar.

### 8.3 Opciones para v1.1 (decisión pendiente de PO)

**Opción A — Markdown con sección de estado explícita**

Un bloque de estado al inicio de `docs/sprint-status.md`:

```markdown
## Estado actual
- Fase: Specification
- Item en progreso: US-001
- Último comando: /opsx:explore
- Próximo comando: /opsx:propose
- Bloqueado: No
```

Ventajas: simple, legible, sin dependencias.
Desventajas: el agente escribe estado manualmente, riesgo de inconsistencias.

---

**Opción B — YAML de estado por sesión**

Archivo `.cafl/session.yaml` con estado estructurado, similar al `.openspec.yaml` de OpenSpec:

```yaml
sprint: 1
phase: specification
active_change: US-001-add-dark-mode
last_command: /opsx:explore
next_command: /opsx:propose
status: in_progress
blocked: false
artifacts:
  proposal: pending
  specs: pending
  design: pending
  tasks: pending
timestamp: 2026-06-07T10:30:00Z
```

Ventajas: estructura consistente, parseable, compatible con el modelo de OpenSpec.
Desventajas: requiere comandos `/project:session-save` y `/project:session-restore`.

---

**Opción C — Estado en el backlog (extensión del MVP)**

Ampliar las columnas del backlog con más detalle de estado. Sin archivos adicionales.

Ventajas: el backlog es la única fuente de verdad.
Desventajas: backlog se vuelve denso en proyectos grandes.

---

**Decisión pendiente:** PO elige A, B o C antes de iniciar v1.1. El MVP opera con la estrategia del backlog simple (sección 8.2).

---

## 9. CDD — Contract Driven Development

CDD opera desde Definition hasta Closure como bloques en los artefactos existentes. No son archivos separados en el MVP.

| Fase | Artefacto | Bloque CDD |
|---|---|---|
| Definition | `docs/prd.md` | Sección `## Contratos esperados` |
| Specification | `openspec/changes/<change>/specs/` | Escenarios con inputs/outputs/errores |
| Delivery | `docs/test-plan.md` | Casos que validan el contrato |
| Closure | `docs/review-report.md` | Verificación de cumplimiento |

**Formato mínimo de contrato:**

```markdown
## Contrato: <nombre>
- Input: <qué recibe>
- Output: <qué devuelve>
- Reglas: <reglas de negocio aplicables>
- Errores esperados: <casos de error>
- Casos borde: <edge cases relevantes>
- Validado por: <prueba o criterio de aceptación>
```

---

## 10. TDD — Test Driven Development

TDD opera desde el plan de pruebas hasta la verificación final.

**Flujo TDD en el framework:**

```
/project:test-plan   → define casos con estado RED esperado
/opsx:apply          → implementación
/project:test        → ejecución real, estado GREEN esperado
/project:review      → evidencia final
```

**Formato mínimo en test-plan.md:**

```markdown
## Caso: <nombre>
- Dado: <contexto>
- Cuando: <acción>
- Entonces: <resultado esperado>
- Estado inicial esperado: FALLA (RED)
- Estado post-implementación esperado: PASA (GREEN)
```

---

## 11. Comandos del framework

### 11.1 Comandos OpenSpec — no construidos por cafl

**Perfil core (MVP):**

| Comando | Función |
|---|---|
| `/opsx:explore` | Investigar antes de comprometerse |
| `/opsx:propose` | Genera proposal + specs + design + tasks en un paso |
| `/opsx:apply` | Implementa siguiendo tasks.md |
| `/opsx:sync` | Sincroniza delta specs sin archivar (commits intermedios) |
| `/opsx:archive` | Cierra y archiva el cambio |

**Perfil expandido (v1.1 — pendiente de integración):**

| Comando | Función |
|---|---|
| `/opsx:new` | Init formal del cambio, alternativa a propose para control granular |
| `/opsx:continue` | Construcción incremental de artefactos, permite pausar y retomar |
| `/opsx:ff` | Fast-forward: genera todos los artefactos en batch cuando hay claridad |
| `/opsx:verify` | Validación antes de archivar |
| `/opsx:bulk-archive` | Archivado masivo de cambios completados |
| `/opsx:onboard` | Escaneo de código existente para generar specs iniciales (brownfield) |

### 11.2 Comandos cafl — construidos en el MVP

| Comando | Fase | Agente | Salida |
|---|---|---|---|
| `/project:discover-repo` | Discovery | Discovery Agent | `docs/repo-assessment.md` |
| `/project:elicit` | Discovery | BA Agent | `docs/elicitation-notes.md`, `docs/business-context.md`, `docs/risks-assumptions-constraints.md` |
| `/project:prd` | Definition | BA Agent | `docs/prd.md` |
| `/project:backlog` | Definition | BA Agent | `docs/backlog.md` |
| `/project:prioritize` | Definition | BA Agent + PO | `docs/prioritization.md` |
| `/project:test-plan` | Specification | QA Agent | `docs/test-plan.md` |
| `/project:test` | Delivery | QA Agent | `docs/test-results/<change>.md` |
| `/project:review` | Closure | Tech Lead Agent | `docs/review-report.md` |
| `/project:retro` | Closure | BA Agent | `docs/retro.md`, `docs/lessons-learned.md` |

9 comandos propios. Los 5 de OpenSpec core ya existen.

### 11.3 Comandos cafl — versiones futuras

| Comando | Versión | Función |
|---|---|---|
| `/project:sprint-plan` | v1.1 | Sprint planning Scrum |
| `/project:daily` | v1.1 | Daily standup de agentes |
| `/project:sprint-review` | v1.1 | Sprint review con PO |
| `/project:refine` | v1.1 | Backlog refinement entre sprints |
| `/project:dod` | v1.1 | Definition of Done |
| `/project:release-notes` | v1.1 | Release notes formales |
| `/project:session-save` | v1.1 | Guarda estado en session.yaml |
| `/project:session-restore` | v1.1 | Restaura estado desde session.yaml |
| `/project:ci` | v1.2 | Checklist o pipeline CI/CD |
| `/project:docs` | v1.2 | Documentación técnica automática |

---

## 12. Estructura de archivos

```
.opencode/
  commands/
    project-discover-repo.md
    project-elicit.md
    project-prd.md
    project-backlog.md
    project-prioritize.md
    project-test-plan.md
    project-test.md
    project-review.md
    project-retro.md

openspec/                        ← gestionado por OpenSpec, no tocar
  changes/
    active/
    archive/
  specs/

docs/
  repo-assessment.md
  elicitation-notes.md
  business-context.md
  risks-assumptions-constraints.md
  prd.md
  backlog.md
  prioritization.md
  test-plan.md
  test-results/
  review-report.md
  retro.md
  lessons-learned.md

README.md                        ← happy path + instrucciones de uso
```

---

## 13. Alcance del MVP

### Incluido

| Elemento | Notas |
|---|---|
| 9 comandos `/project:*` con prompt y template | Core del MVP |
| Integración con OpenSpec perfil core (5 comandos) | Caja negra, no se construye |
| Happy path de 13 pasos ejecutable sobre repo real | Validado en repo real |
| Backlog con estructura mínima y estado por item | Fuente de verdad de persistencia |
| CDD como bloque en spec y test-plan | Sin archivos separados |
| TDD como formato RED/GREEN en test-plan | Sin test runner específico en MVP |
| Estructura de carpetas completa | Lista para clonar |
| README ejecutable | Suficiente para que un agente arranque solo |

### No incluido — Post-MVP

| Elemento | Versión |
|---|---|
| Scrum completo (sprint planning, daily, review, retro, refinement) | v1.1 |
| Persistencia con session.yaml | v1.1 |
| Integración OpenSpec perfil expandido (new, continue, ff, verify) | v1.1 |
| `/opsx:onboard` para brownfield | v1.1 |
| Release notes formales | v1.1 |
| CI/CD | v1.2 |
| Documentación técnica automática | v1.2 |
| Validadores JSON Schema | v1.2 |
| Gates automáticos entre fases | v1.2 |
| Agentes con memoria persistente entre sesiones | v2.0 |
| Motor de workflow propio | v2.0 |
| Métricas de velocidad y calidad | v2.0 |

---

## 14. Requisitos funcionales

**RF-001** — El framework cubre las 5 fases: Discovery, Definition, Specification, Delivery, Closure.

**RF-002** — Cada fase tiene comandos con salidas markdown definidas.

**RF-003** — OpenSpec se usa como caja negra para Specification. Ningún comando cafl duplica comandos OpenSpec.

**RF-004** — El backlog tiene columnas de estado que permiten retomar trabajo entre sesiones sin archivo adicional.

**RF-005** — CDD aparece como bloque de contrato en specs y test-plan, trazable desde PRD hasta review.

**RF-006** — TDD aparece como formato RED/GREEN en test-plan con caso ejecutable real.

**RF-007** — El happy path es ejecutable de inicio a fin siguiendo solo el README.

**RF-008** — Cada comando cafl tiene un archivo de prompt en `.opencode/commands/` y un template de salida definido.

**RF-009** — Los comandos OpenSpec del perfil expandido están documentados como pendientes de integración para v1.1.

---

## 15. Requisitos no funcionales

**RNF-001 — Simplicidad:** Sin motores de workflow, sin validadores complejos, sin ceremonias en MVP.

**RNF-002 — Trazabilidad:** Cada fase produce evidencia en markdown versionable en git.

**RNF-003 — Adaptabilidad:** Ajusta profundidad sin cambiar estructura base.

**RNF-004 — No duplicación:** Ningún comando cafl duplica funcionalidad de OpenSpec.

**RNF-005 — Separación de responsabilidades:** Comandos OpenSpec, comandos cafl y comandos del stack técnico son explícitamente distintos.

**RNF-006 — Control de alucinación:** Solo se integran en MVP comandos OpenSpec del perfil core confirmados. Los del perfil expandido quedan documentados como pendientes.

**RNF-007 — Legibilidad por agentes:** Todos los artefactos son markdown plano, sin formatos propietarios.

---

## 16. Criterios de aceptación del MVP

**CA-001** — Los 9 comandos `/project:*` del MVP existen como archivos en `.opencode/commands/` con prompt y template.

**CA-002** — El happy path completo puede ejecutarse sobre un repo real produciendo evidencias en `docs/`.

**CA-003** — El backlog generado por `/project:backlog` tiene columnas de estado que permiten retomar entre sesiones.

**CA-004** — Ningún comando cafl invoca ni replica comandos OpenSpec existentes.

**CA-005** — CDD aparece como bloque de contrato en al menos un caso real de spec y test-plan.

**CA-006** — TDD aparece como formato RED/GREEN en test-plan en al menos un caso real ejecutado.

**CA-007** — Las tres opciones de persistencia para v1.1 están documentadas en el README con trade-offs.

**CA-008** — Los comandos OpenSpec del perfil expandido están marcados como "pendientes de integración v1.1" en la documentación.

**CA-009** — El README es suficiente para que un agente nuevo ejecute el happy path sin contexto adicional.

**CA-010** — La cobertura de OpenSpec en Specification está documentada como "principal" y no como "completa".

---

## 17. Riesgos

| Riesgo | Impacto | Mitigación |
|---|---|---|
| Duplicar comandos OpenSpec | Alto | Separación explícita en sección 11 |
| Alucinar funciones OpenSpec | Alto | Solo perfil core en MVP; expandido marcado como pendiente |
| Backlog como estado insuficiente para proyectos grandes | Medio | Aceptado para MVP; session.yaml entra en v1.1 |
| TDD sin test runner real | Medio | El runner depende del stack del proyecto; test-plan define los casos, el agente los ejecuta con las herramientas del stack |
| CDD ambiguo sin contrato formal | Medio | Formato mínimo definido en sección 9 |
| 5 días optimistas para 9 comandos + validación real | Medio | MVP acotado a comandos simples tipo markdown/prompt; validación sobre repo real como gate de cierre |
| Volverse burocrático antes de ser útil | Alto | Si el happy path no corre completo en 5 días, el MVP no pasa el CA-002 |

---

## 18. Plan de versiones

| Versión | Contenido | Estado |
|---|---|---|
| **v1.0 MVP** | 9 comandos cafl, happy path 13 pasos, OpenSpec core, CDD/TDD como bloques, backlog como estado | Este sprint |
| **v1.1** | Scrum completo, persistencia session.yaml, OpenSpec expandido, onboard brownfield, release notes | Post-MVP |
| **v1.2** | CI/CD, docs automáticas, validadores JSON Schema, gates entre fases | Siguiente ciclo |
| **v2.0** | Agentes con memoria persistente, motor de workflow, métricas, orquestación multi-agente | Largo plazo |

---

## 19. Decisión de producto

Se aprueba construir cafl-framework v1.0 como flujo mínimo viable, implementado en markdown + comandos OpenCode + OpenSpec perfil core, ejecutable sobre un repo real.

Scrum completo queda para v1.1. La estrategia de persistencia para v1.1 (Opción A, B o C de la sección 8.3) se decide antes de iniciar esa versión.

La prioridad del MVP es claridad, trazabilidad y repetibilidad. No automatización, no ceremonias, no complejidad innecesaria.
