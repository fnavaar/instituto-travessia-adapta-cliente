# Matriz RLS inicial — F1-T001 (SPEC-1-001 / SPEC-1-003)

> **Status:** proposta — aguarda aprovação formal de Segurança/qualidade (**BLOQUEIO-1C**).
> Nenhuma permissão global é alterada nesta task; a matriz é registrada para aprovação.

## Papéis

| Papel | Descrição |
|---|---|
| `champion` | Consulta e encaminha o piloto; resolve pendências de domínio |
| `contratos` | Gestão de Contratos — cria/edita contrato e documentos contratuais |
| `dprh` | DP/RH — consulta e registra dados do seu domínio |
| `financeiro` | Financeiro — consulta relatórios financeiros autorizados |
| `direcao` | Consulta visão agregada autorizada |
| *(qualquer outro)* | **Negado por padrão** |

## Matriz por coleção (proposta para a superfície Skip/PocketBase)

| Coleção | list | view | create | update | delete |
|---|---|---|---|---|---|
| `contracts` | autenticado | autenticado | `contratos` | `contratos` | negado |
| `periods` | autenticado | autenticado | `contratos` | `contratos` | negado |
| `sources` | autenticado | autenticado | `champion` | `champion` | negado |
| `lots` | autenticado | autenticado | `champion`,`contratos`,`dprh`,`financeiro` | dono do domínio | negado |
| `documents` | autenticado | autenticado | `contratos`,`financeiro` | dono do domínio | negado |
| `pendencies` | autenticado | autenticado | `champion` | `champion` | negado |

Regras de expressão (PocketBase) a usar na implementação das coleções:

```text
list/view : @request.auth.id != ''
create    : @request.auth.id != '' && @request.auth.role in (<papéis permitidos>)
update    : @request.auth.id != '' && owner = @request.auth.id
delete    : null  (superuser apenas — nenhum papel de app apaga)
```

## Identidades de teste (a criar na F1-T011)

| Identidade | Papel | Uso |
|---|---|---|
| `autorizado.champion@piloto.test` | `champion` | acesso autorizado |
| `negado.externo@piloto.test` | *(sem papel)* | deve receber negação sem revelar conteúdo |

> **BLOQUEIO-1C aberto:** esta matriz precisa de aprovação de Segurança/qualidade **antes de
> qualquer dado real**. A carga real permanece bloqueada até a evidência de RLS autorizado/negado
> ser aceita pelo champion e responsáveis (CA-1-005).
