# Relatório de rollback sintético — F1-T004

> Após teste humano OK (2026-09-24).

## Fluxo de prova

### Antes (cenário 4)

| lot_ref | versão | status | corrente |
|---|---|---|---|
| LOT-RB-001 | v1 | valido | não |
| LOT-RB-001 | v2 | valido (quality bloqueada) | sim |

### Depois (cenário 5)

| lot_ref | versão | status | corrente | rollback |
|---|---|---|---|---|
| LOT-RB-001 | v1 | valido | **sim** | — |
| LOT-RB-001 | v2 | **revertido** | não | motivo + actor + timestamp |

### Contagens

| métrica | regra |
|---|---|
| lots.length | **nunca diminui** (map → `revertido`, não filter-delete) |
| logs | append `kind: rollback` |
| fonte | `source_ref` de v1 e v2 permanece |

### 5b — sem motivo

`result: 'erro'` · `lots_before === lots_after` · nenhuma mutação.
