# Análise — F1-T013 (proxima-task)

> **Gerado em:** 2026-09-24T12:12-03:00 · **Estado:** aguardando_autorizacao
> **Pedido:** "execute" com `sem_task` → roteado para próxima elegível (Onda 3 restante).
> **Não implementar** até autorização explícita em nova mensagem.

## 1. Seleção

| Campo | Valor |
|---|---|
| Task | **F1-T013** — Executar testes RLS autorizado/negado sem vazamento |
| Onda | 3 (última restante) |
| Dono formal | Segurança/qualidade |
| Coordenação | Champion JP/Iverson |
| SPEC | SPEC-1-003 |
| Subseção | cenário `Negado`; RN-1.003-01; CA-1-012; TDD RED/GREEN |
| Pré-condições | F1-T011 ✓ · F1-T012 ✓ |
| Critério binário | Cada papel vê **somente** seu recorte; papel sem permissão recebe **negação sem revelar** conteúdo/existência sensível; tentativa é **auditada** |

**Por que esta:** única task pendente da Onda 3. F1-T004/F1-T009 (Onda 4) têm pré-condição ok, mas a ordem de onda prioriza fechar F1-T013.

## 2. Critérios de aceite (prova)

| ID | Exigência |
|---|---|
| **CA-1-012** | Usuário não autorizado bloqueado por RLS em dado/documento sensível **sem vazamento**; tentativa **auditada** |
| **RN-1.003-01** | Fora do papel/área/contrato → negar; não revelar conteúdo nem existência sensível; log da tentativa |
| TDD RED | Papel sem permissão tenta objeto sensível → negado + auditado |
| TDD GREEN | Papéis autorizados veem só recorte aprovado |

**Fora deste aceite:** auditoria profunda/cobertura (F1-T014); falha/rollback visão (F1-T015); aceite formal BLOQUEIO-1C/1G/1H/1I (continua aberto).

## 3. Insumos existentes

| Fonte | Útil para F1-T013 |
|---|---|
| F1-T011 `fixture-rls.json` | 6 authorized_objects + 4 denied_objects + coverage_incomplete |
| F1-T011 `identidades-teste.md` | champion/contratos/dprh/fin/dir + `negado.externo@piloto.test` |
| F1-T011 `matriz-rls-completa.md` | papéis × área × contrato (proposta; 1C aberto) |
| F1-T012 `/visao` | seletor de papel de teste + `allowed_roles` (leve; **não** é CA-1-012 completo) |
| Skip PB Cloud | **só** `users` — migrations domínio pending (dívida F1-T002) |

## 4. Achados e riscos

1. **Aceite é teto:** executar e **evidenciar** matriz auth/negado por papel + log de tentativa. **Não** = aprovação formal Segurança (1C) nem gate de dado real (F1-T005).
2. **PB Cloud sem coleções de domínio** → RLS runtime PocketBase **não** está applied. Caminho viável (AP-1113/1130):
   - motor puro `authorizeAccess(role, object)` + store de `audit_logs`
   - UI de bateria de testes `/rls` (ou `/acesso`)
   - **não** inventar permissão global no IdP; **não** criar usuários reais fora de `@piloto.test`
3. Negação **sem vazamento:** resposta negada = `{ allowed: false, code: 'FORBIDDEN' }` **sem** payload de conteúdo, **sem** confirmar existência sensível (ex.: não retornar título/campos de DOC-OUTRO-999).
4. Log de auditoria: `actor, role, object_ref, action, result, reason, timestamp` — append-only na sessão (F1-T014 endurece).
5. Filtro UI `/visao` **não** substitui esta prova — F1-T013 precisa de matriz preenchida + capturas auth/negado + logs.
6. Parar **imediatamente** se qualquer caso vazar conteúdo em negado (ponto de parada da fase).
7. BLOQUEIO-1C/1G abertos: testes usam identidades **sintéticas**; resultado = evidência de teste, não aceite formal Segurança.

## 5. Plano de implementação (após autorização)

### 5.1 Entregáveis Skip

1. **`src/lib/rls-authorize.ts`** (funções puras):
   - carregar fixture F1-T011 (authorized + denied)
   - `canAccess(role, object_ref)` → `{ allowed: true, object } | { allowed: false, code: 'FORBIDDEN' }`  
     - negado: **zero** campos de conteúdo no retorno
   - `probeAccess(role, object_ref)` → grava `AuditEvent` sempre
   - `runBattery(roles)` → matriz papel × objeto (auth/negado esperado vs obtido)
2. **Rota `/rls`** — bateria de testes:
   - seletor de papel (identidades F1-T011)
   - grid: objetos auth do piloto + objetos denied
   - ao “consultar”: mostra **permitido** (metadados mínimos do recorte) ou **NEGADO** genérico (sem conteúdo)
   - painel **Log de autorização** (append-only)
   - botão **Rodar bateria completa** → preenche matriz resultado + contagem PASS/FAIL
   - casos obrigatórios:
     - `negado` × qualquer objeto → FORBIDDEN + audit
     - `dprh` × DOC-FIN-SENSIVEL-001 → FORBIDDEN
     - `financeiro` × REG-DP-MASCARADO-001 → FORBIDDEN
     - qualquer papel piloto × CTR-OUTRO-999 / DOC-OUTRO-999 → FORBIDDEN sem payload
     - `contratos` × DOC-CONTRATO-001 → allowed
     - `financeiro` × DOC-FIN-001 / PEND-001 → allowed
3. Link na home
4. Pacote `04_fase-atual/F1-T013/`:
   - `manifesto-rls-testes.md`
   - `matriz-resultados.md` (template + instrução de preenchimento no teste humano)
   - `verificacao-automatica.md`
   - `log-autorizacao-sintetico.md` (exemplos auth/negado)

### 5.2 Fora de escopo

- Aceite formal Segurança 1C/1G/1H/1I
- Migrations PB RLS no Cloud (dívida plataforma; declarar desvio se apply falhar)
- Alterar permissões globais de identidade
- F1-T014 (auditoria/cobertura profunda), F1-T015, publish, dado real

### 5.3 Testes

| Tipo | O quê |
|---|---|
| Auto | build Skip; `canAccess` nega denied_objects sem payload; battery cobre 6 auth + 4 denied; audit append |
| Humano | abrir `/rls`; rodar bateria; conferir PASS em auth e negado; log presente; **nenhum** conteúdo em negado; reportar vazamento imediatamente |

## 6. Portão

**Parado em `aguardando_autorizacao`.**  
Implementação só após: *Autorizo implementar a F1-T013 conforme o plano*.
