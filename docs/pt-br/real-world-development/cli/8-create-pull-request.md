---
title: "Lição 8 - Crie e mescle o PR da funcionalidade"
description: "Revise em conjunto a filtragem, as instruções, a skill atualizada, o perfil de QA e os testes; depois, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora é hora de revisá-los em conjunto e abrir um pull request. Você mesmo mesclou o pull request (PR) de classificação por estrelas; desta vez, deixe o **Agent Merge** gerenciar o processo.

> [!NOTE]
> Em geral, dividiríamos a funcionalidade, as atualizações das instruções, a atualização da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma conversa e branch, e incluiremos esse trabalho neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem;
- examinar a alteração completa da funcionalidade e as evidências de verificação;
- criar o PR de filtragem;
- habilitar o Agent Merge somente depois da revisão e confirmar que o PR foi mesclado.

## Cenário

Ao longo do fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar uma funcionalidade. Agora, a Tailspin Toys quer automatizar o restante do trabalho do PR, mantendo a autorização para mesclar sob o controle da pessoa desenvolvedora.

## Apresentação do Agent Merge

O **Agent Merge** automatiza o trabalho restante para concluir um pull request. Quando você o habilita, o Copilot resolve os itens que impedem a mesclagem — corrige verificações de integração contínua (CI) com falha, responde a comentários de revisão e atualiza a branch com rebase quando necessário — e, em seguida, habilita a mesclagem automática do GitHub quando o repositório permite.

Até agora, você mesmo selecionou **Merge pull request**. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que está pronto antes de habilitar o Agent Merge.

## Use o Agent Merge para gerenciar o PR

Com todo o código pronto, vamos revisá-lo em conjunto, criar o PR e deixar o Agent Merge gerenciar o restante do processo.

1. Volte ao codespace.
2. Abra a caixa de diálogo do agente digitando `/agent`.
3. Selecione **Default** na lista de opções e pressione <kbd>Enter</kbd>.
4. Crie um novo PR com o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua no PR. Quando tudo for aprovado, ele fará a mesclagem.
7. Confirme que aparece uma mensagem do Copilot semelhante a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de mesclagem, configurações do repositório nem permissões ausentes. Se o processo for bloqueado, leia o motivo informado e faça manualmente a mesclagem revisada quando o repositório permitir.

## Resumo e próximas etapas

Você automatizou várias etapas do desenvolvimento, incluindo geração de código, testes e validação e, agora, o processo do pull request. Nesta lição, você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem;
- examinou a alteração completa da funcionalidade e as evidências de verificação;
- criou o PR de filtragem;
- habilitou o Agent Merge somente depois da revisão e confirmou que o PR foi mesclado.

Em seguida, [explore outros comandos de barra úteis do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
