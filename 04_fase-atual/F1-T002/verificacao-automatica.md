# Verificação automática — F1-T002

**Data:** 2026-09-24  
**Skip project:** 60853 · Travessia

## Resultados

| Check | Status | Evidência |
|---|---|---|
| Frontend build | PASSOU | QA build ok nas versões 0.0.2–0.0.5 |
| Static analysis | PASSOU | QA staticAnalysis ok |
| Testes scaffold | PASSOU | QA test ok |
| Rota `/piloto` | PASSOU | `skip_project_get` lista path `/piloto` → Piloto |
| Arquivos migration/hook/UI no tree | PASSOU | `skip_file_list` |
| Migrations aplicadas no Cloud | FALHOU | abort / 502 Backend unavailable em retries |
| Seed CTR-PILOTO-001 no Cloud | FALHOU (dependente) | não observável sem migration applied |
| Dado real ausente no código | PASSOU | fixture sintética; e-mails @piloto.test |

## Veredito parcial

**Código e UI entregues; backend Skip Cloud não aplicou schema/seed nesta sessão.**  
Task permanece aberta para teste humano da UI e/ou debug quando o Cloud voltar.
