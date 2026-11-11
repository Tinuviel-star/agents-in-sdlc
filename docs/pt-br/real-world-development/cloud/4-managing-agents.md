---
title: "Exercício 4 - Monitoramento e gerenciamento de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você atribuiu três tarefas diferentes ao agente de nuvem do Copilot para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente de nuvem trabalhe de forma assíncrona e autônoma, ainda é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nessa central de controle, você vê todas as tarefas do agente que têm pull requests (PRs) abertos. Também pode examinar as operações realizadas e orientar uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar outra funcionalidade.

## Cenário

Depois que a equipe atribuiu ao agente a criação do modo de alto contraste, percebeu que seria útil adicionar também um modo claro. Como o trabalho já inclui mudanças de estilo e controles para alternar modos, faz sentido incluir essa funcionalidade. Oriente o agente para que ele adicione o modo claro junto com o modo de alto contraste.

## Revise as tarefas do agente de nuvem do Copilot

Confira o estado atual das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Ela deve incluir as tarefas atribuídas ao Copilot, como:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações realizadas pelo Copilot. Elas podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores do Model Context Protocol (MCP).
   - executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho e a abordagem do Copilot.

4. Observe também o painel do pull request à direita. Ele permite examinar o pull request (PR) e os arquivos alterados.

## Oriente o agente de nuvem

Agora que você viu as tarefas em andamento, peça ao Copilot que inclua o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente ao modo de alto contraste. O título exato varia de acordo com o nome usado pelo Copilot e o estado atual do trabalho.

   ![Sessão de acessibilidade no painel de controle](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que a configuração seja concluída e o trabalho comece. Você verá mensagens semelhantes às apresentadas no exercício.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Captura de tela da tarefa do agente de nuvem na página de agentes com a caixa de diálogo para orientar a sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento do prompt e o inclui no fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot trabalhará na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa que estava executando quando você enviou a mensagem.

Isso também pode levar alguns minutos. Aproveite para fazer uma pausa e refletir sobre o que aprendeu até aqui.

## Resumo e próximos passos

Neste exercício, você explorou a página de agentes do Copilot, a central para acompanhar e orientar as tarefas do agente de nuvem do GitHub Copilot. Nela, você pode acompanhar tarefas ativas e concluídas, revisar o trabalho e redirecionar uma sessão em andamento para ajustar o escopo ou fornecer mais orientações.

Você explorou:

- como usar a central de controle e a página de agentes para acompanhar as tarefas do agente de nuvem.
- como redirecionar uma sessão em andamento para solicitar outra funcionalidade.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, prepare-se para [iterar nos pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Exercício anterior: Agentes personalizados][previous-lesson] | [Próximo exercício: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
