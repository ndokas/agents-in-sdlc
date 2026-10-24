---
title: "Lição 2 - Adicione classificações por estrelas: uma vitória rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cartões de jogos, revise-a no navegador com encaminhamento de porta e mescle-a no seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e experimentou uma conversa, é hora de fazer sua primeira alteração no projeto. Você manterá o escopo pequeno: os jogos já têm uma classificação por estrelas nos dados, mas ela ainda não aparece nos cartões da página inicial. Peça ao agente que a exiba, revise a alteração e a mescle no seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa focada do Copilot em uma branch de funcionalidade;
- pedir ao agente que faça uma pequena alteração no projeto;
- revisar a alteração com `/diff`;
- executar o aplicativo e confirmar a alteração em um navegador com encaminhamento de porta;
- abrir e mesclar seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma classificação por estrelas, que já aparece na página de detalhes do jogo. Os cartões na página inicial, porém, exibem apenas o título, a categoria, a editora e a descrição. Como aquecimento, você pedirá ao agente que mostre nos cartões a classificação existente — uma alteração pequena e independente, ideal para sua primeira sessão.

## Estrutura de uma conversa

Uma **conversa** é o espaço em que você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal da CLI usa o repositório e a branch atualmente selecionados no terminal, em vez de criar um worktree dedicado. As conversas salvas permitem retomar o mesmo diálogo mais tarde, enquanto os arquivos e a branch continuam sendo o estado normal do Git no disco.

Em uma conversa, você verá três elementos: seus prompts e as respostas do agente, as atividades do agente enquanto ele explora e edita arquivos e as alterações que podem ser examinadas com `/diff`.

## Inicie uma conversa e solicite a alteração

Vamos iniciar uma conversa para começar a implementar a funcionalidade.

1. Volte ao codespace.
2. Se o terminal ainda não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se o Copilot ainda não estiver aberto, inicie-o com este comando:

   ```bash
   copilot --yolo
   ```

4. Confirme que uma nova sessão foi iniciada digitando o comando de barra `/new` e pressionando <kbd>Enter</kbd>.
5. Use este prompt para solicitar a alteração:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

O Copilot explorará o projeto, localizará os arquivos usados para exibir os detalhes do jogo e criará o código necessário. Agora você adicionou uma funcionalidade com o Copilot CLI.

## Revise o diff

Todas as alterações geradas por IA devem ser revisadas antes da mesclagem, mesmo as pequenas. Examine as alterações diretamente no Copilot CLI.

1. Digite `/diff` e examine todos os arquivos alterados.
2. Confirme que o cartão do jogo exibe a classificação numérica quando ela existe e `No rating yet` quando `starRating` é `null`.
3. Confirme que os testes abrangem os dois estados.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija as falhas.
5. Ao concluir a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, e não determinístico; por isso, o código exato pode variar. Revise o comportamento, em vez de esperar uma implementação específica.

## Verifique as alterações

Não basta ler o código e presumir que ele funciona. Peça ao Copilot que inicie o site para que você examine a interface do usuário (UI) atualizada no navegador pela porta encaminhada do Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cartões dos jogos exibem as classificações em uma escala de cinco estrelas.
4. Volte ao Copilot e peça que pare o servidor iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abra e mescle seu primeiro pull request

Agora que você criou a funcionalidade, abra um pull request (PR) para mesclar o novo código no projeto.

1. Peça ao agente padrão que faça commit da alteração:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Digite `/pr create`. Ao criar o PR, o Copilot CLI enviará o commit existente e exibirá a URL do PR.
3. Abra o PR mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme a mesclagem.
6. Volte ao codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua branch `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximas etapas

Parabéns! Você entregou sua primeira alteração usando o GitHub Copilot CLI. Nesta lição, você:

- iniciou uma conversa focada do Copilot em uma branch de funcionalidade;
- orientou o agente a fazer uma pequena alteração nos cartões dos jogos;
- revisou a alteração com `/diff`;
- executou o aplicativo e confirmou a classificação por estrelas no navegador com encaminhamento de porta;
- abriu e mesclou seu primeiro pull request.

Em seguida, [comece pela issue de filtragem e use os modos Plan e Autopilot][next-lesson] para desenvolver uma funcionalidade maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
