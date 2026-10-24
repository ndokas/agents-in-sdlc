---
title: "Lição 0 - Pré-requisitos"
description: "Prepare-se para as lições do aplicativo GitHub Copilot: instale o Node.js para o projeto Tailspin Toys e crie sua própria cópia do repositório a partir do modelo."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O aplicativo GitHub Copilot é um aplicativo para desktop que serve como ponto central para o Copilot e o GitHub. Ele oferece acesso rápido a issues e pull requests e, naturalmente, permite desenvolver usando o GitHub Copilot. Durante este workshop, você trabalhará localmente com o aplicativo Tailspin Toys, criado com Astro, usando o aplicativo GitHub Copilot. Antes de começar, confirme que o Node.js está instalado localmente e, em seguida, instale o aplicativo Copilot.

Nesta lição, você vai:

- instalar o Node.js para que os testes do projeto possam ser executados no seu computador;
- criar sua própria cópia do projeto Tailspin Toys a partir do modelo.

## Instale o Node.js

Várias lições pedirão a um agente que desenvolva funcionalidades e execute localmente o conjunto de testes do Tailspin Toys. Para isso, é necessário o [Node.js][nodejs] — o único runtime exigido pelo projeto. Instale a versão **LTS** atual.

A opção mais simples em todas as plataformas é usar o instalador oficial:

1. No sistema operacional, abra uma janela de terminal usando o Windows Terminal, o Terminal do macOS ou o aplicativo que você costuma usar.
2. Execute os comandos a seguir para verificar a versão do Node.js instalada:

    ```shell
    node --version
    npm --version
    ```

3. Se os dois comandos forem executados e a versão do Node.js atender aos requisitos do README e do `package.json` do projeto, pule para a próxima seção.

> [!TIP]
> Você só precisa concluir estas etapas se não tiver o Node instalado ou se precisar atualizá-lo.

4. Abra a [página de download do Node.js][node-download].
5. Baixe a versão **LTS** para o seu sistema operacional.
6. Execute o instalador e aceite as opções padrão. No Windows, mantenha a opção **Add to PATH** selecionada.
7. Depois da instalação, abra uma nova janela de terminal.
8. Confirme a instalação na nova janela de terminal executando:

    ```bash
    node --version
    npm --version
    ```

9. Você deverá ver as versões instaladas do Node.js e do npm.

> [!IMPORTANT]
> O agente do aplicativo Copilot também precisa conseguir executar `node --version` e `npm --version`. O fato de os comandos funcionarem no seu terminal habitual não garante que estejam disponíveis no aplicativo. Feche o aplicativo completamente e reabra-o depois de instalar o Node.js ou alterar o PATH. Se algum dos comandos ainda falhar em uma sessão, siga as [etapas de solução de problemas do ambiente da sessão][session-environment] antes de continuar.

> [!IMPORTANT]
> Cada worktree também precisa das dependências do projeto e do Chromium do Playwright para executar verificações de ponta a ponta (E2E). Siga o README do repositório Tailspin Toys ao preparar um worktree e revise qualquer solicitação de instalação antes de aprová-la.

## Configure o repositório do laboratório

Você trabalhará na sua própria cópia do projeto Tailspin Toys. Crie-a agora a partir do [repositório de modelo][template-repository]. O novo repositório contém todos os arquivos necessários para o laboratório, e você o conectará ao instalar o aplicativo.

1. Em uma nova janela do navegador, acesse o repositório GitHub deste laboratório: `https://github.com/github-samples/tailspin-toys`.
2. Crie sua própria cópia do repositório selecionando o botão **Use this template** na página do repositório do laboratório. Em seguida, selecione **Create a new repository**.

    ![Botão Use this template com Create a new repository selecionado no menu suspenso](../../../_images/app-0-use-template.png)

3. Se você estiver participando de um workshop em um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções fornecidas pelas pessoas mentoras. Caso contrário, crie o novo repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

    ![Formulário Create a new repository com github-samples/tailspin-toys definido como modelo e o nome do repositório preenchido](../../../_images/app-0-create-repository.png)

4. Anote o caminho do repositório que você criou (**nome-da-organização-ou-usuário/nome-do-repositório**), pois fará referência a ele mais adiante no laboratório.

> [!NOTE]
> Ao criar o repositório a partir do modelo, um backlog de issues do GitHub será criado automaticamente. Você trabalhará nessas issues durante todo o workshop — não precisa criar nenhuma por conta própria.

Use uma cópia nova do modelo do workshop. Ela inclui instruções do repositório, código do aplicativo, testes, uma skill de verificações de qualidade e uma extensão de canvas existente. Você personalizará a skill e criará um agente de QA durante o workshop. Se usar uma cópia mais antiga, confirme com a pessoa que conduz o workshop se ela contém os arquivos necessários.

## Resumo e próximas etapas

Tudo pronto! Nesta lição, você:

- instalou o Node.js para que o projeto possa ser criado e testado no seu computador;
- criou sua própria cópia do repositório Tailspin Toys a partir do modelo.

Em seguida, [instale o aplicativo GitHub Copilot][next-lesson], conecte o repositório que acabou de criar e conheça o espaço de trabalho.

## Recursos

- [Baixar o Node.js][node-download]
- [Criar um repositório a partir de um modelo][template-repository]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#inicie-uma-sessao-a-partir-da-issue
