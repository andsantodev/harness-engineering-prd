# Notas de verificação / bootstrap (docs/verification-notes.md + init.sh)

Esta skill não executa comandos. Ela entrega dois artefatos:

- `docs/verification-notes.md` — as notas: Passo 0 de criação do projeto, comandos de
  bootstrap e o que conferir para a verificação falhar de verdade.
- `init.sh` na raiz do projeto — o health check de **bootstrap** (não é documento, por
  isso fica fora de `docs/`), pronto para o usuário salvar e rodar.

## Passo 0: criar o projeto (feito pelo usuário, manualmente)

A skill **nunca** executa o comando. Ela apenas registra, com o nome do projeto como
placeholder (`<nome>`), o comando do stack escolhido. Ordem:

1. Criar o projeto com o comando abaixo.
2. Adicionar os scripts de bootstrap ao `package.json` (seção seguinte).
3. Copiar `AGENTS.md` e a pasta `docs/` para a raiz do projeto criado. O
   `create-next-app` recusa pasta com arquivos pré-existentes, por isso a cópia vem
   **depois** da criação.
4. Rodar `init.sh`.
5. Abrir o agente de codificação na pasta do projeto.

### Next.js

```bash
npx create-next-app@latest <nome> --typescript --eslint --app --src-dir \
  --import-alias "@/*" --tailwind --no-cache-components --no-agents-md \
  --no-react-compiler --no-agent-feedback --use-npm --yes
```

- `--tailwind` vira `--no-tailwind` se a entrevista escolher outra estilização.
- `--no-cache-components` sai do comando se a entrevista responder "sim" para Cache
  Components; nesse caso ligue `cacheComponents: true` e `partialPrefetching: true` no
  `next.config.ts`.
- `--use-npm` fixa o npm como gerenciador de pacotes e vence a preferência salva. Sem
  ele, `--yes` reaproveita preferências salvas para o que não foi passado — inclusive o
  gerenciador. Se você escolher pnpm/yarn/bun, troque por `--use-pnpm`, `--use-yarn` ou
  `--use-bun` e mantenha o mesmo gerenciador em todos os comandos.
- `--no-agents-md` evita o `AGENTS.md`/`CLAUDE.md` gerados pelo create-next-app, que
  colidiriam com o AGENTS.md do harness.
- Com Next >= 16.3, o `next dev` injeta um bloco gerenciado no `AGENTS.md`. A F001 deve
  definir `agentRules: false` no `next.config.ts` — antes de aplicar, confira se a versão
  instalada suporta a opção (foi adicionada na 16.3).
- O script `lint` precisa existir antes do `init.sh`; se o template não trouxer, adicione-o
  no Passo 0 junto com o `type-check`. A F001 pode refiná-lo (regras de `any`,
  `no-console`, etc.).
- O test runner não vem no template: Vitest + React Testing Library é critério da F001.

#### Scripts de bootstrap do Next

O template do Next 16 cria `dev`, `build`, `start` e `lint` — **não** cria `type-check`,
`test:unit` nem `test:e2e`. Adicione ao `package.json` (sem instalar dependência nova):

```json
{
  "scripts": {
    "type-check": "next typegen && tsc --noEmit",
    "lint": "eslint . --max-warnings 0"
  }
}
```

- O `next typegen` (Next >= 15.5) gera os tipos de rota (`LayoutProps`, `PageProps`) em
  `.next/types`. Sem ele, `tsc --noEmit` falha num projeto limpo com
  `Cannot find name 'LayoutProps'`.
- Se o template já tiver um `lint` funcional, mantenha-o e apenas acrescente
  `--max-warnings 0`; não troque o comando por outro linter.
- Os scripts de teste continuam sendo entregáveis da F001.

### Se a pasta já contém o harness

Quando o repositório já tem `AGENTS.md` e `docs/` (harness criado antes do app), o
`create-next-app` recusa a pasta. Crie o projeto numa pasta temporária e mescle, sem
sobrescrever os arquivos do harness:

