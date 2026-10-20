# Como gerar o docs/feature_list.json

Cada requisito funcional do PRD vira um item aqui — quebrado em pedaços pequenos o
bastante para caber numa única sessão de agente.

## Schema

```json
{
  "features": [
    {
      "id": "F001",
      "title": "Título curto",
      "description": "O que precisa ser feito, em 1-2 frases",
      "status": "todo",
      "acceptance_criteria": [
        "Critério verificável 1",
        "Critério verificável 2"
      ],
      "verification_command": "comando que prova que está pronto",
      "verification_evidence": null,
      "depends_on": []
    }
  ]
}
```

`status` é sempre um de: `todo`, `in_progress`, `done`, `blocked`.

`verification_evidence` começa `null` e só é preenchido por um agente depois de rodar o
comando de verificação e registrar (ou resumir) o resultado — nunca preencha isso na
geração inicial.

## Regras de geração
- Um item por requisito funcional do PRD, quebrado em partes menores quando o requisito
  for grande demais para uma sessão.
- `acceptance_criteria` sempre verificável — nunca algo subjetivo como "funciona bem".
- Preencha `depends_on` quando uma feature precisar de outra pronta antes (use os `id`s).
- Todas as features começam como `"status": "todo"` e `"verification_evidence": null` —
  isso é gerado uma única vez, no início do projeto.
- `verification_command` precisa ser algo que termina sozinho e diz passou/falhou (ex.:
  build, teste automatizado, query, lint). Nunca inclua ali um comando que sobe servidor
  e fica pendurado esperando alguém olhar (ex.: `npm run dev`, watch mode) — isso trava
  qualquer execução sem supervisão humana constante. Se a feature tiver critério visual
  (comum em telas), ele continua fazendo parte do `acceptance_criteria` e é conferido
  manualmente — mas como evidência registrada à parte (ex.: "conferido no navegador:
  rotas X/Y/Z carregam"), nunca dentro do `verification_command` automático.

### Walking skeleton (obrigatório para projetos com mais de uma tela/rota)
Features de produto construídas isoladas, sem nada que já as ligue umas às outras,
resultam em componentes que passam na verificação individual mas nunca formam um app
navegável (tela em branco no final, mesmo com tudo "done"). Para evitar isso:
- `F001` é sempre o esqueleto do app: roteamento básico, navegação entre as telas
  previstas, e um shell mínimo — sem lógica de negócio ainda. Todas as outras features
  dependem dela (`"depends_on": ["F001"]` no mínimo).
- `acceptance_criteria` de F001 inclui algo verificável no navegador, não só testes
  unitários — ex.: "app abre e renderiza a rota inicial sem tela em branco", "navegação
  entre as rotas previstas funciona".
- Se o projeto tiver uma única tela/rota (ex.: landing page, CLI, script), F001 pode ser
  a primeira feature de produto normalmente — o esqueleto só se justifica quando há mais
  de uma tela/rota para conectar.

### Ordenação de features transversais
`depends_on` correto garante que nada quebra — não garante que nada seja retrabalhado.
Uma feature "transversal" (design system, layout compartilhado além do skeleton,
contrato de API compartilhado, etc.) pode estar tecnicamente liberada desde o início do
projeto e mesmo assim acabar no fim do array, porque nada no grafo de dependências
obriga ela a vir antes. Um agente que processa o array em sequência segue a posição, não
a intenção — o resultado é páginas construídas com estilo/estrutura genérica e depois
retrofitadas, trabalho duplicado.
- Ao montar o array (não como passada de revisão depois), posicione toda feature
  transversal — cujo entregável outras features vão consumir estrutural ou
  visualmente, mesmo sem depender dela no grafo — o mais cedo que suas dependências
  reais permitirem, logo após o walking skeleton (F001) e antes de qualquer feature de
  página/produto individual que vá consumi-la.
