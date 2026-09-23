# AP-2026-09-23-1652 — Links diretos e arquivos no chat no portão de teste humano

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: F1-T006 · SPEC-1-002 (padrão observado também em F1-T001)
- Sinal: após a F1-T001 o champion não encontrou os artefatos no GitHub; a localização em pasta nova e a ausência de links diretos no roteiro de teste atrasaram o portão humano.
- Evidência: mensagem "não encontrei essas informações" na F1-T001; resolução com links da pasta `04_fase-atual/F1-T00X/` e entrega dos arquivos no chat; na F1-T006 os artefatos já foram enviados no mesmo turno da implementação.
- Regra reutilizável: no portão de teste humano de task de pacote/documentação, incluir sempre (1) caminho no repo, (2) links diretos main e (3) os arquivos no chat.
- Quando aplicar: toda task cujo resultado é arquivo no repo operacional (manifesto, fixture, matriz).
- Quando não aplicar: task cujo resultado é só comportamento na UI do Skip, sem artefato de handoff.
- Confiança: alta — falha observada e corrigida na F1-T001; prevenção aplicada na F1-T006.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto
