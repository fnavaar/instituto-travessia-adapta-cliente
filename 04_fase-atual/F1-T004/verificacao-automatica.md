# Verificação automática — F1-T004

**Data:** 2026-09-24T13:18-03:00  
**Skip project:** 60853 · Travessia · UI **0.0.20**

## Resultados

| Check | Status | Evidência |
|---|---|---|
| setup / static / build / test / integrations | PASSOU | QA Skip apply 0.0.20 ok:true |
| Rota `/lotes` | PASSOU | já existente + UI estendida |
| `rollbackLot` exige motivo/actor | PASSOU | código |
| `rollbackLot` não diminui `lots.length` | PASSOU | código (map, não filter-delete) |
| `getPresentedLot` após rollback | PASSOU | código |
| log kind `rollback` append | PASSOU | código |
| Dado real ausente | PASSOU | @piloto.test |

## Veredito parcial

**Código e UI entregues; gate = teste humano em /lotes.**
