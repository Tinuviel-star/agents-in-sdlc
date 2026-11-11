---
title: "Lição 2 - Adicione classificações por estrelas: uma vitória rápida"
description: "Use o Copilot CLI para fazer uma pequena mudança nos cartões de jogos, revisar no navegador encaminhado e mesclar seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e iniciou uma conversa, faça sua primeira mudança no projeto. Ela será pequena: os dados dos jogos já incluem uma classificação por estrelas, mas os cartões da página inicial ainda não a exibem. Você pedirá ao agente que mostre a classificação, revisará a mudança e a mesclará como seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa focada do Copilot em uma branch de funcionalidade.
- pedir ao agente que faça uma pequena mudança no projeto.
- revisar a mudança com `/diff`.
- executar o aplicativo e confirmar a mudança em um navegador encaminhado.
- abrir e mesclar seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma classificação por estrelas, que já aparece na página de detalhes do jogo. Os cartões da página inicial, porém, mostram apenas o título, a categoria, a editora e a descrição. Como exercício inicial, peça ao agente para exibir a classificação existente em cada cartão: uma mudança pequena e independente, ideal para sua primeira sessão.

## Entenda uma conversa

Uma **conversa** é onde você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal do CLI usa o repositório e a branch Git atualmente em uso no terminal, em vez de criar uma worktree dedicada. As conversas salvas permitem retomar a discussão depois, enquanto os arquivos e a branch continuam sendo o estado Git normal no disco.

Em uma conversa, você verá seus prompts e as respostas do agente, a atividade das ferramentas enquanto o agente explora e edita arquivos e as mudanças que pode inspecionar com `/diff`.

## Inicie uma conversa e solicite a mudança

Comece uma conversa nova para iniciar a implementação da funcionalidade.

1. Volte ao seu Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se o Copilot ainda não estiver em execução, use este comando:

   ```bash
   copilot --yolo
   ```

4. Inicie uma sessão nova com o comando slash `/new` e pressione <kbd>Enter</kbd>.
5. Use este prompt para solicitar a mudança:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

O Copilot explorará o projeto, localizará os arquivos usados para exibir os detalhes dos jogos e criará o código necessário. Você adicionou uma funcionalidade com o Copilot CLI.

## Revise as diferenças

Todas as mudanças geradas por inteligência artificial devem ser revisadas antes da mesclagem, mesmo as pequenas. Revise as mudanças no Copilot CLI.

1. Digite `/diff` e inspecione cada arquivo alterado.
2. Confirme se o cartão do jogo mostra a classificação numérica quando ela está disponível e `No rating yet` quando `starRating` é `null`.
3. Confirme se os testes cobrem os dois estados.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija as falhas.
5. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diferenças.

> [!NOTE]
> Como todas as ferramentas de inteligência artificial generativa, o Copilot é probabilístico, não determinístico, então o código exato pode variar. Revise o comportamento em vez de esperar uma implementação idêntica à de um exemplo.

## Verifique as mudanças

Não basta ler o código e presumir que funciona. Peça ao Copilot que inicie o site para que você examine a interface atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme se os cartões dos jogos mostram as classificações de zero a cinco.
4. Volte ao Copilot e peça que encerre o servidor iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abra e mescle seu primeiro pull request

Você criou a funcionalidade. Agora, abra um pull request (PR) para mesclar o novo código ao projeto.

1. Peça ao agente padrão que faça commit da mudança:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Digite `/pr create`. O Copilot CLI enviará o commit existente ao criar o PR e exibirá a URL do PR.
3. Abra o PR mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme a mesclagem.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua cópia local de `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira mudança usando o GitHub Copilot CLI. Você:

- iniciou uma conversa focada do Copilot em uma branch de funcionalidade.
- orientou o agente a fazer uma pequena mudança nos cartões de jogos.
- revisou a mudança com `/diff`.
- executou o aplicativo e confirmou a classificação por estrelas em um navegador encaminhado.
- abriu e mesclou seu primeiro pull request.

Em seguida, [comece pela issue de filtragem e use os modos Plan e Autopilot][next-lesson] para desenvolver uma funcionalidade maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
