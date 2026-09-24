# Matriz de resultados — F1-T013

> Preenchida após teste humano OK (2026-09-24) + revalidação independente.

## Resumo

| Métrica | Valor |
|---|---|
| PASS | bateria completa (papel × objeto F1-T011) |
| FAIL | 0 (teste humano OK) |
| LEAK | **0** |
| all_pass | sim |

## Amostra RED (obrigatória)

| papel | objeto | esperado | obtido | leak? |
|---|---|---|---|---|
| negado | DOC-CONTRATO-001 | denied | denied | não |
| dprh | DOC-FIN-SENSIVEL-001 | denied | denied | não |
| financeiro | REG-DP-MASCARADO-001 | denied | denied | não |
| contratos | CTR-OUTRO-999 | denied | denied | não |
| contratos | DOC-OUTRO-999 | denied | denied | não |

## Amostra GREEN

| papel | objeto | esperado | obtido |
|---|---|---|---|
| contratos | DOC-CONTRATO-001 | allowed | allowed |
| financeiro | DOC-FIN-001 | allowed | allowed |
| financeiro | PEND-001 | allowed | allowed |
| champion | DEC-ORFA-001 | allowed | allowed |

## JSON negado (observado)

```json
{"allowed":false,"code":"FORBIDDEN"}
```
