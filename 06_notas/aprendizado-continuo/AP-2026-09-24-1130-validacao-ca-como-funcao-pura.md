# AP-2026-09-24-1130 — Validação de CA como função pura na fixture

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: F1-T007 · SPEC-1-002
- Sinal: CA-1-007 (bloquear aprovação sem fonte/vigência ou conflitante) foi implementada como `canApproveRule` pura em `regras-fixture.ts`, separada da UI. Isso permitiu RED/GREEN observável no browser e revalidação por leitura de código sem depender do PocketBase (já instável).
- Evidência: `src/lib/regras-fixture.ts`; UI `/regras` v0.0.13; teste humano OK; revalidação independente.
- Regra reutilizável: quando o critério é uma regra de negócio de aceite/bloqueio, extrair função pura testável na camada de fixture/domínio e só depois plugar botões/logs na UI — especialmente se o backend gerenciado estiver indisponível.
- Quando aplicar: CAs de validação/estado (aprovar, rejeitar, idempotência simples) em tasks com entrada controlada.
- Quando não aplicar: fluxos que exigem persistência, RLS no servidor ou efeito colateral obrigatório no backend como parte do critério.
- Confiança: média-alta — padrão simples e comprovado nesta task; complementa AP-2026-09-24-1113.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto
