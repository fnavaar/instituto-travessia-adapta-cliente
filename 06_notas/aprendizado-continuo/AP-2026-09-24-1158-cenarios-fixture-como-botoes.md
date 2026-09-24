# AP-2026-09-24-1158 — Cenários da fixture como botões de prova

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: F1-T003 · SPEC-1-001
- Sinal: F1-T001 já descrevia `scenarios.duplicate` e `scenarios.corrected_version`. Em vez de pedir ao champion que monte o caso manualmente, a UI `/lotes` expôs **um botão por cenário** + form livre para RED. Isso reduziu ambiguidade no teste humano e alinhou a prova ao TDD RED/REFACTOR.
- Evidência: `src/pages/Lotes.tsx` botões 1–3; `lote-validate.ts`; fixture F1-T001 scenarios; teste humano OK.
- Regra reutilizável: quando a fixture da task anterior já nomeia cenários de borda, transformá-los em ações de um clique na UI de exercício — o form manual fica complementar, não o caminho principal da prova.
- Quando aplicar: tasks de borda/erro (duplicidade, rejeição, versionamento, rollback) com scenarios já documentados.
- Quando não aplicar: quando o critério exige interação livre do usuário ou dados que só o humano fornece na hora.
- Confiança: alta — padrão simples e comprovado nesta task.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto
