---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho ocorre em tempo real no seu computador, o Copilot Chat no VS Code mostra cada chamada de ferramenta e cada edição de arquivo enquanto acontecem. Você pode revisar cada diff no chat, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts complementares para aprimorar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- acompanhar em tempo real o trabalho do agente de acessibilidade;
- revisar e aceitar as edições de arquivo propostas;
- orientar o agente durante a sessão para adicionar uma alternância para o modo claro e outra para o alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design fez outra solicitação: também quer uma alternância para o modo claro, para as pessoas que consideram o tema padrão escuro demais. Em vez de iniciar uma nova conversa, você orientará a sessão existente para ampliar o trabalho.

## Acompanhe o progresso do Copilot

A exibição Copilot Chat mostra cada etapa do agente: os arquivos que explora, os comandos que executa e as edições que propõe. Cada edição aparece na conversa com um diff e os controles **Keep** e **Undo**.

1. Volte à exibição Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra a conversa para acompanhar cada chamada de ferramenta. Você verá leituras e edições de arquivos e comandos de terminal que ele pede para executar.
3. Quando o agente propuser uma edição, o diff aparecerá na conversa:
   - selecione **Keep** para aceitar a alteração;
   - selecione **Undo** para revertê-la e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver todos os arquivos alterados pelo agente nesta sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão — por exemplo, para executar um comando de terminal —, o agente fará uma pausa e exibirá a solicitação **Allow**. Aprove apenas os comandos com os quais você se sentir confortável.

5. Aguarde o agente concluir a implementação do modo de alto contraste. Uma mensagem final deverá resumir as alterações feitas.

## Oriente o agente para adicionar o modo claro

Agora que o modo de alto contraste está pronto, amplie a mesma conversa para adicionar uma alternância para o modo claro. Como você continuará na mesma sessão de chat, o agente manterá o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o padrão de armazenamento local.

1. No campo de entrada do Copilot Chat, envie este prompt complementar:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente continuará o trabalho, examinará as alterações anteriores e proporá adições para o modo claro.
3. Revise cada nova edição assim que aparecer, como fez com o modo de alto contraste. Use **Keep** ou **Undo** em cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar a interface de alternância de uma forma que você não prefere —, interrompa-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem complementar para esclarecer.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts complementares para pedir ajustes ("simplifique as folhas de estilo em cascata (CSS) — use variáveis CSS em vez de duplicar seletores"), corrigir a abordagem ("não adicione uma terceira alternância; inclua o modo claro no painel de configurações existente") ou ampliar o escopo ("adicione também atalhos de teclado para as duas alternâncias").

## Revise o conjunto de alterações

Antes de fazer commit do trabalho, examine rapidamente tudo o que o agente alterou.

1. Abra a exibição **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá encontrar atualizações em componentes Astro, estilos e testes relacionados.
3. Abra alguns arquivos alterados e examine os diffs. Confirme que a implementação reflete os padrões de acessibilidade do agente personalizado: atributos de Aplicações de Internet Ricas Acessíveis (ARIA), navegação por teclado, linguagem de marcação de hipertexto (HTML) semântica e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare as alterações e faça commit no painel Source Control. Você as publicará [na próxima lição][next-lesson].

## Resumo e próximas etapas

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou cada edição de arquivo, aceitou ou rejeitou alterações e enviou prompts complementares para ampliar o trabalho sem perder o contexto.

Nesta lição, você:

- acompanhou o trabalho de um agente em tempo real;
- aceitou e rejeitou edições individuais de arquivos;
- orientou uma sessão em andamento com prompts complementares.

Na [próxima lição][next-lesson], publique o trabalho de acessibilidade em um pull request e revise as alterações feitas pelo agente personalizado local ao longo deste laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
