# Como gerar o feature_list.json

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
