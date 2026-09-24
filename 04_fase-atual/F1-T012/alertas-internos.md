# Alertas internos — F1-T012

> Somente painel na UI. **Sem** e-mail, WhatsApp, webhook ou SMTP.

## Chave de deduplicação (RN-1.003-06)

```
{object_ref}|{state}|{due|sem-prazo}
```

Função: `upsertAlert` em `src/lib/visao-fixture.ts`.

## Alertas iniciais gerados de `buildAlertsFromItems`

| key | título | owner / encaminhamento |
|---|---|---|
| `PEND-001\|aberta\|2026-09-30` | Pendência PEND-001 | responsavel.financeiro@piloto.test |
| `DEC-ORFA-001\|orfa\|2026-09-30` | Decisão órfã | champion@piloto.test (escala) |
| `LOT-002\|cobertura-incompleta\|sem-prazo` | Cobertura incompleta LOT-002 | responsavel.financeiro@piloto.test |
| `PEND-001\|cobertura-incompleta\|2026-09-30` | Cobertura incompleta PEND-001 | responsavel.financeiro@piloto.test |
| `IND-COBERTURA-PILOTO\|cobertura-incompleta\|sem-prazo` | Indicador cobertura | champion@piloto.test |

## Prova de não-duplicidade

Botão na UI: **Reemitir alerta PEND-001 (teste dedupe)**  
Efeito esperado: mesma `key` atualiza `updated_at`/motivo; **não** cria segunda linha.

## Metadados CA-1-013 em cada alerta

fonte (`source_ref`) · período · regra/versão (se houver) · cobertura · próxima ação
