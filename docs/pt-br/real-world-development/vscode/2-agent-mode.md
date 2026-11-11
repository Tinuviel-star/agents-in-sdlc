---
title: "Exercício 2 - Adicione funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de um aplicativo podem exigir mudanças em vários arquivos e operações como executar testes. Em geral, a pessoa desenvolvedora precisa localizar todos os arquivos necessários, fazer as mudanças, executar testes, depurar, identificar o arquivo que ficou faltando, fazer outra alteração e repetir o processo.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot trabalha com mais autonomia no ambiente de desenvolvimento integrado (IDE). Ele se comporta de maneira semelhante a uma pessoa desenvolvedora: começa explorando a estrutura do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas que encontra. Veja como usar o modo de agente para adicionar uma funcionalidade ao site.

> [!NOTE]
> Apesar de terem nomes parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente executa tarefas no IDE, com ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona, como uma pessoa parceira da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar mudanças.
- o modo de agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados.
- revisar mudanças e testes gerados pelo modo de agente do Copilot antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem os jogos por categoria e editora. No exercício anterior, você adicionou um auxiliar de editoras. Agora, concluirá o trabalho restante na camada de dados, na interface e nos testes com o modo de agente do Copilot.

## Execute o site Tailspin Toys

Antes de fazer mudanças, explore o site Tailspin Toys para entender as funcionalidades atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro que renderiza páginas em HyperText Markup Language (HTML) estático durante o build. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o Drizzle ORM (mapeador objeto-relacional). Não há uma interface de programação de aplicações (API) de backend separada nem um framework de interface executado no cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada com pequenos elementos `<script>` do Astro que usam as APIs padrão do Document Object Model (DOM).

### Inicie o site

Para facilitar a execução do site, o projeto inclui a tarefa `npm run dev`, que inicia o único servidor de desenvolvimento Astro. Execute-a no GitHub Codespaces:

1. Volte ao Codespace. Depois do exercício anterior, você deve estar na branch `main`.
2. Abra uma nova janela de terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, será exibido um aviso com a URL, semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo solicitar que você abra uma janela do navegador para `http://localhost:4321`, feche-a selecionando o **x**.

5. Para abrir o site, mantenha pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecione o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um Codespace, selecionar o link `localhost` no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o Codespace, que agora hospeda o servidor Web.

### Explore o site

Depois que o site estiver em execução, explore suas funcionalidades principais:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá uma página com mais informações, incluindo título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de continuar, volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; as etapas indicarão qual opção usar.

A primeira versão do site funciona, mas queremos adicionar recursos. Comece examinando o backlog. Ao criar seu projeto a partir do template, você recebeu automaticamente um backlog de issues. Peça ao GitHub Copilot que mostre esses itens.

1. Na lista de agentes da visualização Chat, selecione **Agent**. O agente planeja e implementa mudanças em vários arquivos de forma autônoma, executa comandos do terminal e invoca ferramentas.

   ![Seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Na lista de modelos disponíveis, selecione **Claude Sonnet 4.5**.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo específico. Durante a criação do workshop, usamos Claude Sonnet 4.5 e o incluímos nas instruções para que as sugestões de código sejam relativamente consistentes. Como os modelos de linguagem grandes (LLMs) são probabilísticos, as sugestões podem ser diferentes das descritas aqui. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando do Model Context Protocol (MCP) diferente, mas ainda deve conseguir concluir a tarefa.

3. Envie este prompt ao Copilot para consultar o backlog de issues:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista gerada.

Observe que o Copilot também priorizou os itens de acordo com o que considera mais útil para as pessoas usuárias.

## Revise os arquivos de instruções

Antes de pedir ao agente que gere código, examine o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de [instruções da interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No Codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, capacidade de teste e acessibilidade.
   - links para arquivos de instruções específicos de tipos de arquivo, incluindo:
     - Astro.
     - Tailwind CSS (folhas de estilo em cascata).
     - Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Isso permite reutilizar conteúdo, dividir instruções complexas em partes menores e incluir exemplos e modelos.

## Implemente a filtragem

Para concluir a filtragem, serão necessárias pelo menos três mudanças no aplicativo:

- adicionar ou aprimorar a lógica restante de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento de filtragem.
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser aprovados antes da mesclagem. O modo de agente do Copilot pode executar essas tarefas. Adicione a funcionalidade:

1. Continue a conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como o arquivo de instruções.

   ![Exemplo de seleção do arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. Na lista de agentes da visualização Chat, confirme se **Agent** continua selecionado.

   ![Seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme se **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base na issue relacionada do backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe como o Copilot começa explorando o projeto e localizando os arquivos relacionados à funcionalidade, inclusive auxiliares da camada de dados, elementos da interface do usuário (UI) e testes. Depois, ele modifica arquivos e executa testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, os controles **Keep** e **Undo** podem aparecer no editor. Quando o Copilot terminar, haverá um controle **Keep** ou **Undo** para cada mudança; não é necessário selecioná-los enquanto o trabalho estiver em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Caixa de diálogo do Copilot Chat solicitando autorização para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e alguns testes podem falhar durante o processo. O Copilot alterna entre a geração de código e os testes até concluir a tarefa sem detectar erros.

   ![Sessão concluída do Copilot Chat no modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examine o código gerado para identificar possíveis problemas.

> [!CAUTION]
> É sempre importante revisar o código gerado pelo Copilot ou por outras ferramentas de inteligência artificial.

10. Volte ao navegador em que o site está em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Neste exercício, você usou o modo de agente do GitHub Copilot para adicionar funcionalidades ao site Tailspin Toys. Você aprendeu como:

- o modo de agente explora o projeto, identifica arquivos relevantes e coordena mudanças.
- o modo de agente do GitHub Copilot implementa funcionalidades na interface e na camada de dados.
- revisar mudanças e testes gerados pelo modo de agente antes de mesclá-los à base de código.

Agora, [teste a funcionalidade com o servidor Playwright MCP][next-lesson] e abra um pull request.

### Exercício extra de exploração — implemente paginação

À medida que a lista de jogos crescer, será necessário implementar paginação. Com base no que aprendeu neste exercício, peça ao Copilot que atualize o site para incluir paginação. Considere:

- seguir as práticas recomendadas existentes, inclusive usar os arquivos de instruções disponíveis.
- decidir como a paginação deve funcionar: se a pessoa usuária poderá escolher o tamanho da página ou se ele será fixo.
- fornecer ao Copilot as orientações necessárias no prompt.
- iterar com o GitHub Copilot, solicitando mudanças e fornecendo contexto. Esse é o fluxo normal ao trabalhar com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
