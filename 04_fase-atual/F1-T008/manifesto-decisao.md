# Manifesto — decisão com comparativo F1-T008 (SPEC-1-002)

> **Task:** F1-T008 · **Dono formal:** Champion · **Coordenação:** Champion (JP/Iverson)
> **Gerado em:** 2026-09-24 · **Concluída em:** 2026-09-24T12:08-03:00
> **Regra:** nenhum dado real; fixture F1-T006; sugestão ≠ veredito (RN-1.002-04).

## 1. Objetivo

Registrar/exibir decisão com comparativo e sugestão/inferência **separada** do veredito
humano (CA-1-008 · RN-1.002-04 · TDD GREEN).

## 2. Superfície

| Item | Valor |
|---|---|
| Projeto Skip | Travessia · id 60853 |
| Preview | https://travessia-8e0be--preview.goskip.app |
| Rota | `/decisoes` |
| Versão UI | 0.0.18 |
| Modo | entrada controlada (fixture F1-T006) |
| Produção | não publicada |

## 3. Artefatos Skip

| Artefato | Caminho |
|---|---|
| Fixture + applyVerdict | `src/lib/decisao-fixture.ts` |
| UI | `src/pages/Decisoes.tsx` |
| Rota | `src/App.tsx` → `/decisoes` |

## 4. Evidência de aceite

- Teste humano do champion: **OK** (2026-09-24).
- Revalidação independente: rota, DEC-PILOTO-001, separação sugestão/veredito, bloqueio auto-sugestão, sem dado real — PASSOU.

## 5. Fora de escopo

CA-1-009 (F1-T009) · reabertura CA-1-010 (F1-T010) · F1-T013 · publish · dado real.
