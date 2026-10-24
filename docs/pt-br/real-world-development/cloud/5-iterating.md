---
title: "Exercício 5: Iterar no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que adicionasse documentação ao código, criasse uma funcionalidade de jogos relacionados para a equipe de design explorar e implementasse recursos de acessibilidade, incluindo alternâncias para os modos de alto contraste e claro. Agora, examine as alterações no código e, se necessário, dê feedback ao Copilot para aprimorar o trabalho.

### Cenário

Como já destacamos várias vezes, a IA generativa não muda os fundamentos do design de software e do DevOps. Sempre revisamos o código gerado e seguimos nosso processo normal de DevOps. Com isso em mente, vamos revisar as sugestões do GitHub Copilot para criar a documentação, a funcionalidade de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, foram implementadas algumas restrições de segurança:

- O Copilot tem acesso somente de leitura ao repositório e acesso de gravação **apenas** à branch que usará para o código.
- O agente de nuvem é executado no GitHub Actions, que cria um ambiente separado e temporário para o trabalho.
- Uma pessoa precisa aprovar qualquer fluxo de trabalho do GitHub Actions antes da execução.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), inclusive a servidores MCP.

## Revise a documentação gerada

Vamos começar examinando o primeiro pull request (PR) gerado pelo agente de nuvem do GitHub Copilot — o que adiciona documentação ao código. Você fará isso usando a interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao examinar o PR, talvez você veja um aviso de que um firewall bloqueou o GitHub Copilot. Isso é **esperado**, pois o acesso do Copilot a recursos externos, incluindo servidores MCP externos, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem do Copilot][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request intitulado **Add missing documentation** ou com um título mais descritivo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request conterá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos; faça uma pausa ou reflita sobre o que aprendeu até agora.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que inclui novos comentários de documentação TSDoc e outros elementos. As alterações exatas variam.

    Ao examinar as alterações, procure comentários de documentação TSDoc, convenções de TypeScript e cabeçalhos de comentário. Eles vêm dos arquivos de instruções personalizadas que você examinou no início do Exercício 2.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Deve aparecer um indicador informando que alguns fluxos de trabalho aguardam aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão ser adicionados à seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Solicite alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma interação de mão única. Você também pode mencionar o Copilot em comentários do pull request ou em comentários diretamente no código, como faria com outras pessoas da equipe. O Copilot lerá esses comentários e iniciará outra sessão para atendê-los. Como os resultados são não determinísticos, não há um texto específico que possamos recomendar. Algumas ideias de alterações que você pode solicitar:

- adicionar cabeçalhos de comentário no início de cada arquivo de código com uma breve descrição da finalidade;
- adicionar comentários de documentação TSDoc a arquivos TypeScript e Astro;
- criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração à documentação gerada e mencione **@copilot**, como faria com qualquer pessoa. Use uma das ideias acima ou outra sugestão de documentação para a base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe como ele inicia uma nova sessão para fazer as atualizações.
3. Você pode selecionar **Back to pull request** para voltar ao pull request.

    ![Botão Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, um novo commit deverá aparecer no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Quando estiver tudo certo, você poderá converter o PR de rascunho em pronto para revisão e mesclá-lo na branch principal.

![Converter PR em pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise a funcionalidade de jogos relacionados

Agora, volte ao PR gerado pelo Copilot para resolver a issue que solicita a exibição de jogos relacionados na página de detalhes.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page** ou com um título mais descritivo.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
6. Deve aparecer um indicador informando que alguns fluxos de trabalho aguardam aprovação.
7. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

8. Os fluxos de trabalho deverão ser adicionados à seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** mude para esta branch no codespace para testar manualmente a funcionalidade de jogos relacionados. Acesse o codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou a funcionalidade de jogos relacionados. Como antes, você pode trabalhar iterativamente com o agente de nuvem e solicitar atualizações. Por exemplo, você pode pedir ao Copilot que ajuste a quantidade de jogos relacionados ou adicione cabeçalhos de comentário e comentários TSDoc (lembre-se: esta issue foi atribuída **antes** da atualização das instruções personalizadas). Como nos outros casos, adicione um comentário na guia **Conversation** para que o Copilot o leia e inicie outra sessão.

## Revise os recursos de acessibilidade

Por fim, revise os recursos de acessibilidade implementados com o agente personalizado de acessibilidade. Este PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no painel de agentes no Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website** ou com um título mais descritivo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request conterá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho, o que pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - componentes de interface para alternar entre os modos;
   - uso do armazenamento local para persistir as preferências;
   - alterações de CSS ou de estilo para os modos de alto contraste e claro;
   - atributos de acessibilidade (rótulos ARIA, navegação por teclado etc.);
   - código JavaScript/TypeScript que gerencia a alternância entre os modos.
6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Deve aparecer um indicador informando que alguns fluxos de trabalho aguardam aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão ser adicionados à seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** mude para esta branch no codespace para testar manualmente os recursos de acessibilidade. Acesse o codespace, abra o terminal e execute estes comandos (substitua `<branch-name>` pelo nome da branch criada pelo Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste as alternâncias dos modos de alto contraste e claro no navegador para confirmar que funcionam e persistem após recarregar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot a implementar essas funcionalidades de acordo com as práticas recomendadas de acessibilidade. Se encontrar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário para solicitar alterações, como fez nos PRs anteriores.

## Exercício opcional — continue delegando

O agente de nuvem funciona melhor quando você pode delegar itens reais do backlog e se dedicar a outras tarefas. Para criar esse hábito, registre mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo;
- implementar paginação na página de lista de jogos;
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Ao longo destas lições, você:

- **examinou os arquivos de instruções personalizadas fornecidos pelo repositório** para observar o efeito deles no trabalho do agente de nuvem;
- **atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona;
- **criou e usou um agente personalizado** para acessibilidade e adicionou alternâncias para os modos de alto contraste e claro;
- **usou a página de agentes do Copilot como painel central** para acompanhar e orientar a sessão de acessibilidade em andamento;
- **revisou e iterou nos pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando fluxos de trabalho.

## Revisão e próximas etapas

Você concluiu o percurso do agente de nuvem. Se quiser continuar explorando, os outros percursos complementam o que você praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, MCP do Playwright, Agent Merge e uma integração opcional com `/delegate` para voltar ao agente de nuvem.

No seu próprio repositório, experimente estas ideias:

- atribua ao agente de nuvem uma issue de refatoração ou de cobertura de testes;
- crie um agente personalizado para outro domínio;
- configure `copilot-setup-steps.yml` para outra pilha tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
