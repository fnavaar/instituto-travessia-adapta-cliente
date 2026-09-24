# Verificação automática — F1-T004

**Data (implementação):** 2026-09-24T13:18-03:00  
**Revalidação pós-teste humano:** 2026-09-24T13:40-03:00  
**Skip project:** 60853 · Travessia · UI **0.0.20**

## Resultados (revalidação independente)

| Check | Status | Evidência |
|---|---|---|
| setup / static / build / test / integrations | PASSOU | QA Skip 0.0.20 ok:true |
| Rota `/lotes` | PASSOU | `skip_project_get` → Lotes |
| `rollbackLot` exige motivo/actor | PASSOU | código |
| `rollbackLot` não diminui lots (map, não delete) | PASSOU | código |
| `getPresentedLot` após rollback | PASSOU | código |
| log kind `rollback` append | PASSOU | código |
| status `revertido` | PASSOU | LotRecord type |
| Dado real ausente | PASSOU | @piloto.test |
| Teste humano | PASSOU | champion OK 2026-09-24 |

## Veredito

**PASSOU.** CA-1-004 e RN-1.001-05 demonstrados via `rollbackLot` + UI `/lotes` sem hard-delete.
