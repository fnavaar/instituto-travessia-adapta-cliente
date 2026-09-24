# Manifesto — decisão com comparativo F1-T008 (SPEC-1-002)

> **Task:** F1-T008 · **Dono formal:** Champion · **Coordenação:** Champion (JP/Iverson)
> **Gerado em:** 2026-09-24 · **Por:** agente do cliente (Adapta Cliente)
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
| Home | `src/pages/Index.tsx` (link) |

## 4. DEC-PILOTO-001

| Campo | Valor |
|---|---|
| pergunta | Qual valor de referencia usar no periodo piloto? |
| comparativo | VALOR-A (SRC-FIN-001, 1000) vs VALOR-B (SRC-FIN-002, 1150) |
| recommendation.label | `sugestao/inferencia` |
| owner / alçada | responsavel.financeiro@piloto.test / aprovador.financeiro@piloto.test |
| veredito inicial | `null` · state `aberta` |

## 5. Comportamentos

- Card **Sugestão/inferência** visualmente separado (borda tracejada + badge).
- Botão **Tentar usar sugestão como veredito** → **bloqueia** (sem justificativa/evidência; actor ≠ alçada).
- Form de veredito humano: só alçada + veredito + justificativa + evidência.
- Sugestão **não** muta `state` sozinha.

## 6. Fora de escopo

CA-1-009 (F1-T009) · reabertura CA-1-010 (F1-T010) · F1-T013 · publish · dado real · alçadas nominais (1E).

## 7. Teste humano sugerido

1. Hard refresh https://travessia-8e0be--preview.goskip.app/decisoes
2. Ver DEC-PILOTO-001: pergunta, contexto, related_records, comparativo A/B
3. Card sugestão com badge `sugestao/inferencia` **separado** do bloco veredito
4. Clicar **Tentar usar sugestão como veredito** → deve **bloquear**; estado permanece `aberta`
5. (Opcional) Registrar veredito humano com alçada + justificativa + evidência
6. Confirmar só @piloto.test
