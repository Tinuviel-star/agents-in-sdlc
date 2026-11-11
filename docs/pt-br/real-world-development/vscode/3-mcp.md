---
title: "Exercício 3 - Teste a funcionalidade com o servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Exercício anterior: Modo de agente][previous-lesson] · [Próximo exercício: Agentes personalizados →][next-lesson]

Você acabou de criar a funcionalidade de filtragem com o modo de agente. Antes de abrir um pull request, confirme se ela funciona no navegador. Em vez de testar o aplicativo manualmente, conecte o **servidor Playwright MCP** e deixe o Copilot controlar um navegador real para testar a funcionalidade. Depois, publique sua branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas disponíveis ao Copilot.
- adicionar o servidor Playwright MCP ao espaço de trabalho.
- pedir ao Copilot que use o servidor para testar manualmente a funcionalidade no navegador.
- publicar sua branch e abrir um pull request do trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica muito mais poderoso quando pode acessar ferramentas fora do editor. O Model Context Protocol (MCP) é o padrão que permite ao Copilot se comunicar com ferramentas e serviços externos.

![Diagrama da interação entre modo de agente, contexto, modelo de linguagem grande (LLM) e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de inteligência artificial se comuniquem com ferramentas e serviços externos. Com o MCP, os agentes acessam informações atualizadas usando recursos e executam ações em seu nome usando ferramentas.

Essas ferramentas e esses recursos são acessados por um servidor MCP, que atua como ponte entre o agente e as ferramentas e serviços externos. O servidor gerencia a comunicação entre o agente e as ferramentas externas, como interfaces de programação de aplicações (APIs) existentes ou ferramentas locais — por exemplo, pacotes npm. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente pode acessar.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: fornece acesso a APIs para gerenciar repositórios do GitHub. Ele permite que o agente crie repositórios, atualize os existentes e gerencie issues e pull requests.
- **[Servidor Playwright MCP](https://github.com/microsoft/playwright-mcp)**: oferece recursos de automação de navegador com Playwright. Ele permite que o agente acesse páginas, preencha formulários e selecione botões.

Há muitos outros servidores MCP que fornecem acesso a diferentes ferramentas e recursos. O GitHub mantém um [registro de MCP](https://github.com/mcp) para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Por segurança, trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise o código-fonte, verifique o publicador e considere as implicações de segurança. Use somente servidores MCP confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revise a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste espaço de trabalho do VS Code.

1. Abra `.vscode/mcp.json` no Codespace.
2. Você verá um servidor `github` já configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

Essa entrada `github` vem do template do projeto e permitiu que o Copilot lesse o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não é necessário instalar nada localmente. O VS Code se conecta pelo protocolo de transferência de hipertexto (HTTP), e você autentica com o GitHub na primeira vez que o Copilot usa uma das ferramentas.

## Adicione o servidor Playwright MCP

Agora, adicione um segundo servidor. O [servidor Playwright MCP][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar, o que permitirá testar a funcionalidade.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, deixando o arquivo assim:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, o `playwright` é um servidor **local**: o VS Code o inicia no computador executando `npx @playwright/mcp@latest`. A opção `--headless` faz com que o Playwright execute o navegador sem uma janela visível, o que é necessário em um Codespace sem ambiente gráfico.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para testes de ponta a ponta, então o navegador necessário geralmente já está instalado. Se o Copilot informar que não há um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Inicie e aprove o servidor

O VS Code inicia os servidores MCP **sob demanda**: quando o Copilot usa as ferramentas de um servidor pela primeira vez, o VS Code o inicia e pede que você confirme se confia nele.

1. Em `.vscode/mcp.json`, aparecerá a ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor agora e confirmar se a configuração está correta.
2. Se o VS Code solicitar confirmação de confiança no servidor, revise a configuração e escolha confiar nele para permitir a inicialização.

Iniciá-lo manualmente é opcional. Se você ignorar esta etapa, o VS Code iniciará o servidor e solicitará que você confie nele quando o Copilot usar as ferramentas na próxima etapa.

## Teste a funcionalidade de filtragem

O [servidor Playwright MCP][playwright-mcp-server] fornece ao Copilot um navegador real. Em vez de navegar pelo aplicativo para verificar o trabalho, o agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumir o que observou. Assim, você verifica rapidamente o comportamento da funcionalidade sem sair da conversa.

Nos bastidores, o servidor Playwright MCP usa a [árvore de acessibilidade][playwright-mcp-server] da página, não capturas de tela. Isso significa que o agente analisa elementos estruturados e rotulados — como botões, links e itens de lista — da mesma forma que a tecnologia assistiva. Portanto, uma verificação funcional rápida também serve como uma avaliação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste a funcionalidade de filtragem:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador pelo servidor Playwright MCP, executará cada etapa e relatará o que encontrou. Compare o resumo com os critérios de aceitação da issue. Se algo parecer incorreto, faça perguntas complementares ou peça que o Copilot corrija o código antes de abrir o pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` para esse teste. Se você encerrou o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor Playwright MCP, talvez seja necessário baixar um navegador. Se o Copilot informar que não há um navegador, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publique a branch e crie um pull request

Depois de confirmar que a funcionalidade funciona, abra um pull request (PR) para que a equipe possa revisá-la. Primeiro, publique a branch `filtering-vscode`.

1. Abra o painel **Source Control** no Codespace e revise as mudanças feitas pelo Copilot.
2. Prepare as mudanças selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Painel Source Control com as mudanças feitas pelo Copilot](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Crie o pull request

Há várias formas de criar um pull request, inclusive pelo github.com e pela interface de linha de comando do GitHub (CLI). Como você já está usando o GitHub Copilot, peça que ele crie o PR. Ele pode localizar a issue relevante e associá-la ao PR.

1. Abra o painel Copilot Chat e selecione **New Chat** para iniciar uma sessão nova.
2. Confirme se **Agent** está selecionado na lista de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie o PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot reúna informações e execute operações. Na primeira vez que ele usar uma ferramenta do GitHub, talvez seja necessário autenticar. Siga as instruções para permitir o acesso.
5. Observe como o Copilot pesquisa as issues, encontra a correta e cria o PR.
6. Selecione o link gerado pelo Copilot para revisar o pull request, mas **não o mescle ainda**.

## Resumo e próximos passos

Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas disponíveis ao Copilot.
- adicionou o servidor Playwright MCP ao espaço de trabalho.
- usou o servidor para testar manualmente a funcionalidade de filtragem no navegador antes de publicá-la.
- publicou a branch e abriu um pull request do trabalho de filtragem.

Você usou um servidor MCP para *testar* uma funcionalidade, mas o MCP é apenas uma das formas de ampliar os recursos do Copilot. Em seguida, [crie um agente personalizado][next-lesson] para facilitar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor Playwright MCP da Microsoft][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro de MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
