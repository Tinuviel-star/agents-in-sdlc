---
title: "Exercício 5: Itere sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Exercício anterior: Gerenciamento de agentes][previous-lesson] |
|:--|

## Revise o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse uma funcionalidade de jogos relacionados para a equipe de design avaliar e implementasse recursos de acessibilidade, incluindo modos de alto contraste e claro. Agora, examine as mudanças no código e, se necessário, envie comentários ao Copilot para melhorar o resultado.

### Cenário

Como já destacamos várias vezes, os fundamentos do design de software e de desenvolvimento e operações (DevOps) não mudam com a inclusão da inteligência artificial generativa. Sempre revise o código gerado e siga o processo normal de DevOps. Revise as sugestões do GitHub Copilot para a documentação, a funcionalidade de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão direta, há restrições de segurança para proteger o ambiente:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O agente de nuvem é executado no GitHub Actions e cria um ambiente separado e temporário para trabalhar.
- Uma pessoa precisa aprovar todos os workflows do GitHub Actions antes da execução.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores do Model Context Protocol (MCP).

## Revise a documentação gerada

Comece explorando o primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot: o PR para adicionar documentação ao código. Use a interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao abrir o PR, você poderá ver um aviso de que o firewall bloqueou o GitHub Copilot. Isso é **esperado**: por padrão, o Copilot tem acesso limitado a recursos externos, incluindo chamadas a servidores MCP externos. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem do Copilot][agent-firewall].

1. Volte ao seu repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com um título semelhante a **Add missing documentation**. O título pode ser mais detalhado.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]** (*work in progress*, em andamento). Nesse caso, aguarde a conclusão, que pode levar alguns minutos. Aproveite para fazer uma pausa ou refletir sobre o que aprendeu.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as mudanças.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que inclui os novos comentários TSDoc e outros elementos de documentação. As mudanças exatas podem variar.

   Procure comentários TSDoc, convenções TypeScript e cabeçalhos de comentário. Eles vêm dos arquivos de instruções personalizadas que você examinou no início do Exercício 2.

6. Depois da revisão, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns workflows aguardam aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Botão Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão aparecer na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Peça mudanças ao GitHub Copilot

O trabalho com o Copilot em um pull request não é uma via de mão única. Você pode mencionar o Copilot em comentários do PR — como faria com outra pessoa da equipe — ou em comentários inline no código. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados são não determinísticos, não há um texto exato para prescrever. Você pode pedir, por exemplo, que o Copilot:

- adicione cabeçalhos de comentário no início de cada arquivo de código, descrevendo brevemente sua finalidade.
- adicione comentários TSDoc a arquivos TypeScript e Astro.
- crie um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma mudança na documentação gerada e mencione **@copilot**, como faria com qualquer pessoa. Use uma das sugestões acima ou peça outra melhoria de documentação para a base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe como ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao pull request.

   ![Botão Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as mudanças, deverá aparecer um novo commit no pull request.
5. Selecione a guia **Files changed** para revisar as mudanças.

Continue iterando até ficar satisfeito. Depois, você poderá marcar o PR como pronto para revisão e mesclá-lo à branch principal.

![Opção para marcar um PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise a funcionalidade de jogos relacionados

Agora, volte ao PR criado pelo Copilot para resolver a issue sobre exibir jogos relacionados na página de detalhes.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page**. O título pode ser mais detalhado.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois da revisão, volte à guia **Conversation** e role a página para baixo.
6. Você deverá ver um indicador de que alguns workflows aguardam aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Botão Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows deverão aparecer na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você pode alternar para essa branch no Codespace para testar manualmente a funcionalidade de jogos relacionados. Acesse o Codespace, abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, como **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot criou a funcionalidade de jogos relacionados. Como antes, você pode iterar com o agente de nuvem e pedir atualizações, por exemplo, mudar o número de jogos relacionados ou adicionar cabeçalhos de comentário e comentários TSDoc. Lembre-se de que essa tarefa foi atribuída **antes** da atualização das instruções personalizadas. Para pedir mudanças, adicione um comentário na guia **Conversation**; o Copilot iniciará outra sessão.

## Revise os recursos de acessibilidade

Por fim, revise os recursos de acessibilidade implementados pelo agente personalizado. Este PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no painel de controle durante o Exercício 4.

1. Volte ao seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website**. O título pode ser mais detalhado.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão, que pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, com atenção especial a:
   - componentes da interface do usuário (UI) para alternar entre os modos.
   - uso do armazenamento local para manter as preferências.
   - mudanças de folhas de estilo em cascata (CSS) ou de estilo para os modos de alto contraste e claro.
   - atributos de acessibilidade, como rótulos de Accessible Rich Internet Applications (ARIA) e navegação pelo teclado.
   - código JavaScript/TypeScript que gerencia a alternância entre modos.
6. Depois da revisão, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns workflows aguardam aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Botão Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão aparecer na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** você pode alternar para essa branch no Codespace para testar manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os modos de alto contraste e claro no navegador. Confirme se funcionam e se as preferências permanecem depois de recarregar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot a implementar esses recursos seguindo práticas recomendadas de acessibilidade. Se encontrar problemas ou melhorias possíveis, mencione **@copilot** em um comentário para solicitar atualizações, como fez nos outros PRs.

## Exercício opcional — continue delegando

O agente de nuvem é mais útil quando você pode delegar itens reais do backlog e se dedicar a outras tarefas. Para praticar, crie mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na página de listagem de jogos.
- adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu a trilha do agente de nuvem. Ao longo dos exercícios, você:

- **Examinou os arquivos de instruções personalizados incluídos no repositório** para observar mais tarde seu efeito nos resultados do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como central de controle** para acompanhar e orientar a sessão de acessibilidade em andamento.
- **Revisou e iterou nos pull requests do agente de nuvem**, mencionando `@copilot` para solicitar mudanças e aprovando workflows.

## Revisão e próximos passos

Você concluiu a trilha do agente de nuvem. Para continuar explorando, as outras trilhas complementam o que você praticou aqui:

- 🖥️ **[Trilha do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no ambiente de desenvolvimento integrado (IDE).
- 💻 **[Trilha do Copilot CLI](../../cli/)** — entregue duas mudanças revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente de garantia de qualidade (QA) personalizado, Playwright MCP, Agent Merge e uma ponte opcional `/delegate` para o agente de nuvem.

No seu repositório, experimente estas ideias:

- atribuir ao agente de nuvem uma issue de refatoração ou de cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Exercício anterior: Gerenciamento de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
