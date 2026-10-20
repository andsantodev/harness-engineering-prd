# Notas de verificação / bootstrap (docs/verification-notes.md + init.sh)

Esta skill não executa comandos. Ela entrega dois artefatos:

- `docs/verification-notes.md` — as notas: Passo 0 de criação do projeto, comandos e o
  que conferir para a verificação falhar de verdade.
- `init.sh` na raiz do projeto — o script executável (não é documento, por isso fica
  fora de `docs/`), pronto para o usuário salvar e rodar.

## O que incluir
1. Instalação de dependências (comando do gerenciador de pacotes do stack escolhido)
2. Comando de build (se aplicável)
3. Comando de testes
4. Comando de lint/typecheck
5. Um "health check" simples — algo rápido que confirma que o ambiente está OK antes de
   começar a trabalhar

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

Entregue como um bloco de código bash, pronto para colar num arquivo `init.sh`:

```bash
#!/usr/bin/env bash
set -e

echo "Instalando dependências..."
[comando real]

echo "Rodando verificação..."
[comando real]

echo "Ambiente OK."
```

Se a entrevista não trouxer informação suficiente sobre os comandos reais do stack, use
os comandos padrão mais comuns para aquele stack e marque com um comentário
`# confirmar` no lugar do comando.
