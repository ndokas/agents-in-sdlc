---
title: "Lição 1 - Instale o aplicativo GitHub Copilot"
description: "Instale o aplicativo GitHub Copilot, conecte o repositório criado a partir do modelo, conheça o espaço de trabalho e experimente um chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O **[aplicativo GitHub Copilot][about-copilot-app]** é um aplicativo para desktop voltado ao desenvolvimento orientado por agentes. Ele foi criado com base no GitHub Copilot CLI e tem integração nativa com o GitHub, então seus repositórios, branches e pipelines de integração contínua (CI) funcionam sem configuração adicional. O aplicativo foi projetado para fluxos de trabalho em que você orienta vários agentes em paralelo, cada um em seu próprio espaço de trabalho isolado, em vez de fazer todo o trabalho por conta própria, além de automatizar tarefas repetitivas. Com o Node.js instalado e sua cópia do projeto pronta, a próxima etapa é instalar o aplicativo e conectar o repositório.

Nesta lição, você vai:

- instalar o aplicativo GitHub Copilot e entrar na sua conta;
- adicionar o projeto ao aplicativo a partir do repositório GitHub;
- conhecer o espaço de trabalho, incluindo o backlog criado pelo modelo;
- experimentar um chat rápido para saber mais sobre o próprio aplicativo.

## Cenário

Sua equipe está adotando agentes de IA para trabalhar em um backlog crescente. O aplicativo Copilot oferece um único lugar para orientar esse trabalho: escolher issues, executar agentes, revisar alterações e mesclar pull requests. Nesta lição, você instalará e conectará o aplicativo e aprenderá a iniciar uma conversa sobre o projeto.

> [!NOTE]
> É necessário ter um plano elegível do Copilot: Copilot Free, Copilot Student ou qualquer plano pago (Pro, Pro+, Business ou Enterprise). Se você usa o Copilot Business ou o Copilot Enterprise, o administrador precisa manter habilitada a política do **aplicativo GitHub Copilot**.

## Instale e configure o aplicativo GitHub Copilot

Para usar o aplicativo GitHub Copilot, primeiro instale-o. Há versões disponíveis para Windows, macOS e Linux. Vamos instalar o aplicativo, autenticar a conta e adicionar o repositório Tailspin Toys.

1. Em um navegador, abra a [página inicial do aplicativo GitHub Copilot][download-app].
2. Baixe o aplicativo para sua plataforma e instale-o seguindo as instruções da página.
3. Abra o aplicativo depois de instalá-lo.
4. Selecione **Sign in to GitHub** e siga as instruções para se autenticar. Se você usa o GitHub Enterprise Cloud com residência de dados, selecione **Use GitHub Enterprise** e informe o nome de host `*.ghe.com` quando solicitado. O GitHub Enterprise Server não é compatível.
5. Se o processo inicial oferecer a seleção de repositório, selecione o repositório Tailspin Toys criado a partir do modelo. Você também pode ignorar essa seleção e adicionar o projeto depois.
6. Quando solicitado, escolha um tema e conclua o processo inicial.
7. Procure o repositório em **Sessions** na barra lateral. Se ele já estiver listado, não é necessário adicioná-lo novamente.
8. Se o repositório ainda não estiver conectado, selecione **+** ao lado de **Sessions** para criar uma sessão e, em seguida, selecione **Add GitHub repository**. Informe o nome do proprietário e do repositório anotado na lição de pré-requisitos.
9. Confirme que o repositório aparece na barra lateral. No layout usado aqui, as pastas de repositório, como `tailspin-toys`, ficam agrupadas em **Sessions**, com sessões individuais abaixo de cada repositório.

## Conheça o espaço de trabalho

Com o projeto conectado, conheça o espaço de trabalho. O aplicativo organiza os recursos em algumas áreas na barra lateral:

