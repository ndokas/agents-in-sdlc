---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Explore os modos de agente: use Plan para definir uma abordagem, Autopilot para desenvolver a filtragem a partir de uma issue e Interactive para revisar e verificar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Começamos adicionando uma pequena funcionalidade ao projeto. Mudanças maiores, porém, exigem um processo mais robusto. Felizmente, o aplicativo GitHub Copilot foi criado para trabalhar com o fluxo existente de uma organização e ajudar a desenvolver as coisas certas da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento com agentes: começará com uma issue para gerar uma funcionalidade, verificará se o código é válido e se a funcionalidade se comporta como esperado e, por fim, fará a mesclagem no projeto.

> [!NOTE]
> Você usará a mesma sessão ao continuar o fluxo de trabalho da funcionalidade. Em geral, seriam usadas sessões ou pull requests (PRs) diferentes para os tipos de arquivo em que você trabalharia, mas faremos esse atalho para manter o foco nos conceitos principais.

Nesta lição, você vai:

- iniciar uma sessão de agente a partir de uma issue do GitHub;
- definir os requisitos no modo **Plan**;
- implementar a funcionalidade no modo **Autopilot**;
- revisar o código;
- validar a funcionalidade manualmente em um canvas de navegador.

Ao continuar o desenvolvimento, você atualizará as instruções do repositório, personalizará a skill `quality-checks` existente, adicionará a validação com o Model Context Protocol (MCP), criará um agente de garantia de qualidade (QA) e abrirá o pull request (PR) da funcionalidade.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve a funcionalidade, mas detalhes como a combinação de categorias precisam ser definidos antes da implementação. Você usará o modo Plan para resolver essas decisões e, depois, autorizará uma implementação com escopo definido no modo Autopilot.

## Contexto

Incorporar agentes de programação com inteligência artificial (IA) ao fluxo de desenvolvimento não muda os fundamentos. Na verdade, eles se tornam ainda mais importantes. A maioria das pessoas desenvolvedoras segue um fluxo semelhante a este:

1. Abra uma issue registrada com detalhes do que precisa ser feito.
2. Crie um plano do que será desenvolvido.
3. Desenvolva e revise o código.
4. Execute os testes para validar o código.
5. Valide manualmente a nova funcionalidade.
6. Crie um PR.
7. Depois que o código for revisado e o processo de integração contínua for concluído com sucesso, faça a mesclagem.

> [!NOTE]
> Os detalhes variam conforme a equipe e a organização, mas a maioria dos processos segue uma variação dessas etapas.

Ao seguir essa abordagem padrão, você garante que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de validação que o código escrito manualmente.

## Modos de sessão

O **modo de sessão** controla o nível de autonomia do agente. Você pode defini-lo no menu suspenso abaixo do campo de prompt e alterá-lo a qualquer momento:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano primeiro. Você o revisa e aprova antes que o agente o execute.
- **Autopilot**: o agente trabalha de forma totalmente autônoma — escreve código, executa testes e faz iterações sem aguardar sua orientação.

Comece no modo Plan, revise o plano e, em seguida, use Autopilot para implementá-lo.

## Inicie uma sessão a partir da issue

Antes de começar, confirme que o PR de classificação por estrelas foi mesclado e que sua branch `main` local está atualizada.

1. Em **Sessions** na barra lateral, selecione a pasta do repositório Tailspin Toys e, na navegação superior do repositório, selecione **Issues**. Abra **Allow users to filter games by category and publisher**. Essa issue já foi criada pelo modelo; não é necessário criar outra.
2. Selecione **New session** e escolha uma **new working tree** baseada na versão atualizada de `main`.

    ![Visualização da issue no aplicativo GitHub Copilot com uma seta apontando para o botão New session](../../../_images/app-new-session-from-issue.png)

3. Confirme que a issue está anexada à sessão e selecione **Plan** no seletor de modo.

> [!NOTE]
> Se o Node.js ou o npm não forem encontrados:
>
> 1. Peça ao agente que execute `node --version` e `npm --version`.
> 2. Se os comandos funcionarem no terminal, mas não na sessão, verifique o PATH do aplicativo ou a configuração do gerenciador de versões e feche completamente o aplicativo antes de reabri-lo.
> 3. Tente novamente a operação e as verificações que falharam. Não reinstale o npm apenas porque o agente não o encontrou.

