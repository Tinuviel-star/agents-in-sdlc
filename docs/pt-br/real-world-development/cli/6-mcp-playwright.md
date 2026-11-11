---
title: "Lição 6 - Valide a funcionalidade com o Playwright MCP"
description: "Inspecione ou configure o Playwright MCP no Copilot CLI e use-o para explorar a filtragem no navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já destacamos, escrever código envolve mais do que apenas programar. É preciso trabalhar com dados, serviços externos e automações adicionais que o Copilot pode usar. É aí que entram os servidores MCP. Eles permitem que o Copilot CLI use ferramentas e serviços que não fazem parte do próprio CLI.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionar o servidor Playwright MCP, caso ele ainda não esteja disponível.
- pedir ao agente que controle um navegador e explore a funcionalidade de filtragem.

## Cenário

Testes unitários e de ponta a ponta são importantes, mas validar mudanças na interface do usuário (UI) exige interagir com ela. Você permitirá que o Copilot use o site como uma pessoa usuária faria para automatizar a verificação de que as mudanças funcionam como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de inteligência artificial se comuniquem com ferramentas e serviços externos. Com o MCP, os agentes acessam informações atualizadas e executam ações em seu nome.

Essas ferramentas e esses recursos são acessados por um servidor MCP, que atua como uma ponte entre o agente e as ferramentas e serviços externos. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente pode acessar.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub][github-mcp]**: dá acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Servidor Playwright MCP][playwright-mcp-server]**: oferece recursos de automação de navegador com Playwright.

Há muitos outros servidores MCP disponíveis. O GitHub mantém um [registro de MCP][mcp-registry] para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise o código-fonte, verifique o publicador e considere as implicações de segurança.

## Adicione o servidor Playwright MCP

Se o servidor Playwright MCP ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como uma pessoa usuária.

> [!NOTE]
> Se o Playwright MCP já estiver configurado no Copilot CLI, ignore estas etapas e continue na próxima seção.

1. Volte ao seu Codespace.
2. No Copilot CLI, abra a caixa de diálogo para adicionar um servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Digite `playwright` como nome e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o texto a seguir na caixa de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo do MCP.

## Peça ao Copilot para explorar a funcionalidade com o Playwright

Você já confirmou manualmente que a funcionalidade funciona. Agora, use o servidor Playwright MCP disponível no Copilot para fazer a mesma verificação.

1. Use este prompt para pedir ao Copilot que valide a funcionalidade com o servidor Playwright MCP:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Não é necessário pedir explicitamente ao Copilot que use o servidor MCP, pois ele geralmente o identifica por conta própria. Mesmo assim, mencionar a ferramenta pode ajudar a direcionar o agente e tornar os resultados mais consistentes.

2. Observe as etapas que o Copilot executa no navegador para confirmar se a funcionalidade funciona.
3. Leia o relatório e confirme se tudo se comporta como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, encerrará o servidor e fornecerá um relatório.

## Resumo e próximos passos

Você usou o servidor Playwright MCP para explorar a funcionalidade em um navegador real por meio do Copilot CLI. Você:

- aprendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionou o servidor Playwright MCP, caso ele ainda não estivesse disponível.
- pediu ao agente que controlasse um navegador e explorasse a funcionalidade de filtragem.

Em seguida, [crie um agente personalizado de QA][next-lesson] que reúna a skill e as ferramentas do navegador em uma função especializada.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor Playwright MCP da Microsoft][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
