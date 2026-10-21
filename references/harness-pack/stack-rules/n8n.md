# Regras — n8n

Leia só quando o projeto usar n8n. O resultado vai para `docs/engineering-rules.md`; não
copie isto para o AGENTS.md.

- Credenciais ficam no n8n (credentials), nunca no JSON exportado nem no repositório.
- Workflows exportados e versionados no repo.
- Webhook com autenticação e validação da entrada antes de executar.
- Contrato de dados entre app e n8n descrito no PRD (payload, campos, resposta).
- A verificação de feature de n8n é a evidência de uma execução de teste real; não há
  teste unitário para o workflow. Registre o resultado em `verification_evidence` no
  `docs/feature_list.json`; em `docs/progress.md`, só uma linha resumindo.
