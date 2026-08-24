# SPEC-1-001 — Central de fontes e registro do piloto

**Fase:** 1  
**Status:** planejada  
**Dono:** Champion, com validação de Gestão de Contratos, DP/RH e Financeiro  
**Origem no escopo:** D-001, D-002, D-004, D-006, D-008; C-001, C-004; RQ-001, RQ-005, RQ-009, RQ-010  
**Degrau da solução:** dependência existente — usar a superfície autorizada do Ethos/cliente para registros, arquivos e permissões; não criar nova stack nem ativar conector sem prova de acesso e cobertura.

## Contexto e decisões fechadas

- **Estado atual:** contratos, documentos, planilhas, relatórios e dados do processo estão distribuídos em SharePoint/nuvem Microsoft, Excel, Domínio e relatórios das áreas. O fluxo demonstrado não possui uma visão única de contrato/período, fonte, lote, responsável e qualidade.
- **Estado desejado:** existe um registro central demonstrável de um contrato/período piloto, com lote de entrada controlada, documentos/relatórios associados, proveniência, versão, qualidade, responsável, status e pendências.
- **Decisões já fechadas:** a primeira entrega é integrada; entrada controlada é fallback obrigatório; o legado não é apagado; dados reais só entram após o teste de RLS; cada registro relevante tem fonte, dono, período, versão e estado; integração é opt-in após prova de acesso, permissão, formato, cobertura e rollback.
- **Bloqueios:** **BLOQUEIO-1A:** o champion deve indicar o contrato e período piloto. **BLOQUEIO-1B:** Gestão de Contratos/DP-RH/Financeiro devem entregar uma amostra mascarada e confirmar os responsáveis. **BLOQUEIO-1C:** a matriz inicial de campos e RLS deve ser aprovada antes de qualquer dado real. Sem esses itens, executar apenas com fixture sintética e parar antes da carga real.

## Resultado observável

Em uma demonstração, um papel autorizado abre o contrato/período piloto, encontra os documentos e lotes associados, identifica fonte, versão, responsável, qualidade, pendências e status, e consegue distinguir um lote atual de uma versão anterior. Um lote duplicado não cria uma segunda versão silenciosa; um lote inválido fica bloqueado; a origem permanece recuperável.

## Limites e dependências

- **Inclui:** cadastro do contrato e período piloto; cadastro de áreas e responsáveis necessários ao piloto; lote de entrada controlada; associação de documentos/relatórios; metadados de proveniência; versão e qualidade; estados mínimos; pendências; filtros por contrato, período, status e responsável; rollback de lote novo.
- **Fora de escopo:** interpretação automática de cláusulas; cálculo legal ou financeiro; admissão completa; integração ativa com Domínio/eSocial/SharePoint; emissão de Nota Fiscal; substituição do legado; fluxos detalhados de Produção, Compras, Comercial ou Projetos Sociais; dados reais antes do aceite de RLS.
- **Entradas e pré-condições:** contrato/período piloto nomeado; fixture mascarada; identificadores e documentos de origem; responsável por domínio; campos obrigatórios aprovados; matriz inicial de RLS; procedimento de coexistência com o legado.
- **Saídas/artefatos:** registros centralizados do piloto; lote com status e proveniência; documentos associados; log de validação; lista de pendências; evidência de versão, duplicidade, rollback e acesso autorizado/negado.
- **Dependências e responsáveis:** champion coordena o piloto; Gestão de Contratos valida contrato, vigência e documentos; DP/RH valida dados de folha/ocorrências disponíveis; Financeiro valida relatórios financeiros; responsável pela superfície Ethos confirma identidades, permissões e armazenamento.
- **Atores e permissões mínimas:** champion pode consultar e encaminhar o piloto; Gestão de Contratos pode criar/editar contrato e documentos contratuais; DP/RH pode consultar e registrar dados do seu domínio; Financeiro pode consultar relatórios financeiros autorizados; direção consulta visão agregada autorizada; qualquer outro papel é negado por padrão.
- **Superfícies/arquivos/configurações afetadas:** registros e arquivos do ambiente Ethos/cliente autorizados para o piloto; `03-Projeto/02-Plano_de_acao/01.Fase_1/01-SPECs/`; evidências de execução em local definido pelo cliente. Não alterar fontes legadas.
- **Risco e plano B:** fonte ou conector indisponível, campo ausente, versão concorrente ou permissão excessiva. Usar fixture mascarada e importação controlada, registrar a lacuna, bloquear o lote dependente e preservar a fonte original.
- **Rollback ou reversão:** marcar o lote novo como revertido e retirar somente seus registros derivados; manter arquivo original, lote anterior, log, motivo, autor e timestamp; reabrir a pendência para reconciliação. Nunca apagar histórico nem sobrescrever a fonte.

