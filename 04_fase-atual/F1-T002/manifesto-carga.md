# Manifesto de carga — F1-T002 (SPEC-1-001)

> **Task:** F1-T002 · **Dono formal:** Gestão de Contratos · **Coordenação:** Champion (JP/Iverson)
> **Gerado em:** 2026-09-24 · **Concluída em:** 2026-09-24T11:13-03:00
> **Regra:** nenhum dado real; fixture sintética F1-T001.

## 1. Objetivo

Registrar contrato/período piloto e lote controlado com proveniência na superfície Skip
(Travessia id 60853), consultável (CA-1-001 / TDD GREEN).

## 2. Superfície

| Item | Valor |
|---|---|
| Projeto Skip | Travessia · id 60853 |
| Hostname Cloud | `travessia-8e0be` |
| Preview | https://travessia-8e0be--preview.goskip.app |
| Rota da central | `/piloto` |
| Versão UI | 0.0.12 |
| Publicado em produção | não |

## 3. Artefatos no Skip

| Artefato | Caminho |
|---|---|
| Schema (fatiado; Cloud pending) | `pocketbase/migrations/0001_central_fontes_schema.js` |
| Seed user (Cloud pending) | `pocketbase/migrations/0002_seed_user_piloto.js` |
| Seed contrato (Cloud pending) | `pocketbase/migrations/0003_seed_piloto_dados.js` |
| Fixture embutida (entrada controlada) | `src/lib/piloto-fixture.ts` |
| UI | `src/pages/Piloto.tsx` · rota em `src/App.tsx` |

## 4. Modo de demonstração (aceite)

**Modo fixture (entrada controlada):** quando PB não tem coleções de domínio, a UI carrega
CTR-PILOTO-001, fontes, LOT-001/002, documentos e PEND-001 a partir de `piloto-fixture.ts`,
com badge `modo: entrada controlada (fixture F1-T001)`.

**Modo PocketBase (futuro):** quando Cloud aplicar migrations, a mesma UI prefere registros PB.

## 5. Seed sintético exibido

| Ref | Conteúdo |
|---|---|
| CTR-PILOTO-001 | contrato/período 2026-01..2026-12 |
| SRC-CONTRATO-001 / SRC-FIN-001 | fontes |
| LOT-001 | válido · quality ok |
| LOT-002 | com_pendencia · quality bloqueada |
| DOC-CONTRATO-001 / DOC-FIN-001 | documentos |
| PEND-001 | pendência aberta |
| champion@piloto.test | usuário de consulta sintético |

## 6. Evidência de aceite

- Teste humano do champion: **OK** (2026-09-24) — viu contrato/lotes no modo fixture.
- Revalidação independente: rota, fixture, UI, sem dado real — PASSOU.
- Desvio: schema/seed PB não applied no Cloud (dívida plataforma).

## 7. Fora do escopo

F1-T003 (duplicidade/rollback), F1-T007, F1-T012, publish produção, dado real.
