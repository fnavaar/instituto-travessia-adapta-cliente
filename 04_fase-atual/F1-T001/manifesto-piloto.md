# Manifesto do piloto — F1-T001 (SPEC-1-001)

> **Task:** F1-T001 · **Dono:** Champion (JP/Iverson - Champions)
> **Fase:** 1 — Amplificar · **SPEC:** SPEC-1-001 Central de fontes e registro do piloto
> **Gerado em:** 2026-09-23 · **Por:** agente do cliente (Adapta Cliente)
> **Regra cumprida:** nenhum dado real. Toda a fixture é **sintética**.

## 1. Objetivo e resultado observável

Registrar o pacote de pré-condições da central: contrato/período piloto, fontes, responsáveis,
campos obrigatórios, matriz RLS inicial e fixture mascarada — sem dado real — para que as tasks
F1-T002, F1-T006 e F1-T011 possam executar.

## 2. Contrato/período piloto

| Campo | Valor |
|---|---|
| Identificador do contrato | `CTR-PILOTO-001` |
| Período | `2026-01` a `2026-12` |
| Área/unidade | Unidade piloto (a nomear) |
| Status do piloto | **SINTÉTICO** — aguarda nomeação real (BLOQUEIO-1A) |
| Sensibilidade | Interna |

> **BLOQUEIO-1A aberto:** o champion ainda deve indicar o contrato e período **reais** do piloto.
> Enquanto isso, todo o pacote usa identificadores sintéticos e nenhuma carga real é permitida.

## 3. Fontes e responsáveis

| Domínio | Fonte prevista (SPEC) | Responsável nominal | Status |
|---|---|---|---|
| Gestão de Contratos | SharePoint/nuvem Microsoft + documentos contratuais | a confirmar | **BLOQUEIO-1B** |
| DP/RH | Domínio/eSocial (exportação comprovada ou fallback manual) | a confirmar | **BLOQUEIO-1B** |
| Financeiro | Excel/relatório financeiro | a confirmar | **BLOQUEIO-1B** |
| Superfície Ethos | Projeto Skip `Travessia` (id 60853) | a confirmar | pendente |
| Champion | — | JP/Iverson - Champions | confirmado |

> **BLOQUEIO-1B aberto:** Gestão de Contratos / DP-RH / Financeiro devem confirmar responsáveis
> nominais e entregar amostra mascarada. A SPEC permite executar com fixture sintética até lá.

## 4. Campos obrigatórios por registro

| Entidade | Campos obrigatórios |
|---|---|
| Contrato | `contract_ref`, `period`, `owner`, `status`, `source_ref`, `source_version` |
| Lote | `lot_ref`, `source_ref`, `source_version`, `period`, `owner`, `sensitivity`, `quality`, `status` |
| Documento | `doc_ref`, `lot_ref`, `doc_type`, `source_ref`, `source_version` |
| Pendência | `pendency_ref`, `lot_ref`, `reason`, `owner`, `due`, `next_action`, `status` |
| Fonte | `source_ref`, `source_kind`, `owner`, `sensitivity` |

Chave de idempotência do lote: **`source_ref + source_version + period`** (RN-1.001-03).

## 5. Estados mínimos

- **Lote:** `rascunho` → `aguardando fonte` → `com pendência` → `válido` → `revertido`
- **Qualidade:** `ok` | `bloqueada`
- **Pendência:** `aberta` | `reconciliando` | `resolvida`

## 6. Matriz RLS inicial

Ver `matriz-rls.md` (mesma pasta). Resumo: papéis `champion`, `contratos`, `dprh`, `financeiro`,
`direcao`; **qualquer outro papel é negado por padrão**. Aprovação formal é o **BLOQUEIO-1C**.

## 7. Fixture sintética

Arquivo: `fixture-piloto.json` (mesma pasta). Conteúdo: 1 contrato, 1 período, 2 fontes,
1 lote válido, 1 documento contratual, 1 relatório financeiro, 1 responsável, 1 pendência
intencional, 1 duplicata e 1 versão corrigida. **Sem CPF, salário ou dado bancário real.**

- **Identificação:** `fixture-piloto.json` · SHA-256 calculado após o commit (ver §9).
- **Fonte:** sintética, produzida pelo agente — não derivada de arquivo legado.

## 8. Pré-condições e pontos de parada

| Pré-condição | Situação |
|---|---|
| Contrato/período nomeado | sintético (BLOQUEIO-1A) |
| Fixture mascarada | entregue (sintética) |
| Fontes e responsáveis | pendentes (BLOQUEIO-1B) |
| Campos obrigatórios | registrados (este manifesto) |
| Matriz RLS inicial | proposta (BLOQUEIO-1C) |
| Coexistência com legado | não alterar legado (respeitado) |

**Pontos de parada:** parar se faltar contrato/período, responsável, campo, RLS, ou se houver
qualquer dado real.

## 9. Evidências

| Evidência | Onde |
|---|---|
| Manifesto do piloto | este arquivo |
| Fixture sintética | `fixture-piloto.json` |
| Matriz RLS | `matriz-rls.md` |
| Hash da fixture | registrado no changelog após o commit |
| Aceite humano do champion | **pendente** (portão de teste humano) |

## 10. Dados/fixtures e caminhos de erro cobertos

Caminhos de erro obrigatórios que a fixture habilita para as próximas tasks: campo ausente,
fonte indisponível, formato inválido, duplicidade, versão concorrente, importação interrompida,
acesso negado e rollback.