## Dados e integrações

| Origem/destino | Fonte de verdade | Campos/contrato | Autenticação/permissão | Timeout/retry/idempotência | Tratamento de erro |
|---|---|---|---|---|---|
| SharePoint/nuvem Microsoft → lote controlado | arquivo original e referência da pasta autorizada | `source_ref`, nome, tipo, versão, período, contrato, responsável, data de recebimento, sensibilidade | leitura somente no local aprovado; escrita somente na área designada e auditada | sem conector até prova; lote idempotente por `source_ref + source_version + period`; não repetir automaticamente sem confirmar duplicidade | marcar `aguardando fonte` ou `erro corrigível`; preservar referência original |
| Excel/relatório financeiro → lote controlado | arquivo entregue pelo Financeiro | identificador do arquivo, período, contrato/unidade, colunas presentes, versão, responsável, qualidade | upload manual controlado; Financeiro valida conteúdo | rejeitar duplicata por identificador de lote e versão; retry manual após correção | registrar campo ausente, formato inválido ou cobertura insuficiente |
| Domínio/eSocial → registro controlado | exportação comprovada ou fallback manual | somente campos disponíveis na amostra; não presumir cobertura | sem acesso automático nesta SPEC; registrar evidência de permissão ou bloqueio | importação manual versionada; sem retry automático | `aguardando fonte`, `erro corrigível` ou `devolvido` com próximo passo |
| Lote controlado → central de fontes | registro versionado do lote | contrato, período, fonte, dono, versão, qualidade, status, pendência e evidência | RLS da superfície autorizada | operação idempotente por chave de negócio e versão; transação parcial deve ser marcada como falha | não publicar lote parcial como válido; permitir reprocessamento do lote corrigido |

| Regra de negócio | Condição | Ação/resultado | Exceção | Fonte |
|---|---|---|---|---|
| RN-1.001-01 | Contrato/período não tem responsável ou identificador | impedir estado `pronto` e abrir pendência | champion pode registrar exceção com motivo e prazo; não remove a pendência | RQ-001; Fase 1 |
| RN-1.001-02 | Lote não informa fonte, versão, período ou responsável | rejeitar o lote antes de disponibilizá-lo para decisão | nenhuma | D-004; Fase 1 |
| RN-1.001-03 | Mesmo identificador de fonte, período e versão reaparece | não duplicar; mostrar lote existente e permitir comparação | nova versão exige nova referência e motivo | D-006; Fase 1 |
| RN-1.001-04 | Dado obrigatório está ausente, inconsistente ou duplicado | marcar qualidade como `bloqueada` e abrir pendência | correção gera nova versão, sem editar silenciosamente a anterior | D-004; RQ-010 |
| RN-1.001-05 | Lote novo é revertido | retirar derivados do lote e manter fonte, histórico e motivo | rollback parcial vira erro e exige reconciliação manual | RQ-010; Fase 1 |

## Fluxo e regras

1. Champion registra o contrato e período piloto, informa responsáveis e anexa/indica as fontes autorizadas.
2. O executor cria um lote com identificador, fonte, período, versão, responsável, sensibilidade e referência ao arquivo original.
3. O executor valida campos obrigatórios, duplicidade, formato e cobertura declarada; qualquer falha vira pendência e impede o estado válido.
4. O responsável do domínio revisa a qualidade, corrige por novo lote/versionamento ou devolve com motivo.
5. Após validação, o sistema associa documentos/relatórios ao contrato/período e disponibiliza a visão central para os papéis autorizados.
6. Uma alteração posterior cria nova versão e registra o motivo; o lote anterior permanece consultável conforme RLS.
7. Em falha ou rollback, o executor interrompe o consumo dependente, registra a causa e retorna à última versão aprovada.

| Cenário | Dado/condição | Resultado esperado | Caminho de erro/recuperação |
|---|---|---|---|
| Principal | fixture mascarada tem contrato, período, fonte, versão e responsável | contrato/período e lote ficam consultáveis; documentos associados aparecem com proveniência | se a validação falhar, lote fica pendente e não é publicado como válido |
| Limite | fonte existe, mas falta campo obrigatório ou o contrato tem dois responsáveis conflitantes | estado `aguardando fonte`/`com pendência`; responsável e próxima ação ficam visíveis | corrigir em novo lote ou registrar exceção humana com prazo; não preencher por suposição |
| Duplicidade | mesmo arquivo/versão é reenviado | nenhuma duplicação de registro; comparação mostra lote existente | se a versão for realmente nova, exigir nova versão e motivo |
| Falha | importação é interrompida ou lote contém registros inválidos | lote não é consumido parcialmente como válido; log identifica erro e quantidade afetada | corrigir e reprocessar lote; manter fonte original e versão anterior |
| Rollback | lote novo altera dados do piloto e é declarado inválido | derivados do lote são revertidos; histórico e fonte permanecem; última versão válida volta a ser apresentada | se não for possível reverter integralmente, marcar bloqueio grave e parar o piloto |

