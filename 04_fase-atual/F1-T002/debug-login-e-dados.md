# Debug F1-T002 — login e dados (2026-09-24)

## Sintomas

1. Login inicial falhou (usuário seed inexistente).
2. Após botão create-user: login OK, **lista vazia** (reteste B).

## Causa

Skip Cloud **não aplica** migrations (`0001`–`0003` ficam `pending`; apply → abort/502). PocketBase só tem coleção `users`.

## Correções

| Versão | Mudança |
|---|---|
| 0.0.8 | Botão "Criar usuário de teste e entrar" |
| 0.0.11–0.0.12 | Schema fatiado (ainda pending no Cloud) |
| **0.0.12** | **Fallback fixture F1-T001** em `src/lib/piloto-fixture.ts` + `Piloto.tsx` — entrada controlada quando PB falha |

## Reteste

1. Hard refresh https://travessia-8e0be--preview.goskip.app/piloto
2. Entrar (criar usuário se preciso)
3. Deve aparecer badge **modo: entrada controlada (fixture F1-T001)** e CTR-PILOTO-001 / LOT-001 / LOT-002 / docs / PEND-001

## Dívida

- adapta-divida: fixture embutida na UI; upgrade quando Skip Cloud aplicar migrations PB
