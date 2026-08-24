# Instituto Travessia — pasta operacional do projeto

> **O que é:** a camada 3 da arquitetura (decisão D5) — o repositório que o cliente clona e
> onde o champion trabalha. Contém **apenas o contexto do projeto do cliente**: objetivo,
> a fase atual (SPECs com TDD + tasks), reuniões, documentos e resultados. Nada de metodologia interna,
> proposta bruta, análise crítica ou fases futuras.
>
> **Como nasce:** esta pasta foi preparada pela consultoria a partir do workspace do projeto,
> com conteúdo curado da Fase 1. A criação de repositório remoto e a publicação ficam fora deste
> handoff até confirmação específica.
>
> **Como vive:** quando o plugin `adapta-cliente` estiver disponível, ele dará ao champion os comandos compostos
> `/adapta-cliente:trabalhar`, `/adapta-cliente:destravar-task` e
> `/adapta-cliente:finalizar-task`, além das skills atomicas `proxima-task`, `debug-task`,
> `concluir-task` e `status`. A sincronização remota só será ativada depois da publicação autorizada.

## Mapa da pasta

| Pasta/arquivo | O que vive aqui |
|---|---|
| `CLAUDE.md` | Contexto do projeto + como o champion trabalha aqui. |
| `AGENTS.md` | Espelho mínimo para outros agentes. |
| `STATUS.md` | Fase atual, progresso, travas, próxima reunião. |
| `changelog.md` | Registro de tudo que aconteceu (inclusive dúvidas para o consultor). |
| `01_projeto/` | O objetivo do projeto e o que está fora de escopo — em linguagem de negócio. |
| `02_reunioes/` | Atas das reuniões do projeto. |
| `03_documentos/` | Documentos do cliente + materiais entregues pela consultoria. |
| `04_fase-atual/` | A unidade em execução: `fase.md` (tasks) + `specs/`. **Uma fase por vez.** |
| `05_entregas/` | Fases concluídas, arquivadas com suas evidências. |
| `.claude/` | Plugin `adapta-cliente` habilitado (+ skills extras criadas para este cliente). |

## Regras do repositório

1. O champion trabalha **task a task** (`/adapta-cliente:trabalhar`); tasks fecham só com o
   critério de pronto cumprido e o TDD da SPEC validado (`/adapta-cliente:finalizar-task`). Se travar, usa
   `/adapta-cliente:destravar-task`.
2. **Specs e plano não se editam aqui** — dúvida ou discordância vira registro no `changelog.md`
   (`DÚVIDA: …`); o consultor responde na sincronização seguinte.
3. A próxima fase aparece quando a atual fecha na reunião de ciclo — o caminho completo é
   conduzido pela consultoria.
