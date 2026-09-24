# Verificação automática — F1-T003

**Data:** 2026-09-24T11:55-03:00  
**Skip project:** 60853 · Travessia · UI **0.0.17**

## Resultados

| Check | Status | Evidência |
|---|---|---|
| setup / static / build / test / integrations | PASSOU | QA Skip apply 0.0.17 ok:true |
| Rota `/lotes` | PASSOU | App.tsx + project routes |
| `validateLot` rejeita sem fonte/versão/período/owner | PASSOU | `lote-validate.ts` |
| `registerLot` idempotente por chave | PASSOU | código |
| `correctLot` preserva versão anterior + motivo | PASSOU | código |
| Fixture F1-T001 scenarios | PASSOU | initialLotStore + botões UI |
| Dado real ausente | PASSOU | @piloto.test |
| Rollback CA-1-004 | N/A | fora de escopo (F1-T004) |

## Veredito parcial

**Código e UI entregues; gate = teste humano em /lotes.**
