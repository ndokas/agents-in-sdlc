---
title: "Exercício 3 - Testar sua funcionalidade com o servidor do Model Context Protocol (MCP) para o Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de desenvolver a funcionalidade de filtragem no modo de agente. Antes de abrir um pull request, confirme que ela funciona no navegador. Em vez de navegar pelo aplicativo por conta própria, conecte o **servidor do Model Context Protocol (MCP) para o Playwright** e deixe o Copilot controlar um navegador real para testar a funcionalidade. Depois, publique sua branch e abra um pull request (PR).

Neste exercício, você vai:

- aprender como o MCP amplia os recursos do Copilot;
- adicionar o servidor MCP do Playwright ao espaço de trabalho;
- pedir ao Copilot que o use para testar manualmente a filtragem no navegador;
- publicar sua branch e abrir um pull request para o trabalho de filtragem.

## O que é o MCP?

O modo de agente fica muito mais poderoso quando pode acessar recursos além do editor. O MCP permite isso: é um padrão para que o agente se comunique com ferramentas e serviços externos.

![Diagrama que mostra o funcionamento interno do modo de agente e sua interação com o contexto, um modelo de linguagem grande (LLM) e ferramentas, incluindo as fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [MCP](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de inteligência artificial (IA) se comuniquem com ferramentas e serviços externos. Com o MCP, eles podem acessar informações atualizadas por meio de recursos e executar ações em seu nome por meio de ferramentas.

Essas ferramentas e esses recursos são acessados por meio de um servidor MCP, que atua como uma ponte entre o agente de IA e os serviços externos. O servidor MCP gerencia a comunicação entre o agente e as ferramentas externas, como APIs existentes ou ferramentas locais, incluindo pacotes npm. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente pode acessar.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: fornece acesso a interfaces de programação de aplicações (APIs) para gerenciar repositórios do GitHub. O agente de IA pode executar ações como criar repositórios, atualizar os existentes e gerenciar issues e pull requests.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: oferece recursos de automação do navegador com o Playwright. O agente de IA pode navegar por páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP que fornecem acesso a diferentes ferramentas e recursos. O GitHub mantém um [registro de MCP](https://github.com/mcp) para facilitar a descoberta e as contribuições ao ecossistema.

> [!CAUTION]
> Em relação à segurança, trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor MCP, examine cuidadosamente o código-fonte, verifique o publicador e considere as implicações de segurança. Use somente servidores MCP confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revise a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste espaço de trabalho do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Você verá que um servidor `github` já está configurado:

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

Essa entrada `github` vem com o modelo do projeto. Por isso, o Copilot conseguiu ler o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não é preciso instalar nada localmente. O VS Code se conecta por HTTP, e você se autentica no GitHub na primeira vez que o Copilot usa uma das ferramentas.

## Adicione o servidor MCP do Playwright

Agora adicione um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] oferece ao Copilot um navegador que ele pode controlar, exatamente o que é necessário para testar a funcionalidade.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github` para que o arquivo fique assim:

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

Diferentemente do servidor remoto `github`, `playwright` é um servidor **local**: o VS Code o inicia no computador executando `npx @playwright/mcp@latest`. A flag `--headless` faz o Playwright executar o navegador sem uma janela visível. Isso é necessário em um codespace, que não tem uma área de trabalho para exibir a janela.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright nos testes de ponta a ponta; por isso, o navegador necessário normalmente já está instalado. Se o Copilot informar que falta um navegador:
>
> 1. Peça que execute `npx playwright install chromium`.
> 2. Tente novamente.

## Inicie e confie no servidor

O VS Code inicia os servidores MCP **sob demanda**: na primeira vez que o Copilot precisar das ferramentas de um servidor, o VS Code o iniciará e pedirá confirmação de confiança.

1. Em `.vscode/mcp.json`, aparecerá uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor agora e confirmar que a configuração está correta.
2. Se o VS Code pedir confirmação de confiança no servidor, revise a configuração e escolha confiar nele para que possa iniciar.

Iniciá-lo manualmente é opcional. Se você ignorar esta etapa, o VS Code iniciará o servidor e pedirá que você confie nele na primeira vez que o Copilot precisar de suas ferramentas, na próxima etapa.

## Teste a funcionalidade de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] dá ao Copilot acesso a um navegador real. Em vez de navegar pelo aplicativo para verificar o trabalho, o agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumir o que observou. É a maneira mais rápida de confirmar que uma funcionalidade funciona como esperado sem sair da conversa.

Nos bastidores, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, e não capturas de tela. Assim, o agente interpreta elementos estruturados e identificados — botões, links e itens de lista — de maneira semelhante à tecnologia assistiva. Uma verificação funcional rápida também funciona como uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste a funcionalidade de filtragem criada.

1. Envie este prompt ao Copilot:

    ```text
    Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

    1. Go to the games page and note how many games are listed.
    2. Apply a category filter and confirm the list updates to only show games in that category.
    3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
    4. Combine a category and a publisher filter and confirm the results respect both.

    Report what you observe at each step, and call out anything that does not behave as expected.
    ```

2. Compare o resumo do Copilot com os critérios de aceitação da issue. Se algo parecer incorreto, faça perguntas complementares ou peça que o Copilot corrija o código antes de abrir um pull request.

O Copilot abrirá um navegador pelo servidor MCP do Playwright, seguirá as etapas e informará o que encontrou.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` para este teste.
>
> 1. Se você tiver parado o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt.
> 2. Na primeira vez que o Copilot usar o servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se ele informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publique a branch e crie um pull request

Agora que confirmou que a funcionalidade funciona, abra um PR para que a equipe possa revisá-la. Primeiro, publique a branch `filtering-vscode`.

1. No codespace, acesse o painel **Source Control** e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Captura de tela do painel Source Control com as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Crie o pull request

Há várias maneiras de criar um pull request, incluindo pelo github.com e pela interface de linha de comando (CLI) do GitHub. Como você já está usando o GitHub Copilot, deixe que ele crie o PR. Ele pode encontrar a issue relevante e associá-la ao PR.

1. Acesse o painel Copilot Chat e selecione **New Chat** para iniciar uma nova sessão.
2. Confirme que **Agent** está selecionado no menu suspenso de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie o PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot colete informações e execute as operações. Na primeira vez que ele usar uma ferramenta do GitHub, talvez seja necessário autenticar-se. Siga as instruções para autorizá-lo.
5. Observe como o Copilot pesquisa as issues, encontra a correta e cria o PR.
6. Selecione o link gerado pelo Copilot para revisar o pull request, mas **não o mescle ainda**.

## Resumo e próximas etapas

Parabéns! Neste exercício, você:

- aprendeu como o MCP amplia os recursos do Copilot;
- adicionou o servidor MCP do Playwright ao espaço de trabalho;
- usou o servidor para testar manualmente a funcionalidade de filtragem no navegador antes de publicá-la;
- publicou a branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* uma funcionalidade, mas o MCP é apenas uma maneira de ampliar os recursos do Copilot. Em seguida, [crie um agente personalizado][next-lesson] para agilizar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Playwright da Microsoft][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
