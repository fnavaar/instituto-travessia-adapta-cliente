# Log de validação sintético — F1-T003

Exemplos dos três caminhos (gerados na UI durante o teste; store em sessão).

## 1. Rejeição (CA-1-002)

| kind | lot | message |
|---|---|---|
| rejeitado | LOT-INCOMPLETO | CA-1-002 / RN-1.001-02: campos obrigatórios ausentes: source_ref, source_version, owner |

Pendência: `PEND-LOT-INCOMPLETO-source_ref-source_version-owner` · status `aberta`.

## 2. Idempotência (CA-1-003)

| kind | lot | chave | message |
|---|---|---|---|
| idempotente | LOT-001 | SRC-CONTRATO-001\|v1\|2026-01..2026-12 | reenvio idêntico — retornado lote existente; nenhuma duplicata |

## 3. Nova versão (RN-1.001-04)

| kind | lot | versão | message |
|---|---|---|---|
| nova_versao | LOT-002 | v2 | anterior v1 preservada; motivo: inclusao do campo source_version |

PEND-001 → `resolvida`.
