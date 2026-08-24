# SPEC-1-002 — Catálogo inicial de regras e central de decisões

**Fase:** 1  
**Status:** planejada  
**Dono:** Champion, com validação de Financeiro, DP/RH e Gestão de Contratos  
**Origem no escopo:** D-002, D-004, D-005, D-007; C-002, C-003, C-007; RQ-006, RQ-007, RQ-009, RQ-010  
**Degrau da solução:** nativo da plataforma — registrar regras, parâmetros, decisões, evidências e estados na mesma superfície autorizada da central; não introduzir motor de cálculo nem inferência que não esteja no escopo da Fase 1.

## Contexto e decisões fechadas

- **Estado atual:** regras de contrato, cálculo, fonte de dados, margem/DRE e decisões ficam dispersas em documentos, planilhas e conversas. A análise crítica identificou fonte e fórmulas financeiras não confirmadas.
- **Estado desejado:** cada regra e decisão do piloto possui identificador, fonte, vigência, versão, responsável, alçada, estado, evidência, exceções e próxima ação. Sugestão/inferência fica separada da decisão humana.
- **Decisões já fechadas:** regra não aprovada não pode ser usada como fato; decisão não pode ser encerrada sem dono, alçada, justificativa e evidência; conflito ou ausência de fonte abre pendência; alteração relevante cria nova versão; margem/DRE será medida a partir de implementação, mas seus cálculos não serão inventados nesta fase.
- **Bloqueios:** **BLOQUEIO-1D:** Financeiro/DP-RH/Gestão de Contratos devem indicar as regras e fontes iniciais do piloto. **BLOQUEIO-1E:** o champion deve confirmar alçadas e aprovadores. **BLOQUEIO-1F:** qualquer fórmula, rateio, arredondamento ou precedência não fornecida deve permanecer `pendente`, sem cálculo automático.

## Resultado observável

Para um contrato/período piloto, o usuário consegue abrir uma regra ou decisão, ver a fonte e a versão usada, comparar opções/valores disponíveis, identificar responsável e alçada, registrar veredito humano, justificativa, evidência, prazo e próxima ação. Uma decisão sem fonte, dono, alçada ou evidência permanece aberta/devolvida e não libera o fluxo.

## Limites e dependências

- **Inclui:** catálogo inicial de regras de contrato e processo; parâmetros recebidos do cliente; vínculo regra–fonte–período–contrato; versão e vigência; exceções; central de decisões; comparativos; recomendação/inferência identificada; decisão humana; estados; prazo; responsável; evidência; reabertura e histórico.
- **Fora de escopo:** definir regra trabalhista ou contratual pelo agente; interpretar cláusula; calcular VR/VT, folha, memória, DRE ou margem; aprovar financeiro; decidir aptidão legal; executar ação externa; substituir documentos oficiais.
- **Entradas e pré-condições:** registros da SPEC-1-001; regras e documentos-fonte fornecidos por responsáveis; alçadas confirmadas; período/contrato piloto; fixture com uma regra aprovada, uma regra pendente e uma decisão com divergência conhecida.
- **Saídas/artefatos:** regras versionadas; decisões com estados; comparativo; trilha de aprovação/devolução/reabertura; pendências; vínculos entre regra, fonte, registro e evidência; relatório de decisões sem dono/alçada.
- **Dependências e responsáveis:** Financeiro valida regras de receita/custo/margem; DP/RH valida regras de dados de pessoal e ocorrência; Gestão de Contratos valida vigência e obrigação; champion coordena e não substitui a alçada de cada domínio.
- **Atores e permissões mínimas:** responsável do domínio pode propor e revisar regras do próprio recorte; aprovador autorizado pode aprovar/reprovar regra e decisão; champion acompanha e encaminha; direção consulta e decide somente dentro da alçada; executor não pode transformar sugestão em decisão.
- **Superfícies/arquivos/configurações afetadas:** catálogo e central no ambiente Ethos/cliente autorizado; vínculos aos registros da SPEC-1-001; evidências e logs. Não editar fonte original nem reescrever decisões históricas.
- **Risco e plano B:** fonte conflitante, regra incompleta, alçada desconhecida ou inferência tratada como fato. Marcar `aguardando fonte`/`devolvido`, mostrar ambas as versões, encaminhar ao responsável e manter fila manual.
- **Rollback ou reversão:** reabrir regra/decisão e voltar à última versão aprovada sem apagar o veredito anterior; desassociar apenas a recomendação não aprovada; preservar a evidência e o motivo da reversão.

## Dados e integrações