- **New**: inicie uma nova sessão de chat com o Copilot.
- **Pull requests**: acesse seus pull requests por meio da integração nativa com o GitHub. Você pode procurar e filtrar pull requests, verificar o status de CI e revisá-los sem sair do aplicativo.
- **Issues**: acesse suas issues da mesma forma. Você pode procurar e filtrar issues e iniciar uma sessão a partir de uma issue.
- **Automations**: acesse tarefas de agente salvas que são executadas de acordo com uma agenda ou sob demanda. Elas são úteis para gerenciar listas de tarefas, fazer a manutenção regular do projeto ou delegar outras atividades repetitivas. O encerramento inclui links para elas como próxima etapa, não como outro exercício do workshop.
- **Customize**: adicione recursos e funções ao aplicativo Copilot na forma de servidores MCP, plugins, skills e outros componentes. Você usará esta área para configurar o MCP do Playwright.
- **Chats**: acesse conversas leves para perguntas e brainstorming que não precisam de uma branch ou espaço de trabalho próprios. Esta seção aparece depois de iniciar o primeiro chat. Se ela não estiver visível, selecione **+** ao lado de **Sessions** para iniciar um chat.
- **Sessions**: veja grupos de pastas de repositório e as sessões dos agentes. Selecione o nome do repositório, como `tailspin-toys`, para abrir a exibição do repositório ou selecione uma sessão abaixo dele para voltar à conversa. Cada sessão é executada em seu próprio espaço de trabalho isolado; assim, você pode executar várias ao mesmo tempo sem que as alterações entrem em conflito. Você iniciará sua primeira sessão ao adicionar classificações por estrelas.

Ao longo do workshop, você explorará o espaço de trabalho.

> [!TIP]
> Na dúvida, pergunte ao Copilot. Se não souber como fazer algo ou se algo é possível, o Copilot poderá orientar você.

### Localize o backlog criado pelo modelo

Provavelmente não existe projeto sem backlog, e o Tailspin Toys não é diferente. Vamos explorar o backlog criado quando você usou o modelo.

1. Em **Sessions** na barra lateral, selecione a pasta do repositório Tailspin Toys, e não uma sessão individual abaixo dela. Em seguida, selecione **Issues** na navegação superior do repositório.
2. Encontre estas issues pelo título, sem presumir seus números:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecione uma issue para ler os detalhes. Cada issue também serve como ponto de partida para uma sessão de agente. Depois de fazer uma primeira alteração rápida, você começará pela issue de filtragem.

> [!NOTE]
> As listas em **Issues** e **Pull requests** são filtradas automaticamente para mostrar apenas itens dos repositórios adicionados ao aplicativo Copilot. Para ver itens de trabalho de outros repositórios, adicione-os ao aplicativo.

## Experimente um chat rápido

Uma ótima maneira de se familiarizar com o aplicativo é usá-lo para saber mais sobre o *próprio aplicativo*. Um **chat rápido** é ideal para isso: permite fazer perguntas ou gerar ideias sem criar uma branch ou worktree, sendo perfeito para uma pergunta rápida e descartável, sem exigir uma sessão.

1. Na barra lateral, selecione **+** ao lado de **Chats** para abrir um novo chat. Se **Chats** ainda não estiver visível, selecione **+** ao lado de **Sessions** para iniciar seu primeiro chat; **Chats** aparecerá depois disso.
2. Pergunte ao aplicativo como as próprias sessões funcionam:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Leia a resposta na exibição da conversa. Você verá que cada sessão é executada em seu próprio git worktree isolado, o que permite executar vários agentes em paralelo sem que as alterações entrem em conflito. Você pode continuar a conversa ou iniciar um novo chat a qualquer momento.

## Resumo e próximas etapas

Parabéns! Você instalou o aplicativo GitHub Copilot, conectou o projeto e explorou o espaço de trabalho. Você aprendeu a:

- instalar o aplicativo e entrar no GitHub;
- adicionar um projeto a partir do repositório GitHub;
- conhecer o espaço de trabalho e localizar o backlog criado em **Issues**;
- usar um chat rápido para fazer uma pergunta rápida e descartável.

Em seguida, [inicie sua primeira sessão de agente][next-lesson] e use-a para exibir classificações por estrelas nos cartões dos jogos.

## Recursos

- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Introdução ao aplicativo GitHub Copilot][getting-started]
- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
