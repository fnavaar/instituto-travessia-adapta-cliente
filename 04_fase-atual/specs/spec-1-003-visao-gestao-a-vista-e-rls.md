# SPEC-1-003 — Visão de gestão à vista, RLS e recuperação do piloto

**Fase:** 1  
**Status:** planejada  
**Dono:** Champion, com validação de direção, Segurança/qualidade, Gestão de Contratos, DP/RH e Financeiro  
**Origem no escopo:** D-003, D-004, D-005, D-008; C-008, C-009; RQ-001, RQ-005, RQ-009, RQ-010  
**Degrau da solução:** nativo da plataforma — compor a visão e os controles a partir dos registros das SPECs-1-001/1-002 e das permissões existentes; não liberar acesso amplo nem criar canal externo.

## Contexto e decisões fechadas

- **Estado atual:** a organização consulta dados em fontes paralelas e indicadores manuais; não há uma visão única de contrato/período, pendências, decisões e qualidade com teste de acesso autorizado/negado.
- **Estado desejado:** direção, champion e responsáveis consultam uma visão filtrável por contrato, período, área, responsável e status; pendências, decisões sem dono, erros de qualidade e prazos aparecem com próxima ação; RLS, auditoria, rollback e estados de recuperação são demonstráveis com dados mascarados.
- **Decisões já fechadas:** acesso mínimo necessário por papel, área e contrato; negar por padrão; dados pessoais/financeiros não aparecem fora do recorte; indicador é derivado de registros; não há publicação externa; falha crítica bloqueia uso dependente; o legado e a última versão aprovada permanecem como retorno.
- **Bloqueios:** **BLOQUEIO-1G:** confirmar identidade/papel/área/contrato de cada usuário de teste. **BLOQUEIO-1H:** aprovar matriz de campos sensíveis, retenção e exportação. **BLOQUEIO-1I:** fornecer fixtures autorizadas e negadas. Sem isso, não usar dados reais nem considerar o RLS aceito.

## Resultado observável

Na demonstração do piloto, um usuário autorizado visualiza apenas os contratos, períodos, documentos, pendências e decisões previstos para seu papel; um usuário sem o recorte recebe acesso negado sem vazamento de dados. A visão apresenta status, atraso, qualidade, decisões sem dono e próxima ação, com links para fonte, regra, versão e evidência. Cada alteração relevante, tentativa negada, devolução, reabertura e rollback fica auditável.

## Limites e dependências

- **Inclui:** visão de contrato/período; filtros; cards/listas de pendência; alertas internos de atraso, qualidade e decisão órfã; indicadores derivados da central; matriz inicial de RLS; testes autorizado/negado; logs de acesso/alteração; estados de erro, devolução, reabertura, cancelamento e rollback; relatório de cobertura do piloto.
- **Fora de escopo:** painel executivo completo de todas as áreas; DRE/margem calculada; notificação externa; automação autônoma; publicação de decisões; gestão de credenciais; mudança de permissões no diretório; uso de dados reais sem aceite de segurança.
- **Entradas e pré-condições:** registros válidos da SPEC-1-001; regras/decisões da SPEC-1-002; papéis e recortes confirmados; fixture mascarada; matriz de campos sensíveis; política de retenção/exportação; responsável para aprovar o teste de segurança.
- **Saídas/artefatos:** visão filtrável; matriz RLS; roteiro e resultado de testes; logs de acesso autorizado/negado; relatório de pendências e cobertura; evidência de rollback e recuperação; lista de riscos residuais.
- **Dependências e responsáveis:** responsável pela superfície Ethos configura a visualização e aplicação das permissões; Segurança/qualidade aprova os testes; champion e responsáveis de domínio validam cobertura; direção valida a utilidade da visão sem ampliar alçada.
- **Atores e permissões mínimas:** direção consulta indicadores e pendências agregados autorizados; champion consulta recorte transversal aprovado; Gestão de Contratos vê contratos/documentos do seu recorte; DP/RH vê dados de pessoal do contrato/área permitidos; Financeiro vê relatórios financeiros do recorte; papel sem permissão recebe negação; nenhum ator ganha acesso apenas para facilitar o teste.
- **Superfícies/arquivos/configurações afetadas:** visão e filtros na superfície Ethos/cliente; política RLS; logs e evidências; não alterar permissões do sistema de identidade sem aprovação explícita nem exportar dados pessoais/financeiros.
- **Risco e plano B:** RLS mal aplicada, filtro que vaza dados, indicador calculado fora da fonte, alerta duplicado, log incompleto ou rollback que apaga histórico. Suspender dados reais, usar dados mascarados e voltar à fila manual/visão controlada até correção.
- **Rollback ou reversão:** desativar a visão/alerta defeituoso, preservar registros e logs, restabelecer última configuração aprovada e manter consulta manual controlada; reabrir o gate se houver qualquer acesso indevido.

## Dados e integrações

| Origem/destino | Fonte de verdade | Campos/contrato | Autenticação/permissão | Timeout/retry/idempotência | Tratamento de erro |
|---|---|---|---|---|---|
| Central de fontes → visão do piloto | registros válidos da SPEC-1-001 | contrato, período, área, responsável, status, qualidade, pendência, versão e fonte | RLS por papel/área/contrato; filtro não substitui autorização | consulta somente leitura; alerta deduplicado por registro/estado/prazo | ocultar registro não autorizado; não retornar dado parcial sensível |
| Central de decisões → painel de decisões | decisões da SPEC-1-002 | pergunta, estado, dono, alçada, prazo, próxima ação, evidência e risco | somente campos permitidos ao papel | atualização por estado/versão; não duplicar alerta | decisão sem dono vai para fila crítica do champion |
| Superfície → log de auditoria | evento de criação, consulta, alteração, negação, devolução, reabertura e rollback | ator, papel, objeto, contrato/área, ação, resultado, motivo, timestamp e correlação | log protegido contra edição pelo usuário comum | registrar uma vez por evento; reprocessamento não duplica | se auditoria indisponível, bloquear operação sensível e registrar indisponibilidade |
| Visão/alerta → operação manual | fila de pendências | estado, motivo, dono, prazo, fonte, próxima ação e escalonamento | sem envio externo nesta SPEC | alerta interno deduplicado; retry somente após falha registrada | manter pendência e sinalizar falha de notificação |

| Regra de negócio | Condição | Ação/resultado | Exceção | Fonte |
|---|---|---|---|---|
| RN-1.003-01 | usuário consulta objeto fora do papel/área/contrato | negar e não revelar conteúdo, existência sensível ou campos | registrar tentativa no log; nunca abrir exceção silenciosa | D-003; RQ-010 |
| RN-1.003-02 | indicador é exibido | derivar de registros com fonte, período, regra e versão; mostrar cobertura | se cobertura incompleta, exibir `cobertura incompleta` e não estimar | C-008; D-007 |
| RN-1.003-03 | pendência, atraso, divergência ou decisão sem dono existe | mostrar motivo, responsável, prazo, próxima ação e estado | sem responsável, escalar ao champion sem atribuir por suposição | D-004; RQ-005 |
| RN-1.003-04 | tentativa de alteração, devolução, reabertura ou rollback ocorre | exigir papel, motivo e registrar antes/depois | negar operação fora da alçada e auditar | RQ-007; RQ-010 |
| RN-1.003-05 | log, RLS ou fonte crítica está indisponível | bloquear uso dependente e manter modo manual/última versão válida | nenhuma operação irreversível pode continuar | C-009; Fase 1 |
| RN-1.003-06 | alerta já foi emitido para mesmo objeto/estado/prazo | não duplicar a notificação interna; atualizar o estado existente | novo prazo/estado cria novo evento correlacionado | C-008; RQ-010 |

## Fluxo e regras

1. O responsável configura a visão somente depois de validar os campos e fontes das SPECs-1-001/1-002.
2. Segurança/qualidade cadastra os papéis de teste e a matriz de objetos/campos autorizados e proibidos.
3. O executor publica uma visão de contrato/período com filtros por contrato, período, área, responsável e status.
4. O executor cria fixtures de pendência, atraso, decisão sem dono, dado incompleto, versão e rollback; a visão deve apontar próxima ação e fonte.
5. O executor testa cada papel autorizado e negado, captura o resultado e verifica que o log não expõe conteúdo indevido.
6. O executor simula erro de fonte/RLS/log, bloqueia o uso dependente, retorna à última versão válida e registra a recuperação.
7. Champion e responsáveis aceitam a demonstração; qualquer vazamento, log ausente ou indicador sem fonte reprova o gate de dados reais.

| Cenário | Dado/condição | Resultado esperado | Caminho de erro/recuperação |
|---|---|---|---|
| Principal | papel autorizado consulta contrato/período do seu recorte | vê status, pendências, decisões, fonte, versão, prazo e próxima ação | se dado de origem estiver inválido, mostrar cobertura incompleta e bloquear uso dependente |
| Limite | papel possui área, mas não possui contrato específico | consulta só registros autorizados pela matriz; demais ficam inacessíveis | registrar tentativa se houver acesso solicitado fora do recorte |
| Negado | papel sem permissão tenta abrir documento/painel sensível | acesso negado sem revelar conteúdo ou existência sensível; evento auditado | corrigir matriz somente por aprovador; repetir teste depois |
| Qualidade | lote tem dado ausente ou decisão sem dono | painel mostra pendência, motivo, dono/encaminhamento e prazo; não mascara como zero/ok | reprocessar após correção; preservar versão anterior |
| Falha | RLS/log/fonte fica indisponível | operação sensível para; modo manual/última versão aprovada permanece; alerta interno registrado | restaurar configuração, repetir testes e reabrir aceite se necessário |
| Rollback | visão/alerta publicado com filtro incorreto | desativar configuração defeituosa sem apagar registros; restaurar última versão | nova revisão e teste completo de autorização antes de republicar |

## Instruções de execução para o Ethos

1. **Ler antes de alterar:** `03-Projeto/02-Escopo-Definitivo.md`, seções 5, 6, 8/Fase 1, 9–11; `03-Projeto/02-Plano_de_acao/matriz-de-rastreabilidade.md`; SPECs-1-001 e 1-002; matriz RLS e política de retenção fornecidas pelo cliente; esta SPEC.
2. **Alterar somente:** visão do piloto, filtros, alertas internos, regras RLS do recorte aprovado, testes e evidências.
3. **Não alterar:** diretório de identidade, permissões globais, fontes legadas, dados reais, conectores externos, canais de comunicação ou cálculos não especificados.
4. **Executar nesta ordem:** confirmar matriz e fixtures → configurar visão somente leitura → configurar filas/alertas internos → aplicar RLS → testar autorizado/negado → testar auditoria → simular falha/rollback → obter aceite.
5. **Parar e pedir validação quando:** usuário/recorte, campo sensível, retenção, exportação, alçada, log, RLS, fonte ou critério de indicador estiver indefinido; qualquer teste revelar vazamento; houver pedido de bypass para acelerar.
6. **Estado válido ao parar:** nenhum dado real liberado; visão em modo restrito ou desativada; última configuração aprovada preservada; logs e pendências disponíveis; falha com dono e próxima ação.

## Checklist de execução

- [ ] Papéis, áreas, contratos e campos sensíveis foram mapeados e aprovados.
- [ ] Fixtures mascaradas autorizadas e negadas foram preparadas.
- [ ] Visão de contrato/período e filtros foram configurados a partir das fontes válidas.
- [ ] Pendências, atrasos, decisões sem dono e cobertura incompleta aparecem com próxima ação.
- [ ] Testes de acesso autorizado/negado passaram sem vazamento.
- [ ] Logs de consulta, alteração, negação, devolução, reabertura e rollback foram conferidos.
- [ ] Falha de RLS/fonte/log e recuperação foram exercitadas.
- [ ] Champion e responsáveis aceitaram a evidência ou registraram reprovação/bloqueio.

## Critérios de aceite

- [ ] **CA-1-011:** usuário autorizado consulta a visão do contrato/período piloto e encontra pendências, decisões, fonte, versão, responsável, prazo e próxima ação.
- [ ] **CA-1-012:** usuário não autorizado é bloqueado por RLS em dado/documento sensível sem vazamento e a tentativa é auditada.
- [ ] **CA-1-013:** indicador/alerta exibido informa fonte, período, regra/versão e cobertura; cobertura incompleta não é apresentada como resultado completo.
- [ ] **CA-1-014:** alterações, devoluções, reaberturas, acessos negados e rollback têm ator, objeto, antes/depois, motivo e timestamp no log protegido.
- [ ] **CA-1-015:** indisponibilidade de fonte, RLS ou auditoria bloqueia o uso dependente e permite retorno à última versão aprovada/modo manual sem apagar histórico.

## TDD da SPEC

| Etapa | Prova | Comando/ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | papel sem permissão tenta abrir objeto sensível; visão recebe registro sem fonte | executar consultas com fixture negada e incluir item incompleto | acesso negado sem vazamento; item aparece como cobertura incompleta/bloqueada; evento registrado | captura por papel + log de autorização |
| GREEN | papéis autorizados consultam visão com contrato/período, pendência e decisão | publicar visão restrita e executar consultas por papel | cada papel vê somente recorte aprovado; pendências e decisões têm fonte, dono, prazo e próxima ação | roteiro de demonstração + evidências de RLS |
| REFACTOR/REGRESSÃO | alterar filtro, repetir alerta, indisponibilizar log/fonte e reverter | simular regressões e restaurar última configuração | sem vazamento/duplicidade; operação sensível bloqueia; rollback preserva registros e auditoria | relatório de regressão, log e aceite |

**Dados/fixtures:** contrato/período mascarado com documento contratual, relatório financeiro sem dado real, registro de DP mascarado, pendência, decisão sem dono, decisão autorizada, objeto de outra área/contrato, papel autorizado e papel negado.

**Caminhos de erro obrigatórios:** permissão negada, filtro malformado, dado sem fonte, decisão sem dono, alerta duplicado, log indisponível, fonte indisponível, timeout de consulta e rollback.

**Evidência exigida:** matriz RLS aprovada; capturas/URLs internas da visão por papel; logs de acesso e alteração; resultado dos testes negados; relatório de cobertura/pendências; prova de rollback; aceite humano de Segurança/qualidade e champion.

## Handoff e operação

- **Como demonstrar:** abrir a visão como champion, Gestão de Contratos, DP/RH, Financeiro e papel negado → comparar recortes → abrir pendência/decisão → mostrar fonte/versão → testar acesso proibido → simular falha → voltar à última versão válida.
- **Como operar depois:** champion revisa a fila; responsáveis resolvem pendências; Segurança/qualidade revisa acessos e logs; direção usa somente indicadores com cobertura declarada.
- **Como monitorar:** acessos negados, falhas de RLS, logs ausentes, registros sem fonte, decisões órfãs, pendências vencidas, alertas duplicados, cobertura incompleta e rollbacks.
- **Pendência conhecida:** matriz nominal de RLS, política de retenção/exportação, fixtures e identidades de teste precisam ser aprovadas antes de liberar dados reais.

## Tasks vinculadas

| Onda | ID | Task | Dono | SPEC | Subseção da SPEC | Critério binário | Recorte da prova | Evidência esperada | Pré-condições | Ponto de parada | Status |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | F1-T011 | Preparar matriz RLS, identidades de teste, fixtures e política de dados | Segurança/qualidade | SPEC-1-003 | `Limites e dependências`; `Instruções de execução`, itens 1 e 5; checklist | Papéis, áreas, contratos, campos sensíveis, identidades autorizado/negado e retenção/exportação estão aprovados | Fixtures autorizadas e negadas da seção `Dados/fixtures` | Matriz RLS, identidades de teste, política de retenção/exportação e aprovação de Segurança/qualidade | Superfície Ethos disponível; nenhuma permissão global será alterada | Parar se identidade, recorte, campo sensível, retenção ou exportação estiver indefinido | pendente |
| 2 | F1-T012 | Configurar visão do contrato/período e alertas internos do piloto | Responsável pela superfície Ethos | SPEC-1-003 | `Fluxo e regras`, itens 1–4; RN-1.003-02/03/06; CA-1-011/013; TDD GREEN | Visão filtra contrato/período/área/responsável/status e mostra pendências, decisões, fonte, cobertura e próxima ação sem alerta externo | Fixture com pendência, atraso, decisão sem dono e cobertura incompleta | Capturas/URL interna, configuração dos filtros, alertas deduplicados e fonte de cada indicador | F1-T001, F1-T006 e F1-T011 concluídas; registros válidos disponíveis | Parar se indicador não tiver fonte/regra/versão, se alerta duplicar ou se houver publicação externa | pendente |
| 3 | F1-T013 | Executar testes RLS autorizado/negado sem vazamento | Segurança/qualidade | SPEC-1-003 | cenário `Negado`; RN-1.003-01; CA-1-012; TDD RED/GREEN | Cada papel vê somente seu recorte; papel sem permissão recebe negação sem revelar conteúdo ou existência sensível; tentativa é auditada | Fixtures por papel, objeto sensível e objeto fora do recorte | Matriz preenchida, capturas autorizada/negada e logs de autorização | F1-T011 e F1-T012 concluídas | Parar imediatamente diante de vazamento, acesso fora do recorte ou log ausente | pendente |
| 4 | F1-T014 | Validar auditoria, cobertura e estados de recuperação | Segurança/qualidade | SPEC-1-003 | RN-1.003-02/03/04/06; cenários `Qualidade`; CA-1-013/014; TDD REFACTOR/REGRESSÃO | Cobertura incompleta, pendência, decisão órfã, alteração, devolução, reabertura e rollback são visíveis e auditados sem alerta duplicado | Fixtures de dado sem fonte, decisão sem dono, alteração, devolução e reabertura | Relatório de cobertura, logs protegidos, antes/depois e correlação de alertas | F1-T013 concluída; eventos e fixtures disponíveis | Parar se indicador não for rastreável, log for editável/ausente ou alerta duplicar | pendente |
| 5 | F1-T015 | Simular indisponibilidade e executar rollback da visão | Responsável pela superfície Ethos | SPEC-1-003 | RN-1.003-05; cenários `Falha` e `Rollback`; CA-1-015; TDD REFACTOR/REGRESSÃO | Falha de fonte, RLS ou auditoria bloqueia uso dependente; última configuração/modo manual é restaurado sem apagar registros | Indisponibilidade simulada de fonte, RLS e log; configuração defeituosa | Relatório de falha, alerta interno, configuração restaurada, registros e logs preservados | F1-T014 concluída; última configuração aprovada disponível | Parar se operação sensível continuar, rollback perder registros ou não houver estado válido | pendente |

## Emendas

<!-- Append-only (D19): mudanças aprovadas depois da geração. A história não é reescrita. -->

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
| | | | |
