---
title: "Lição 2 - Adicione classificações por estrelas: uma vitória rápida"
description: "Inicie sua primeira sessão de agente no aplicativo GitHub Copilot, faça uma pequena alteração nos cartões dos jogos e mescle-a no seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Agora que você conheceu o espaço de trabalho e usou um chat rápido, é hora de iniciar uma **sessão de agente** e fazer sua primeira alteração no projeto. A mudança será pequena: os jogos já têm uma classificação por estrelas nos dados, mas os cartões da página inicial ainda não a exibem. Peça ao agente que mostre essa classificação, revise a alteração e faça a mesclagem no seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma sessão de agente e aprender como ela é estruturada;
- pedir ao agente que faça uma alteração pequena e focada no projeto;
- revisar a alteração na visualização de diff do espaço de trabalho;
- executar o aplicativo localmente para confirmar a alteração no navegador;
- abrir e mesclar seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma classificação por estrelas, que já aparece na página de detalhes do jogo. Os cartões da página inicial, porém, exibem apenas o título, a categoria, a editora e a descrição. Como aquecimento, você pedirá ao agente que mostre nos cartões a classificação existente — uma alteração pequena e independente, ideal para sua primeira sessão.

## Estrutura de uma sessão

Uma **sessão** é uma conversa com um agente executada em seu próprio espaço de trabalho isolado. Cada sessão recebe um **git worktree e uma branch dedicados**, o que permite executar várias sessões ao mesmo tempo — uma adicionando uma funcionalidade e outra corrigindo um bug — sem que as alterações entrem em conflito. Suas sessões aparecem na barra lateral, agrupadas por repositório. Selecione uma delas para acessá-la.

Em uma sessão, você verá três elementos: a **conversa** com o agente, a **atividade de ferramentas** do agente enquanto ele explora e edita arquivos e a lista de **arquivos alterados** com os respectivos diffs.

## Inicie uma sessão e solicite a alteração

Vamos iniciar uma nova sessão para explorar o projeto e implementar a funcionalidade. Durante a [configuração do aplicativo][prior-lesson], você adicionou o projeto a partir do repositório GitHub. Agora, criaremos uma nova sessão para esse repositório e solicitaremos a alteração.

1. Volte ao aplicativo GitHub Copilot ou abra-o.
2. Selecione **+** na parte superior da barra lateral para iniciar uma sessão.
3. Selecione `tailspin-toys` como repositório.
4. Escolha uma **new working tree** e o modo **Interactive** abaixo da caixa de prompt. Use este prompt para solicitar a alteração:

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. Pressione <kbd>Enter</kbd> para enviar o prompt ao Copilot.

O aplicativo Copilot começa criando um novo worktree, uma cópia isolada do projeto. Em seguida, ele explora o projeto, localiza os arquivos que precisam ser atualizados e cria o código necessário para adicionar a nova funcionalidade. Você acabou de adicionar uma funcionalidade com o aplicativo Copilot.

## Revise o diff

Todas as alterações geradas por IA devem ser revisadas antes da mesclagem, mesmo as pequenas. Vamos explorar as alterações diretamente no aplicativo Copilot.

1. No canto superior direito do aplicativo, selecione **Toggle review panel**. No painel que abrir, selecione **Changes** para ver os arquivos alterados e seus diffs.

    ![Barra de ferramentas superior do aplicativo GitHub Copilot com uma seta apontando para o botão Toggle review panel à direita de Create PR](../../../_images/app-2-review-panel.png)

2. Você deverá encontrar código adicionado a `GameCard.astro`, o arquivo principal usado para exibir os detalhes dos jogos. Ele deve ser semelhante ao exemplo a seguir: um pequeno bloco que mostra a classificação quando ela existe e usa `No rating yet` quando `starRating` é `null`:

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, e não determinístico; por isso, o código exato pode variar em relação ao exemplo. Ainda assim, deve ser relativamente semelhante.

## Verifique as alterações

Revise os resultados das verificações automatizadas do agente antes de abrir um navegador. Confirme que os testes cobrem um `starRating` numérico e a alternativa para `null`. Um pré-requisito ausente ou uma verificação ignorada não conta como aprovação; revise qualquer solicitação de instalação antes de aprová-la.

Não basta ler o código e presumir que funciona. Peça ao Copilot que abra o site para que você examine a interface do usuário atualizada. Para isso, peça que ele inicie o site e o abra em um canvas de navegador.

> [!TIP]
> Um canvas é um widget interativo disponível dentro do próprio aplicativo Copilot. Você explorará canvases personalizados e até criará o seu mais adiante, mas, por enquanto, usaremos o canvas de navegador integrado.

1. Use este prompt para pedir ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador será aberta dentro do aplicativo Copilot.
3. Confirme que os cartões dos jogos com classificação exibem o valor em uma escala de cinco estrelas.
4. Quando terminar, use este prompt para pedir ao Copilot que pare o servidor de desenvolvimento iniciado para esta sessão:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Abra e mescle seu primeiro pull request

Agora que você criou a funcionalidade, abra um pull request (PR) para mesclar o novo código à base de código existente.

1. Selecione **Create PR** no canto superior direito.
2. Se solicitado, selecione **Sign in with your browser** e siga as instruções para se autenticar.
3. Se o Copilot explicar que a política do repositório exige uma issue correspondente antes de criar o PR, confirme que ele deve seguir esse processo.
4. Aguarde enquanto o Copilot cria o PR.
5. Selecione o indicador **PR** logo acima do chat para abrir o PR no painel de revisão. Revise o PR conforme necessário.
6. Quando estiver tudo pronto, selecione **Ready to merge**.
7. Na nova caixa de diálogo, selecione **Merge pull request** para mesclar o pull request.

## Resumo e próximas etapas

Parabéns! Você entregou sua primeira alteração usando o aplicativo GitHub Copilot. Nesta lição, você:

- iniciou uma sessão de agente e aprendeu como as sessões são estruturadas;
- orientou o agente a fazer uma alteração pequena e focada nos cartões dos jogos;
- revisou a alteração na visualização de diff do espaço de trabalho;
- executou o aplicativo localmente para confirmar a classificação por estrelas no navegador;
- abriu o PR 1, revisou as verificações e fez a mesclagem explicitamente.

Em seguida, [comece pela issue de filtragem e use os modos Plan e Autopilot][next-lesson] para desenvolver uma funcionalidade maior.

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#instale-e-configure-o-aplicativo-github-copilot
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
