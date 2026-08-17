# Entrevista de alinhamento (Etapa 1)

Duas partes, sempre nesta ordem: bloco fixo primeiro, sabatina depois. Nunca inverta —
a pergunta de intenção comercial (bloco fixo) é o gatilho condicional do Pilar 1, e o
Pilar 3 depende do stack já levantado no bloco fixo.

## Parte A — Bloco fixo

Apresente as perguntas abaixo de uma vez, como uma lista única — nunca uma por vez, e
nunca em pares. Pule qualquer pergunta cuja resposta já esteja clara na descrição
original do projeto.

1. Nome do projeto (ou provisório)
2. Greenfield (do zero) ou já existe repositório/código em andamento?
3. Este projeto é só teste/portfólio pessoal, ou você pretende torná-lo produto real ou
   monetizado? (Define se o Pilar 1, na Parte B, cobra validação de mercado/monetização
   ou não.)
4. Linguagem/framework já definidos, ou quer sugestão?
5. Frontend, backend, banco de dados, hospedagem — o que já está decidido?
6. Existe alguma referência visual/orientação de design já definida para este projeto?
   Pode ser: um `DESIGN.md` existente, print, link do Figma, Google Stitch, outro style
   guide, um MCP já conectado (ex.: Figma, Google Stitch), ou uma skill nomeada que você
   já usa/quer usar (ex.: `frontend-design`). Se não houver nada definido, siga sem
   forçar nenhuma diretriz de design.
7. Como você vai saber que uma feature está pronta? (testes automatizados, checklist
   manual, os dois)
8. Já existe algum comando de build/test/lint no projeto, ou isso ainda vai ser
   definido do zero?
9. Qual(is) ferramenta(s) de codificação com IA você vai usar nesse projeto? (Claude
   Code, OpenCode, Cursor, outro)
10. Tem algum plugin ou framework de desenvolvimento instalado globalmente nessa
    ferramenta (ex.: um plugin "always-on" no OpenCode)? Se sim, qual? Isso importa
    porque um plugin global pode competir com o fluxo do Harness — ver aviso na Etapa 3.

## Parte B — Sabatina adaptativa (5 pilares)

Sem lista fixa de perguntas aqui — ataque o pilar com a lacuna mais grave a cada
rodada, até os 5 estarem cobertos. Nunca repita o que o bloco fixo já revelou.

### Os 5 pilares
1. **Problema e Motivação** — a ideia resolve uma fricção real? Se a resposta 3 (bloco
   fixo) indicar projeto de teste/portfólio, NÃO cobre validação de mercado/modelo de
   negócio — só que a fricção é genuína. Só cobra monetização se a resposta 3 indicar
   intenção comercial real.
2. **Lógica de Negócio e UX** — fluxo de entrada, ação principal do usuário, o que
   garante uso contínuo. Inclua aqui a pergunta de como o usuário vai medir sucesso do
   projeto como um todo.
3. **Viabilidade Técnica** — a arquitetura faz sentido pro stack já levantado no bloco
   fixo (resposta 4/5)? Como o frontend consome dados? Gargalos em integrações de API
   ou automações em background? Inclua aqui o levantamento de integrações obrigatórias.
4. **Casos Limite** — o que quebra se o usuário agir de forma inesperada, um webhook
   falhar, ou dados vierem incompletos?
5. **Escopo do MVP** — o escopo declarado está enxuto o suficiente para validação
   rápida? (Reforçado depois, na Etapa 3, pelo walking skeleton/F001 — aqui é sobre o
   escopo declarado, antes de qualquer feature existir.)

### Como conduzir
1. Leia a ideia / última resposta do usuário.
2. Identifique o pilar com a lacuna mais grave ainda não coberta.
3. Faça no máximo DUAS perguntas diretas sobre essa lacuna específica.
4. Espere a resposta — nunca presuma.
5. Se a resposta for rasa/defensiva, aponte a falha e peça profundidade (respeitando o
   teto abaixo). Se for sólida, marque o pilar como coberto e siga para o próximo com
   lacuna mais grave.
6. Repita até os 5 pilares estarem cobertos.

### Teto de rodadas (circuit breaker)
Máximo de ~3 idas e voltas por pilar. Se ainda sobrar lacuna depois disso, NÃO trave o
fluxo — registre o que ficou em aberto como risco/`[a definir]` no PRD (Riscos e
premissas, ou Perguntas em aberto) e siga para o próximo pilar. Existe para não gastar
contexto/créditos à toa numa sabatina longa demais para um projeto pessoal pequeno.

### Regra de ouro
Só avance para a Etapa 2 (gerar o PRD) depois que os 5 pilares estiverem cobertos — por
validação completa ou por saída via teto com lacunas registradas. Isto estende o gate de
aprovação que a skill já usa (nada é gerado sem aprovação) também para a fase de
descoberta, não só para edição de artefato já criado.
