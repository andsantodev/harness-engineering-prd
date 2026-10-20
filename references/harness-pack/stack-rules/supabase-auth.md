# Regras — Supabase Auth

Leia só quando o projeto tiver autenticação com Supabase. O resultado vai para
`docs/engineering-rules.md`; não copie isto para o AGENTS.md.

- RLS habilitado em todas as tabelas, com políticas explícitas. `service_role` nunca vai
  para o client.
- Em Next.js, use `@supabase/ssr` (não só `supabase-js`) para a sessão viver em cookie
  HttpOnly e ser lida no servidor.
- No servidor, valide a sessão com `getClaims()` — ele verifica o JWT contra o JWKS e é o
  método recomendado pela doc atual. Use `getUser()` em ações sensíveis ou quando for
  preciso garantir que a sessão não foi revogada.
- Tratar `getSession()` no servidor como dado não verificado (ele só lê o cookie local,
  sem validar o JWT). Nunca autorize com base nele.
- A doc ressalva: se o projeto não usa chave de assinatura assimétrica (ECC/RSA),
  `getClaims()` cai para uma chamada ao Auth server, como `getUser()`.
- Middleware leve: só renovação/checagem de sessão, sem consulta pesada a banco.
- `onAuthStateChange` no `useEffect` deve cancelar a assinatura no cleanup.
