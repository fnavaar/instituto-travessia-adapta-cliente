# Manifesto — bordas de lote F1-T003 (SPEC-1-001)

> **Task:** F1-T003 · **Dono formal:** Responsável pela superfície Ethos · **Coordenação:** Champion (JP/Iverson)
> **Gerado em:** 2026-09-24 · **Por:** agente do cliente (Adapta Cliente)
> **Regra:** nenhum dado real; fixture F1-T001; entrada controlada; sem rollback (F1-T004).

## 1. Objetivo

Exercitar rejeição, pendência, duplicidade e versionamento do lote
(CA-1-002 / CA-1-003 · RN-1.001-02/03/04).

## 2. Superfície

| Item | Valor |
|---|---|
| Projeto Skip | Travessia · id 60853 |
| Preview | https://travessia-8e0be--preview.goskip.app |
| Rota | `/lotes` |
| Versão UI | 0.0.17 |
| Modo | entrada controlada (store em memória + funções puras) |
| Produção | não publicada |

## 3. Artefatos Skip

| Artefato | Caminho |
|---|---|
| Domínio puro | `src/lib/lote-validate.ts` (`validateLot`, `registerLot`, `correctLot`, `idempotencyKey`) |
| UI | `src/pages/Lotes.tsx` |
| Rota | `src/App.tsx` → `/lotes` |
| Home | `src/pages/Index.tsx` (link) |

## 4. Comportamentos

| Cenário | Ação | Resultado esperado |
|---|---|---|
| RED incompleto | botão 1 / form vazio | `rejeitado` + pendência (motivo, dono, próxima ação) |
| Idempotente | botão 2 reenvio LOT-001 | `idempotente` — mesma chave, **sem** 2ª linha |
| Nova versão | botão 3 LOT-002 → v2 | v1 preservada; v2 `valido` com motivo; PEND-001 → resolvida |

Chave canônica: `source_ref|source_version|period`.

## 5. Fora de escopo

Rollback CA-1-004 (F1-T004) · decisão/veredito (F1-T008) · RLS completo (F1-T013) · publish · dado real.

## 6. Teste humano sugerido

1. Hard refresh https://travessia-8e0be--preview.goskip.app/lotes
2. **Submeter lote incompleto** → bloqueio + pendência no log
3. **Reenviar LOT-001 idêntico** → idempotente; contar LOT-001 continua 1 linha v1
4. **Corrigir LOT-002 → v2** → v1 e v2 na tabela; motivo visível; PEND-001 resolvida
5. Confirmar só dados sintéticos @piloto.test
