# AP-2026-09-24-1225 — Negação como tipo fechado sem payload

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: F1-T013 · SPEC-1-003
- Sinal: CA-1-012 / RN-1.003-01 exigem negação sem revelar conteúdo nem existência sensível. O tipo `AccessDenied = { allowed: false; code: 'FORBIDDEN' }` (sem outros campos) + `hasLeak` que falha se qualquer chave extra aparecer transformou o anti-vazamento em invariante de compilação/runtime e em prova JSON no teste humano.
- Evidência: `src/lib/rls-authorize.ts` AccessDenied/hasLeak/canAccess; UI `/rls` exibe JSON; bateria + teste humano OK; 0 LEAK.
- Regra reutilizável: para negações de autorização, modele o resultado negado como tipo fechado mínimo (código + allowed) e adicione um detector de vazamento que rejeita qualquer campo de conteúdo; exponha o JSON no gate humano.
- Quando aplicar: RLS, ACL, “existe mas você não pode ver”, APIs de probe.
- Quando não aplicar: quando o produto exige mensagem de erro rica ao usuário final (aí separar canal de UX do canal de autorização).
- Confiança: alta — alinha AP-1113, AP-1130 e AP-1208.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto
