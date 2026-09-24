# Manifesto — bordas de lote F1-T003 (SPEC-1-001)

> **Task:** F1-T003 · **Dono formal:** Responsável pela superfície Ethos · **Coordenação:** Champion (JP/Iverson)
> **Gerado em:** 2026-09-24 · **Concluída em:** 2026-09-24T11:58-03:00
> **Regra:** nenhum dado real; fixture F1-T001; entrada controlada; sem rollback (F1-T004).

## 1. Objetivo

Exercitar rejeição, pendência, duplicidade e versionamento do lote
(CA-1-002 / CA-1-003 · RN-1.001-02/03/04).

## 2. Superfície

| Item | Valor |
|---|---|
| Projeto Skip | Travessia · id 60853 |
| Preview | https://travessia-8e0be--preview.goskip.app |
| Rota | `/lotes` |
| Versão UI | 0.0.17 |
| Modo | entrada controlada (store em memória + funções puras) |
| Produção | não publicada |

## 3. Artefatos Skip

| Artefato | Caminho |
|---|---|
| Domínio puro | `src/lib/lote-validate.ts` |
| UI | `src/pages/Lotes.tsx` |
| Rota | `src/App.tsx` → `/lotes` |

## 4. Evidência de aceite

- Teste humano do champion: **OK** (2026-09-24).
- Revalidação independente: rota, `validateLot`/`registerLot`/`correctLot`, cenários F1-T001, sem dado real — PASSOU.

## 5. Fora de escopo

Rollback CA-1-004 (F1-T004) · F1-T008 · F1-T013 · publish · dado real.
