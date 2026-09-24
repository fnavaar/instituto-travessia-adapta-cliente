# STATUS — Projeto Instituto Travessia

> **Atualizado em:** 2026-09-24 · **Por:** Agente do cliente (Adapta Cliente)
> O painel do projeto: fase atual, progresso e o que precisa de atenção.

## Onde estamos

- **Fase atual:** 1 — Amplificar: central da verdade e das decisões · aberta em 2026-08-24 · reunião de fechamento a agendar
- **Objetivo desta fase:** consultar uma visão única de dados, regras, pendências e decisões, usando dados controlados e RLS validado.
- **No prazo?** em risco — Onda 1 concluída; F1-T002 em teste humano com **bloqueio Skip Cloud** (migrations 502/abort).

## Progresso da fase

- **Tasks:** 3/15 concluídas (20%) · 1/15 em teste humano (F1-T002)
- **Ativa:** F1-T002 — UI `/piloto` e migrations no working tree; apply no Cloud **não confirmado** (backend unavailable).

## Travas ativas

| Trava | Desde | Quem resolve | Ação em curso |
|---|---|---|---|
| BLOQUEIO-1A — contrato/período piloto real | 2026-09-23 | Champion | Nomear contrato e período reais; fixture sintética em uso até lá. |
| BLOQUEIO-1B — responsáveis e amostra mascarada | 2026-09-23 | Gestão de Contratos / DP-RH / Financeiro | Confirmar responsáveis nominais e entregar amostra mascarada. |
| BLOQUEIO-1C — aprovação da matriz RLS | 2026-09-23 | Segurança/qualidade | Aprovar `04_fase-atual/F1-T011/matriz-rls-completa.md`. |
| BLOQUEIO-1D — regras e fontes iniciais | 2026-09-23 | Financeiro / DP-RH / Gestão de Contratos | Indicar as regras e fontes iniciais do piloto. |
| BLOQUEIO-1E — alçadas e aprovadores | 2026-09-23 | Champion | Confirmar alçadas e aprovadores nominais (`F1-T006/matriz-alcadas.md`). |
| BLOQUEIO-1F — fórmulas/precedência não fornecidas | 2026-09-23 | Responsáveis de domínio | Permanecem `pendente`; nunca inventar cálculo. |
| BLOQUEIO-1G — identidades nominais de teste | 2026-09-24 | Segurança/qualidade + champion | Confirmar identidades em `F1-T011/identidades-teste.md`. |
| BLOQUEIO-1H — retenção/exportação/campos sensíveis | 2026-09-24 | Segurança/qualidade | Aprovar `F1-T011/politica-dados.md`. |
| BLOQUEIO-1I — aceite fixtures auth/negado | 2026-09-24 | Segurança/qualidade | Aceitar `F1-T011/fixture-rls.json` formalmente. |
| Skip Cloud migrations F1-T002 | 2026-09-24 | Plataforma Skip / agente | Reaplicar 0001/0002 quando Cloud sair de 502/abort. |

## Entregas concluídas

| Fase | O que foi entregue | Fechada em |
|---|---|---|
| 1 | F1-T001 — pacote de pré-condições da central | 2026-09-23 |
| 1 | F1-T006 — fixture de regras/decisões e matriz de alçadas | 2026-09-23 |
| 1 | F1-T011 — matriz RLS, identidades, fixtures auth/negado e política de dados | 2026-09-24 |
| 1 | Central operacional, catálogo de regras/decisões, visão inicial e RLS em dados controlados | Em execução |

## Próxima reunião

A agendar — demonstrar as tasks concluídas da Fase 1, incluindo proveniência, decisões, pendências, RLS e recuperação.
