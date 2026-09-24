# Verificação automática — F1-T002

**Data (inicial):** 2026-09-24  
**Revalidação pós-teste humano:** 2026-09-24T11:13-03:00  
**Skip project:** 60853 · Travessia  
**Versão UI observada:** 0.0.12

## Resultados (revalidação independente)

| Check | Status | Evidência |
|---|---|---|
| Frontend build / static / test | PASSOU | QA Skip nas builds até 0.0.12 |
| Rota `/piloto` | PASSOU | `skip_project_get` → path `/piloto` → Piloto |
| UI + fixture embutida | PASSOU | `src/pages/Piloto.tsx` + `src/lib/piloto-fixture.ts` |
| CTR-PILOTO-001 / LOT-001 / LOT-002 / docs / PEND-001 | PASSOU | presente na fixture; teste humano OK (modo entrada controlada) |
| Auth sintético (create-user + login) | PASSOU | botão UI + auth `users`; reteste B + OK final |
| Dado real ausente | PASSOU | refs @piloto.test; fixture sintética F1-T001 |
| Migrations PB aplicadas no Cloud | FALHOU (plataforma) | só coleção `users`; 0001–0003 não applied |
| Seed no PocketBase | FALHOU (dependente) | não observável sem migration |

## Desvio declarado (aceite como teto)

CA-1-001 demonstrado via **entrada controlada** (fixture F1-T001 embutida na UI) porque o Skip Cloud aborta apply de migrations (502/abort). Schema/seed PB permanece dívida de plataforma (`adapta-divida`), não bloqueia o critério binário da task com fixture mascarada.

## Veredito

**PASSOU com desvio declarado.** Teste humano aprovado; revalidação independente confirma UI/rota/fixture sintética e ausência de dado real. Backend PB não é gate desta conclusão.
