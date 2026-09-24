# Log de autorização sintético — F1-T013

Exemplos gerados na UI (sessão browser; append-only).

## Negado

| actor | role | object_ref | result | reason |
|---|---|---|---|---|
| negado.externo@piloto.test | negado | DOC-CONTRATO-001 | denied | RN-1.003-01 / CA-1-012: FORBIDDEN |
| autorizado.dprh@piloto.test | dprh | DOC-FIN-SENSIVEL-001 | denied | FORBIDDEN |
| autorizado.financeiro@piloto.test | financeiro | REG-DP-MASCARADO-001 | denied | FORBIDDEN |
| autorizado.contratos@piloto.test | contratos | CTR-OUTRO-999 | denied | FORBIDDEN |

## Autorizado

| actor | role | object_ref | result | reason |
|---|---|---|---|---|
| autorizado.contratos@piloto.test | contratos | DOC-CONTRATO-001 | allowed | acesso no recorte autorizado |
| autorizado.financeiro@piloto.test | financeiro | DOC-FIN-001 | allowed | acesso no recorte autorizado |

Campos do evento: `at, actor, role, object_ref, action, result, reason`.
