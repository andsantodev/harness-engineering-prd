# Notas de verificação / bootstrap (equivalente ao init.sh)

Esta skill não executa comandos — ela entrega o conteúdo pronto para o usuário salvar
como `init.sh` (ou rodar manualmente) no próprio projeto.

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
