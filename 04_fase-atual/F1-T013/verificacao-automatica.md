# Verificação automática — F1-T013

**Data:** 2026-09-24T12:20-03:00  
**Skip project:** 60853 · Travessia · UI **0.0.19**

## Resultados

| Check | Status | Evidência |
|---|---|---|
| setup / static / build / test / integrations | PASSOU | QA Skip apply 0.0.19 ok:true |
| Rota `/rls` | PASSOU | App.tsx + project routes |
| Fixture F1-T011 6 auth + 4 denied | PASSOU | `rls-authorize.ts` |
| `canAccess` negado = só FORBIDDEN | PASSOU | código + `hasLeak` |
| `probeAccess` grava audit | PASSOU | código |
| `runBattery` cobre papel × objeto | PASSOU | código |
| Dado real ausente | PASSOU | @piloto.test |
| Permissão global inalterada | PASSOU | só motor/UI |
| PB RLS Cloud applied | N/A (dívida) | fora do gate com desvio declarado |

## Veredito parcial

**Código e UI entregues; gate = teste humano em /rls.**
