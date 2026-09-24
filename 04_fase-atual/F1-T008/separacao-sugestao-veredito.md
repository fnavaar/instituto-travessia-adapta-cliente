# Separacao sugestao vs veredito — F1-T008

## Layout (prova visual CA-1-008 / RN-1.002-04)

1. **Comparativo** — tabela de fatos (VALOR-A/B + source_ref + value)
2. **Card Sugestao/inferencia** — borda tracejada âmbar + badge `sugestao/inferencia`
   - texto, fonte, confianca, limites
   - botao de prova: tentar auto-aplicar → bloqueado
3. **Card Veredito humano** — bloco distinto
   - inicial: veredito/justificativa/evidencia ausentes, state `aberta`
   - form: actor=alcada, veredito em options, justificativa, evidencia

## Invariante

```
suggestionCannotClose(d) === (d.recommendation.label === 'sugestao/inferencia' && d.veredito == null)
```

Sugestao **nunca** escreve `veredito` nem `state` sem `applyVerdict` humano completo.
