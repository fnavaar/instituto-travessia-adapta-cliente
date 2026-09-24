# AP-2026-09-24-1208 — Botão de prova: sugestão não fecha sozinha

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: F1-T008 · SPEC-1-002
- Sinal: RN-1.002-04 exige que sugestão/inferência não mude estado sozinha. Além do layout separado, a UI expôs o botão **Tentar usar sugestão como veredito** que chama `applyVerdict` com actor inválido e campos vazios — falha observável e logada. Isso transforma a invariante em prova clicável no teste humano.
- Evidência: `Decisoes.tsx` handleApplySuggestionAsVerdict; `applyVerdict` + `suggestionCannotClose`; teste humano OK.
- Regra reutilizável: quando a SPEC proíbe que um artefato (sugestão, auto-aprovação, estimativa) altere estado sozinho, além de não ligar o caminho feliz, ofereça um controle de **tentativa proibida** que demonstra o bloqueio no gate humano.
- Quando aplicar: CAs de separação sugestão/veredito, não-autoaprovação, não-publicação de incompleto.
- Quando não aplicar: quando o bloqueio só existe no servidor e a UI não pode simular a tentativa com segurança.
- Confiança: alta — alinha AP-1130 e AP-1158.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto
