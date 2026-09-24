# Verificação automática — F1-T012

**Data:** 2026-09-24T11:45-03:00  
**Skip project:** 60853 · Travessia · UI **0.0.14**

## Resultados

| Check | Status | Evidência |
|---|---|---|
| setup / static / build / test / integrations | PASSOU | QA Skip apply 0.0.14 (ok:true) |
| Rota `/visao` | PASSOU | `skip_project_get` → Visao |
| Fixture agregada (PEND, DEC, DEC-ORFA, LOT-002, indicador) | PASSOU | `src/lib/visao-fixture.ts` |
| Filtros contrato/período/área/owner/status | PASSOU | `applyVisionFilters` |
| Recorte por papel + negado vazio | PASSOU | `filterItemsByRole` |
| `upsertAlert` dedupe object\|state\|due | PASSOU | código |
| Cobertura incompleta rotulada (não como completa) | PASSOU | items + UI badge |
| Sem alerta externo | PASSOU | só painel local |
| Dado real ausente | PASSOU | @piloto.test |
| PB coleções domínio | N/A (fixture) | só `users` — fora do gate |

## Veredito parcial

**Código e UI entregues; gate = teste humano em /visao.**
