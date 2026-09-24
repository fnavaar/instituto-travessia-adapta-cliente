# Análise — F1-T008 (proxima-task)

> **Gerado em:** 2026-09-24T12:00-03:00 · **Estado:** aguardando_autorizacao
> **Pedido:** "execute" com `sem_task` → roteado para próxima elegível (Onda 3).
> **Não implementar** até autorização explícita em nova mensagem.

## 1. Seleção

| Campo | Valor |
|---|---|
| Task | **F1-T008** — Registrar decisão com comparativo e sugestão separada |
| Onda | 3 |
| Dono formal | Champion |
| Coordenação | Champion JP/Iverson |
| SPEC | SPEC-1-002 |
| Subseção | `Fluxo e regras` 3–5; RN-1.002-04; CA-1-008; TDD GREEN |
| Pré-condição | F1-T007 **concluída** ✓; DEC-PILOTO-001 + fontes em F1-T006 |
| Critério binário | Decisão exibe pergunta, contexto, registros, comparativo, opções, **sugestão/inferência rotulada**, dono, alçada e evidência |

**Por que esta e não F1-T013/F1-T004:** mesma Onda 3; menor ID restante (F1-T003 já fechada); continua SPEC-1-002 após F1-T007. F1-T013 e F1-T004 permanecem elegíveis com pedido explícito separado.

## 2. Critérios de aceite (prova)

| ID | Exigência |
|---|---|
| **CA-1-008** | Decisão do piloto exibe origem, comparativo, opções, sugestão/inferência **separada**, dono, alçada, veredito, justificativa, evidência, prazo e próxima ação |
| **RN-1.002-04** | Sugestão/inferência com rótulo, fonte, confiança/limitação; **nunca** muda estado sozinha |
| TDD GREEN | Decisão com comparativo consultável; sugestão ≠ veredito |

**Fora deste aceite (próximas tasks):**
- Bloquear encerramento sem dono/alçada/evidência completo (CA-1-009) → **F1-T009**
- Reabertura/versionamento de decisão (CA-1-010) → **F1-T010**

## 3. Insumos existentes

| Fonte | Útil para F1-T008 |
|---|---|
| F1-T006 `decision` DEC-PILOTO-001 | pergunta, contexto, VALOR-A/B, recommendation rotulada, owner, alçada, veredito=null, state=aberta |
| F1-T006 matriz-alcadas | aprovador.financeiro@piloto.test (teste; 1E aberto) |
| F1-T007 `/regras` | RN-PILOTO-003 conflitante vinculável |
| F1-T012 `/visao` | DEC-PILOTO-001 já listada (sem detalhe CA-1-008) |
| Skip rotas | `/piloto` `/regras` `/visao` `/lotes` — **sem** `/decisoes` |

## 4. Achados e riscos

1. **Aceite é teto:** registrar/exibir decisão com comparativo + sugestão separada. Pode incluir **registro de veredito humano** na UI (GREEN da SPEC), mas:
   - sugestão **não** pode auto-aplicar veredito (RN-1.002-04)
   - botão "usar sugestão como veredito" **proibido** sem ação explícita do aprovador + justificativa/evidência
2. Fixture já tem `veredito: null` — estado inicial `aberta` é correto; F1-T009 exercita bloqueio de encerramento incompleto com mais rigor.
3. BLOQUEIO-1E: só aprovadores `@piloto.test`; não inventar nominais.
4. BLOQUEIO-1F: não escolher precedência automática entre VALOR-A/B.
5. PB Cloud só `users` → entrada controlada (AP-1113/1130).
6. Risco de escopo: reabertura (F1-T010), bloqueio órfão completo (F1-T009), RLS (F1-T013).

## 5. Plano de implementação (após autorização)

### 5.1 Entregáveis Skip

1. **`src/lib/decisao-fixture.ts`** (derivado F1-T006):
   - DEC-PILOTO-001 com comparison, options, recommendation rotulada
   - `applyVerdict(decision, { veredito, justificativa, evidence, actor })` — só preenche se actor = alçada **e** campos obrigatórios; **não** copia sugestão sozinha
   - sugestão permanece imutável no objeto (só leitura)
2. **Rota `/decisoes`** (ou `/decisao`):
   - Detalhe DEC-PILOTO-001: pergunta, contexto, related_records, tabela comparativo VALOR-A vs VALOR-B (fonte + valor)
   - Card **Sugestão/inferência** com badge explícito + fonte + confiança + limits (separado visualmente do veredito)
   - Bloco **Veredito humano**: dono, alçada, prazo, próxima ação; form para aprovador de teste registrar veredito/justificativa/evidência (opcional no GREEN mínimo, recomendado para CA-1-008 completo)
   - Estado inicial: veredito vazio / `aberta`
   - Aviso permanente: "Sugestão não altera estado"
3. Link na home + eventual deep-link a partir de `/visao` (opcional, não obrigatório)
4. Pacote `04_fase-atual/F1-T008/`: manifesto, verificação, captura textual da separação sugestão/veredito

### 5.2 Fora de escopo

- CA-1-009 bloqueio rigoroso de encerramento incompleto (F1-T009)
- Reabertura CA-1-010 (F1-T010)
- Autoaprovação / executor como aprovador
- Publish, dado real, alçadas nominais (1E)

### 5.3 Testes

| Tipo | O quê |
|---|---|
| Auto | build Skip; fixture DEC com comparison + recommendation.label; applyVerdict não roda sem actor/campos; sugestão não muta state |
| Humano | abrir `/decisoes`; ver DEC-PILOTO-001; comparativo A/B; badge sugestão separado; veredito vazio ou registrado manualmente; confirmar sugestão não fechou sozinha; sem dado real |

## 6. Portão

**Parado em `aguardando_autorizacao`.**  
Implementação só após: *Autorizo implementar a F1-T008 conforme o plano*.
