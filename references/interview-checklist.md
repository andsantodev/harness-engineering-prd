# Entrevista de alinhamento (Etapa 1)

Duas partes, sempre nesta ordem: sabatina primeiro, bloco fixo depois. A sabatina
debate a ideia até você estar convencido de que o escopo é sólido; o bloco fixo só
formaliza o que ainda não apareceu nesse debate (nome definitivo, ferramenta de
codificação, comandos). Nunca inverta.

**Resposta recomendada:** em toda pergunta de **escolha técnica** (stack, banco,
hospedagem, libs, estilização, Cache Components, provedor de auth), sugira uma resposta
recomendada ao lado da pergunta, para o usuário confirmar ou corrigir. Em perguntas de
**produto e negócio** (problema, público, UX, métricas, escopo do MVP), nunca sugira
resposta — o usuário é a fonte da verdade. O teto de rodadas continua valendo.

## Parte 1 — Sabatina adaptativa (5 pilares)

### Postura
Mantenha uma postura rigorosa e analítica ao longo da sabatina — seu objetivo não é
aprovar a ideia rápido, é submetê-la a um teste de estresse real antes de qualquer
documento ser gerado. Não aceite resposta superficial ou defensiva só para manter a
conversa fluindo: aponte a falha e peça profundidade. Isso não é hostilidade — é o
valor do processo. Só suavize isso ao atingir o teto de rodadas abaixo, nunca por
conta própria.

Sem lista fixa de perguntas aqui — ataque o pilar com a lacuna mais grave a cada
rodada, até os 5 estarem cobertos.

### Os 5 pilares
1. **Problema e Motivação** — comece perguntando se o projeto é só teste/portfólio
   pessoal ou se o usuário pretende torná-lo produto real/monetizado; isso decide o
   resto do pilar. Se for teste/portfólio, valide só que a ideia resolve uma fricção
   real, sem cobrar validação de mercado ou modelo de negócio. Se houver intenção
   comercial real, aprofunde: a ideia resolve uma dor real e monetizável, ou é solução
   em busca de problema?
2. **Lógica de Negócio e UX** — fluxo de entrada, ação principal do usuário, o que
   garante uso contínuo. Inclua aqui a pergunta de como o usuário vai medir sucesso do
   projeto como um todo.
3. **Viabilidade Técnica** — a arquitetura faz sentido? Se a stack ainda não estiver
   clara, pergunte o suficiente aqui para avaliar viabilidade (o bloco fixo formaliza
   os detalhes depois). Como o frontend consome dados? Gargalos em integrações de API
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
contexto/créditos à toa numa sabatina longa demais para um projeto pessoal pequeno. O
teto limita quanto tempo insiste, não quão firme é a pergunta enquanto insiste — a
postura rigorosa vale até o último round.

## Parte 2 — Bloco fixo (finalização)

Só começa depois que a Parte 1 cobrir os 5 pilares. Apresente as perguntas abaixo de
uma vez, como uma lista única — nunca uma por vez, nunca em pares. Pule qualquer
pergunta cuja resposta já tenha saído durante a sabatina ou da descrição original do
projeto.

1. Nome do projeto (ou provisório)
2. Greenfield (do zero) ou já existe repositório/código em andamento?
3. Linguagem/framework já definidos, ou quer sugestão?
4. Frontend, backend, banco de dados, hospedagem — o que já está decidido?
5. (Só para frontend React/Next) Estilização: Tailwind (padrão) ou outra?
6. (Só se o stack for Next.js) Cache Components? Padrão: não. Se sim, o projeto liga
   `cacheComponents` e usa `<Suspense>`/`'use cache'`; o Passo 0 deixa de passar
   `--no-cache-components`.
7. (Só se o projeto tiver autenticação) Qual provedor/estratégia? Usa Supabase Auth? Se
   não tiver autenticação, pule esta pergunta.
8. Existe alguma referência visual/orientação de design já definida para este projeto?
   Pode ser: um `DESIGN.md` existente, print, link do Figma, Google Stitch, outro style
   guide, um MCP já conectado (ex.: Figma, Google Stitch), ou uma skill nomeada que você
   já usa/quer usar (ex.: `frontend-design`). Se houver um `DESIGN.md` pronto, ele entra
   como `docs/DESIGN.md` e vira a referência citada no AGENTS.md. Se não houver nada
   definido, siga sem forçar nenhuma diretriz de design.
9. Como você vai saber que uma feature está pronta? (testes automatizados, checklist
   manual, os dois)
10. Já existe algum comando de build/test/lint no projeto, ou isso ainda vai ser
    definido do zero?
11. Qual(is) ferramenta(s) de codificação com IA você vai usar nesse projeto? (Claude
    Code, OpenCode, Cursor, outro)
12. Tem algum plugin ou framework de desenvolvimento instalado globalmente nessa
    ferramenta (ex.: um plugin "always-on" no OpenCode)? Se sim, qual? Isso importa
    porque um plugin global pode competir com o fluxo do Harness — ver aviso na Etapa 3.
13. O projeto usa a skill `code-review-lite`? Se sim, o critério de revisão de segurança
    da feature final de inspeção cita essa skill pelo nome; se não, use um critério
    genérico de revisão de segurança antes do push.

## Regra de ouro
Só avance para o Bloco fixo (Parte 2) depois que a sabatina declarar os 5 pilares
cobertos — por validação completa ou por saída via teto com lacunas registradas. Só
avance para a Etapa 2 (gerar o PRD) depois que o Bloco fixo estiver respondido. Isto
estende o gate de aprovação que a skill já usa (nada é gerado sem aprovação) também
para a fase de descoberta, não só para edição de artefato já criado.
