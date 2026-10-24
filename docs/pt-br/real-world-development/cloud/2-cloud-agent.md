---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Poucas organizações, se houver alguma, não enfrentam problemas com dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de funcionalidades que ficaram esquecidas no backlog por falta de tempo. O agente de nuvem do GitHub Copilot foi criado para executar tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele cria um pull request (PR) em rascunho para que uma pessoa desenvolvedora revise o trabalho. Isso permite delegar tarefas repetitivas, acelerar o desenvolvimento e liberar as pessoas desenvolvedoras para se concentrarem em questões mais amplas.

Neste exercício, você vai explorar como o agente de nuvem do Copilot permite:

- personalizar o ambiente para gerar código;
- garantir que as operações sejam executadas com segurança;
- compreender a importância de issues com escopo bem definido;
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys quer resolver alguns problemas de dívida técnica. As pessoas contratadas inicialmente para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, você verá que ela está completamente ausente. Como primeira etapa, a equipe quer adicionar comentários de documentação TypeScript (TSDoc) a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer facilitar a descoberta de jogos. A página de detalhes de cada jogo deve mostrar outros títulos da mesma categoria para que as pessoas continuem explorando. A equipe ainda não precisa de uma implementação refinada; quer apenas algo que possa usar para testes de aceitação da experiência do usuário (UX). Essa tarefa está bloqueada no momento, mas há outras issues de prioridade mais alta.

Esses são exemplos de tarefas que podem ser adiadas facilmente e que são ótimas candidatas para o agente de nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona enquanto você se dedica a outras tarefas e, depois, você pode revisar o trabalho para confirmar que está correto.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano de forma semelhante a uma pessoa desenvolvedora. Há várias maneiras de trabalhar com ele, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois que recebe a atribuição, o Copilot cria um pull request em rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar na tarefa. Você pode examinar a sessão enquanto ela está em andamento ou depois de concluída. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções com escopo bem definido

O GitHub Copilot não faz mágica: você ainda precisa pensar. Mesmo tarefas aparentemente simples podem ser complexas quando examinadas em detalhes; por isso, [defina cuidadosamente o escopo das tarefas para o agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a chegada da IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram as instruções personalizadas e como elas orientam o Copilot. Se você já fez o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação. Caso contrário, este é um bom momento para examiná-lo. Antes de atribuir trabalho ao agente de nuvem do Copilot, leia os arquivos de instruções já incluídos no repositório, sem alterá-los, para observar o efeito deles mais adiante.

Abra estes arquivos na interface Web do GitHub ou em um codespace, caso já tenha um aberto:

- `.github/copilot-instructions.md`: examine a seção **Code standards**, em especial as convenções de TypeScript e os comentários de documentação TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que associa estas instruções aos arquivos `**/*.test.ts`.

Quando você atribuir ao agente de nuvem a issue `Code lacks documentation` na próxima seção, observe no pull request os comentários de documentação TSDoc, as convenções de TypeScript e os cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções. Voltaremos a esse assunto ao revisar os PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente de nuvem

Criar código normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja funcionando. O mesmo vale para tarefas atribuídas ao Copilot, que trabalha de maneira semelhante a uma pessoa engenheira de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente criando um [fluxo de trabalho de configuração especial][setup-workflow] no arquivo `.github/workflows/copilot-setup-steps.yml`, que será executado antes de o agente começar. Assim, ele terá acesso às ferramentas de desenvolvimento e dependências necessárias. Esse arquivo já foi configurado para facilitar o workshop e o aprendizado. Ele disponibiliza Node.js, dependências do projeto, binários do navegador para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório durante as etapas de configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se as etapas não clonarem o repositório, o Copilot fará isso automaticamente após a conclusão delas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo - Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
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

      # Migre e preencha o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer outro fluxo de trabalho do GitHub, mas há alguns pontos importantes:

- Ele contém um único trabalho chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o acionador `workflow_dispatch`, que permite executar manualmente o fluxo de trabalho na guia **Actions** do repositório. Isso é útil para verificar se o fluxo funciona, em vez de esperar que o Copilot o execute.

## Adicione documentação

Embora todos reconheçam a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação. Essa dívida técnica costuma permanecer sem solução, reduzindo a produtividade e dificultando a manutenção da base de código ou a integração de pessoas desenvolvedoras. O Copilot é eficaz na criação de documentação, por isso esta é uma ótima issue para atribuir ao agente de nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o trabalho.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-gerado, que terá um título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá ter criado uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar vários minutos para aparecer no PR. O Copilot está configurando o ambiente (executando o fluxo de trabalho descrito anteriormente), analisando o projeto e determinando a melhor abordagem.

13. Revise a lista e as tarefas que o Copilot pretende concluir.
14. Role a linha do tempo do pull request para baixo. Você deverá ver uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

    ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão ao vivo e acompanhar como o Copilot resolve o problema: ele explora o código, entende o estado do projeto, faz uma pausa para pensar e definir a abordagem e, em seguida, cria o código.

Isso provavelmente levará alguns minutos. Um dos principais objetivos do agente de nuvem do Copilot é executar tarefas de forma assíncrona e liberar você para outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente de nuvem e, depois, voltar nossa atenção para a criação de funcionalidades no aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Uma das grandes vantagens do agente de nuvem do GitHub Copilot é permitir dividir o trabalho, enquanto você se concentra em um conjunto de tarefas e ele trabalha em outro. Adicionar uma seção de jogos relacionados para a equipe de design talvez não demore muito, mas ainda é um tempo que você pode dedicar a outras tarefas. Vamos atribuir o trabalho ao agente de nuvem.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. Defina **Title** como `Show related games on the game details page`.
6. Defina **Description** como:

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

Pouco depois, você verá um emoji 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando na segunda solicitação. O agente de nuvem funciona de forma semelhante a uma pessoa engenheira de software, então não é necessário acompanhá-lo ativamente; basta revisar o trabalho quando estiver concluído. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximas etapas

Nesta lição, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para execução assíncrona. O Copilot pode ajudar a resolver dívida técnica, criar funcionalidades ou migrar código de uma estrutura para outra.

Você explorou estes conceitos:

- personalizar o ambiente para gerar código;
- garantir que as operações sejam executadas com segurança;
- compreender a importância de issues com escopo bem definido;
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, vamos criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores do Model Context Protocol (MCP)][cloud-agent-mcp] e tem acesso a instruções personalizadas, que exploramos em módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
