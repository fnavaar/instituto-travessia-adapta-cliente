# Identidades de teste — F1-T011 (SPEC-1-003)

> **Status:** sintéticas — aguardam confirmação nominal (**BLOQUEIO-1G**).
> E-mails `@piloto.test` **não** são contas reais. Nenhuma senha/token neste arquivo.

## Identidades autorizadas

| ID | E-mail | Papel | Área | Contrato | Uso no teste |
|---|---|---|---|---|---|
| ID-AUTH-CHAMPION | `autorizado.champion@piloto.test` | `champion` | transversal | `CTR-PILOTO-001` | visão transversal do piloto |
| ID-AUTH-CONTRATOS | `autorizado.contratos@piloto.test` | `contratos` | `AREA-CONTRATOS` | `CTR-PILOTO-001` | docs/contrato do recorte |
| ID-AUTH-DPRH | `autorizado.dprh@piloto.test` | `dprh` | `AREA-DPRH` | `CTR-PILOTO-001` | dados de pessoal mascarados |
| ID-AUTH-FIN | `autorizado.financeiro@piloto.test` | `financeiro` | `AREA-FIN` | `CTR-PILOTO-001` | relatório financeiro do recorte |
| ID-AUTH-DIR | `autorizado.direcao@piloto.test` | `direcao` | `AREA-DIR` | agregado piloto | indicadores autorizados |

## Identidade negada

| ID | E-mail | Papel | Resultado esperado (RN-1.003-01) |
|---|---|---|---|
| ID-NEGADO | `negado.externo@piloto.test` | *(nenhum)* | acesso **negado** sem revelar conteúdo nem existência sensível; tentativa **auditada** |

## Objetos fora do recorte (para teste negado de papel autorizado)

| Objeto | Contrato/área | Quem NÃO deve ver |
|---|---|---|
| `CTR-OUTRO-999` / `DOC-OUTRO-999` | outro contrato | todos os papéis do piloto, exceto superuser |
| `DOC-FIN-SENSIVEL-001` | financeiro do piloto | `dprh` sem alçada financeira; `negado` |
| Registro DP mascarado | `AREA-DPRH` | `financeiro` sem alçada DP |

## Notas

- Criação real de usuários no Skip fica para F1-T013 (ou preparação da superfície), **após** aprovação da matriz.
- BLOQUEIO-1G: Segurança/qualidade + champion confirmam identidades nominais antes de dados reais.
