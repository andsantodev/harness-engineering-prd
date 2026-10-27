# Regras — Next.js (App Router)

Leia só quando o stack do projeto for Next.js. O resultado vai para
`docs/engineering-rules.md`; não copie isto para o AGENTS.md.

- Antes de escrever código Next, leia o guia relevante em
  `node_modules/next/dist/docs/` — a doc instalada corresponde à versão do projeto.
- `tsc --noEmit` sozinho não valida os tipos de rota do Next 16: rode `next typegen`
  antes. O script `type-check` deve ser `next typegen && tsc --noEmit`, senão um projeto
  limpo falha com `Cannot find name 'LayoutProps'`/`PageProps`.
- Módulos de serviço/banco usam `import 'server-only'` para não vazarem para o client.
- Segredo nunca em `NEXT_PUBLIC_*`: tudo com esse prefixo vai para o browser.
- `'use client'` só onde há interatividade (estado, evento, efeito). O resto fica como
  Server Component.
- Busque dados independentes com `Promise.all` em vez de `await` em cascata.
- Server Action que muta dado chama `revalidatePath`/`revalidateTag` no caminho de
  sucesso.
- Server Action valida autenticação **e** o schema de entrada antes de tocar no banco.
- Com Next >= 16.3, defina `agentRules: false` no `next.config.ts` (o `next dev` escreve
  um bloco gerenciado no `AGENTS.md`, que aqui é o arquivo canônico do harness).

## Cache Components (só se a entrevista respondeu "sim")

- `cacheComponents: true` e `partialPrefetching: true` no `next.config.ts`.
- Dado dinâmico: componente dentro de `<Suspense>` com fallback (o shell estático
  continua sendo servido).
- Dado cacheado: `'use cache'` com `cacheLife(...)` e, quando houver mutação,
  `cacheTag(...)` + `revalidateTag(...)`.
- Dentro de `'use cache'` não use `cookies()`/`headers()`/`searchParams` diretamente:
  extraia o valor fora e passe como argumento, ou use `'use cache: private'`.
- Cache Components exige runtime Node: não exporte `runtime` no route segment.
- Confirme os detalhes na doc instalada antes de implementar.
