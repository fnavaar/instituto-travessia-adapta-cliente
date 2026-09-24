# Matriz de resultados — F1-T013

> Preencher no teste humano (ou copiar da UI após **Rodar bateria completa**).

## Resumo

| Métrica | Valor |
|---|---|
| PASS | _(preencher)_ |
| FAIL | _(preencher)_ |
| LEAK | **deve ser 0** |
| all_pass | sim/não |

## Amostra RED (obrigatória)

| papel | objeto | esperado | obtido | leak? |
|---|---|---|---|---|
| negado | DOC-CONTRATO-001 | denied | | não |
| dprh | DOC-FIN-SENSIVEL-001 | denied | | não |
| financeiro | REG-DP-MASCARADO-001 | denied | | não |
| contratos | CTR-OUTRO-999 | denied | | não |
| contratos | DOC-OUTRO-999 | denied | | não |

## Amostra GREEN

| papel | objeto | esperado | obtido |
|---|---|---|---|
| contratos | DOC-CONTRATO-001 | allowed | |
| financeiro | DOC-FIN-001 | allowed | |
| financeiro | PEND-001 | allowed | |
| champion | DEC-ORFA-001 | allowed | |

## JSON negado (exemplo esperado)

```json
{"allowed":false,"code":"FORBIDDEN"}
```

Qualquer chave além de `allowed`/`code` em negado = **VAZAMENTO** → parar a task.
