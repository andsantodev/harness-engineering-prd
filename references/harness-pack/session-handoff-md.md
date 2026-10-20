# Como gerar o docs/session-handoff.md

Existe para dar continuidade entre sessões (ou múltiplos agentes): resume o estado atual
em poucas linhas, sem precisar reler todo o docs/progress.md.

## Estrutura

```markdown
# Session Handoff

**Estado atual:** Projeto iniciado, nenhuma feature implementada ainda.
**Última feature concluída:** nenhuma
**Próxima feature sugerida:** [id + título da primeira feature de docs/feature_list.json,
geralmente a que não tem depends_on]
**Bloqueios conhecidos:** nenhum
**Comando para retomar:** [comando de verificação principal, do AGENTS.md]
```

## Regras de geração
- "Próxima feature sugerida" deve apontar para uma feature real de docs/feature_list.json sem
  dependências pendentes.
- Este arquivo é reescrito (não anexado) a cada sessão — sempre reflete o estado atual,
  nunca o histórico completo.
