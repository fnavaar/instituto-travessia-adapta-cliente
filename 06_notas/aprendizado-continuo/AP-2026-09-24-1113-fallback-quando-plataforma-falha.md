# AP-2026-09-24-1113 — Fallback de entrada controlada quando a plataforma falha

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: F1-T002 · SPEC-1-001
- Sinal: Skip Cloud abortou apply de migrations (502/pending) e deixou o PB só com `users`. O critério CA-1-001 pede consulta de contrato/lote com proveniência em fixture mascarada/entrada controlada — não exige que o backend gerenciado esteja saudável. UI com fallback para fixture F1-T001 desbloqueou o teste humano sem inventar schema paralelo nem dado real.
- Evidência: migrations listadas pending/abort; coleções só `users`; reteste B (login OK, lista vazia); correção v0.0.12 `piloto-fixture.ts` + badge modo fixture; teste humano OK; revalidação independente.
- Regra reutilizável: se a prova da task admite fixture/entrada controlada e a plataforma externa impede o caminho ideal, entregar o caminho controlado observável, declarar o desvio/dívida e não prender o gate humano à recuperação da plataforma.
- Quando aplicar: falha de Cloud/migração/API de terceiros que não faz parte do critério binário da task; SPEC já prevê fixture mascarada.
- Quando não aplicar: quando o critério exige explicitamente persistência real, RLS no backend ou integração live; aí a task fica bloqueada/em correção.
- Confiança: alta — causa e correção observáveis; aceite humano no modo fixture.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto
