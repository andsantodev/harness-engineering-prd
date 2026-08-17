# Adaptador — Claude Code

Claude Code lê `CLAUDE.md`, não `AGENTS.md` diretamente — mesmo com AGENTS.md sendo o
padrão adotado por Codex, Cursor e outras ferramentas, o Claude Code continua exigindo
seu próprio arquivo (sem suporte nativo a AGENTS.md confirmado até meados de 2026). O
jeito recomendado oficialmente é fazer o CLAUDE.md importar o AGENTS.md com a sintaxe
`@`, em vez de duplicar o conteúdo.

## Arquivo a gerar: `CLAUDE.md`

```markdown
@AGENTS.md

## Notas específicas do Claude Code
[deixe em branco, ou adicione algo só se o usuário pedir algo específico do Claude Code]
```

## Regra de geração
- Nunca copie o conteúdo do AGENTS.md para dentro do CLAUDE.md — apenas a linha de
  import. Isso evita os dois arquivos saírem de sincronia depois.
