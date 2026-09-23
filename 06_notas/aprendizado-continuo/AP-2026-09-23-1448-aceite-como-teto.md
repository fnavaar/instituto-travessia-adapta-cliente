# AP-2026-09-23-1448 — Aceite é teto: não antecipar o insumo da próxima task

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: F1-T001 · SPEC-1-001
- Sinal: a F1-T001 pedia "pacote de pré-condições registrado" (manifesto, fontes, responsáveis, campos, matriz RLS, fixture). O plano inicial previa também criar coleções/hooks no Skip; durante a execução ficou claro que esse schema é o insumo de F1-T002 ("registrar contrato/período e lote controlado"), não um critério da F1-T001.
- Evidência: critério binário da F1-T001 em `04_fase-atual/fase.md`; seções `Limites e dependências` e `Fluxo e regras` da SPEC-1-001; verificação automática registrada em `.adapta-cliente/estado-atual.md`.
- Regra reutilizável: implementar exatamente o critério binário da task ativa; infraestrutura que só é exigida pelo critério de uma task posterior fica para essa task.
- Quando aplicar: sempre que o plano de execução incluir um componente cujo consumo/prova aparece apenas em task de onda seguinte.
- Quando não aplicar: quando o critério da task ativa exigir explicitamente o componente (ex.: task cujo critério é "existe coleção X com regra Y").
- Confiança: alta — o critério binário e a separação entre tasks foram lidos diretamente na fase e na SPEC.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto
