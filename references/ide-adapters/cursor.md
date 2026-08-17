# Adaptador — Cursor

O formato antigo `.cursorrules` está obsoleto e é ignorado no modo Agent do Cursor desde
a versão 0.43. O formato atual (2026) é `.cursor/rules/*.mdc` — arquivos Markdown com
frontmatter YAML, dentro da pasta `.cursor/rules/`.

## Arquivo a gerar: `.cursor/rules/harness.mdc`

```markdown
---
description: Regras de harness do projeto — ler antes de qualquer tarefa
alwaysApply: true
---

Antes de qualquer tarefa, siga o AGENTS.md na raiz do projeto: leia progress.md e
feature_list.json, trabalhe em uma feature por vez, e rode a verificação antes de marcar
algo como concluído.
```

## Regra de geração
- Não duplique o conteúdo do AGENTS.md dentro do `.mdc` — ele deve apenas apontar para
  lá e reforçar o essencial (uma feature por vez, verificação obrigatória), do mesmo
  jeito que o CLAUDE.md.
