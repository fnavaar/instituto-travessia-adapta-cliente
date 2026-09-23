# Matriz de alçadas — F1-T006 (SPEC-1-002)

> **Status:** proposta — aguarda confirmação do champion (**BLOQUEIO-1E**).
> Nenhuma alçada real é assumida; os aprovadores abaixo são **de teste** (e-mails `@piloto.test`).

## Papéis e responsabilidades

| Papel | Propõe regra | Revisa/confecciona | Aprova regra | Aprova decisão | Consulta |
|---|---|---|---|---|---|
| Responsável de domínio (Contratos / DP-RH / Financeiro) | sim (próprio recorte) | sim (próprio recorte) | não | não | sim |
| Aprovador da alçada | não | não | sim (próprio domínio) | sim (próprio domínio) | sim |
| Champion | não | encaminha | não | não | sim |
| Direção | não | não | só dentro da alçada | só dentro da alçada | sim |
| Executor (agente) | não | não | **não** | **não** | conforme RLS |

## Alçadas por domínio (proposta)

| Domínio | Propõe/revisa | Aprova | Aprovador de teste |
|---|---|---|---|
| Contratos | `responsavel.contratos@piloto.test` | `aprovador.contratos@piloto.test` | a confirmar (1E) |
| DP/RH | `responsavel.dprh@piloto.test` | `aprovador.dprh@piloto.test` | a confirmar (1E) |
| Financeiro | `responsavel.financeiro@piloto.test` | `aprovador.financeiro@piloto.test` | a confirmar (1E) |
| Champion | acompanha/encaminha | não substitui a alçada | `champion@piloto.test` |

## Regras de ouro aplicadas

- **RN-1.002-03:** decisão sem dono, alçada, evidência ou justificativa **não encerra**.
- **RN-1.002-04:** sugestão/inferência é rotulada e **nunca** muda estado sozinha.
- **RN-1.002-06:** reabertura fora da alçada é **negada e auditada**.
- O executor (agente) **não** pode transformar sugestão em decisão nem autoaprovar.

> **BLOQUEIO-1E aberto:** o champion deve confirmar as alçadas e os aprovadores nominais antes de
> qualquer uso real. Até lá, valem apenas os aprovadores de teste.
