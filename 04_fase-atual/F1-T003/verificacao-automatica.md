# Verificação automática — F1-T003

**Data (implementação):** 2026-09-24T11:55-03:00  
**Revalidação pós-teste humano:** 2026-09-24T11:58-03:00  
**Skip project:** 60853 · Travessia · UI **0.0.17**

## Resultados (revalidação independente)

| Check | Status | Evidência |
|---|---|---|
| setup / static / build / test / integrations | PASSOU | QA Skip 0.0.17 ok:true |
| Rota `/lotes` | PASSOU | `skip_project_get` → Lotes |
| `validateLot` rejeita sem fonte/versão/período/owner | PASSOU | `lote-validate.ts` |
| `registerLot` idempotente por chave | PASSOU | código |
| `correctLot` preserva v1 + motivo obrigatório | PASSOU | código |
| Fixture F1-T001 scenarios (duplicate / corrected_version) | PASSOU | initialLotStore + UI |
| Dado real ausente | PASSOU | @piloto.test |
| Teste humano | PASSOU | champion OK 2026-09-24 |
| Rollback CA-1-004 | N/A | fora de escopo (F1-T004) |

## Veredito

**PASSOU.** CA-1-002/003 e RN-1.001-02/03/04 demonstrados via funções puras + UI `/lotes`.
