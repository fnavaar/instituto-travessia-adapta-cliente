# Manifesto de regras e decisões — F1-T006 (SPEC-1-002)

> **Task:** F1-T006 · **Dono:** Champion (JP/Iverson - Champions)
> **Fase:** 1 — Amplificar · **SPEC:** SPEC-1-002 Catálogo inicial de regras e central de decisões
> **Gerado em:** 2026-09-23 · **Por:** agente do cliente (Adapta Cliente)
> **Regra cumprida:** nenhuma fórmula, rateio, arredondamento, precedência ou vigência foi
> inventado. Tudo é sintético e o que não foi fornecido permanece `pendente`.

## 1. Objetivo e resultado observável

Preparar os insumos do catálogo: uma regra aprovada, uma pendente, uma conflitante, uma decisão
comparável e os aprovadores de teste identificados — base para F1-T007 (regra versionada) e
F1-T008 (decisão com comparativo).

## 2. Insumos entregues

| Insumo | Onde | Situação |
|---|---|---|
| Fixture de regras/decisões | `fixture-regras.json` | sintética |
| Matriz de alçadas | `matriz-alcadas.md` | proposta (BLOQUEIO-1E) |
| Aprovadores de teste | `fixture-regras.json` → `test_approvers` | identificados (de teste) |
| Vínculo com a central | contrato/período e fontes da F1-T001 | referenciado |

## 3. Regras da fixture

| ID | Nome | Estado | Fonte/vigência | Observação |
|---|---|---|---|---|
| RN-PILOTO-001 | Vigência contratual mínima | `aprovada` | SRC-CONTRATO-001 v1 · 2026-01..2026-12 | regra fornecida (sintética) |
| RN-PILOTO-002 | Regra sem fonte declarada | `pendente` | ausente | não aprovável (RN-1.002-01) |
| RN-PILOTO-003 | Regra com fontes conflitantes | `conflitante` | SRC-FIN-001 vs SRC-FIN-002 | precedência não fornecida (1F) |

## 4. Decisão comparável

- **ID:** `DEC-PILOTO-001` · **Estado:** `aberta`
- **Comparativo:** VALOR-A (SRC-FIN-001) × VALOR-B (SRC-FIN-002)
- **Sugestão:** rotulada `sugestao/inferencia`, com fonte, confiança e limites — **não** altera estado.
- **Dono:** `responsavel.financeiro@piloto.test` · **Alçada:** `aprovador.financeiro@piloto.test`
- **Veredito/justificativa/evidência:** ausentes por desenho (para exercitar CA-1-009 na F1-T009).

## 5. Bloqueios registrados

| Bloqueio | Descrição | Quem resolve |
|---|---|---|
| BLOQUEIO-1D | Regras e fontes iniciais do piloto | Financeiro / DP-RH / Gestão de Contratos |
| BLOQUEIO-1E | Alçadas e aprovadores nominais | Champion |
| BLOQUEIO-1F | Fórmula, rateio, arredondamento ou precedência não fornecidos ficam `pendente` | Responsáveis de domínio (nunca inventar) |

## 6. Caminhos de erro habilitados

fonte ausente · fonte conflitante · vigência fora do período · alçada inexistente · decisão sem
evidência · tentativa de autoaprovação · reabertura sem motivo · RLS negado.

## 7. Evidências

| Evidência | Onde |
|---|---|
| Fixture de regras/decisões | `fixture-regras.json` |
| Matriz de alçadas | `matriz-alcadas.md` |
| Aprovadores de teste | `fixture-regras.json` → `test_approvers` |
| Aceite humano do champion | **pendente** (portão de teste humano) |

## 8. Handoff

- **Para F1-T007:** registrar as regras versionadas com fonte, vigência e estado de aprovação.
- **Para F1-T008:** registrar a decisão com comparativo e sugestão separada do veredito.
