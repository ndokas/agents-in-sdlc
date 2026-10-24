---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use Plan para definir uma abordagem, Autopilot para desenvolver a filtragem a partir de uma issue e Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando uma pequena funcionalidade ao projeto. Mudanças maiores, porém, exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi criado para trabalhar com o fluxo existente de uma organização e ajudar a desenvolver as coisas certas da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento com agentes: começará com uma issue para gerar uma funcionalidade, verificará se o código é válido e se a funcionalidade se comporta como esperado e, por fim, fará a mesclagem no projeto.

> [!NOTE]
> Você usará a mesma conversa e branch ao continuar o fluxo de trabalho da funcionalidade. Em geral, seriam usadas branches ou pull requests (PRs) diferentes para cada tipo de arquivo, mas faremos esse atalho para manter o foco nos conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa do Copilot a partir de uma issue do GitHub;
- definir os requisitos no modo Plan;
- implementar a funcionalidade no modo Autopilot;
- revisar o código;
- validar a funcionalidade manualmente em um navegador com encaminhamento de porta.

Ao continuar o desenvolvimento, você atualizará as instruções do repositório, personalizará a skill `quality-checks` existente, adicionará a validação com MCP, criará um agente de QA e abrirá o PR da funcionalidade.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve a funcionalidade, mas detalhes como a combinação de categorias precisam ser definidos antes da implementação. Você usará o modo Plan para resolver essas decisões e, depois, autorizará uma implementação com escopo definido no modo Autopilot.

## Contexto

Incorporar agentes de programação com IA ao fluxo de desenvolvimento não muda os fundamentos. Na verdade, eles se tornam ainda mais importantes. A maioria das pessoas desenvolvedoras segue um fluxo semelhante a este:

1. Comece com uma issue registrada que descreva o que precisa ser feito.
2. Planeje o que será desenvolvido.
3. Desenvolva e revise o código.
4. Execute os testes para validar o código.
5. Valide manualmente a nova funcionalidade.
6. Crie um pull request (PR).
7. Depois que o código for revisado e a integração contínua for aprovada, faça a mesclagem.

> [!NOTE]
> Os detalhes variam conforme a equipe e a organização, mas a maioria dos processos segue uma variação dessas etapas.

Ao seguir essa abordagem padrão, você garante que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de validação que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano primeiro e não pode editar os arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma — escreve código, executa testes e faz iterações até considerar a tarefa concluída.

Comece no modo Plan, revise o plano e, em seguida, use Autopilot para implementá-lo.

## Comece pela issue

Antes de iniciar o trabalho de filtragem, volte ao codespace e confirme que o repositório e o terminal estão prontos.

1. Volte ao codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme que o PR de classificação por estrelas foi mesclado.
3. Se o terminal ainda não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as teclas de seta para destacar a issue intitulada **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que agora o prompt começa com `#7` (ou um número semelhante). O caractere `#` permite incluir no contexto uma issue ou um pull request (PR) do GitHub.

## Planeje a funcionalidade de filtragem

O planejamento permite definir a abordagem para implementar uma funcionalidade ou executar tarefas antes de delegá-las ao Copilot. É recomendável reservar algum tempo para planejar tarefas complexas. Vamos mudar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme que o indicador abaixo do prompt exibe **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, digite este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começará a preparar o plano. Ele explorará o projeto e, em seguida, determinará a melhor abordagem.

3. Durante o processo, o Copilot poderá perguntar como a filtragem deve funcionar. Responda de acordo com suas preferências. Não há respostas erradas.
4. Quando o plano estiver concluído, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Percorra o plano para revisá-lo.
6. Peça ao Copilot que revise qualquer parte do plano que não corresponda às suas decisões.

## Aprove o Autopilot

Com o plano escrito e revisado, implemente-o. Deixe o Copilot trabalhar no modo Autopilot.

O Autopilot permite que o Copilot itere sobre o problema até considerar que ele foi resolvido.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção equivalente na versão instalada.
2. Confirme que o indicador abaixo do prompt exibe **Autopilot**.
3. Observe o Copilot seguir o plano, gerar código e executar testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente; por isso, revise o plano antes. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revise e verifique a implementação

Depois que o código for gerado, revise-o antes da mesclagem, como faria com qualquer outro código. Vamos revisar o código e executar o site para confirmar que tudo está funcionando.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador não exibe **Plan** nem **Autopilot**.
2. Digite `/diff` e examine a implementação da filtragem e os testes.
3. Compare o resultado com a issue e com as decisões tomadas durante o planejamento.
4. Depois de revisar o código, pressione <kbd>Esc</kbd> para sair da visualização do diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot que corrija as falhas.

## Explore a nova funcionalidade

O código parece correto, mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Experimente os filtros por categoria e editora, além das combinações definidas no plano.
4. Confirme se o comportamento de redefinição e de resultados vazios corresponde à issue e às suas decisões.
5. Volte ao codespace e peça ao Copilot que pare o servidor de desenvolvimento iniciado.

## Resumo e próximas etapas

Você usou diferentes modos de conversa para desenvolver e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma conversa do Copilot a partir de uma issue do GitHub;
- definiu os requisitos no modo Plan;
- implementou a funcionalidade no modo Autopilot;
- revisou o código;
- validou a funcionalidade manualmente em um navegador com encaminhamento de porta.

Em seguida, aprofunde-se na geração de código e garanta que ela siga as práticas documentadas [usando instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
