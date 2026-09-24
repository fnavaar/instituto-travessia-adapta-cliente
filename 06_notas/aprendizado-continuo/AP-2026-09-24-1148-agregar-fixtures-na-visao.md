# AP-2026-09-24-1148 — Agregar fixtures de ondas anteriores na visão

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: F1-T012 · SPEC-1-003
- Sinal: a visão gerencial precisava de pendência (F1-T001), decisão com/sem dono (F1-T006/F1-T011) e cobertura incompleta (F1-T011) ao mesmo tempo. Em vez de recriar dados, `visao-fixture.ts` agregou as três fixtures já aceitas e expôs filtros/alertas em cima delas.
- Evidência: `src/lib/visao-fixture.ts`; UI `/visao` v0.0.14; teste humano OK; revalidação independente.
- Regra reutilizável: quando a task de visão/painel depende de insumos de tasks anteriores já concluídas, compor uma fixture agregada referenciando os object_refs existentes — não inventar novos registros nem esperar o backend unificado.
- Quando aplicar: tasks de “visão”, “dashboard” ou “fila” após pacotes de fixture de onda anterior; backend indisponível ou em modo entrada controlada.
- Quando não aplicar: quando o critério exige persistência única ou join real no servidor como prova.
- Confiança: alta — padrão direto e comprovado; alinha AP-1113 e AP-1130.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto
