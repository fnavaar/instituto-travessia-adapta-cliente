# Verificação automática — F1-T008

**Data:** 2026-09-24T12:05-03:00  
**Skip project:** 60853 · Travessia · UI **0.0.18**

## Resultados

| Check | Status | Evidência |
|---|---|---|
| setup / static / build / test / integrations | PASSOU | QA Skip apply 0.0.18 ok:true |
| Rota `/decisoes` | PASSOU | App.tsx + project routes |
| Fixture DEC-PILOTO-001 comparison + recommendation | PASSOU | `decisao-fixture.ts` |
| `recommendation.label` = sugestao/inferencia | PASSOU | código |
| `applyVerdict` exige alçada + campos | PASSOU | código |
| Sugestão não fecha sozinha (`suggestionCannotClose`) | PASSOU | código |
| Dado real ausente | PASSOU | @piloto.test |
| CA-1-009 / reabertura | N/A | fora de escopo |

## Veredito parcial

**Código e UI entregues; gate = teste humano em /decisoes.**
