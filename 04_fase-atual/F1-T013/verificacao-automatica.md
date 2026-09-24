# Verificação automática — F1-T013

**Data (implementação):** 2026-09-24T12:20-03:00  
**Revalidação pós-teste humano:** 2026-09-24T12:25-03:00  
**Skip project:** 60853 · Travessia · UI **0.0.19**

## Resultados (revalidação independente)

| Check | Status | Evidência |
|---|---|---|
| setup / static / build / test / integrations | PASSOU | QA Skip 0.0.19 ok:true |
| Rota `/rls` | PASSOU | `skip_project_get` → Rls |
| Fixture F1-T011 6 auth + 4 denied | PASSOU | `rls-authorize.ts` |
| `canAccess` negado = só FORBIDDEN | PASSOU | código + `hasLeak` |
| `probeAccess` grava audit | PASSOU | código |
| `runBattery` cobre papel × objeto | PASSOU | código |
| Dado real ausente | PASSOU | @piloto.test |
| Permissão global inalterada | PASSOU | só motor/UI |
| Teste humano | PASSOU | champion OK 2026-09-24 |
| PB RLS Cloud applied | N/A (dívida) | desvio declarado |

## Veredito

**PASSOU com desvio declarado.** CA-1-012 e RN-1.003-01 demonstrados via motor puro + UI `/rls`. Onda 3 completa. BLOQUEIO-1C/1G seguem abertos (teste sintético ≠ aceite formal Segurança).
