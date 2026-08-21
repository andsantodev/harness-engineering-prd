# Harness Engineering PRD

Uma Skill do Claude que transforma a descrição de um projeto de software em um **PRD** (Product Requirements Document) completo e um pacote de documentos **"Harness"** — `AGENTS.md`, `feature_list.json`, `progress.md`, `session-handoff.md` — a base de instruções e estado que qualquer agente de codificação de IA (Claude Code, OpenCode, Cursor e outros) precisa para trabalhar de forma confiável ao longo de várias sessões, sem perder contexto.

Baseada nos padrões do curso *Learn Harness Engineering* ([walkinglabs/learn-harness-engineering](https://github.com/walkinglabs/learn-harness-engineering)). A entrevista de alinhamento começa com uma sabatina adaptativa em 5 pilares que testa a ideia, e só depois passa por um bloco fixo de perguntas diretas (stack, ferramentas, design) para fechar o que não apareceu na sabatina.

🇺🇸 Prefer English? See [README.md](README.md).

---

## Como funciona

Sempre 3 etapas, nesta ordem. Nenhum documento é gerado antes do gate anterior ser cumprido.

1. **Entrevista** — sabatina adaptativa em 5 pilares que testa a ideia primeiro, depois um bloco fixo que fecha o que ainda não foi coberto.
2. **PRD** — gerado, apresentado e aguarda sua aprovação explícita. Nada mais é gerado até você aprovar.
3. **Pacote Harness** — só depois da aprovação: `docs/PRD.md`, `AGENTS.md`, `feature_list.json`, `progress.md`, `session-handoff.md`, notas de verificação/bootstrap, além do arquivo nativo do(s) agente(s) de codificação escolhido(s) (`CLAUDE.md` para Claude Code, `.cursor/rules/*.mdc` para Cursor, suporte direto no OpenCode).

Esta skill cobre **apenas descoberta e documentação** — ela não escreve código do projeto nem executa scripts. Ela prepara o terreno para um agente de codificação (ou um fluxo harness-creator) assumir a partir daí.

## Instalação

Duas formas de instalar: o CLI `skills` (mais rápido — funciona com Claude Code, Cursor, OpenCode, Codex e outros), ou copiando a pasta crua manualmente. As duas leem direto deste repositório no GitHub — sem precisar de hospedagem separada.

### Opção A — CLI

```bash
npx skills add andsantodev/harness-engineering-prd
```

Isso baixa `SKILL.md` + `references/` deste repositório e instala automaticamente na pasta de skills do seu agente. Para instalar em um agente específico:

```bash
npx skills add andsantodev/harness-engineering-prd -a claude-code
```

Adicione `-g` para instalar globalmente em vez de só no projeto atual. Documentação completa: [skills.sh/docs/cli](https://skills.sh/docs/cli).

### Opção B — Manual

Este repositório contém a **pasta crua da skill** (`SKILL.md` + `references/`) — não um arquivo `.skill` zipado. Copie para o lugar certo da sua ferramenta.

**claude.ai**
Settings → Capabilities → Skills → faça upload da pasta `harness-engineering-prd` (ou de um zip `.skill` dela).

**Claude Code**
Skills seguem o padrão aberto "Agent Skills". Copie a pasta para:
- `.claude/skills/harness-engineering-prd/` (escopo de projeto), ou
- `~/.claude/skills/harness-engineering-prd/` (disponível em todos os projetos)

**OpenCode / Cursor**
Ambos leem o mesmo formato `SKILL.md`. Copie a pasta para o diretório de skills da sua ferramenta — confira a documentação atual da ferramenta para o caminho exato, já que isso muda rápido entre os agentes.

```bash
git clone https://github.com/andsantodev/harness-engineering-prd.git
cp -r harness-engineering-prd/SKILL.md harness-engineering-prd/references \
      <pasta-de-skills-do-seu-agente>/harness-engineering-prd/
```

## Prompts de início rápido

Use estes prompts depois de instalar a skill, já dentro de um projeto.

<details>
<summary><strong>1. Início de projeto</strong></summary>

```markdown
Leia AGENTS.md, docs/PRD.md, session-handoff.md e feature_list.json antes de fazer qualquer coisa.

Comece pela feature **POR PROJETO: ID da primeira feature elegível — normalmente a que não tem depends_on** — ela não tem dependências e as demais (ou a maioria) dependem dela:

1. Implemente o código necessário.
2. **POR PROJETO: se o projeto usa uma skill de design própria (ex. frontend-design), inclua aqui "use a skill [nome] para decisões de layout/visuais"**
3. Rode o comando de verificação: npm run test:unit
4. Se os testes passarem, atualize os arquivos:
   - feature_list.json (mude status para "done" e insira a evidência)
   - progress.md (adicione o registro do que foi feito)
   - session-handoff.md (atualize o estado atual e a próxima tarefa)

Não avance para nenhuma outra funcionalidade sem minha aprovação.
```
</details>

<details>
<summary><strong>2. Próximas features</strong></summary>

```markdown
Consulte o AGENTS.md e o session-handoff.md.

Selecione a PRÓXIMA funcionalidade com status "todo" na feature_list.json cujas dependências estejam concluídas (respeitando o campo depends_on).

Trabalhe EXCLUSIVAMENTE nessa funcionalidade:
1. Desenvolva o código seguindo os critérios de aceite definidos no JSON.
2. Execute o comando de verificação configurado para essa feature (ex: npm run test:unit ou npm run test:e2e).
3. Mostre o resultado dos testes aqui no chat.
4. Se tudo estiver aprovado, atualize o feature_list.json, progress.md e session-handoff.md.
```
</details>

<details>
<summary><strong>3. Modo loop autônomo</strong></summary>

```markdown
Trabalhe nas próximas features elegíveis de feature_list.json em sequência, seguindo o AGENTS.md (uma feature por vez, verificação antes de concluir).

Para CADA funcionalidade:
1. Identifique a próxima feature pendente ("todo") com dependências concluídas.
2. Desenvolva o código necessário.
3. Execute rigorosamente o comando de verificação correspondente (ex: npm run test:unit ou npm run test:e2e).

REGRAS DE CONDIÇÃO DE PARADA E SEGURANÇA:
- 🔴 SE OS TESTES FALHAREM OU OCORRER ERRO DE BUILD/LINT: PARE O LOOP IMEDIATAMENTE. Não marque a funcionalidade como "done". Explique o erro no chat e aguarde minha intervenção.
- 🟢 SE OS TESTES PASSAREM COM SUCESSO: Atualize feature_list.json (status "done" + evidência), progress.md e session-handoff.md — SEMPRE os três juntos, a cada feature concluída (não deixe o session-handoff.md só pra pausa final: se o loop for interrompido no meio, ele precisa refletir a última feature realmente concluída).
- 🟡 SE NÃO HOUVER NENHUMA FEATURE "todo" COM DEPENDÊNCIAS SATISFEITAS (tudo "done" ou "blocked"): PARE O LOOP. Isso também é uma condição de parada — não é falha, mas não há trabalho seguro pra continuar. Explique o estado no chat.
- Em seguida (fora dos dois casos acima), passe AUTOMATICAMENTE para a próxima funcionalidade pendente sem me perguntar.

Limite de segurança: Processe no máximo 4 funcionalidades nesta rodada e depois faça uma pausa para revisão. Pode iniciar!
```
</details>

<details>
<summary><strong>4. Correção de bugs ou falhas nos testes</strong></summary>

```markdown
A feature **A CADA USO: ID da feature** falhou na verificação com este resultado:

**A CADA USO: colar aqui a saída do comando de verificação / erro / teste que falhou**

Não marque a feature como concluída. Investigue a causa raiz, corrija, rode a verificação de novo do zero (não assuma que passou) e só então atualize feature_list.json com o status real e a evidência.
Registre em progress.md o que quebrou e como foi corrigido.
```
</details>

<details>
<summary><strong>5. Encerramento / handoff de sessão</strong></summary>

```markdown
Vamos encerrar esta sessão de desenvolvimento. Por favor:
1. Rode os comandos gerais de verificação: npm run lint e npm run build.
2. Garanta que o repositório está em um estado limpo e sem erros de compilação.
3. Atualize o arquivo session-handoff.md descrevendo exatamente o que foi feito nesta sessão, quais arquivos foram modificados e qual é a instrução exata para retomar no F00X.
4. Atualize o progress.md com os últimos avanços.
5. Só faça commit se o projeto estiver num estado seguro para retomar depois — se não estiver, me avise antes de commitar.
```
</details>

<details>
<summary><strong>6. Retomada de nova sessão</strong></summary>

```markdown
Leia atentamente os arquivos AGENTS.md, progress.md e, em especial, o session-handoff.md para entender o contexto atual do projeto e onde a sessão anterior foi encerrada.

Antes de escrever ou alterar qualquer código:
1. Execute os comandos de verificação inicial para confirmar que o repositório está num estado saudável e compilável.
2. Identifique no session-handoff.md e na feature_list.json qual é a próxima funcionalidade pendente (status "todo") cujas dependências já foram concluídas.
3. Resuma brevemente para mim em qual funcionalidade você vai trabalhar nesta sessão e aguarde minha confirmação para começar.

Lembre-se: trabalhe em APENAS UMA funcionalidade por vez (WIP = 1) e só atualize a documentação e o status para "done" após rodar e passar nos testes de verificação.
```
</details>

## Exemplo: o que é gerado

Trecho apenas ilustrativo (construído para este README, não extraído de um teste real) — uma amostra pequena de `feature_list.json` e `AGENTS.md` pra mostrar o formato da saída:

```json
// feature_list.json (trecho)
{
  "id": "F001",
  "title": "Walking skeleton: fluxo ponta a ponta com estado vazio",
  "status": "todo",
  "depends_on": [],
  "acceptance_criteria": [
    "Rota inicial renderiza com fonte de dados real conectada (mesmo que vazia)",
    "npm run test:unit passa"
  ]
}
```

```markdown
<!-- AGENTS.md (trecho) -->
## Invariantes
- Uma feature ativa por vez (WIP = 1).
- Nunca marcar uma feature como "done" sem evidência do comando de verificação.
- Fatos específicos do projeto (stack, convenções) vivem nos docs do projeto, não na skill.
```

## Licença

[MIT](LICENSE).