| Origem/destino | Fonte de verdade | Campos/contrato | Autenticação/permissão | Timeout/retry/idempotência | Tratamento de erro |
|---|---|---|---|---|---|
| Documento/contrato/relatório → regra | fonte aprovada associada ao lote | `rule_id`, nome, tipo, descrição/formula fornecida, vigência, escopo, fonte, versão, responsável, aprovação, exceção | leitura do documento conforme RLS; só aprovador grava aprovação | criação idempotente por `rule_id + version`; nova versão não sobrescreve anterior | fonte ausente/conflitante vira pendência |
| Regras e registros do piloto → decisão | registros da central de fontes e catálogo | `decision_id`, pergunta, contexto, registros relacionados, comparativo, opções, recomendação, responsável, alçada, veredito, justificativa, evidência, estado, prazo, próxima ação | consulta conforme contrato/área; veredito somente pela alçada | atualizar por versão e estado; reenvio do mesmo veredito não duplica evento | impedir encerramento e mostrar campo faltante |
| Decisão → painel/pendência | central de decisões | estado, dono, prazo, risco, próxima ação, evidência | painel respeita RLS; sem exportação externa nesta SPEC | atualização transacional; alerta duplicado deve ser deduplicado por decisão/estado/prazo | decisão sem dono fica em fila crítica |

| Regra de negócio | Condição | Ação/resultado | Exceção | Fonte |
|---|---|---|---|---|
| RN-1.002-01 | Regra tem fonte, vigência, escopo, responsável e versão | permitir revisão; somente aprovador pode mudar para `aprovada` | documento conflitante mantém regra `pendente` e abre decisão | C-002; D-004 |
| RN-1.002-02 | Regra aprovada é alterada | criar nova versão, preservar a anterior e exigir nova aprovação | correção editorial sem mudança de significado pode registrar justificativa, sem apagar histórico | D-004; RQ-010 |
| RN-1.002-03 | Decisão não tem dono, alçada, evidência ou justificativa | impedir estado final e abrir pendência | champion pode encaminhar, não aprovar por substituição | C-003; D-004 |
| RN-1.002-04 | Sistema gera sugestão ou inferência | exibir rótulo `sugestão/inferência`, fonte, confiança/limitação e opções; aguardar veredito humano | nenhuma sugestão pode mudar estado sozinha | D-002; C-003 |
| RN-1.002-05 | Fonte está ausente ou conflitante | estado `aguardando fonte` ou `com divergência`; não preencher valor | responsável pode anexar nova evidência e reabrir | AC-001; D-004 |
| RN-1.002-06 | Decisão aprovada é reaberta | preservar aprovação anterior, exigir motivo, novo responsável e nova evidência | reabertura fora da alçada é negada e auditada | RQ-007; D-004 |

## Fluxo e regras

1. O responsável cria uma regra com identificador, descrição, fonte, vigência, escopo, versão, exceções e responsável.
2. O revisor do domínio confere se a regra está suportada pela fonte e marca `pendente`, `aprovada` ou `devolvida`; o executor não escolhe fórmula.
3. O usuário cria uma decisão vinculando contrato/período, regras, registros de fonte, pergunta, comparativo, opções, responsável e alçada.
4. O sistema apresenta fatos/valores de origem separados de sugestão/inferência e mantém os registros consultáveis.
5. O aprovador registra veredito, justificativa, evidência, prazo e próxima ação; sem esses campos a decisão não encerra.
6. Divergência, ausência de fonte ou conflito de alçada abre pendência e encaminha ao dono; não é resolvida silenciosamente.
7. Alteração de regra ou decisão cria versão/reabertura, preserva histórico e atualiza o painel da SPEC-1-003.

| Cenário | Dado/condição | Resultado esperado | Caminho de erro/recuperação |
|---|---|---|---|
| Principal | regra fornecida e decisão com comparativo, dono, alçada e evidência | decisão passa por análise e é encerrada pelo aprovador correto; vínculo e histórico ficam recuperáveis | se faltar campo, impedir encerramento e criar pendência |
| Limite | regra tem descrição, mas não tem fonte ou vigência | regra fica `pendente`; nenhuma decisão pode usá-la como aprovada | encaminhar ao responsável e anexar fonte em nova versão |
| Divergência | duas fontes apresentam valores/regras diferentes | decisão mostra ambas, identifica conflito e exige veredito humano | estado `com divergência`; não escolher precedência automaticamente |
| Sugestão | cruzamento aponta uma opção provável | sugestão aparece separada, com fonte e limitação; estado permanece aberto | se a fonte não tiver cobertura, marcar `aguardando fonte` |
| Reabertura | decisão aprovada precisa ser alterada | nova versão/estado reaberto com motivo, autor e nova aprovação | tentativa sem alçada é bloqueada e registrada |

