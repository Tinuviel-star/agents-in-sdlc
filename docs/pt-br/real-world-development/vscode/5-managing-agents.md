---
title: "Exercício 5 - Monitoramento e gerenciamento de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Exercício anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você usa o GitHub Copilot no modo de agente, ele trabalha de forma autônoma: explora a base de código, propõe mudanças, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real no seu computador, o Copilot Chat no VS Code mostra cada chamada de ferramenta e edição de arquivo à medida que ocorre. Você pode revisar cada diferença no próprio editor, aceitar ou rejeitar mudanças individuais e orientar a conversa com prompts complementares para aprimorar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- acompanhar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições de arquivos propostas.
- orientar o agente durante a sessão para adicionar um controle de modo claro junto ao modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design pediu também um controle de modo claro para pessoas que consideram o tema padrão escuro demais. Em vez de começar uma conversa do zero, você orientará a sessão existente para ampliar o trabalho.

## Acompanhe o progresso do Copilot

A visualização Copilot Chat mostra cada etapa executada pelo agente: os arquivos explorados, os comandos executados e as edições propostas. Cada edição aparece na conversa com uma diferença e os controles **Keep** e **Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra a conversa para acompanhar as chamadas de ferramentas. Você verá leituras e edições de arquivos e os comandos de terminal solicitados.
3. Quando o agente propuser uma edição, a diferença aparecerá na conversa:
   - Selecione **Keep** para aceitar a mudança.
   - Selecione **Undo** para desfazê-la e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver todos os arquivos alterados pelo agente durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão, por exemplo, para executar um comando de terminal, o agente pausará e exibirá uma solicitação **Allow**. Aprove somente os comandos que considerar seguros.

5. Aguarde o agente concluir a implementação do modo de alto contraste. Você deverá ver uma mensagem final que resume as mudanças.

## Oriente o agente para adicionar o modo claro

Com o modo de alto contraste implementado, amplie a mesma conversa para adicionar um controle de modo claro. Como você continuará na mesma sessão de chat, o agente manterá o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o padrão de armazenamento local.

1. No campo de entrada do Copilot Chat, envie este prompt complementar:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. O agente continuará o trabalho, explorando as mudanças já feitas e propondo adições para o modo claro.
3. Revise cada nova edição quando aparecer, como fez com o modo de alto contraste. Use **Keep** ou **Undo** em cada diferença.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar os controles de modo de uma maneira que você não quer —, interrompa-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem complementar para esclarecer o pedido.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts complementares para pedir ajustes, como "simplifique as folhas de estilo em cascata (CSS) — use variáveis CSS em vez de duplicar seletores"; corrigir a direção, como "não adicione um terceiro controle; integre o modo claro ao painel de configurações existente"; ou ampliar o escopo, como "adicione atalhos de teclado para os dois controles".

## Revise o conjunto de mudanças

Antes de fazer commit, revise rapidamente tudo o que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, nos estilos e em testes relacionados.
3. Abra alguns dos arquivos alterados e examine as diferenças. Confirme se a implementação reflete as práticas de acessibilidade do agente personalizado: atributos de Accessible Rich Internet Applications (ARIA), navegação pelo teclado, HyperText Markup Language (HTML) semântica e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das mudanças pelo painel Source Control. Você as publicará no [próximo exercício][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou mudanças e enviou prompts complementares para ampliar o trabalho sem perder o contexto.

Neste exercício, você:

- acompanhou o trabalho do agente em tempo real.
- aceitou e rejeitou edições individuais.
- orientou a sessão em andamento com prompts complementares.

No [próximo exercício][next-lesson], publique o trabalho de acessibilidade em um pull request (PR) e revise as mudanças produzidas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Exercício anterior: Agentes personalizados][previous-lesson] | [Próximo exercício: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
