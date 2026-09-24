# Verificação automática — F1-T007

**Data:** 2026-09-24T11:25-03:00  
**Skip project:** 60853 · Travessia · UI **0.0.13**

## Resultados

| Check | Status | Evidência |
|---|---|---|
| setup / static / build / test | PASSOU | QA Skip apply 0.0.13 |
| Rota `/regras` | PASSOU | App.tsx + project routes após build |
| Fixture 001/002/003 | PASSOU | `src/lib/regras-fixture.ts` |
| `canApproveRule` bloqueia 002 (sem fonte/vigência) | PASSOU | código |
| `canApproveRule` bloqueia 003 (conflitante) | PASSOU | código |
| `nextVersion` r1→r2 | PASSOU | código |
| Dado real ausente | PASSOU | @piloto.test / refs sintéticos |
| Migrations PB Cloud | FALHOU (dívida F1-T002) | fora do critério F1-T007 |

## Veredito parcial

**Código e UI entregues; gate = teste humano em /regras.**
