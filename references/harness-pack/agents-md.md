# Como gerar o AGENTS.md canônico

O AGENTS.md é o arquivo de instrução raiz, agnóstico de agente de codificação — a fonte da verdade
para a qual todo o resto (CLAUDE.md, `.cursor/rules`, etc.) aponta de volta. Ele NÃO deve
virar um manual completo do projeto: seguindo o curso, mantenha-o curto — roteamento e
invariantes, não documentação de arquitetura.

## Estrutura recomendada

```markdown
# AGENTS.md

## Sobre este projeto
[1-2 frases — puxadas do resumo executivo do PRD]
Contexto completo do produto: ver `docs/PRD.md`.
[Se a entrevista trouxe referência de design (bloco fixo) — arquivo, MCP ou skill
nomeada: adicionar uma linha curta dizendo qual é (ex.: "Referência de design: ver
`docs/DESIGN.md`" ou "Referência de design: skill `frontend-design`"). Sem nada
mencionado na entrevista, NÃO adicionar essa linha nem forçar nenhuma diretriz de
design — o usuário quer liberdade para escolher/testar isso por conta própria.]

## Antes de começar (toda sessão)
1. Leia docs/progress.md — o que aconteceu na última sessão
2. Leia docs/feature_list.json — o que está feito, o que falta
3. Rode os comandos de verificação abaixo para confirmar que o projeto está num estado
   saudável
   - Na primeira sessão, antes da F001, rode só install/lint/type-check/build; os scripts
     de teste só passam a existir com a F001

## Como trabalhar aqui
- Escolha exatamente UMA feature não concluída de docs/feature_list.json por vez (WIP=1)
- A primeira feature (F001) é o esqueleto do app (rotas + navegação + shell mínimo,
  rodando no navegador) e também cria a estrutura de pastas, a rota 404 e o test runner,
  se ainda não existirem — não pule para features de produto sem ela estar concluída
- [Se a entrevista (bloco fixo, stack/backend) revelou backend/banco real com MCP
  conectado (ex.: Supabase): adicionar "Toda criação/alteração de schema passa pelo
  MCP do backend — evite SQL de migração solto fora dele." Sem MCP conectado, omitir
  esse bullet.]
- Não marque uma feature como concluída sem rodar a verificação e registrar a evidência
- Antes de marcar `done`, rode `git diff --stat`, compare os arquivos com o
  `acceptance_criteria` da feature e justifique em docs/progress.md qualquer arquivo fora
  do escopo
- [Se o stack for Next.js: "Antes de trabalhar em Next, leia o guia relevante em
  `node_modules/next/dist/docs/` — é a doc da versão instalada."]
- [Se `docs/engineering-rules.md` foi gerado: "Antes de mexer em auth, Server Actions,
  banco ou workflows n8n, leia `docs/engineering-rules.md`." Sem esse arquivo, omitir.]
- Use o gerenciador de pacotes do lockfile do projeto (npm, pnpm, yarn ou bun) em todos
  os comandos; nunca misture gerenciadores
- Documente decisões e fatos específicos do projeto em `docs/`, não aqui

## Estrutura de pastas
[Heurística — registre a escolha aqui e no PRD. Até ~15 telas: por responsabilidade,
com as rotas no diretório que o framework exige (`src/app/` no Next, `src/pages/` ou
equivalente no Vite) e `components/`, `services/` (acesso a dados/API), `hooks/`,
`types/`, `lib/` fora das rotas. Acima de ~15 telas: por feature —
`features/<nome>/` com componentes, hooks, serviços e tipos; as rotas ficam finas e só
compõem a feature. Em Next, `src/app/` só para rotas; em Vite, o diretório de rotas do
template. Para outro stack, adapte ao que a ferramenta exige.]

## Definition of Done
**Automática** (é o que o `verification_command` roda — precisa terminar sozinho e
retornar falha quando algo quebra): typecheck, lint e testes. Testes só a partir da F001,
quando o runner existir. Build entra ao final do projeto, não em toda feature.

**Julgamento** (confira antes de marcar done):
- Nenhuma dependência nova sem aprovação do usuário
- Sem código morto, comentado ou não usado
- A solução mais simples que atende ao critério, sem abstração antecipada
- Cada critério de aceite conferido, não só "os testes passaram"

## Comandos de verificação
[preencher com os comandos reais informados na entrevista — build, test, lint. Se o
usuário não informou, escreva "[a definir]" e sugira os comandos padrão do stack
escolhido. Cada feature usa o seu `verification_command`; este bloco reúne o conjunto]

## Limites (o que NÃO fazer sem aprovação explícita)
- Nunca commitar `.env`, chaves de API, tokens ou outras credenciais
- Não adicionar dependência nova sem aprovação explícita do usuário
[Se o backend for Supabase (ou outro com RLS): adicionar também "Habilitar RLS em toda
tabela nova; nunca expor a `service_role` key no frontend/client-side; toda política de
acesso deve ser explícita — nunca 'permitir tudo' como atalho." Sem backend com RLS,
omitir este bloco.]
[demais limites específicos do projeto — ex.: não alterar schema de banco fora do MCP
combinado acima, não trocar dependências de produção, não fazer push direto — ajustar
conforme o projeto]

## Ao final de cada sessão
1. Atualize docs/progress.md com o que foi feito e verificado — entrada curta (~10
   linhas), sem código, tabelas ou logs completos; o detalhe vai para
   `verification_evidence` e para a mensagem de commit
2. Atualize docs/feature_list.json
3. Atualize docs/session-handoff.md com o estado atual e a próxima feature sugerida
4. Só faça commit se o projeto ficar num estado seguro para retomar depois
```

## Regras de geração
- Preencha "Sobre este projeto" com o resumo executivo do PRD aprovado, nunca com o PRD
  inteiro.
- Puxe os comandos de verificação da resposta sobre build/test/lint no bloco fixo. Se
  não houver, sugira o comando padrão do stack informado e marque como a confirmar.
- A linha de "Referência de design" só existe quando a entrevista trouxe algo explícito
  (bloco fixo, pergunta de design) — nunca por padrão. O mesmo vale para o bullet do
  MCP em "Como trabalhar aqui" e para o bloco de RLS em "Limites": só aparecem quando o
  bloco fixo revelar backend real com MCP/RLS.
- Mantenha o arquivo final abaixo de ~100 linhas. Se o projeto tiver muita informação
  específica, ela vai para `/docs` (referenciado, não copiado para dentro do AGENTS.md).
- A Definition of Done é curta de propósito: os detalhes de como fazer a verificação
  falhar de verdade (typecheck, lint, testes) vão para `docs/verification-notes.md`, e as
  regras específicas de stack para `docs/engineering-rules.md`.
- A estrutura de pastas é heurística, não dogma: adapte os nomes ao stack, registre a
  escolha no PRD e mantenha a seção com no máximo ~15 linhas.
- A linha de `docs/engineering-rules.md` só entra quando esse arquivo foi gerado para o
  stack do projeto; nunca crie um arquivo vazio só para justificar a linha.