## Instruções de execução para o Ethos

1. **Ler antes de alterar:** `03-Projeto/02-Escopo-Definitivo.md`, seção 8/Fase 1 e seções 7, 9–11; `03-Projeto/02-Plano_de_acao/matriz-de-rastreabilidade.md`; `03-Projeto/00-DMO.md`; esta SPEC; a amostra e a matriz de RLS fornecidas pelo cliente.
2. **Alterar somente:** registros, lotes, associações, metadados e configurações do piloto na superfície autorizada; evidências desta SPEC.
3. **Não alterar:** arquivos legados; permissões fora do recorte aprovado; regras financeiras não fornecidas; integração, envio, cálculo legal ou publicação externa.
4. **Executar nesta ordem:** confirmar pré-condições → carregar fixture mascarada → validar lote → revisar qualidade → testar consulta por papel → demonstrar versão/duplicidade → testar rollback → registrar evidências.
5. **Parar e pedir validação quando:** o contrato/período piloto, dono, campo obrigatório, RLS, fonte, permissão, conector ou política de retenção não estiver confirmado; houver dado real não mascarado; houver conflito entre fontes; rollback ou auditoria não estiver disponível.
6. **Estado válido ao parar:** fonte original intacta; nenhum lote inválido publicado; pendências com dono/prazo; última versão válida consultável; log de execução preservado.

## Checklist de execução

- [ ] Contrato e período piloto foram nomeados pelo champion.
- [ ] Fixture mascarada, fontes e responsáveis foram entregues e conferidos.
- [ ] Campos obrigatórios, versionamento, estados e RLS inicial foram aprovados.
- [ ] Lote principal foi validado e associado ao contrato/período.
- [ ] Caminhos de ausência, duplicidade, erro e rollback foram exercitados.
- [ ] Evidências de proveniência, qualidade, versão e acesso foram anexadas.
- [ ] Handoff para a SPEC-1-002 e SPEC-1-003 foi confirmado sem gerar tasks nesta etapa.

## Critérios de aceite

- [ ] **CA-1-001:** uma pessoa autorizada localiza o contrato/período piloto e vê fonte, versão, responsável, qualidade, documentos e pendências sem navegar manualmente pelas fontes legadas.
- [ ] **CA-1-002:** um lote sem fonte, versão, período ou responsável é bloqueado e gera pendência com motivo, dono e próxima ação.
- [ ] **CA-1-003:** o reenvio do mesmo lote é idempotente e não cria duplicidade; uma alteração cria nova versão com motivo e histórico.
- [ ] **CA-1-004:** um lote inválido pode ser revertido sem apagar a fonte, a versão anterior ou o log; o sistema retorna ao último estado válido.
- [ ] **CA-1-005:** nenhum dado real é disponibilizado antes de a evidência de RLS autorizado/negado ser aceita pelo champion e responsáveis.

## TDD da SPEC

| Etapa | Prova | Comando/ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | lote sem `source_ref`, período ou responsável | submeter fixture incompleta pela entrada controlada | lote rejeitado/bloqueado; pendência identificada; nenhum registro válido publicado | log da validação + captura/registro da pendência |
| GREEN | fixture mascarada completa de um contrato/período | importar lote, associar documentos e consultar como champion | contrato/período, lote, fonte, versão, dono e qualidade aparecem em uma visão única | captura/URL interna + exportação do registro |
| REFACTOR/REGRESSÃO | reenviar lote, criar versão nova, introduzir duplicata e reverter lote | repetir operações com mesmas chaves e depois executar rollback | reenvio idempotente; nova versão auditada; duplicata bloqueada; rollback preserva histórico e volta à versão válida | log de lotes, auditoria e evidência de acesso |

**Dados/fixtures:** um contrato e um período definidos pelo champion; fixture mascarada com pelo menos um documento contratual, um relatório financeiro ou operacional, um registro de responsável, uma pendência intencional, uma duplicata e uma versão corrigida. Não usar CPF, salário ou dado bancário real.

**Caminhos de erro obrigatórios:** campo ausente, fonte indisponível, formato inválido, duplicidade, versão concorrente, importação interrompida, acesso negado e rollback.

