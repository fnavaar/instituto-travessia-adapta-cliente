# Manifesto — visão e alertas internos F1-T012 (SPEC-1-003)

> **Task:** F1-T012 · **Dono formal:** Responsável pela superfície Ethos · **Coordenação:** Champion (JP/Iverson)
> **Gerado em:** 2026-09-24 · **Por:** agente do cliente (Adapta Cliente)
> **Regra:** nenhum dado real; fixtures F1-T001+006+011; sem alerta externo; sem alterar permissão global.

## 1. Objetivo

Configurar visão do contrato/período com filtros e alertas internos deduplicados
(CA-1-011 / CA-1-013 · RN-1.003-02/03/06).

## 2. Superfície

| Item | Valor |
|---|---|
| Projeto Skip | Travessia · id 60853 |
| Preview | https://travessia-8e0be--preview.goskip.app |
| Rota | `/visao` |
| Versão UI | 0.0.14 |
| Modo | entrada controlada (fixture agregada) |
| Produção | não publicada |

## 3. Artefatos Skip

| Artefato | Caminho |
|---|---|
| Fixture + filtros + dedupe | `src/lib/visao-fixture.ts` |
| UI | `src/pages/Visao.tsx` |
| Rota | `src/App.tsx` → `/visao` |
| Home | `src/pages/Index.tsx` (link) |

## 4. Conteúdo da visão

| ref | tipo | destaque |
|---|---|---|
| CTR-PILOTO-001 | contract | contrato/período |
| PEND-001 | pendency | aberta · due 2026-09-30 · cobertura incompleta |
| DEC-PILOTO-001 | decision | com dono financeiro |
| DEC-ORFA-001 | decision | órfã → escala champion |
| LOT-001 | lot | válido · cobertura completa |
| LOT-002 | lot | cobertura incompleta (não como OK) |
| IND-COBERTURA-PILOTO | indicator | CA-1-013 metadados + badge incompleta |

## 5. Filtros e papel de teste

- Filtros: contrato, período, área, responsável, status
- Papéis: champion / contratos / financeiro / dprh / direcao / negado
- Recorte via `allowed_roles` (F1-T011); papel **negado** → lista vazia
- Filtro UI **não** substitui autorização (RLS runtime = F1-T013)

## 6. Alertas internos

- Painel local apenas (sem e-mail/webhook)
- Chave dedupe: `object_ref|state|due` (`upsertAlert`)
- Botão "Reemitir alerta PEND-001" prova RN-1.003-06 (atualiza, não duplica)
- Cada alerta: fonte, período, regra/versão, cobertura, próxima ação

## 7. Fora de escopo

F1-T013 (RLS auth/negado completo + audit PB), F1-T008 (veredito), publish, dado real, alerta externo.

## 8. Teste humano sugerido

1. Hard refresh https://travessia-8e0be--preview.goskip.app/visao
2. Papel **Champion** — ver PEND-001, DEC-PILOTO-001, DEC-ORFA-001, LOT-002 cobertura incompleta
3. Filtrar contrato CTR-PILOTO-001 / área AREA-FIN
4. Papel **negado** — lista vazia
5. **Reemitir alerta PEND-001** — não duplicar linha
6. Confirmar só dados sintéticos @piloto.test
