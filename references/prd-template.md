# Template de PRD

Preencha cada seção usando as respostas da entrevista. Onde faltar informação
suficiente para preencher com segurança, escreva `[a definir]` — nunca invente números,
prazos ou decisões técnicas que o usuário não deu.

**Idioma e destino:** ao contrário dos documentos "canônicos" da Etapa 3 (AGENTS.md,
docs/feature_list.json, adaptadores de agente), que ficam em inglês, o PRD é escrito no mesmo
idioma da conversa — é documentação de produto para humanos, não instrução operacional
para o agente de codificação. Depois de aprovado, ele é entregue também como arquivo
`docs/PRD.md`, junto com o resto do pacote Harness da Etapa 3.

## 1. Resumo executivo
2-3 frases: o que é o produto/feature e por que ele importa.

## 2. Problema e motivação
O problema real que motivou o projeto. O que acontece hoje sem ele.

## 3. Objetivos (Goals)
Lista objetiva do que o projeto precisa alcançar.

## 4. Não-objetivos (Non-goals)
O que fica explicitamente fora de escopo nesta fase — tão importante quanto os
objetivos.

## 5. Usuários e jornada
Quem usa isso e em que contexto. Inclua o fluxo principal em alto nível — como o
usuário entra, qual é a ação central que ele repete, o que faz ele voltar. Puxe da
sabatina (Pilar 2, Lógica de Negócio e UX) em vez de reinventar.

## 6. Requisitos funcionais
O que o sistema precisa fazer, numerado, um item por linha. Esta lista vira a base do
`docs/feature_list.json` na Etapa 3 — escreva cada item já pensando em ser quebrado em
uma feature verificável.

## 7. Requisitos não-funcionais
Performance, segurança, escalabilidade, acessibilidade, custo — só o que for relevante
para esse projeto específico.

## 8. Referências de design (opcional)
Só inclua se a entrevista trouxe algo (bloco fixo, pergunta de design): `DESIGN.md`
existente, print, link
do Figma, Google Stitch, style guide, MCP conectado (ex.: Figma, Google Stitch), ou uma
skill nomeada (ex.: `frontend-design`). Registre o que foi indicado e, se for um arquivo,
onde vai viver no projeto (ex.: `docs/DESIGN.md` ou `docs/design/`) — a adição do arquivo
em si é manual, feita pelo usuário depois. Se nada foi indicado, omita esta seção e não
force nenhuma diretriz de design no restante do PRD/Harness.

## 9. Métricas de sucesso
Como saber que o projeto funcionou, em termos mensuráveis sempre que possível.

## 10. Riscos e premissas
O que pode dar errado, e o que está sendo assumido como verdade sem confirmação.

## 11. Perguntas em aberto
Reúna aqui, num só lugar, tudo que ficou marcado como `[a definir]` nas seções acima.

## 12. Fases / Milestones (opcional)
Só inclua se o projeto tiver fases claras vindas da entrevista. Não invente um
cronograma se o usuário não deu um.