## Instruções de execução para o Ethos

1. **Ler antes de alterar:** `03-Projeto/02-Escopo-Definitivo.md`, seções 5, 6, 8/Fase 1 e 11; `03-Projeto/requisitos.md`, RQ-006, RQ-007, RQ-009 e RQ-010; `03-Projeto/02-Plano_de_acao/01.Fase_1/01-SPECs/spec-1-001-central-de-fontes-e-piloto.md`; esta SPEC; documentos e regras fornecidos pelos responsáveis.
2. **Alterar somente:** catálogo inicial, decisões do piloto, vínculos, estados, evidências e logs na superfície autorizada.
3. **Não alterar:** regras não fornecidas; fórmulas financeiras; fontes originais; alçadas; decisões de outros domínios; conectores e ações externas.
4. **Executar nesta ordem:** confirmar fontes e alçadas → cadastrar regra aprovada e pendente → criar decisão com comparativo → testar sugestão separada → registrar veredito humano → testar divergência e reabertura → registrar evidências.
5. **Parar e pedir validação quando:** fonte, fórmula, precedência, alçada, responsável, RLS ou significado do campo estiver ausente; houver pedido para aprovar automaticamente; houver decisão com impacto legal/financeiro fora da alçada registrada.
6. **Estado válido ao parar:** regra não confirmada continua pendente; decisão não encerrada permanece com dono e próxima ação; última versão aprovada e todos os vereditos anteriores continuam auditáveis.

## Checklist de execução

- [ ] Regras e fontes iniciais do piloto foram entregues pelos responsáveis.
- [ ] Vigência, escopo, versão, exceções e aprovadores foram confirmados.
- [ ] Regra aprovada, regra pendente e regra conflitante foram cadastradas sem invenção.
- [ ] Decisão de teste contém pergunta, contexto, comparativo, opções, dono, alçada e evidência.
- [ ] Sugestão/inferência aparece separada do veredito humano.
- [ ] Estados de pendência, devolução, aprovação e reabertura foram exercitados.
- [ ] Evidências e handoff para a visão da SPEC-1-003 foram registrados.

## Critérios de aceite

- [ ] **CA-1-006:** uma regra do piloto exibe fonte, versão, vigência, escopo, responsável, estado, exceções e aprovação sem depender de memória externa.
- [ ] **CA-1-007:** uma regra sem fonte/vigência ou com conflito não pode ser marcada como aprovada.
- [ ] **CA-1-008:** uma decisão do piloto exibe dados de origem, comparativo, opções, sugestão/inferência separada, dono, alçada, veredito, justificativa, evidência, prazo e próxima ação.
- [ ] **CA-1-009:** decisão sem dono, alçada ou evidência não encerra; permanece pendente/devolvida e aparece para o responsável.
- [ ] **CA-1-010:** alteração ou reabertura preserva a versão anterior, registra motivo e exige novo veredito pela alçada correta.

## TDD da SPEC

| Etapa | Prova | Comando/ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | tentar aprovar regra sem fonte e encerrar decisão sem dono/evidência | submeter registros incompletos pela superfície autorizada | operações bloqueadas; campos faltantes e pendências visíveis | log de validação e captura do estado |
| GREEN | cadastrar regra fornecida, decisão com comparativo e veredito humano | relacionar fonte da SPEC-1-001, registrar decisão e aprovar pela alçada | regra e decisão ficam consultáveis com todos os vínculos e evidências | registro exportado/captura + aceite do responsável |
| REFACTOR/REGRESSÃO | gerar sugestão, inserir conflito, alterar versão e reabrir decisão | repetir fluxo com fonte conflitante e depois nova versão | sugestão não aprova; conflito bloqueia; histórico e reabertura preservados; sem decisão órfã | trilha de eventos, estados e relatório de pendências |

**Dados/fixtures:** uma regra de vigência contratual ou regra financeira fornecida pelo responsável; uma regra sem fonte; duas fontes conflitantes; uma decisão com dois valores comparáveis; uma recomendação de teste explicitamente marcada como sugestão; aprovadores de teste definidos.

**Caminhos de erro obrigatórios:** fonte ausente, fonte conflitante, vigência fora do período, alçada inexistente, decisão sem evidência, tentativa de autoaprovação, reabertura sem motivo e RLS negado.

**Evidência exigida:** registros versionados de regra e decisão; fonte associada; logs de aprovação/devolução/reabertura; visão de sugestões versus vereditos; aceite humano dos responsáveis.

