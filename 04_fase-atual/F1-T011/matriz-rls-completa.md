# Matriz RLS completa — F1-T011 (SPEC-1-003)

> **Status:** proposta ampliada — aguarda aprovação formal de Segurança/qualidade (**BLOQUEIO-1C** + **1G**).
> Evolui `04_fase-atual/F1-T001/matriz-rls.md`. **Nenhuma permissão global é alterada** nesta task.

## 1. Princípios

- Negar por padrão (RN-1.003-01).
- Acesso mínimo por **papel + área + contrato**.
- Negação **não** revela conteúdo nem existência sensível.
- Filtro de UI **não** substitui autorização no backend.
- Dados pessoais/financeiros só no recorte autorizado.

## 2. Papéis

| Papel | Área | Contratos autorizados (piloto) | Pode |
|---|---|---|---|
| `champion` | transversal (aprovada) | `CTR-PILOTO-001` | consultar e encaminhar; não aprovar por substituição |
| `contratos` | Gestão de Contratos | `CTR-PILOTO-001` | criar/editar contrato e documentos contratuais do recorte |
| `dprh` | DP/RH | `CTR-PILOTO-001` | consultar/registrar dados de pessoal **mascarados** do recorte |
| `financeiro` | Financeiro | `CTR-PILOTO-001` | consultar relatórios financeiros do recorte |
| `direcao` | Direção | agregados autorizados | consultar indicadores/pendências agregados do recorte |
| `negado` / sem papel | — | nenhum | **tudo negado** |

## 3. Recortes de área × contrato

| Área | Código | Contrato piloto | Fora do recorte (negado) |
|---|---|---|---|
| Gestão de Contratos | `AREA-CONTRATOS` | `CTR-PILOTO-001` | `CTR-OUTRO-999` |
| DP/RH | `AREA-DPRH` | `CTR-PILOTO-001` | objetos de outra área |
| Financeiro | `AREA-FIN` | `CTR-PILOTO-001` | `CTR-OUTRO-999` |
| Direção | `AREA-DIR` | visão agregada do piloto | detalhe sensível fora da alçada |

## 4. Matriz por coleção (superfície Skip/PocketBase — a aplicar em tasks posteriores)

| Coleção | list | view | create | update | delete |
|---|---|---|---|---|---|
| `contracts` | auth + recorte | auth + recorte | `contratos` | `contratos` | null (superuser) |
| `periods` | auth + recorte | auth + recorte | `contratos` | `contratos` | null |
| `sources` | auth + recorte | auth + recorte | `champion` | `champion` | null |
| `lots` | auth + recorte | auth + recorte | papéis de domínio do recorte | dono do domínio | null |
| `documents` | auth + recorte | auth + recorte | `contratos`/`financeiro` no recorte | dono | null |
| `pendencies` | auth + recorte | auth + recorte | `champion` + domínio | `champion`/dono | null |
| `rules` | auth + recorte | auth + recorte | responsável de domínio | responsável | null |
| `decisions` | auth + recorte | auth + recorte | responsável | alçada | null |
| `audit_logs` | Segurança + superuser | Segurança + superuser | sistema | **ninguém (app)** | null |

Expressões-alvo (PocketBase):

```text
list/view : @request.auth.id != '' && (contract_ref = @request.auth.contract_ref || @request.auth.role = 'champion' || @request.auth.role = 'direcao')
create    : @request.auth.id != '' && @request.auth.role in (<papéis>)
update    : @request.auth.id != '' && (owner = @request.auth.id || papel de alçada)
delete    : null
```

> A expressão exata entra na migração (F1-T002/F1-T012). Aqui o pacote **registra** a matriz para aprovação.

## 5. Campos sensíveis (mapa)

| Campo / tipo | Sensibilidade | Papéis com view | Exportação nesta fase |
|---|---|---|---|
| CPF, salário, conta bancária | **alta** — proibido em fixture real | nenhum (só mascarado/sintético) | **proibida** |
| Relatório financeiro (valores) | média | `financeiro`, `direcao` (agregado), `champion` | proibida |
| Documento contratual | média | `contratos`, `champion`, `direcao` | proibida |
| Metadados de proveniência | baixa | autenticados no recorte | proibida |
| Logs de auditoria | alta | Segurança/qualidade | proibida |

## 6. Identidades de teste (resumo)

Ver `identidades-teste.md`. Autorizados por papel + `negado.externo@piloto.test`.

## 7. Aprovação

| Item | Status |
|---|---|
| Matriz (este arquivo) | proposta |
| BLOQUEIO-1C | aberto — Segurança/qualidade |
| BLOQUEIO-1G | aberto — identidades nominais reais |
| Permissões globais / diretório | **não alteradas** |
