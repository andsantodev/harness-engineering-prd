---
name: harness-engineering-prd
description: Gera um PRD (Product Requirements Document) e o pacote completo de documentos Harness (AGENTS.md canônico, feature_list.json, progress.md, session-handoff.md, notas de verificação/bootstrap) a partir da descrição de um projeto de software, seguindo os padrões do curso Learn Harness Engineering. Também gera os arquivos de configuração nativos do IDE/plataforma escolhida (CLAUDE.md para Claude Code, .cursor/rules/*.mdc para Cursor, suporte direto no OpenCode). Use sempre que o usuário descrever uma ideia de projeto, app, feature ou produto e pedir ajuda para começar, estruturar, documentar ou preparar o repositório para um agente de codificação de IA (Claude Code, Cursor, OpenCode, Codex etc.); ou mencionar PRD, harness, AGENTS.md, feature_list, vibe coding com specs, ou documentar antes de codar. Não use para escrever ou depurar código, nem quando o usuário já tem PRD e harness prontos.
---

# Harness Engineering — Gerador de PRD + Harness

Transforma a descrição de um projeto de software em um PRD aprovado e um pacote de
documentos Harness pronto para qualquer agente de codificação de IA trabalhar de forma
confiável. Baseada nos padrões do curso Learn Harness Engineering
(walkinglabs/learn-harness-engineering) — em especial no conceito dos 5 subsistemas de
um harness: Instructions, State, Verification, Scope e Session Lifecycle.

Esta skill cobre a parte de **descoberta e documentação** (PRD → Harness). Ela não
escreve código do projeto em si, não executa scripts, e não substitui um harness-creator
que rode dentro de um repositório real — ela prepara o terreno para um.

## Fluxo — sempre 3 etapas, nesta ordem

Nunca pule etapas. Nunca gere o pacote Harness antes do PRD ser aprovado explicitamente.

1. **Entrevista** — bloco fixo + sabatina adaptativa de 5 pilares
2. **PRD** — gerado, apresentado, aguarda aprovação (gate obrigatório)
3. **Pacote Harness** — só depois do "aprovado"

## Etapa 0 — Ambiente de saída

Decida isto antes de gerar qualquer conteúdo, e mantenha a mesma decisão do início ao
fim da conversa:

- **Se você tem uma ferramenta de criação de arquivos/artifacts** (ex.: create_file,
  artifact): gere cada documento — PRD, AGENTS.md, feature_list.json, etc. — como um
  arquivo separado, entregue para download.
- **Se você NÃO tem essa capacidade** (por exemplo, está rodando como as instruções de
  um Gemini Gem, sem ferramenta de arquivo): gere todo o conteúdo em Markdown estrito,
  direto na resposta. Use um cabeçalho `## nome-do-arquivo` antes de cada documento e um
  bloco de código para o conteúdo. Confira que todo `**negrito**`, `` `código` `` e
  bloco cercado por crases esteja aberto e fechado corretamente antes de terminar a
  resposta — uma tag quebrada derruba a formatação do resto da conversa.

## Etapa 1 — Entrevista de alinhamento

- Leia `references/interview-checklist.md` (duas partes: bloco fixo + sabatina
  adaptativa de 5 pilares).
- Releia a descrição do projeto que o usuário já deu antes de perguntar qualquer coisa:
  pré-preencha o que já dá para responder sozinho e só pergunte o que realmente falta.
- **Parte A (bloco fixo)**: apresente TODAS as perguntas restantes de uma vez, em
  formato de checklist — nunca uma pergunta por mensagem, nunca em pares. Espere a
  resposta completa antes de seguir; se vier parcial, siga com o que tiver e marque
  `[a definir]` o que faltou.
- **Parte B (sabatina)**: só começa depois da Parte A respondida. Conduza pilar por
  pilar, no máximo duas perguntas por rodada, respeitando o teto de rodadas — nunca
  apresente os 5 pilares de uma vez como se fossem uma lista fixa de perguntas.
- Só avance para a Etapa 2 depois que a Parte B declarar os 5 pilares cobertos (regra de
  ouro em `interview-checklist.md`).

## Etapa 2 — PRD (gate de aprovação)

- Leia `references/prd-template.md`.
- Preencha cada seção com as respostas da entrevista. Onde não houver informação
  suficiente, escreva `[a definir]` — nunca invente números, prazos ou decisões técnicas
  que o usuário não deu.
- Entregue o PRD completo (conforme a Etapa 0).
- Termine sempre perguntando explicitamente, por exemplo: "Esse PRD está aprovado, ou
  quer ajustar alguma parte antes de eu gerar o Harness?"
- **Não gere nenhum arquivo Harness nesta etapa**, mesmo que pareça óbvio o que viria a
  seguir.
- Só avance para a Etapa 3 depois de aprovação explícita ("aprovado", "pode seguir",
  "tá bom", ou uma rodada de ajustes seguida de confirmação). Se pedirem mudanças, edite
  o PRD e pergunte de novo.

## Etapa 3 — Pacote Harness + estrutura de pastas

1. Confirme qual(is) IDE(s)/plataforma(s) de codificação com IA serão usadas neste
   projeto (Claude Code, OpenCode, Cursor, outro) — se a entrevista já respondeu isso,
   não pergunte de novo; se não, pergunte agora antes de continuar.
2. Entregue o PRD aprovado (sem alterações) também como arquivo `docs/PRD.md` — mesmo
   idioma da conversa, conforme a nota de idioma e destino no topo do
   `references/prd-template.md`.
3. Leia `references/harness-pack/agents-md.md` e gere o `AGENTS.md` canônico
   (agnóstico de plataforma) a partir do PRD aprovado.
4. Leia `references/harness-pack/feature-list-json.md` e gere `feature_list.json` —
   quebre o escopo do PRD em features pequenas e verificáveis. Se o projeto tiver mais
   de uma tela/rota, a primeira feature é o walking skeleton (ver regra no arquivo de
   referência) — não pule essa etapa mesmo que pareça óbvio partir direto para as
   features de produto.
5. Leia `references/harness-pack/progress-md.md` e gere `progress.md` (log inicial
   vazio, pronto para a primeira sessão real).
6. Leia `references/harness-pack/session-handoff-md.md` e gere `session-handoff.md`
   (estado inicial: nenhuma sessão rodou ainda, próxima feature sugerida).
7. Leia `references/harness-pack/verification-notes.md` e gere as notas/comandos de
   bootstrap e verificação (equivalente ao `init.sh` do curso, como conteúdo pronto para
   colar — esta skill não executa nada).
8. Para cada IDE escolhido no passo 1, leia o adaptador correspondente em
   `references/ide-adapters/` e gere o arquivo nativo daquele IDE. Esses arquivos nunca
   duplicam o conteúdo do AGENTS.md — eles apontam de volta para ele.
9. Mostre a árvore de pastas final do pacote em um bloco de código.
10. Informe que todos os arquivos gerados (`docs/PRD.md`, `AGENTS.md`,
    `feature_list.json`, `progress.md`, `session-handoff.md`, notas de verificação,
    adaptadores de IDE) são estado persistente do harness e devem ir para o Git — não
    são artefatos de sessão. A única exceção seria algo específico de ambiente/segredo
    (`.env`, chaves), que esta skill não gera e cujo commit já fica proibido
    explicitamente no AGENTS.md.
11. Encerre a entrega com um aviso padrão, sempre: ferramentas de codificação com
    plugins/frameworks globais "always-on" (ex.: um plugin que se injeta antes da
    leitura do AGENTS.md) podem competir com o fluxo do Harness — sintomas: perguntas
    extras não previstas aqui, arquivos criados fora do plano, lentidão/consumo alto de
    contexto. Se isso acontecer, recomende desativar o plugin e refazer a sessão. Se a
    resposta sobre plugin global (bloco fixo) identificou um plugin específico, cite-o
    nominalmente no aviso; se não, mantenha o aviso genérico.

## Princípios herdados do curso

Curtos de propósito — são invariantes, não um manual:

- Uma feature ativa por vez (WIP=1); nunca trabalhar em várias ao mesmo tempo.
- Evidência antes de marcar "concluído" — comando de verificação + resultado, nunca
  "parece que funcionou".
- Fatos específicos do projeto (stack, decisões, convenções) vivem nos documentos do
  projeto, não dentro da skill.
- O arquivo de instrução raiz (AGENTS.md) é enxuto: roteamento e invariantes, não
  documentação completa de arquitetura.
- Preferir arquivos de estado (`feature_list.json`, `progress.md`) a depender do
  histórico da conversa.
- Nunca embutir comportamento destrutivo sem deixar explícito que precisa de aprovação
  do usuário.

## Idioma

Assuma o idioma da conversa para a entrevista e para o PRD (incluindo o arquivo
`docs/PRD.md` entregue na Etapa 3) — é documentação de produto para humanos. Para os
arquivos "canônicos" da família AGENTS.md (`AGENTS.md`, `feature_list.json`,
adaptadores de IDE), o padrão é inglês — é a convenção que essas ferramentas e o agente
de codificação que vai lê-las depois esperam. Avise essa escolha ao entregar a Etapa 3 e
ofereça gerar em português se o usuário preferir.

## Fora de escopo (v2 futura)

Geração de documentos SDD (spec-kit) a partir da mesma descrição de projeto não está
implementada nesta versão. Não tente gerar SDD mesmo que o usuário mencione — explique
que está planejado para uma versão futura desta skill.
