# Análise — F1-T007 (proxima-task)

> **Gerado em:** 2026-09-24T11:20-03:00 · **Estado:** aguardando_autorizacao
> **Não implementar** até autorização explícita em nova mensagem.

## 1. Seleção

| Campo | Valor |
|---|---|
| Task | **F1-T007** — Registrar regra versionada com fonte, vigência e estado de aprovação |
| Onda | 2 (restante junto com F1-T012) |
| Dono formal | Responsável de domínio |
| Coordenação | Champion JP/Iverson |
| SPEC | SPEC-1-002 |
| Subseção | `Dados e integrações`; RN-1.002-01/02; CA-1-006/007; TDD RED/GREEN |
| Pré-condição | F1-T006 **concluída** (2026-09-23) |
| Critério binário | Regra completa fica revisável; regra sem fonte/vigência ou conflitante **não** fica aprovada |

**Por que esta e não F1-T012:** mesma onda; F1-T007 é a menor ID restante da Onda 2 e desbloqueia F1-T008. F1-T012 permanece elegível em paralelo após pedido explícito separado.

## 2. Critérios de aceite (prova)

| ID | Exigência |
|---|---|
| **CA-1-006** | Regra do piloto exibe fonte, versão, vigência, escopo, responsável, estado, exceções e aprovação |
| **CA-1-007** | Regra sem fonte/vigência **ou** com conflito **não** pode ser marcada como `aprovada` |
| **RN-1.002-01** | Regra tem fonte, vigência, escopo, responsável e versão; só aprovador muda para `aprovada` |
| **RN-1.002-02** | Alteração de regra aprovada cria **nova versão** (não sobrescreve) |
| TDD RED | Tentar aprovar incompleta → bloqueado + pendência/log |
| TDD GREEN | Cadastrar regra fornecida (RN-PILOTO-001) consultável com vínculos |

## 3. Insumos existentes (F1-T006)

| Item | Ref | Estado fixture |
|---|---|---|
| RN-PILOTO-001 | SRC-CONTRATO-001 v1 · 2026-01..2026-12 | `aprovada` |
| RN-PILOTO-002 | fonte/vigência null | `pendente` (1D) |
| RN-PILOTO-003 | SRC-FIN-001 vs SRC-FIN-002 | `conflitante` (1F) |
| Aprovadores teste | `@piloto.test` | 1E aberto (nominais) |
| Fixture | `04_fase-atual/F1-T006/fixture-regras.json` | SHA blob `7a631d17…` |

## 4. Estado real da superfície

| Check | Resultado |
|---|---|
| Skip 60853 | preview OK; rotas `/`, `/piloto` |
| PocketBase Cloud | **só** coleção `users` (migrations domínio ainda dívida) |
| UI regras | **inexistente** (só central fontes em `/piloto`) |
| Bloqueios 1D/1E/1F | abertos — **não** inventar fórmula/precedência/nominais |

## 5. Achados e riscos

1. **Skip Cloud migrations instáveis** (aprendizado F1-T002 / AP-2026-09-24-1113): não depender do apply PB para o gate humano.
2. **Aceite é teto:** F1-T007 = registrar/consultar regras versionadas + bloquear aprovação inválida. **Não** inclui decisão com comparativo (F1-T008) nem reabertura completa (F1-T010).
3. **BLOQUEIO-1E/1D/1F:** usar apenas aprovadores/regras sintéticos da fixture; estados `pendente`/`conflitante` permanecem; nunca forçar `aprovada` em 002/003.
4. **RLS fino** fica para F1-T011/F1-T013; aqui auth mínimo + sem dado real.
5. Risco de escopo creep: UI de decisão/veredito → F1-T008.

## 6. Plano de implementação (após autorização)

### 6.1 Entregáveis

1. **Catálogo consultável** na superfície Skip:
   - Rota `/regras` (ou seção dedicada) listando as 3 regras da fixture F1-T006.
   - Detalhe de RN-PILOTO-001 com: `rule_id`, nome, tipo, fonte, `source_version`, vigência, escopo, versão `r1`, owner, state `aprovada`, approver, exceptions.
2. **Validação CA-1-007 (RED):**
   - Ação/simulador “tentar aprovar” em RN-PILOTO-002 e RN-PILOTO-003 → **bloqueado** com motivo (`fonte/vigência ausente` ou `conflito/precedência pendente`) + registro de log/evento sintético.
   - Não alterar state para `aprovada` nesses casos.
3. **Versionamento mínimo (RN-1.002-02, GREEN parcial):**
   - Representar histórico: RN-PILOTO-001 `r1` preservada; demonstrar que edição criaria `r2` (pode ser fluxo UI “nova versão” gravando evento, sem apagar `r1`).
4. **Pacote evidência** em `04_fase-atual/F1-T007/`:
   - `manifesto-regras-registradas.md`
   - `verificacao-automatica.md`
   - log sintético de aprovação/bloqueio (JSON ou md)
5. **Fonte de dados:** fixture embutida a partir de F1-T006 (entrada controlada), com tentativade PB **opcional** se Cloud recuperar — **não** bloquear o gate no apply de migration (desvio declarado se necessário).

### 6.2 Fora de escopo

- DEC-PILOTO-001 / veredito (F1-T008/F1-T009)
- Reabertura fora da alçada completa (F1-T010)
- Dado real, alçadas nominais (1E), fórmulas (1F)
- Publish produção

### 6.3 Testes

| Tipo | O que |
|---|---|
| Auto | build/static Skip; fixture contém 001 aprovada + 002/003 não aprováveis; tentativa aprovar 002/003 negada no código/UI |
| Humano | abrir `/regras`; ver RN-PILOTO-001 completa; tentar aprovar 002 e 003 e ver bloqueio; confirmar ausência de dado real |

## 7. Estimativa de arquivos (Skip + repo cliente)

- `src/pages/Regras.tsx` (+ rota App)
- `src/lib/regras-fixture.ts` (derivado F1-T006)
- opcional: migration rules **somente se** Cloud estável; senão só fixture
- `04_fase-atual/F1-T007/*` + changelog/STATUS no fechamento

## 8. Portão

**Parado em `aguardando_autorizacao`.**  
Implementação só após mensagem explícita do tipo: *Autorizo implementar a F1-T007 conforme o plano*.
