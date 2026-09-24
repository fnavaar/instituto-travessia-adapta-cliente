# Verificação automática — F1-T008

**Data (implementação):** 2026-09-24T12:05-03:00  
**Revalidação pós-teste humano:** 2026-09-24T12:08-03:00  
**Skip project:** 60853 · Travessia · UI **0.0.18**

## Resultados (revalidação independente)

| Check | Status | Evidência |
|---|---|---|
| setup / static / build / test / integrations | PASSOU | QA Skip 0.0.18 ok:true |
| Rota `/decisoes` | PASSOU | `skip_project_get` → Decisoes |
| Fixture DEC-PILOTO-001 comparison + recommendation | PASSOU | `decisao-fixture.ts` |
| `recommendation.label` = sugestao/inferencia | PASSOU | código |
| `applyVerdict` exige alçada + campos | PASSOU | código |
| Sugestão não fecha sozinha | PASSOU | `suggestionCannotClose` |
| Dado real ausente | PASSOU | @piloto.test |
| Teste humano | PASSOU | champion OK 2026-09-24 |
| CA-1-009 / reabertura | N/A | fora de escopo |

## Veredito

**PASSOU.** CA-1-008 e RN-1.002-04 demonstrados via `/decisoes` + funções puras.