## Planeje a funcionalidade de filtragem

O planejamento permite revisar a abordagem antes que o Copilot escreva código. Como você começou pela issue, o Copilot já tem a solicitação da funcionalidade como contexto.

1. Envie este prompt ao Copilot:

    ```plaintext
    Build this feature.
    ```

2. Responda às perguntas do Copilot e compare o plano com os critérios de aceitação da issue. Confirme que ele abrange filtros por categoria e editora, controles acessíveis, alterações no acesso a dados e testes. Discuta qualquer comportamento que não esteja claro, como a combinação de várias categorias ou o resultado quando nenhum jogo corresponde aos filtros.
3. Confirme que o plano inclui lint, testes unitários, testes de ponta a ponta (E2E) e verificação de tipos usando as ferramentas existentes do projeto. Mantenha o foco na implementação e nos testes da filtragem; você criará o PR depois de concluir o fluxo de qualidade.
4. Peça alterações no plano antes de aprová-lo e mantenha à mão o endereço da issue e os esclarecimentos acordados para a validação posterior.

## Aprove explicitamente o Autopilot

Quando estiver satisfeito com o plano, aprove-o explicitamente e confirme o modo ativo em vez de presumir que a aprovação o alterou.

1. Selecione **Approve and implement this plan** ou a opção equivalente na sua versão.
2. Verifique se o indicador de modo exibe **Autopilot**.

O Copilot começará a trabalhar na implementação. Você verá que ele percorre o plano estabelecido de forma iterativa, gera código e executa testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente; por isso, revise o plano antes.

3. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas. Interrompa apenas os servidores que você iniciou.

## Revise e verifique a implementação

Depois que o código for gerado, revise-o antes da mesclagem, como faria com qualquer outro código. Vamos revisar as alterações e executar o site para confirmar que tudo está funcionando.

1. Abra **Changes** e examine a implementação da filtragem e os testes.
2. Compare o resultado com a issue e os esclarecimentos aprovados, incluindo as combinações de categorias e editoras. Confirme que as alterações seguem as instruções existentes do repositório.
3. Examine os resultados de lint, testes unitários, testes E2E e verificação de tipos. Uma verificação ignorada não conta como aprovada.
4. Resolva as falhas e execute novamente as verificações afetadas antes de aceitar a implementação. A configuração E2E do Playwright cria e serve uma versão de pré-visualização e pode reutilizar um servidor local. Confirme que o servidor testado pertence a este worktree, e não a uma lição anterior.

## Explore a nova funcionalidade

O código parece correto, mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site em um canvas de navegador.

1. Use este prompt para pedir ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador será aberta dentro do aplicativo Copilot.
3. Selecione uma categoria e confirme que os resultados correspondem a ela. Limpe o filtro, selecione uma editora e confirme que os resultados correspondem a essa editora.
4. Combine os filtros de categoria e editora e compare os resultados com a issue e o plano aprovado. Se houver suporte a várias categorias, verifique o comportamento combinado acordado.
5. Teste uma combinação sem jogos correspondentes e verifique o estado vazio. Limpe ou redefina os filtros e confirme que a lista completa volta a aparecer.
6. Quando terminar, use este prompt para pedir ao Copilot que pare o servidor de desenvolvimento iniciado para esta sessão:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Mantenha esta sessão para as próximas lições

Para manter o trabalho da filtragem disponível para as próximas melhorias, preserve a sessão e a branch sem criar nem mesclar o PR.

1. Não crie nem mescle o PR da filtragem.
2. Mantenha esta sessão, este worktree e esta branch para as próximas lições; depois de parar o servidor de desenvolvimento, deixe a sessão inativa e retorne a ela mais tarde.

Em seguida, você aplicará instruções personalizadas ao código de filtragem e adicionará melhorias à skill e ao agente de garantia de qualidade (QA) antes de criar e mesclar o PR combinado.

## Resumo e próximas etapas

Você usou diferentes modos de agente para desenvolver e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma sessão de agente a partir de uma issue do GitHub;
- definiu os requisitos no modo **Plan**;
- implementou a funcionalidade no modo **Autopilot**;
- revisou o código;
- validou a funcionalidade manualmente em um canvas de navegador.

Em seguida, aprofunde-se na geração de código e garanta que ela siga as práticas documentadas [usando instruções personalizadas][next-lesson].

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
