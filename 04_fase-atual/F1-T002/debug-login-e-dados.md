# Debug F1-T002 — login e dados (2026-09-24) · FECHADO

## Sintomas

1. Login inicial falhou (usuário seed inexistente).
2. Após botão create-user: login OK, lista vazia (reteste B).

## Causa

Skip Cloud não aplica migrations (`0001`–`0003` ficam pending; apply → abort/502). PocketBase só tem coleção `users`.

## Correções

| Versão | Mudança |
|---|---|
| 0.0.8 | Botão "Criar usuário de teste e entrar" |
| 0.0.11–0.0.12 | Schema fatiado (ainda pending no Cloud) |
| **0.0.12** | **Fallback fixture F1-T001** — entrada controlada |

## Aceite

Champion confirmou **OK** no modo fixture (CTR-PILOTO-001 + lotes). Task concluída com desvio PB declarado.

## Dívida

- adapta-divida: fixture embutida na UI; upgrade quando Skip Cloud aplicar migrations PB.