**Evidência exigida:** registro/URL interna da central; logs de validação e auditoria; capturas das visões por papel; relatório de rollback; aceite humano do champion e dos responsáveis de domínio.

## Handoff e operação

- **Como demonstrar:** abrir o contrato/período piloto → mostrar lote e documento de origem → mostrar pendência → mostrar nova versão → reenviar duplicata → executar rollback → repetir consulta com papéis autorizado e não autorizado.
- **Como operar depois:** Gestão de Contratos mantém contrato/documentos; DP/RH e Financeiro revisam seus lotes; champion resolve pendências de domínio e aprova o corte.
- **Como monitorar:** lotes pendentes, erros de validação, duplicidades, versões sem responsável, acessos negados e tentativas de uso de lote revertido.
- **Pendência conhecida:** seleção nominal do piloto, fixture final, responsáveis, campos, RLS, retenção e disponibilidade de conectores ainda precisam ser confirmados antes da execução com dados reais.

## Tasks vinculadas

| Onda | ID | Task | Dono | SPEC | Subseção da SPEC | Critério binário | Recorte da prova | Evidência esperada | Pré-condições | Ponto de parada | Status |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | F1-T001 | Preparar contrato/período piloto, fixture mascarada e pacote de pré-condições da central | Champion | SPEC-1-001 | `Limites e dependências`; `Instruções de execução`, itens 1 e 5; checklist | Contrato/período, fontes, responsáveis, campos obrigatórios, matriz RLS e fixture mascarada estão registrados sem dado real | Fixture sintética/mascarada e manifesto do piloto | Manifesto, identificação da fixture, responsáveis, fontes, campos e matriz RLS | Aceite humano do escopo; champion e responsáveis disponíveis | Parar se faltar contrato/período, responsável, campo, RLS ou se houver dado real | pendente |
| 2 | F1-T002 | Registrar contrato/período e lote controlado com proveniência | Gestão de Contratos | SPEC-1-001 | `Fluxo e regras`, itens 1–5; `Resultado observável`; CA-1-001; TDD GREEN | Papel autorizado localiza contrato/período e vê fonte, versão, responsável, qualidade, documentos e pendências | Fixture mascarada completa de um contrato/período | Registro/URL interna, captura da visão, lote e log de validação | F1-T001 concluída; lote sem dados reais | Parar se o lote for parcial, sem fonte/versão/dono ou não respeitar RLS | pendente |
| 3 | F1-T003 | Exercitar rejeição, pendência, duplicidade e versionamento do lote | Responsável pela superfície Ethos | SPEC-1-001 | RN-1.001-02/03/04; cenários `Limite` e `Duplicidade`; CA-1-002/003; TDD RED/REFACTOR | Lote incompleto é bloqueado; reenvio idêntico é idempotente; correção cria nova versão com motivo e preserva a anterior | Fixtures incompleta, duplicada e corrigida | Logs de validação, estado da pendência, comparação de versões e idempotência | F1-T002 concluída; fixtures de erro disponíveis | Parar se dado inválido for publicado, duplicata for criada ou versão anterior for sobrescrita | pendente |
| 4 | F1-T004 | Executar rollback de lote e provar retorno à versão válida | Responsável pela superfície Ethos | SPEC-1-001 | `Rollback ou reversão`; cenário `Rollback`; CA-1-004; TDD REFACTOR/REGRESSÃO | Lote inválido é revertido sem apagar fonte, histórico ou log e a última versão válida volta a ser apresentada | Lote novo inválido e versão anterior válida | Relatório de rollback, estado restaurado, fonte original e logs | F1-T003 concluída; rollback disponível | Parar se houver exclusão, perda de histórico ou retorno parcial sem bloqueio | pendente |
| 5 | F1-T005 | Confirmar gate de dados reais e bloqueio sem RLS aceito | Champion | SPEC-1-001 | CA-1-005; `Instruções de execução`, itens 5–6; TDD RED | Antes do aceite RLS autorizado/negado, carga real fica bloqueada e somente fixture mascarada é utilizável; o gate é preservado | Tentativa controlada de carga real sem aceite e prova RLS aceita | Registro de bloqueio, aceite humano, evidência RLS e estado da carga | F1-T013 concluída; champion e responsáveis disponíveis | Parar se qualquer dado real aparecer antes do aceite ou se o aceite não tiver evidência | pendente |

## Emendas

<!-- Append-only (D19): mudanças aprovadas depois da geração. A história não é reescrita. -->

| Data | Origem do sinal | Micro-spec/task | Motivo |
|---|---|---|---|
| | | | |
