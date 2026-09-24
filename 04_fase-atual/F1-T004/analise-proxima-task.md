# Análise — F1-T004 (proxima-task)

> **Gerado em:** 2026-09-24T13:12-03:00 · **Estado:** aguardando_autorizacao
> **Pedido:** "execute" com `sem_task` → roteado para próxima elegível (Onda 4).
> **Não implementar** até autorização explícita em nova mensagem.

## 1. Seleção

| Campo | Valor |
|---|---|
| Task | **F1-T004** — Executar rollback de lote e provar retorno à versão válida |
| Onda | 4 |
| Dono formal | Responsável pela superfície Ethos |
| Coordenação | Champion JP/Iverson |
| SPEC | SPEC-1-001 |
| Subseção | `Rollback ou reversão`; cenário `Rollback`; CA-1-004; TDD REFACTOR/REGRESSÃO |
| Pré-condição | F1-T003 **concluída** ✓; motor `/lotes` + `lote-validate.ts` |
| Critério binário | Lote inválido é **revertido** sem apagar fonte, histórico ou log; **última versão válida** volta a ser apresentada |

**Por que esta e não F1-T009/T014:** mesma Onda 4; menor ID; continua SPEC-1-001 após F1-T003. F1-T009 e F1-T014 permanecem elegíveis com pedido explícito separado.

## 2. Critérios de aceite (prova)

| ID | Exigência |
|---|---|
| **CA-1-004** | Lote inválido revertido **sem** apagar fonte, versão anterior ou log; sistema retorna ao último estado válido |
| **RN-1.001-05** | Rollback: retirar derivados do lote; manter fonte, histórico e motivo; rollback parcial → erro + reconciliação |
| TDD REFACTOR/REGRESSÃO | Reverter lote novo inválido; histórico preservado; versão válida apresentada |

**Ponto de parada:** exclusão, perda de histórico ou retorno parcial sem bloqueio.

## 3. Insumos existentes

| Fonte | Útil para F1-T004 |
|---|---|
| F1-T003 `lote-validate.ts` | `registerLot` / `correctLot` / store / logs — **sem** `rollbackLot` ainda |
| F1-T003 `/lotes` | UI de cenários; manifesto declara rollback fora de escopo |
| F1-T001 fixture | LOT-001 válido; LOT-002 → v2 corrigida; cenários de versão |
| Skip PB Cloud | só `users` — entrada controlada (AP-1113) |

## 4. Achados e riscos

1. **Aceite é teto:** provar rollback de lote inválido + retorno à versão válida. **Não** = gate dado real (F1-T005), nem auditoria/cobertura profunda (F1-T014), nem rollback de visão (F1-T015).
2. Invariantes anti-destruição:
   - **nunca** `delete` de lotes/versões anteriores
   - **nunca** apagar logs de validação
   - fonte (`source_ref`) permanece
   - status do lote inválido → `revertido` (ou equivalente); derivados marcados inativos
3. Fluxo de prova sugerido:
   - base: LOT-001 v1 válida (ou LOT-002 v2 após correct)
   - introduzir lote/versão **nova inválida** (ex.: LOT-003 v1 aceito por engano / quality ok depois marcado inválido, ou v3 ruim)
   - `rollbackLot(store, lot_ref, reason, actor)` → marca inválida como `revertido`; **apresenta** última válida; log `rollback`
4. Rollback parcial (ex.: tentar reverter sem motivo, ou alvo inexistente) → **erro** + bloqueio, sem mutação destrutiva.
5. PB Cloud instável → estender funções puras + UI `/lotes` (ou `/rollback`); sem migration obrigatória.
6. Risco de escopo: F1-T009 (decisão incompleta), F1-T014 (auditoria), hard-delete.

## 5. Plano de implementação (após autorização)

### 5.1 Entregáveis Skip

1. **Estender `src/lib/lote-validate.ts`:**
   - tipo status incluir `revertido`
   - `rollbackLot(store, lot_ref | lot_id, reason, actor)`:
     - exige motivo não vazio
     - localiza versão-alvo (nova inválida) e última válida anterior do mesmo `lot_ref` (ou cadeia `supersedes`)
     - marca alvo `status: revertido`, `quality: bloqueada`, `rollback_reason`, `rollback_at`, `rollback_actor`
     - **não remove** do array `lots`; **não limpa** `logs`
     - define `presented_lot` / flag `is_current` na válida restaurada
     - append log `kind: 'rollback'`
   - `getPresentedLot(store, lot_ref)` → última não-revertida válida
2. **UI `/lotes` (estender):**
   - cenário botão: **4. Introduzir lote inválido novo** (ex. LOT-RB-001 v1 “publicado” por engano)
   - cenário botão: **5. Rollback LOT-RB-001** com motivo → volta a apresentar válida anterior (ou estado sem derivado inválido)
   - tabela: badge `revertido`; coluna motivo rollback; destaque “versão apresentada”
   - tentativa de rollback sem motivo → erro visível
   - log inclui eventos rollback
3. Pacote `04_fase-atual/F1-T004/`:
   - `manifesto-rollback.md`
   - `verificacao-automatica.md`
   - `relatorio-rollback-sintetico.md` (antes/depois: lots count, presented, logs intactos)

### 5.2 Fora de escopo

- F1-T005 gate dado real
- F1-T009 / F1-T010 decisão
- F1-T014 auditoria/cobertura
- F1-T015 rollback de visão
- `rm`/delete de registros, publish, dado real

### 5.3 Testes

| Tipo | O quê |
|---|---|
| Auto | build Skip; `rollbackLot` preserva length de lots/logs; presented = válida; sem motivo → erro |
| Humano | `/lotes`: criar inválido → rollback → v1/válida apresentada; histórico e log intactos; contagem de lots não diminui; sem dado real |

## 6. Portão

**Parado em `aguardando_autorizacao`.**  
Implementação só após: *Autorizo implementar a F1-T004 conforme o plano*.
