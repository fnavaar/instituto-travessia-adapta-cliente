# Análise — F1-T012 (proxima-task)

> **Gerado em:** 2026-09-24T11:36-03:00 · **Estado:** aguardando_autorizacao
> **Pedido:** "execute" com `sem_task` → roteado para próxima elegível.
> **Não implementar** até autorização explícita em nova mensagem.

## 1. Seleção

| Campo | Valor |
|---|---|
| Task | **F1-T012** — Configurar visão do contrato/período e alertas internos do piloto |
| Onda | 2 (última restante) |
| Dono formal | Responsável pela superfície Ethos |
| Coordenação | Champion JP/Iverson |
| SPEC | SPEC-1-003 |
| Subseção | `Fluxo e regras` 1–4; RN-1.003-02/03/06; CA-1-011/013; TDD GREEN |
| Pré-condições | F1-T001 ✓ · F1-T006 ✓ · F1-T011 ✓ |
| Critério binário | Visão filtra contrato/período/área/responsável/status e mostra pendências, decisões, fonte, cobertura e próxima ação **sem alerta externo** |

**Por que esta:** única task pendente da Onda 2. F1-T003/F1-T008 (Onda 3) também têm pré-condição ok, mas a ordem de onda e o fechamento da Onda 2 priorizam F1-T012.

## 2. Critérios de aceite (prova)

| ID | Exigência |
|---|---|
| **CA-1-011** | Autorizado consulta visão do contrato/período e encontra pendências, decisões, fonte, versão, responsável, prazo e próxima ação |
| **CA-1-013** | Indicador/alerta informa fonte, período, regra/versão e cobertura; cobertura incompleta **não** aparece como resultado completo |
| **RN-1.003-02** | Indicador deriva de registros com fonte/período/regra/versão; senão `cobertura incompleta` |
| **RN-1.003-03** | Pendência/atraso/decisão sem dono → motivo, responsável, prazo, próxima ação; sem dono → escala champion |
| **RN-1.003-06** | Alerta interno **deduplicado** por objeto/estado/prazo (sem envio externo) |

## 3. Insumos existentes

| Fonte | Conteúdo útil |
|---|---|
| F1-T001 fixture | CTR-PILOTO-001, LOT-001/002, PEND-001, docs |
| F1-T006 fixture | DEC-PILOTO-001 (com dono), RN-PILOTO-001 |
| F1-T011 fixture | DEC-ORFA-001 (sem dono), coverage_incomplete LOT-002, objetos auth/denied |
| Skip UI | `/piloto`, `/regras` (v0.0.13); **sem** visão gerencial |
| PB Cloud | só `users` — usar entrada controlada (AP-1113) |

## 4. Achados e riscos

1. **Aceite é teto:** configurar visão + filtros + filas/alertas **internos**. **Não** = testes RLS auth/negado completos (F1-T013), auditoria/rollback profundo (F1-T014/015), nem decisão com veredito (F1-T008).
2. **Sem alerta externo** (e-mail/WhatsApp/webhook) — só painel interno deduplicado.
3. **Cobertura incompleta** (LOT-002 / F1-T011) deve aparecer rotulada, nunca como OK/zero.
4. **DEC-ORFA-001** na fila crítica do champion (RN-1.003-03).
5. Filtro UI **não substitui** autorização; recorte por papel pode ser seletor de identidade de teste (champion/contratos/fin/dprh) filtrando objetos `allowed_roles` da fixture F1-T011 — sem alterar permissões globais de identidade.
6. PB Cloud instável → fixture embutida; sem migration nova obrigatória.

## 5. Plano de implementação (após autorização)

### 5.1 Entregáveis Skip

1. **Rota `/visao`** (ou `/gestao`) — visão do piloto:
   - Filtros: contrato, período, área, responsável, status
   - Cards/lista: pendências (PEND-001), decisão com dono (DEC-PILOTO-001), decisão órfã (DEC-ORFA-001), cobertura incompleta (LOT-002)
   - Cada item: fonte, versão, owner/encaminhamento, prazo, próxima ação, status
2. **Indicadores** com metadados CA-1-013: `source_ref`, período, `rule_id`/versão quando houver, badge de cobertura
3. **Alertas internos** (painel, não push externo):
   - Chave de dedupe: `object_ref|state|due` (RN-1.003-06)
   - Eventos: pendência aberta, cobertura incompleta, decisão órfã, atraso (due ≤ hoje sintético se aplicável)
   - Re-emitir mesmo trio → atualiza estado, **não** duplica linha
4. **Seletor de papel de teste** (champion / contratos / financeiro / dprh / negado) aplicando recorte `allowed_roles` da fixture F1-T011 nos objetos da visão — demonstração CA-1-011 no recorte; negado vê vazio/sem conteúdo sensível (sem F1-T013 completo)
5. Fixture agregada `src/lib/visao-fixture.ts` (F1-T001+006+011)
6. Link na home

### 5.2 Pacote evidência repo

`04_fase-atual/F1-T012/`:
- `manifesto-visao.md`
- `verificacao-automatica.md`
- `alertas-internos.md` (chaves de dedupe + exemplos)

### 5.3 Fora de escopo

- Matriz RLS runtime/server completa e capturas por papel com audit log PB (F1-T013)
- Publish, e-mail/SMTP, webhooks
- Dado real; aceite formal 1C/1G/1H/1I
- Veredito em DEC-PILOTO-001 (F1-T008)

### 5.4 Testes

| Tipo | O quê |
|---|---|
| Auto | build Skip; fixture com PEND + DEC + DEC-ORFA + cobertura incompleta; dedupe de alerta; filtros |
| Humano | abrir `/visao`; filtrar CTR-PILOTO-001; ver pendência/decisões/cobertura; trocar papel teste; confirmar alerta interno sem duplicar; sem dado real |

## 6. Portão

**Parado em `aguardando_autorizacao`.**  
Implementação só após: *Autorizo implementar a F1-T012 conforme o plano*.
