# Manifesto de carga — F1-T002 (SPEC-1-001)

> **Task:** F1-T002 · **Dono formal:** Gestão de Contratos · **Coordenação:** Champion (JP/Iverson)
> **Gerado em:** 2026-09-24 · **Por:** agente do cliente (Adapta Cliente)
> **Regra:** nenhum dado real; fixture sintética F1-T001.

## 1. Objetivo

Registrar contrato/período piloto e lote controlado com proveniência na superfície Skip
(Travessia id 60853), de forma consultável (CA-1-001 / TDD GREEN).

## 2. Superfície

| Item | Valor |
|---|---|
| Projeto Skip | Travessia · id 60853 |
| Hostname Cloud | `travessia-8e0be` |
| Preview | https://travessia-8e0be--preview.goskip.app |
| Rota da central | `/piloto` |
| Publicado em produção | não |

## 3. Artefatos no Skip (working tree + versões 0.0.2–0.0.5)

| Artefato | Caminho |
|---|---|
| Schema | `pocketbase/migrations/0001_central_fontes_schema.js` |
| Seed F1-T001 + user teste | `pocketbase/migrations/0002_seed_piloto_f1t001.js` |
| Hook RN-1.001-02/03 | `pocketbase/hooks/lots_validate.js` |
| UI | `src/pages/Piloto.tsx` · rota em `src/App.tsx` |
| Home | `src/pages/Index.tsx` (link para `/piloto`) |

## 4. Coleções previstas

`contracts`, `periods`, `sources`, `lots`, `documents`, `pendencies`  
Campos alinhados ao manifesto F1-T001; deleteRule = null (app); list/view/create/update = autenticado.

## 5. Seed sintético

| Ref | Conteúdo |
|---|---|
| CTR-PILOTO-001 | contrato/período 2026-01..2026-12 |
| SRC-CONTRATO-001 / SRC-FIN-001 | fontes |
| LOT-001 | válido · quality ok |
| LOT-002 | com_pendencia · quality bloqueada |
| DOC-CONTRATO-001 / DOC-FIN-001 | documentos |
| PEND-001 | pendência aberta |
| champion@piloto.test | usuário de consulta sintético |

## 6. Verificação automática (observada)

| Etapa | Resultado |
|---|---|
| setup / static / build / test (Skip QA) | **passou** (versões 0.0.2–0.0.5) |
| Rota `/piloto` no preview | **presente** (`project_get` routes) |
| apply migrations Skip Cloud | **falhou** — abort / 502 Backend unavailable (várias tentativas) |
| Seed aplicado no Cloud | **não confirmado** (migrations pending ou Cloud indisponível) |

> Sem PASS de migration no Cloud, a consulta autenticada dos registros **pode falhar** até o backend aplicar 0001/0002. Código e UI estão versionados; o gate de dados no PB depende da recuperação do Skip Cloud.

## 7. Teste humano sugerido

1. Abrir https://travessia-8e0be--preview.goskip.app/piloto
2. Login sintético: `champion@piloto.test` / `Piloto@F1T002`
3. Confirmar CTR-PILOTO-001, LOT-001/002, docs e PEND-001
4. Se login/consulta falhar por migration pending ou 502, reportar — não inventar dado

## 8. Fora do escopo (próximas tasks)

F1-T003 (duplicidade/rollback completo), F1-T007 (regras), F1-T012 (visão gerencial), publish produção, dado real.
