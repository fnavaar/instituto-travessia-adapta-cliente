# Manifesto — regras registradas F1-T007 (SPEC-1-002)

> **Task:** F1-T007 · **Dono formal:** Responsável de domínio · **Coordenação:** Champion (JP/Iverson)
> **Gerado em:** 2026-09-24 · **Concluída em:** 2026-09-24T11:30-03:00
> **Regra:** nenhum dado real; fixture F1-T006; sem inventar fórmula/precedência (1F).

## 1. Objetivo

Registrar regras versionadas com fonte, vigência e estado de aprovação de forma
consultável (CA-1-006) e bloquear aprovação inválida (CA-1-007 / RN-1.002-01/02).

## 2. Superfície

| Item | Valor |
|---|---|
| Projeto Skip | Travessia · id 60853 |
| Preview | https://travessia-8e0be--preview.goskip.app |
| Rota | `/regras` |
| Versão UI | 0.0.13 |
| Modo | entrada controlada (fixture F1-T006 embutida) |
| Produção | não publicada |

## 3. Artefatos Skip

| Artefato | Caminho |
|---|---|
| Fixture + validação | `src/lib/regras-fixture.ts` (`canApproveRule`, `nextVersion`) |
| UI catálogo | `src/pages/Regras.tsx` |
| Rota | `src/App.tsx` → `/regras` |
| Home | `src/pages/Index.tsx` (link) |

## 4. Regras expostas

| rule_id | versão | estado | fonte/vigência | CA |
|---|---|---|---|---|
| RN-PILOTO-001 | r1 | aprovada | SRC-CONTRATO-001 v1 · 2026-01..2026-12 | CA-1-006 (detalhe completo) |
| RN-PILOTO-002 | r1 | pendente | ausente | CA-1-007 — aprovar bloqueado |
| RN-PILOTO-003 | r1 | conflitante | SRC-FIN-001 vs SRC-FIN-002 | CA-1-007 — aprovar bloqueado |

## 5. Comportamentos

- **Tentar aprovar:** usa `canApproveRule`; 002/003 gravam log `bloqueada` e **não** mudam state para `aprovada`.
- **Nova versão (RN-1.002-02):** a partir de aprovada, cria `rN+1` `pendente` e **preserva** `r1`.
- **Log:** trilha de aprovação/bloqueio na própria tela (sintético).

## 6. Evidência de aceite

- Teste humano do champion: **OK** (2026-09-24).
- Revalidação independente: rota, fixture, `canApproveRule`, sem dado real — PASSOU.
- Desvio: schema PB Cloud não applied (dívida F1-T002; fora do gate desta task).

## 7. Fora de escopo

DEC-PILOTO-001 / veredito (F1-T008), reabertura completa (F1-T010), alçadas nominais (1E), publish.
