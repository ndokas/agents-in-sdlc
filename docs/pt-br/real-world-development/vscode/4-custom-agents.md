---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de inteligência artificial (IA) especializados para tarefas ou áreas específicas do fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você fornece ao Copilot instruções focadas, práticas recomendadas, padrões de código e conhecimento do domínio para ajudá-lo a executar determinados tipos de trabalho com mais eficiência. As equipes podem transformar seu conhecimento em agentes reutilizáveis — por exemplo, um agente de acessibilidade que aplica as [Diretrizes de Acessibilidade para Conteúdo Web (WCAG)][wcag], um agente de segurança que segue práticas seguras de codificação ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo contém um frontmatter YAML com, no mínimo, `name` e `description`, seguido de um prompt Markdown que define o comportamento, a experiência e as instruções do agente.

### Agentes personalizados e skills de agente

Há alguma sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e orientam uma IA na execução de operações. A distinção mais simples é: um **agente personalizado** é quem executa o trabalho, enquanto as **skills** são ferramentas.

Os agentes personalizados têm sua própria janela de contexto e são criados para coordenar skills e até outros agentes durante o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisa e atualiza o site de acordo com as diretrizes de acessibilidade. Nesse trabalho, ele pode chamar skills como uma skill de fluxo de pull request ou uma que execute e gerencie testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente personalizado. Como em qualquer trabalho com IA, teste e faça iterações para descobrir o que funciona nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer garantir que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas capacidades ou preferências visuais. Comentários recentes de usuários apontam que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do baixo contraste entre o texto e o plano de fundo. Para resolver esse problema de acessibilidade, a equipe de design solicitou um modo de alto contraste que as pessoas possam ativar e desativar.

Como a acessibilidade é essencial, você quer implementar a funcionalidade o mais rápido possível. Para isso, usará um agente personalizado.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente;
- usar o agente de acessibilidade no Copilot Chat para implementar um modo de alto contraste.

## Revise o agente personalizado de acessibilidade

Já existe um agente personalizado de acessibilidade. Crie uma branch separada para que o pull request (PR) de filtragem do exercício anterior permaneça independente e revise o conteúdo para entender como ele orientará o Copilot.

1. Volte ao codespace, abra um terminal e crie uma branch de trabalho a partir de `main`:

    ```bash
    git checkout main
    git pull
    git checkout -b accessibility-vscode
    ```

2. Abra `.github/agents/accessibility.md`.
3. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

4. Examine as seções seguintes, que destacam:
   - as principais responsabilidades ao gerar código para um site acessível;
   - as práticas recomendadas de acessibilidade;
   - exemplos de código em linguagem de marcação de hipertexto (HTML), folhas de estilo em cascata (CSS) e JavaScript;
   - problemas e erros comuns.

## Use o agente personalizado no Copilot Chat

O VS Code exibe todos os agentes personalizados definidos em `.github/agents` no menu suspenso de agentes, na parte inferior da exibição Copilot Chat. Você pode selecionar um agente personalizado para limitar uma sessão de chat às instruções e ferramentas dele.

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo:
>
> 1. Volte ao codespace e abra o painel do Copilot Chat.
> 2. Selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; as etapas indicarão as opções relevantes.

1. Se **Agent** ainda não estiver selecionado no menu suspenso de agentes da exibição Chat, selecione-o.

   ![Captura de tela do seletor de agente na exibição Chat](../../../_images/shared-chat-mode-selector.png)

2. Na parte inferior da exibição Chat, selecione o menu suspenso de agentes, que mostra o agente ativo — por padrão, **default**.
3. Selecione **Accessibility agent** na lista de agentes disponíveis.
4. Envie este prompt ao agente de acessibilidade:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. O Copilot Chat começará a trabalhar: explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá na conversa, junto com o caminho do arquivo e um diff para revisão.
6. Conforme as edições forem feitas, o indicador **Files changed** será atualizado para mostrar o conjunto de arquivos alterados pelo agente.

> [!NOTE]
> O processo provavelmente levará alguns minutos. O Copilot está alterando o repositório: poderá editar componentes Astro, CSS e testes relacionados.

Você acompanhará e orientará esse trabalho em andamento no próximo exercício.

## Resumo e próximas etapas

Nesta lição, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes especializados para tarefas e áreas específicas. Com eles, você pode transformar a experiência e os padrões da equipe em agentes reutilizáveis que orientam o Copilot.

Você explorou estes conceitos:

- como definir agentes personalizados;
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, [acompanhe e oriente o trabalho do agente][next-lesson], revise as alterações enquanto são feitas e adicione uma alternância para o modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no ambiente de desenvolvimento integrado (IDE)][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar sua funcionalidade com o servidor do Model Context Protocol (MCP) para o Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
