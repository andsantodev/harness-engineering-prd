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
