# Manifesto — testes RLS auth/negado F1-T013 (SPEC-1-003)

> **Task:** F1-T013 · **Dono formal:** Segurança/qualidade · **Coordenação:** Champion (JP/Iverson)
> **Gerado em:** 2026-09-24 · **Concluída em:** 2026-09-24T12:25-03:00
> **Regra:** nenhum dado real; fixture F1-T011; negação sem vazamento; nenhuma permissão global alterada.

## 1. Objetivo

Executar e evidenciar testes RLS autorizado/negado (CA-1-012 · RN-1.003-01).

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
| Motor | `src/lib/rls-authorize.ts` |
| UI | `src/pages/Rls.tsx` |
| Rota | `src/App.tsx` → `/rls` |

## 4. Evidência de aceite

- Teste humano do champion: **OK** (2026-09-24).
- Revalidação independente: rota, bateria, FORBIDDEN sem payload, audit log, sem dado real — PASSOU.
- Desvio: PB Cloud RLS não applied (dívida F1-T002); prova via motor puro.

## 5. Fora de escopo / abertos

Aceite formal Segurança 1C/1G/1H/1I · F1-T014 · F1-T015 · F1-T005 · publish · dado real.
