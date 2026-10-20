# Regras — React + Vite

Leia só quando o stack do projeto for React com Vite. O resultado vai para
`docs/engineering-rules.md`; não copie isto para o AGENTS.md.

- Variáveis `VITE_*` são públicas: aparecem no bundle e qualquer pessoa consegue ler.
  Segredo fica no backend.
- Rota protegida no cliente é só UX (esconder tela, redirecionar). A autorização real é
  validada no backend, em toda requisição.
- Existe uma rota 404 de verdade, não só um fallback genérico do roteador.
