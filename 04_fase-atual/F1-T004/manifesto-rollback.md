# Manifesto — rollback de lote F1-T004 (SPEC-1-001)

> **Task:** F1-T004 · **Dono formal:** Responsável pela superfície Ethos · **Coordenação:** Champion (JP/Iverson)
> **Gerado em:** 2026-09-24 · **Concluída em:** 2026-09-24T13:40-03:00
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

## 4. Evidência de aceite

- Teste humano do champion: **OK** (2026-09-24).
- Revalidação independente: `rollbackLot`/`getPresentedLot`, status `revertido`, lots/logs preservados, sem dado real — PASSOU.

## 5. Fora de escopo

F1-T005 · F1-T009 · F1-T014 · F1-T015 · hard-delete · publish · dado real.