```bash
# rode o comando do stack acima apontando para <nome>-app
rsync -a --exclude='.git/' <nome>-app/ <nome>/
rm -rf <nome>-app
```

Não traga o `.git/` da pasta temporária: o repositório deve continuar sendo o do harness
(se ele ainda não tiver um, rode `git init` na pasta final). Depois do merge, adicione os
scripts de bootstrap ao `package.json` e confira que nenhum arquivo do harness foi
sobrescrito antes de rodar `init.sh`.

### React (Vite)

```bash
npm create vite@latest <nome> -- --template react-ts
cd <nome>
npm install
```

O template já traz ESLint. O test runner (Vitest + React Testing Library) não vem: é
critério da F001. O npm é o padrão; se o projeto usar pnpm/yarn/bun, troque os comandos
equivalentes e não misture gerenciadores.

### Outros stacks

Registre o comando equivalente informado pelo usuário ou peça o comando a ele. Não
invente flags.

## Bootstrap x Definition of Done

- **Bootstrap (`init.sh`)** — prova que o projeto instala, linta, tipa e builda logo
  depois de criado. É um health check de ambiente, não uma feature pronta.
- **Definition of Done** — é o gate de cada feature: o `verification_command` da feature
  roda typecheck, lint e testes e precisa terminar sozinho.
- **Testes** — só entram no bootstrap depois que a F001 instalar e configurar o runner.
  Antes disso, `init.sh` pula a etapa com uma mensagem explícita; não invente
  `test:unit`/`test:e2e`.
- **Build** — no bootstrap é um smoke build. Na Definition of Done, o build de produção
  entra no final do projeto (feature de inspeção de release).

## O que incluir no init.sh
1. Instalação de dependências (npm por padrão; use o mesmo gerenciador do projeto)
2. Lint (com `--max-warnings 0`)
3. Typecheck (`next typegen && tsc --noEmit` no Next)
4. Build de smoke
5. Testes **somente** se o runner já existir; caso contrário, pule com aviso (F001)

## Faça a verificação falhar de verdade

Comando que sempre passa não verifica nada. Antes de confiar no `verification_command`:

- `tsc --noEmit` valida tipos, mas **não proíbe `any` explícito**. Para isso é preciso uma
  regra de lint (`@typescript-eslint/no-explicit-any` como `error`).
- As regras de hooks do React (`react-hooks/*`) costumam vir como **warning**, não erro.
  Rode o lint com `--max-warnings 0` (ou promova as regras a erro) para que warning
  quebre a verificação.
- Ligue `no-console` (error ou warning + `--max-warnings 0`) para não deixar log de debug
  ir para produção.
- Teste o caminho de falha: quebre algo de propósito uma vez e confirme que o comando
  retorna código de saída diferente de zero. Se não retornar, o harness não tem evidência.

## Formato de entrega

Entregue o `init.sh` com o gerenciador escolhido (o exemplo usa npm; troque por
`pnpm`/`yarn`/`bun` se for o caso):

```bash
#!/usr/bin/env bash
set -euo pipefail
cd "$(dirname "$0")"

echo "Instalando dependências..."
npm install

echo "Lint..."
npm run lint

echo "Typecheck..."
npm run type-check

echo "Build de smoke..."
npm run build

echo "Test runner: será criado na F001 — pulando."
echo "Bootstrap OK."
```

- `install` sem `--frozen-lockfile` no bootstrap; frozen fica para CI.
- O `cd "$(dirname "$0")"` permite rodar o script da raiz ou de qualquer subpasta,
  inclusive `docs/`.
- Se o projeto já tiver test runner, troque a linha de skip por `npm test`.
- Se a entrevista não trouxer informação suficiente sobre os comandos reais do stack, use
  os comandos padrão mais comuns para aquele stack e marque com um comentário
  `# confirmar` no lugar do comando.
