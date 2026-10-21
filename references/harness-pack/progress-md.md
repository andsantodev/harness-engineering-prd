# Como gerar o docs/progress.md

Log de sessões, entrada mais recente no topo. Na geração inicial, o arquivo começa
vazio — só com a estrutura pronta para a primeira sessão real de trabalho.

O objetivo é responder "onde estou?" em poucos segundos. Não é relatório de feature nem
transcrição da sessão.

## Estrutura

```markdown
# Progress Log

<!-- Nova entrada sempre no topo. Entrada curta (~10 linhas).
     Nunca apague sem resumir/arquivar. -->

## [Sessão] — [preencher na próxima sessão]
- Feito:
- Verificado (comando + resultado):
- Fora do escopo: (só se houver)
- Pendências / quebrado:
- Próximo passo sugerido:
```

## Tamanho e nível de detalhe

- Uma entrada por sessão (ou por feature, quando a sessão inteira foi uma feature), no
  máximo ~10–12 linhas.
- Sem blocos de código, DDL, tabelas, saída completa de comando ou diff arquivo por
  arquivo.
- "Fora do escopo" é uma linha: arquivos e motivo, só quando existirem.
- Decisão permanente não fica só aqui: vai para `AGENTS.md`, `docs/engineering-rules.md`
  ou o PRD.

## O que vai onde

- **Evidência detalhada** → `verification_evidence` no `docs/feature_list.json` e na
  mensagem de commit.
- **Estado atual** → `docs/session-handoff.md` (reescrito a cada sessão).
- **Histórico técnico** → commits, PRs e arquivos do projeto (`docs/sql/`, etc.).
- **`docs/progress.md`** → resumo do que mudou, se a verificação passou, o que ficou
  pendente e o próximo passo.

## Arquivamento

- Quando o arquivo passar de ~300 linhas, mova as sessões mais antigas para
  `docs/progress-archive.md` (append-only, mais recente no topo) e mantenha no
  `progress.md` as ~10 últimas.
- Arquivar não é apagar: o histórico continua no repositório e o contexto lido a cada
  sessão fica enxuto.

## Regras de geração
- Na Etapa 3, gere apenas o cabeçalho e o template de entrada — não invente uma
  "sessão 1" fictícia com conteúdo que nunca aconteceu.
