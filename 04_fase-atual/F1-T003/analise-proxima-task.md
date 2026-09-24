# Análise — F1-T003 (proxima-task)

> **Gerado em:** 2026-09-24T11:50-03:00 · **Estado:** aguardando_autorizacao
> **Pedido:** "execute" com `sem_task` → roteado para próxima elegível (Onda 3).
> **Não implementar** até autorização explícita em nova mensagem.

## 1. Seleção

| Campo | Valor |
|---|---|
| Task | **F1-T003** — Exercitar rejeição, pendência, duplicidade e versionamento do lote |
| Onda | 3 |
| Dono formal | Responsável pela superfície Ethos |
| Coordenação | Champion JP/Iverson |
| SPEC | SPEC-1-001 |
| Subseção | RN-1.001-02/03/04; cenários `Limite` e `Duplicidade`; CA-1-002/003; TDD RED/REFACTOR |
| Pré-condição | F1-T002 **concluída** ✓; fixtures de erro em F1-T001 `scenarios` |
| Critério binário | Lote incompleto é **bloqueado**; reenvio idêntico é **idempotente**; correção cria **nova versão** com motivo e **preserva** a anterior |

**Por que esta e não F1-T008/T013:** mesma Onda 3; menor ID e continua o caminho SPEC-1-001 após F1-T002. F1-T008/T013 permanecem elegíveis em paralelo com pedido explícito separado.

## 2. Critérios de aceite (prova)

| ID | Exigência |
|---|---|
| **CA-1-002** | Lote sem fonte, versão, período ou responsável é **bloqueado** e gera pendência com motivo, dono e próxima ação |
| **CA-1-003** | Reenvio do **mesmo** lote é idempotente (não duplica); alteração cria **nova versão** com motivo e histórico |
| **RN-1.001-02** | Sem fonte/versão/período/responsável → rejeitar antes de disponibilizar |
| **RN-1.001-03** | Mesmo `source_ref + source_version + period` → não duplicar; mostrar existente |
| **RN-1.001-04** | Dado obrigatório ausente → quality `bloqueada` + pendência; correção = nova versão |

**Fora deste aceite (próximas tasks):** rollback completo CA-1-004 (F1-T004); gate dado real (F1-T005).

## 3. Insumos existentes

| Fonte | Útil para F1-T003 |
|---|---|
| F1-T001 `scenarios.duplicate` | reenvio LOT-001 idêntico → idempotente |
| F1-T001 `scenarios.corrected_version` | LOT-002 → v2 com motivo; v1 preservada |
| F1-T001 LOT-002 + PEND-001 | incompleto / quality bloqueada (já no piloto) |
| F1-T002 `/piloto` | consulta; **sem** motor de submit/validação interativa ainda |
| Skip PB Cloud | só `users` — entrada controlada (AP-1113) |

## 4. Achados e riscos

1. **Aceite é teto:** exercitar RED (rejeição + pendência) e REFACTOR parcial (idempotência + nova versão). **Não** = rollback (F1-T004).
2. PB Cloud instável → lógica pura na fixture/UI (padrão `canApproveRule` / AP-1130), não depender de migration.
3. Chave de idempotência canônica: `source_ref|source_version|period` (alinhada a F1-T001 `idempotency_key`).
4. Correção **nunca** edita silenciosamente a versão anterior — só acrescenta registro `vN+1` + log.
5. Risco de escopo: UI de decisão (F1-T008) ou RLS negado completo (F1-T013).

## 5. Plano de implementação (após autorização)

### 5.1 Entregáveis Skip

1. **Módulo de domínio** `src/lib/lote-validate.ts` (funções puras):
   - `validateLot(input)` → `{ ok } | { ok:false, missing[], pendency }` (CA-1-002 / RN-1.001-02/04)
   - `idempotencyKey(source_ref, source_version, period)`
   - `registerLot(store, input)` → se chave existe: **retorna existente** + log `idempotente` (CA-1-003 / RN-1.001-03); senão valida e insere
   - `correctLot(store, lot_ref, patch, reason)` → nova `source_version`, preserva anterior, log com motivo (RN-1.001-04)
2. **UI** — estender `/piloto` **ou** rota `/lotes` de exercício:
   - Formulário "Submeter lote" (campos fonte/versão/período/owner + opcional incompleto)
   - Botões de cenário fixture:
     - **Reenviar LOT-001 idêntico** → idempotente (sem 2ª linha)
     - **Submeter lote incompleto** → bloqueado + PEND nova/atualizada
     - **Corrigir LOT-002 → v2** → v1 permanece; v2 `valido` com motivo
   - Painel: lista de lotes (todas as versões), pendências, **log de validação** (rejeição / idempotente / nova versão)
3. Fixture de erro embutida a partir de F1-T001 `scenarios` (+ lote incompleto explícito sem `owner`/`period` para RED puro)
4. Link na home se rota nova

### 5.2 Pacote evidência repo

`04_fase-atual/F1-T003/`:
- `manifesto-lotes-bordas.md`
- `verificacao-automatica.md`
- `log-validacao-sintetico.md` (exemplos de três caminhos)

### 5.3 Fora de escopo

- Rollback CA-1-004 (F1-T004)
- Decisão/veredito (F1-T008)
- RLS auth/negado completo (F1-T013)
- Publish, dado real, conector SharePoint real

### 5.4 Testes

| Tipo | O quê |
|---|---|
| Auto | build Skip; `validateLot` rejeita incompleto; `registerLot` idempotente; `correctLot` preserva v1 |
| Humano | abrir UI; (1) incompleto → bloqueio+pendência; (2) reenvio LOT-001 → sem duplicata; (3) LOT-002→v2 com motivo e v1 visível; log presente; sem dado real |

## 6. Portão

**Parado em `aguardando_autorizacao`.**  
Implementação só após: *Autorizo implementar a F1-T003 conforme o plano*.
