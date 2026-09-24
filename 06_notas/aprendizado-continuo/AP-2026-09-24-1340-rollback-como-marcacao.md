# AP-2026-09-24-1340 — Rollback como marcação, nunca exclusão

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: F1-T004 · SPEC-1-001
- Sinal: CA-1-004 / RN-1.001-05 exigem reverter lote inválido sem apagar fonte, histórico ou log. A implementação usa `status: 'revertido'` + `is_current` na válida anterior via `map` (não `filter`/`splice`/`delete`). Contagens `lots_before`/`lots_after` e a prova 5b (sem motivo → erro sem mutação) tornaram a anti-destruição observável no gate humano.
- Evidência: `rollbackLot` em `lote-validate.ts`; UI cenários 4/5/5b; teste humano OK; revalidação independente.
- Regra reutilizável: em rollback/reversão de registro versionado, marque estado (`revertido`/`inativo`) e restaure ponteiro de apresentação; nunca remova o registro do store; exija motivo/ator; prove com contagem estável e tentativa inválida bloqueada.
- Quando aplicar: lotes, versões de regra/decisão, configurações de visão, qualquer histórico auditável.
- Quando não aplicar: limpeza GDPR com base legal de eliminação (aí o critério da SPEC/task deve dizer exclusão explícita).
- Confiança: alta — alinha AP-1158 (cenários-botão) e AP-1225 (tipo fechado de negação).
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto
