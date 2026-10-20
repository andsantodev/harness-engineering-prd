# Modo "PRD existente"

Use este modo quando o usuário já tem um PRD (arquivo, documento colado ou descrição
longa já estruturada) e quer o pacote Harness a partir dele. Não use quando o PRD **e**
o harness já existem — nesse caso a skill não tem o que gerar.

Neste modo, a Etapa 1 (entrevista) é substituída pela leitura do PRD + levantamento de
lacunas. O aviso de "nunca pule etapas" do fluxo padrão continua valendo para o modo
normal; aqui os gates que permanecem são o de aprovação do PRD e o da Etapa 3.

## Fluxo

1. **Leia o PRD do usuário sem reescrevê-lo.** Se ele só descreveu o PRD na conversa,
   peça o texto/arquivo. Não "melhore" o documento por conta própria.
2. **Compare com `references/prd-template.md` e com o bloco fixo de
   `references/interview-checklist.md`.** Monte mentalmente o que já está respondido.
3. **Pergunte apenas o que falta para montar o Harness**, pulando tudo o que o PRD já
   responde: stack, backend/auth, agente de codificação usado, referência de design,
   comandos de verificação e se o projeto usa a skill `code-review-lite`.
4. **Registre as lacunas numa lista curta** e marque cada uma como `[a definir]` no PRD.
   Não invente decisão técnica, número ou prazo.
5. **Avise se o PRD não tiver requisitos funcionais verificáveis.** Sem isso, o
   `docs/feature_list.json` sai com features fracas ("funciona bem", "tela pronta") e o
   harness não consegue provar nada. Peça que os requisitos sejam reescritos de forma
   observável ou registre o risco explicitamente.
6. **Peça aprovação** do PRD (com as correções do usuário, se houver) e salve em
   `docs/PRD.md`, no idioma da conversa, conforme a nota de idioma do template.
7. **Siga para a Etapa 3 normalmente** (AGENTS.md, `docs/feature_list.json`,
   `docs/progress.md`, `docs/session-handoff.md`, `docs/verification-notes.md`,
   `init.sh`, `docs/engineering-rules.md` e adaptadores do agente).

## Regras

- O PRD do usuário é a fonte da verdade: correções só entram com o aval dele.
- Lacuna que não bloqueia o harness não vira pergunta; fica como `[a definir]`.
- Não repita a sabatina de 5 pilares: o PRD já passou por ela (ou não vai passar agora).
- Se o PRD não tiver seção de não-objetivos ou riscos, não invente — registre a ausência
  como lacuna.
