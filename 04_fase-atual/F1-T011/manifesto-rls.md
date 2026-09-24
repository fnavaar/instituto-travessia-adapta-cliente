# Manifesto RLS e política de dados — F1-T011 (SPEC-1-003)

> **Task:** F1-T011 · **Dono formal:** Segurança/qualidade · **Coordenação:** Champion (JP/Iverson)
> **Gerado em:** 2026-09-24 · **Por:** agente do cliente (Adapta Cliente)
> **Regra:** nenhum dado real; nenhuma permissão global alterada.

## 1. Objetivo

Registrar o pacote de pré-condições de RLS da visão do piloto: matriz completa, identidades de
teste, fixtures autorizado/negado e política de retenção/exportação — base para F1-T012 e F1-T013.

## 2. Insumos entregues

| Insumo | Arquivo | Status |
|---|---|---|
| Matriz RLS completa | `matriz-rls-completa.md` | proposta (1C) |
| Identidades de teste | `identidades-teste.md` | sintéticas (1G) |
| Fixture auth/negado | `fixture-rls.json` | sintética |
| Política de dados | `politica-dados.md` | proposta (1H) |
| Base F1-T001 | `../F1-T001/matriz-rls.md` + fixture | reutilizada |
| Base F1-T006 | `../F1-T006/fixture-regras.json` | decisão/pendência referenciadas |

## 3. Critério binário

Papéis, áreas, contratos, campos sensíveis, identidades autorizado/negado e retenção/exportação
**estão registrados** (aprovação formal de Segurança/qualidade permanece como bloqueio — não se
inventa aceite).

## 4. Bloqueios

| ID | Descrição | Quem |
|---|---|---|
| BLOQUEIO-1C | Aprovar matriz RLS | Segurança/qualidade |
| BLOQUEIO-1G | Confirmar identidades nominais | Segurança + champion |
| BLOQUEIO-1H | Aprovar retenção/exportação/campos sensíveis | Segurança/qualidade |
| BLOQUEIO-1I | Fixtures auth/negadas — pacote entregue; aceite formal | Segurança/qualidade |

## 5. O que esta task NÃO faz

- Não cria usuários no Skip nem aplica rules no PocketBase (F1-T012/F1-T013).
- Não configura a visão de gestão à vista (F1-T012).
- Não libera dados reais (CA-1-005 / F1-T005).
- Não altera diretório de identidade nem permissões globais.

## 6. Handoff

- **F1-T012:** usar matriz + fixtures para visão/filtros/alertas internos.
- **F1-T013:** executar testes autorizado/negado com identidades e objetos desta fixture.
