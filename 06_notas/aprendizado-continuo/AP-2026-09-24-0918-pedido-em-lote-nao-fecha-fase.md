# AP-2026-09-24-0918 — Pedido em lote não fecha fase nem remove portões

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: F1-T011 · SPEC-1-003 (padrão do método)
- Sinal: o champion pediu "termine toda a fase 1" com várias tasks restantes; o método exige uma task por ciclo, autorização pós-análise e teste humano pós-implementação.
- Evidência: 12 tasks ainda pendentes após Onda 1; estado `aguardando_teste_humano` na F1-T011; MEMORY/plugin Adapta (ritmo e portões).
- Regra reutilizável: pedido em lote autoriza no máximo fechar o gate pendente da task ativa; nunca implementar a fase inteira nem pular análise/teste.
- Quando aplicar: qualquer "faça tudo", "termine a fase", "pode seguir com o resto".
- Quando não aplicar: pedido explícito de uma única task nomeada com gate já cumprido.
- Confiança: alta — regra escrita no SkillMind e observada na sessão.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto
