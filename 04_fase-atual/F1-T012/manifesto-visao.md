# Manifesto — visão e alertas internos F1-T012 (SPEC-1-003)

> **Task:** F1-T012 · **Dono formal:** Responsável pela superfície Ethos · **Coordenação:** Champion (JP/Iverson)
> **Gerado em:** 2026-09-24 · **Concluída em:** 2026-09-24T11:48-03:00
> **Regra:** nenhum dado real; fixtures F1-T001+006+011; sem alerta externo; sem alterar permissão global.

## 1. Objetivo

Configurar visão do contrato/período com filtros e alertas internos deduplicados
(CA-1-011 / CA-1-013 · RN-1.003-02/03/06).

## 2. Superfície

| Item | Valor |
|---|---|
| Projeto Skip | Travessia · id 60853 |
| Preview | https://travessia-8e0be--preview.goskip.app |
| Rota | `/visao` |
| Versão UI | 0.0.14 |
| Modo | entrada controlada (fixture agregada) |
| Produção | não publicada |

## 3. Artefatos Skip

| Artefato | Caminho |
|---|---|
| Fixture + filtros + dedupe | `src/lib/visao-fixture.ts` |
| UI | `src/pages/Visao.tsx` |
| Rota | `src/App.tsx` → `/visao` |
| Home | `src/pages/Index.tsx` (link) |

## 4. Conteúdo da visão

| ref | tipo | destaque |
|---|---|---|
| CTR-PILOTO-001 | contract | contrato/período |
| PEND-001 | pendency | aberta · due 2026-09-30 · cobertura incompleta |
| DEC-PILOTO-001 | decision | com dono financeiro |
| DEC-ORFA-001 | decision | órfã → escala champion |
| LOT-001 | lot | válido · cobertura completa |
| LOT-002 | lot | cobertura incompleta (não como OK) |
| IND-COBERTURA-PILOTO | indicator | CA-1-013 metadados + badge incompleta |

## 5. Evidência de aceite

- Teste humano do champion: **OK** (2026-09-24).
- Revalidação independente: rota, fixture, filtros, dedupe, cobertura incompleta, sem dado real — PASSOU.

## 6. Fora de escopo

F1-T013 (RLS auth/negado completo + audit), F1-T008 (veredito), publish, dado real, alerta externo.
