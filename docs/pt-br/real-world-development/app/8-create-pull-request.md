---
title: "Lição 8 - Crie e mescle o PR da funcionalidade"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, autorize o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

A implementação da filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora é hora de revisá-los em conjunto e abrir um pull request. Você mesmo mesclou o pull request (PR) de classificação por estrelas; desta vez, deixe o **Agent Merge** gerenciar o processo.

> [!NOTE]
> Em geral, dividiríamos a funcionalidade, as atualizações das instruções, a atualização da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma única sessão e branch, e incluiremos esse trabalho neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem;
- examinar o PR completo da funcionalidade e as evidências de verificação;
- autorizar o Agent Merge somente depois da revisão e confirmar que o PR foi mesclado.

## Cenário

Ao longo do fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar uma funcionalidade. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização para mesclar sob o controle da pessoa desenvolvedora.

## Apresentação do Agent Merge

O **Agent Merge** automatiza o trabalho restante necessário para integrar um pull request no aplicativo GitHub Copilot. Quando você o habilita, a sessão do aplicativo lê o pull request e resolve o que estiver bloqueando o processo — corrige verificações de integração contínua (CI) com falha, responde a comentários de revisão e faz rebase quando necessário —, depois mescla o PR assim que o GitHub permite. O processo é executado em segundo plano, continua após a reinicialização do aplicativo e é desativado automaticamente quando o pull request é mesclado.

Até agora, você selecionou **Merge pull request** por conta própria. O Agent Merge pode assumir essa responsabilidade, mas ainda precisa de autorização explícita para editar código e fazer a mesclagem. Revise as ações permitidas e o trabalho antes de conceder a permissão de mesclagem.

## Use o Agent Merge para gerenciar o PR

Com todo o código criado e revisado, vamos permitir que o Agent Merge gerencie o processo do PR.

1. Use o seletor de agentes para selecionar **Default agent**.
2. Selecione o menu suspenso ao lado de **Create PR**.
3. Selecione **Agent Merge**. O botão mudará para **Agent Merge**.
4. Selecione o botão **Agent Merge** para iniciar o processo. Selecionar a opção no menu suspenso, por si só, não inicia o processo.

O Agent Merge iniciará o processo. Ele vai:

- criar o pull request com título e descrição;
- se a sessão tiver começado a partir de uma issue, incluir uma referência a ela no corpo da descrição;
- fazer rebase ou resolver possíveis conflitos de mesclagem com a branch de destino;
- acompanhar a integração contínua para confirmar que todas as verificações foram aprovadas;
- acompanhar comentários de outras pessoas desenvolvedoras ou da revisão de código do Copilot e fazer alterações para resolvê-los;
- opcionalmente, mesclar o PR automaticamente assim que tudo for aprovado.

Vamos permitir que o Agent Merge também mescle o PR quando todas as verificações forem aprovadas.

5. Revise o diff do PR e as evidências de verificação antes de conceder permissão de mesclagem.
6. Selecione o menu suspenso ao lado de **Agent Merge**.
7. Confirme que há uma marca de seleção ao lado de **Merge pull request**.
8. Peça explicitamente ao agente que faça a mesclagem:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Acompanhe o progresso e resolva os bloqueios informados. Antes de continuar, confirme que o status do PR é **Merged**; a permissão de mesclagem habilitada ou um agente em execução não confirma que a mesclagem foi concluída.

> [!IMPORTANT]
> O Agent Merge não ignora as proteções do repositório nem permissões ausentes. Resolva esses bloqueios antes de continuar.

## Resumo e próximas etapas

Você automatizou várias etapas do desenvolvimento, incluindo geração de código, testes e validação e, agora, o processo do pull request. Nesta lição, você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem;
- examinou o PR completo da funcionalidade e as evidências de verificação;
- autorizou o Agent Merge somente depois da revisão e confirmou que o PR foi mesclado.

Em seguida, [use um canvas existente e crie um canvas de triagem][next-lesson] para explorar uma forma mais completa de examinar, planejar e visualizar o trabalho com o agente.

## Recursos

- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
