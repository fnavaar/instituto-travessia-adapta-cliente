# Verificação automática — F1-T012

**Data (implementação):** 2026-09-24T11:45-03:00  
**Revalidação pós-teste humano:** 2026-09-24T11:48-03:00  
**Skip project:** 60853 · Travessia · UI **0.0.14**

## Resultados (revalidação independente)

| Check | Status | Evidência |
|---|---|---|
| setup / static / build / test / integrations | PASSOU | QA Skip 0.0.14 ok:true |
| Rota `/visao` | PASSOU | `skip_project_get` → Visao |
| Fixture PEND + DEC + DEC-ORFA + LOT-002 + indicador | PASSOU | `src/lib/visao-fixture.ts` |
| Filtros contrato/período/área/owner/status | PASSOU | `applyVisionFilters` |
| Papel negado → vazio | PASSOU | `filterItemsByRole` |
| `upsertAlert` dedupe object\|state\|due | PASSOU | código |
| Cobertura incompleta rotulada (CA-1-013) | PASSOU | items + badge |
| Decisão órfã escala champion (RN-1.003-03) | PASSOU | DEC-ORFA-001 |
| Sem alerta externo | PASSOU | painel local only |
| Dado real ausente | PASSOU | @piloto.test |
| Teste humano | PASSOU | champion OK 2026-09-24 |

## Veredito

**PASSOU.** CA-1-011/013 e RN-1.003-02/03/06 demonstrados via entrada controlada. Onda 2 completa.
