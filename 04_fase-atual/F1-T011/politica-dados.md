# Política de dados — retenção e exportação — F1-T011 (SPEC-1-003)

> **Status:** proposta — aguarda aprovação (**BLOQUEIO-1H**).
> Nenhuma exportação externa e nenhum dado real nesta fase.

## 1. Classificação de campos

| Classe | Exemplos | Tratamento no piloto |
|---|---|---|
| Proibida em fixture real | CPF, salário, conta bancária, dados de saúde | só sintético/mascarado; nunca carregar real antes do gate RLS |
| Sensível operacional | valores financeiros, docs contratuais | só recorte RLS; sem export |
| Operacional | `source_ref`, versão, status, pendência | consultável no recorte |
| Auditoria | ator, ação, antes/depois, timestamp | log protegido; app user não edita |

## 2. Retenção (proposta)

| Artefato | Retenção no piloto | Após o piloto |
|---|---|---|
| Fixtures sintéticas no repo | enquanto a Fase 1 estiver aberta | arquivar em `05_entregas/` |
| Logs de teste (quando existirem) | mínimo necessário à demonstração | revisar com Segurança |
| Dados reais | **não aplicável** — bloqueados até CA-1-005 / F1-T005 | só após aceite RLS |

## 3. Exportação

| Canal | Fase 1 |
|---|---|
| Exportação externa (e-mail, planilha pública, API aberta) | **PROIBIDA** |
| Download local de fixture sintética do repo | permitida (já pública no handoff) |
| Publicação de decisões/indicadores externos | **PROIBIDA** (SPEC-1-003) |
| Alertas | **somente internos** |

## 4. Mascaramento

- Fixtures usam identificadores `*-PILOTO-*` e e-mails `@piloto.test`.
- Valores financeiros na fixture de decisão são **ilustrativos** (não reais).
- Nenhum CPF/CNPJ/salário/dado bancário real em qualquer artefato.

## 5. Aprovação

| Item | Quem | Status |
|---|---|---|
| Política (este arquivo) | Segurança/qualidade | **BLOQUEIO-1H** aberto |
| Matriz de campos sensíveis | Segurança/qualidade | proposta na matriz RLS |
| Gate de dados reais | champion + responsáveis (CA-1-005) | bloqueado até F1-T013/F1-T005 |
