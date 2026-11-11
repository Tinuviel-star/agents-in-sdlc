---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Exercício anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que quase todas as organizações enfrentem algum tipo de dívida técnica. Pode ser uma questão de segurança não resolvida, código legado que precisa de atualização ou uma solicitação de funcionalidade que ficou esquecida no backlog por falta de tempo. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele gera um pull request (PR) em rascunho para revisão por uma pessoa desenvolvedora. Assim, é possível delegar tarefas repetitivas, acelerar o desenvolvimento e se concentrar em questões mais abrangentes.

Neste exercício, você explorará como o agente de nuvem do Copilot pode:

- personalizar o ambiente usado para gerar código.
- executar operações de forma segura.
- trabalhar com issues bem delimitadas.
- receber issues atribuídas a ele.

## Cenários

A Tailspin Toys quer resolver parte de sua dívida técnica. As pessoas contratadas inicialmente para criar a primeira versão do site não deixaram a documentação em boas condições: você verá que ela está totalmente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer facilitar a descoberta de jogos. Ela gostaria que a página de detalhes de cada jogo mostrasse outros títulos da mesma categoria para que as pessoas continuem explorando o catálogo. A equipe ainda não precisa de uma implementação refinada; quer algo que possa usar para testes de aceitação da experiência do usuário (UX). Embora seja um bloqueio, há outras issues de prioridade mais alta no momento.

Esses são exemplos de tarefas que podem ser adiadas e que são adequadas para atribuir ao agente de nuvem do Copilot. Ele trabalha de forma assíncrona, permitindo que a pessoa desenvolvedora se concentre em outras tarefas e depois volte para revisar o trabalho.

## Conheça o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano de maneira semelhante a uma pessoa desenvolvedora. Há várias formas de atribuir trabalho, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois de receber a atribuição, o Copilot cria um pull request em rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão durante o trabalho ou depois da conclusão. Quando a solução proposta estiver pronta para revisão, o Copilot marcará você no pull request.

## A importância de instruções bem delimitadas

O GitHub Copilot não faz mágica: você ainda precisa pensar. Mesmo operações aparentemente simples podem se tornar complexas quando examinadas com mais atenção. Por isso, [defina com cuidado o escopo das tarefas do agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com inteligência artificial: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a inclusão da inteligência artificial generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram instruções personalizadas e como elas orientam o Copilot. Se você já fez o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação. Caso contrário, aproveite para examiná-lo agora. Antes de atribuir trabalho ao agente de nuvem do Copilot, leia os arquivos de instruções incluídos no repositório, sem alterá-los, para depois identificar seus efeitos.

Abra os arquivos a seguir na interface Web do GitHub ou em um Codespace que já esteja em execução:

- `.github/copilot-instructions.md`: examine a seção **Code standards**, principalmente as expectativas para convenções TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que associa as instruções aos arquivos `**/*.test.ts`.

Na próxima seção, quando você atribuir ao agente de nuvem a issue `Code lacks documentation`, observe se o pull request inclui comentários TSDoc, segue as convenções TypeScript e contém cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções e serão mencionadas novamente ao revisar os PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente de nuvem

Em geral, o desenvolvimento de código exige um ambiente específico e scripts de configuração para garantir que tudo esteja pronto. O mesmo vale para tarefas atribuídas ao Copilot, que trabalha de forma semelhante a uma pessoa desenvolvedora.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente com um [workflow de configuração especial][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, que é executado antes do início da tarefa. Isso disponibiliza as ferramentas e dependências necessárias. O workflow já está configurado para este laboratório: ele fornece acesso ao Node.js, às dependências do projeto, aos navegadores do Playwright para testes de ponta a ponta e ao banco de dados SQLite migrado e preenchido com dados para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina apenas as permissões mínimas necessárias para estas etapas. O Copilot receberá seu próprio token para as operações dele.
    permissions:
      # Se as etapas de configuração precisarem clonar o repositório, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Se não houver clonagem nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo: Node.js (o aplicativo agora é Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e preenche o banco de dados SQLite local usado pelo build e pelos testes.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer outro workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar o workflow sem esperar que o Copilot o execute.

## Adicione documentação

A maioria dos projetos tem informações desatualizadas ou não tem documentação. Essa dívida técnica pode reduzir a produtividade, dificultar a manutenção da base de código e tornar mais difícil integrar novas pessoas à equipe. O Copilot pode ajudar a criar documentação, então esta é uma issue adequada para atribuir ao agente de nuvem. Ele trabalhará em segundo plano; você revisará o resultado em um exercício futuro.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Code lacks documentation`.
6. No campo **Description**, digite:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

   ![Detalhes da atribuição da issue ao Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request recém-criado, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer na lista, aguarde um ou dois minutos e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está preparando o ambiente, executando o workflow descrito anteriormente, analisando o projeto e definindo uma abordagem.

13. Revise a lista e as tarefas que o Copilot pretende executar.
14. Role a linha do tempo do pull request para baixo. Você deverá ver uma atualização indicando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

   ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Percorra a sessão em tempo real e observe como o Copilot resolve a tarefa: ele explora o código e entende o estado do projeto, pausa para pensar e definir uma abordagem e, então, cria código.

Isso pode levar alguns minutos. Um dos objetivos principais do agente de nuvem do Copilot é executar tarefas de forma assíncrona para que você possa se concentrar em outras atividades. Aproveite esse recurso atribuindo outra tarefa ao agente e, em seguida, volte sua atenção para o desenvolvimento de funcionalidades do aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Uma das vantagens do agente de nuvem do GitHub Copilot é dividir o trabalho: enquanto você se concentra em uma tarefa, o agente pode cuidar de outra. Mesmo que a seção de jogos relacionados não leve muito tempo para ser implementada, esse tempo pode ser usado em outras atividades. Atribua essa tarefa ao agente de nuvem.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Show related games on the game details page`.
6. No campo **Description**, digite:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver o emoji 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot agora está trabalhando na segunda solicitação. Como o agente de nuvem trabalha de forma semelhante a uma pessoa desenvolvedora, não é necessário acompanhá-lo continuamente; revise o resultado quando estiver pronto. Agora, crie e use agentes personalizados.

## Resumo e próximos passos

Neste exercício, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para execução assíncrona. O Copilot pode ajudar a resolver dívida técnica, criar funcionalidades ou migrar código entre frameworks.

Você explorou:

- a personalização do ambiente usado para gerar código.
- a execução segura de operações.
- a importância de delimitar as issues com clareza.
- a atribuição de issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, você pode criar e usar agentes personalizados. O agente de nuvem do Copilot também pode usar [servidores do Model Context Protocol (MCP)][cloud-agent-mcp] e tem acesso a instruções personalizadas, que foram abordadas em exercícios anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próximo exercício: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
