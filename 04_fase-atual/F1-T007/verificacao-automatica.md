# Verificação automática — F1-T007

**Data (implementação):** 2026-09-24T11:25-03:00  
**Revalidação pós-teste humano:** 2026-09-24T11:30-03:00  
**Skip project:** 60853 · Travessia · UI **0.0.13**

## Resultados (revalidação independente)

| Check | Status | Evidência |
|---|---|---|
| setup / static / build / test | PASSOU | QA Skip 0.0.13 |
| Rota `/regras` | PASSOU | `skip_project_get` → Regras |
| Fixture 001 aprovada + 002 pendente + 003 conflitante | PASSOU | `src/lib/regras-fixture.ts` |
| CA-1-006 campos em 001 | PASSOU | source_ref, source_version, vigencia, escopo, version, owner, state, approver, exceptions |
| `canApproveRule` bloqueia 002 (sem fonte/vigência) | PASSOU | código puro |
| `canApproveRule` bloqueia 003 (conflitante / 1F) | PASSOU | código puro |
| `nextVersion` r1→r2 preserva anterior | PASSOU | código + UI |
| Dado real ausente | PASSOU | @piloto.test |
| Teste humano | PASSOU | champion OK 2026-09-24 |
| Migrations PB Cloud | FALHOU (dívida F1-T002) | fora do critério F1-T007 |

## Veredito

**PASSOU.** CA-1-006/007 e RN-1.002-01/02 demonstrados via entrada controlada (fixture F1-T006). Desvio PB Cloud declarado e fora do gate.
