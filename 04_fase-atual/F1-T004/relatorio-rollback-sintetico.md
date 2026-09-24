# Relatório de rollback sintético — F1-T004

## Fluxo de prova

### Antes (após cenário 4)

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

| métrica | antes | depois |
|---|---|---|
| lots.length | N | N (ou N se já existia; **nunca N-1**) |
| logs | M | M+1 (`kind: rollback`) |

### JSON negação parcial (5b)

Rollback com `reason: ''` → `{ result: 'erro', lots_before === lots_after }`.

Fonte `source_ref` de v1 e v2 permanece nos registros.
