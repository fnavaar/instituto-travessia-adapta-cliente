# Manifesto — rollback de lote F1-T004 (SPEC-1-001)

> **Task:** F1-T004 · **Dono formal:** Responsável pela superfície Ethos · **Coordenação:** Champion (JP/Iverson)
> **Gerado em:** 2026-09-24 · **Por:** agente do cliente (Adapta Cliente)
> **Regra:** nenhum dado real; entrada controlada; **nunca** hard-delete de lote/fonte/log.

## 1. Objetivo

Executar rollback de lote inválido e provar retorno à versão válida (CA-1-004 · RN-1.001-05).

## 2. Superfície

| Item | Valor |
|---|---|
| Projeto Skip | Travessia · id 60853 |
| Preview | https://travessia-8e0be--preview.goskip.app |
| Rota | `/lotes` |
| Versão UI | 0.0.20 |
| Modo | entrada controlada (store + funções puras) |
| Produção | não publicada |

## 3. Artefatos Skip

| Artefato | Caminho |
|---|---|
| Domínio | `src/lib/lote-validate.ts` — `rollbackLot`, `getPresentedLot`, status `revertido` |
| UI | `src/pages/Lotes.tsx` — cenários 4 / 5 / 5b |

## 4. Comportamentos

| Cenário | Ação | Resultado esperado |
|---|---|---|
| 4 | Introduzir LOT-RB-001 v1 válida + v2 inválida | lots aumenta; v2 corrente |
| 5 | Rollback com motivo + actor | v2 → `revertido`; v1 `is_current`; lots **não diminui**; log `rollback` |
| 5b | Rollback sem motivo | **erro**; lots inalterado |

Invariantes: fonte (`source_ref`) permanece; logs append-only; pendência de reconciliação aberta.

## 5. Fora de escopo

F1-T005 · F1-T009 · F1-T014 · F1-T015 · hard-delete · publish · dado real.

## 6. Teste humano sugerido

1. Hard refresh https://travessia-8e0be--preview.goskip.app/lotes
2. **4. Introduzir LOT-RB-001 v2 inválida**
3. **5. Rollback LOT-RB** — conferir: v2 `revertido`, v1 corrente, contagem de lots estável/cresce, log `rollback`
4. **5b. Rollback sem motivo** — deve falhar sem mutação
5. Confirmar só @piloto.test