## Handoff e operação

- **Como demonstrar:** abrir regra aprovada e regra pendente → criar decisão com comparativo → mostrar sugestão separada → registrar veredito → reabrir com motivo → localizar a pendência no painel.
- **Como operar depois:** cada responsável de domínio mantém suas regras; Financeiro/DP-RH/Gestão de Contratos aprovam somente suas alçadas; champion acompanha decisões e prazos.
- **Como monitorar:** regras sem fonte/vigência, decisões sem dono/alçada, pendências vencidas, reaberturas, conflitos de fonte e sugestões sem veredito.
- **Pendência conhecida:** regras, fórmulas, precedência, alçadas e aprovadores nominais do piloto ainda precisam ser confirmados; enquanto isso, nenhum valor é tratado como calculado ou aprovado.

## Tasks vinculadas

| Onda | ID | Task | Dono | SPEC | Subseção da SPEC | Critério binário | Recorte da prova | Evidência esperada | Pré-condições | Ponto de parada | Status |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | F1-T006 | Preparar fixture de regras/decisões e matriz de alçadas | Champion | SPEC-1-002 | `Limites e dependências`; `Instruções de execução`, itens 1 e 5; checklist | Há uma regra aprovada, uma pendente, uma conflitante, uma decisão comparável e aprovadores de teste identificados | Fixtures de regra e decisão descritas em `Dados/fixtures` e RN-1.002-01 | Pacote de fontes, regras, alçadas e aprovadores; nenhuma fórmula inventada | Escopo aprovado; fontes do domínio disponíveis; contrato/período pode estar definido ou deve ser registrado como bloqueio, sem depender de F1-T001 | Parar se fonte, vigência, fórmula, precedência ou alçada não forem fornecidas | pendente |
| 2 | F1-T007 | Registrar regra versionada com fonte, vigência e estado de aprovação | Responsável de domínio | SPEC-1-002 | `Dados e integrações`; RN-1.002-01/02; CA-1-006/007; TDD RED/GREEN | Regra completa fica revisável; regra sem fonte/vigência ou conflitante não fica aprovada | Fixture de regra aprovada, pendente e conflitante | Registros versionados, fonte associada, estado e log de aprovação/devolução | F1-T006 concluída; responsável e aprovador confirmados | Parar se o executor precisar inventar regra, precedência ou vigência | pendente |
| 3 | F1-T008 | Registrar decisão com comparativo e sugestão separada | Champion | SPEC-1-002 | `Fluxo e regras`, itens 3–5; RN-1.002-04; CA-1-008; TDD GREEN | Decisão exibe pergunta, contexto, registros, comparativo, opções, sugestão/inferência rotulada, dono, alçada e evidência | Decisão com dois valores comparáveis e recomendação marcada como sugestão | Registro de decisão, captura da separação sugestão/veredito e evidências vinculadas | F1-T007 concluída; decisão e fontes do piloto disponíveis | Parar se sugestão virar veredito, se faltar fonte ou se não houver alçada | pendente |
| 4 | F1-T009 | Bloquear decisão sem dono/alçada/evidência e encaminhar conflito | Aprovador da alçada | SPEC-1-002 | RN-1.002-03/05; cenários `Limite` e `Divergência`; CA-1-009; TDD RED/REFACTOR | Decisão incompleta não encerra; conflito fica pendente com responsável, prazo e próxima ação; nenhuma precedência é inventada | Decisão sem dono/evidência e duas fontes conflitantes | Estado bloqueado, pendência encaminhada e log do conflito | F1-T008 concluída; aprovador e fontes conflitantes disponíveis | Parar se o sistema fechar sem campos obrigatórios ou escolher fonte por suposição | pendente |
| 5 | F1-T010 | Executar versionamento e reabertura de regra/decisão | Responsável pela superfície Ethos | SPEC-1-002 | RN-1.002-02/06; cenário `Reabertura`; CA-1-010; TDD REFACTOR/REGRESSÃO | Alteração cria nova versão; aprovação anterior permanece; reabertura exige motivo e novo veredito; tentativa sem alçada é negada e auditada | Regra aprovada alterada e decisão aprovada reaberta | Histórico, nova versão, motivo, aprovação e tentativa negada auditados | F1-T009 concluída; alçada e regra/decisão aprovadas disponíveis | Parar se histórico for sobrescrito ou reabertura sem alçada for aceita | pendente |

## Emendas

<!-- Append-only (D19): mudanças aprovadas depois da geração. A história não é reescrita. -->

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
| | | | |
