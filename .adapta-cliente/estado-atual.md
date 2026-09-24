# Estado atual — Adapta Cliente

- task_id: F1-T002
- champion: JP/Iverson - Champions
- spec: 04_fase-atual/specs/spec-1-001-central-de-fontes-e-piloto.md
- etapa: aguardando_teste_humano
- autorizacao_implementacao: confirmada 2026-09-24T09:32-03:00 · "Autorizo implementar a F1-T002 conforme o plano"
- teste_humano: falhou 2026-09-24T10:18-03:00 · "login falhou" — correção em curso/reteste
- verificacao_automatica: parcial · UI v0.0.8+ com botão "Criar usuário de teste e entrar"; build OK; migrations Cloud ainda pending/abort (coleções só users); probe auth 400 (user inexistente); create user API 502 intermitente
- aprendizado: pendente
- ultima_acao: debug login — causa = seed/user não no Cloud; UI bypass create+login publicado (0.0.8); schema/seed ainda pending no PB
- proxima_acao: reteste humano em /piloto com botão "Criar usuário de teste e entrar"; se login OK mas sem CTR-PILOTO-001, reportar (migrations domínio)
- atualizado_em: 2026-09-24T10:30-03:00
- nota_dono: dono formal = Gestão de Contratos; champion coordena
