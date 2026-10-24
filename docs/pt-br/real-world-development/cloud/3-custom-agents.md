---
title: "Exercício 3 - Agentes personalizados"
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

Com agentes personalizados, você vai explorar:

- como definir agentes personalizados;
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys quer garantir que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas capacidades ou preferências visuais. Comentários recentes de usuários apontam que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do baixo contraste entre o texto e o plano de fundo. Para resolver esse problema de acessibilidade, a equipe de design solicitou um modo de alto contraste que as pessoas possam ativar e desativar.

Como a acessibilidade é essencial, você quer implementar a funcionalidade o mais rápido possível. Para isso, usará um agente personalizado.

## Revise o agente personalizado de acessibilidade

Já existe um agente personalizado de acessibilidade. Revise o conteúdo para entender como ele orientará o Copilot.

1. Volte ao codespace.
2. Abra `.github/agents/accessibility.md`.
3. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

4. Examine as seções seguintes, que destacam:
   - as principais responsabilidades ao gerar código para um site acessível;
   - as práticas recomendadas de acessibilidade;
   - exemplos de código em linguagem de marcação de hipertexto (HTML), folhas de estilo em cascata (CSS) e JavaScript;
   - problemas e erros comuns.

## Crie e atribua uma issue

O painel de agentes é o espaço central para trabalhar com agentes no ambiente. Você pode atribuir tarefas ao agente de nuvem do Copilot, acompanhar o trabalho e redirecionar sessões em andamento ou fornecer orientações adicionais. Vamos atribuir ao Copilot a tarefa de criar o modo de alto contraste.

1. Acesse seu repositório.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina **Title** como `Add high contrast mode to website`.
6. Defina **Description** como:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.
9. Selecione **Accessibility agent** na lista de agentes personalizados.

    ![Captura de tela da atribuição do agente de nuvem, com as opções de agente personalizado e acessibilidade destacadas](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. O Copilot começará a trabalhar na tarefa em segundo plano.

## Resumo e próximas etapas

Nesta lição, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes especializados para tarefas e áreas específicas. Com eles, você pode transformar a experiência e os padrões da equipe em agentes reutilizáveis que orientam o Copilot.

Você explorou estes conceitos:

- como definir agentes personalizados;
- como atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot implementa o modo de alto contraste, [acompanhe e oriente a sessão do agente][next-lesson] no painel de agentes.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar sua organização para usar agentes personalizados][org-custom-agents]
- [Preparar sua empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
