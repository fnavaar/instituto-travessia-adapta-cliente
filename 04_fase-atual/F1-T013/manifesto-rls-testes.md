# Manifesto — testes RLS auth/negado F1-T013 (SPEC-1-003)

> **Task:** F1-T013 · **Dono formal:** Segurança/qualidade · **Coordenação:** Champion (JP/Iverson)
> **Gerado em:** 2026-09-24 · **Por:** agente do cliente (Adapta Cliente)
> **Regra:** nenhum dado real; fixture F1-T011; negação sem vazamento; nenhuma permissão global alterada.

## 1. Objetivo

Executar e evidenciar testes RLS autorizado/negado (CA-1-012 · RN-1.003-01):
cada papel só o recorte; negado = FORBIDDEN **sem** payload; tentativa auditada.

## 2. Superfície

| Item | Valor |
|---|---|
| Projeto Skip | Travessia · id 60853 |
| Preview | https://travessia-8e0be--preview.goskip.app |
| Rota | `/rls` |
| Versão UI | 0.0.19 |
| Modo | entrada controlada (motor puro + fixture F1-T011) |
| Produção | não publicada |

## 3. Artefatos Skip

| Artefato | Caminho |
|---|---|
| Motor | `src/lib/rls-authorize.ts` (`canAccess`, `probeAccess`, `runBattery`, `hasLeak`) |
| UI | `src/pages/Rls.tsx` |
| Rota | `src/App.tsx` → `/rls` |
| Home | `src/pages/Index.tsx` (link) |

## 4. Desvio declarado

PocketBase Cloud continua só com `users` (dívida F1-T002). RLS runtime PB **não** applied.
Prova via funções puras + UI (mesmo padrão AP-1113/1130). BLOQUEIO-1C/1G abertos —
teste sintético ≠ aceite formal Segurança.

## 5. Casos obrigatórios

| Papel | Objeto | Esperado |
|---|---|---|
| negado | qualquer | FORBIDDEN + audit |
| dprh | DOC-FIN-SENSIVEL-001 | FORBIDDEN |
| financeiro | REG-DP-MASCARADO-001 | FORBIDDEN |
| contratos | CTR-OUTRO-999 / DOC-OUTRO-999 | FORBIDDEN sem payload |
| contratos | DOC-CONTRATO-001 | allowed |
| financeiro | DOC-FIN-001 / PEND-001 | allowed |

## 6. Fora de escopo

Aceite formal 1C/1G/1H/1I · F1-T014 · F1-T015 · F1-T005 · publish · dado real · IdP global.

## 7. Teste humano sugerido

1. Hard refresh https://travessia-8e0be--preview.goskip.app/rls
2. **Rodar bateria completa** — todos PASS, 0 LEAK
3. Atalhos RED (negado, dprh×fin, fin×DP, contratos×outro) — JSON só `allowed:false, code:FORBIDDEN`
4. Atalhos GREEN (contratos×doc, fin×doc) — permitidos com metadados mínimos
5. Log de autorização preenchido
6. **PARAR** e reportar se qualquer negado mostrar label/conteúdo
