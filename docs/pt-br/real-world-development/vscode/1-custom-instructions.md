---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com inteligência artificial generativa. Se uma tarefa precisa ser feita de uma forma específica — ou se há informações de contexto que o Copilot deve conhecer —, disponibilize esse contexto. Há várias maneiras de fornecer informações específicas ao Copilot. Uma das principais são os [arquivos de instruções][instruction-files], que orientam a geração de código.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de codificação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos associados a caminhos;
- gerar a primeira parte de dados para a filtragem (um helper de editoras) usando as instruções atuais;
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`;
- enviar o mesmo prompt novamente e observar o código gerado adotar o novo padrão;
- fazer commit da atualização das instruções e da parte de filtragem em `main` para que o Copilot possa usar as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir alguns dos padrões definidos. O Copilot é não determinístico. O objetivo deste exercício é observar a *tendência* de mudança no comportamento depois da atualização das instruções, e não reproduzir exatamente cada caractere da saída.

## Arquivos de instruções

### Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes unitários.
- A interface do usuário deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir comentários de documentação TypeScript (TSDoc).
- Um bloco de comentários no início de cada arquivo deve descrever a finalidade dele.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de codificação e seus requisitos. Esse recurso ajuda a orientar o Copilot para gerar sugestões e trechos de código mais relevantes. Você pode definir convenções de codificação, bibliotecas e até os tipos de comentários que prefere. As instruções podem abranger todo o repositório ou tipos específicos de arquivo.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em **todas** as solicitações para o repositório. Use-o para informações do projeto relevantes à maioria das solicitações de chat ou da CLI, como a pilha tecnológica, uma visão geral do que está sendo desenvolvido, práticas recomendadas e orientações gerais.
- Arquivos `.github/instructions/*.instructions.md`, que podem ser criados para tarefas ou tipos de arquivo específicos. Use-os para orientar linguagens, como TypeScript ou Astro, ou tarefas, como criar um componente de interface ou um conjunto de testes.

> [!NOTE]
> Ao trabalhar no IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, mas não para conclusões de código ou sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam tanto as instruções do repositório quanto os arquivos `*.instructions.md` com frontmatter `applyTo`.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], incluindo AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Não é possível abordar em detalhes a criação de arquivos de instruções neste exercício, mas os exemplos do projeto mostram uma abordagem representativa. Em linhas gerais:

- Mantenha as instruções em `copilot-instructions.md` focadas em orientações para o projeto, como uma descrição do que está sendo desenvolvido, a estrutura do projeto e os padrões gerais de codificação.
- Use arquivos `*.instructions.md` para tipos de arquivo específicos (testes unitários, componentes Astro e camada de dados, por exemplo) ou para tarefas específicas.
- Escreva em linguagem natural e mantenha as orientações claras. Inclua exemplos de como o código deve — e não deve — ser escrito.

Não existe uma única maneira de criar arquivos de instruções, assim como não há uma única maneira de usar IA. Experimente para descobrir o que funciona melhor no seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Ao explorar os arquivos deste projeto, você verá orientações para diversos tipos de tarefa, incluindo [atualizações da interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa opção de uma forma diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI). A lição referente à interface que você está usando indicará essa opção quando for relevante.
>
> Para encontrar modelos ou um ponto de partida, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas deste projeto

Leia os arquivos de instruções fornecidos pelo repositório: há um arquivo principal `copilot-instructions.md` e vários arquivos `*.instructions.md` para diferentes tarefas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam a todas as interações com o Copilot.
3. Abra a pasta `.github/instructions` e examine os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros itens.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um glob relativo à raiz do repositório que determina quais arquivos são abrangidos. Neste caso, ele corresponde a arquivos de teste TypeScript, como os que atendem a `**/*.test.ts`.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e examine o final do arquivo. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes no projeto. Isso permite dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e indicar exemplos para o Copilot. Os caminhos são relativos ao arquivo de instruções, e não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de codificação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Crie uma branch para as alterações

Vamos seguir algumas práticas recomendadas e criar uma branch para as alterações.

1. Volte ao codespace usado no exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e selecione uma nova branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Use o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; as etapas indicarão as opções relevantes.

Para observar o impacto das instruções personalizadas, primeiro envie um prompt com o arquivo de instruções atual. Depois, atualize o arquivo e envie o mesmo prompt novamente para comparar os resultados.

1. Feche as guias de editor abertas nos exercícios anteriores para que o Copilot use apenas o contexto desejado.
2. Abra `src/lib/publishers.ts` para indicar ao Copilot onde o helper deve ficar.
3. Na exibição Chat, selecione **Agent** no menu suspenso de agentes para permitir que o Copilot aplique alterações nos arquivos.

   ![Captura de tela do seletor de agente na exibição Chat](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará as atualizações de código, muitas vezes alterando o arquivo do helper e os testes.
6. Observe que o helper proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras. Isso vem das convenções da camada de dados em `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observe que o código proposto **não inclui** comentários de documentação TSDoc nem um cabeçalho de comentário no arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, ele pode incluir comentários de documentação mesmo sem essa orientação. Se isso acontecer, tudo bem: o objetivo continua sendo observar o ganho de *consistência* após atualizar as instruções.

## Adicione um padrão para o repositório

Como mencionado, `.github/copilot-instructions.md` fornece informações do projeto ao Copilot. Documente os padrões de codificação do repositório para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe como ela documenta os padrões de codificação do projeto, mas ainda não contém regras para documentação no código — por isso o helper gerado não tinha comentários de documentação.
3. Adicione as linhas Markdown a seguir logo abaixo dos padrões existentes para orientar o Copilot a adicionar cabeçalhos de comentário nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados no nível do repositório (`.github/copilot-instructions.md`) para orientações globais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o lugar adequado para padrões gerais, como a regra de comentários de documentação que você acabou de adicionar.

## Envie o prompt novamente e observe a mudança

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto da conversa.
2. Selecione novamente `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie **o mesmo prompt** de antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você orientou o Copilot a seguir um novo padrão de projeto e aplicá-lo ao código que servirá de base para o próximo exercício.

## Faça commit das instruções e envie a branch

Os arquivos de instruções são recursos do repositório e são gerenciados com o mesmo processo de controle do código-fonte usado para outros arquivos. Vamos fazer commit das alterações e enviar a branch.

1. Abra uma nova janela do terminal pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. No terminal, confirme se a atualização das instruções e as alterações do helper estão presentes:

   ```bash
   git status
   ```

3. No terminal, prepare e faça commit da atualização das instruções e de todos os arquivos alterados pelo Copilot para criar a base do helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras alterações de suporte (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch para o repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Crie e mescle um pull request

Com a branch enviada, crie um pull request e associe-o à issue sobre o padrão de documentação no backlog. O Copilot pode fazer isso usando as ferramentas do GitHub já conectadas ao projeto. Peça que encontre a issue, crie um PR associado a ela e, em seguida, faça a mesclagem.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executará ações no GitHub em seu nome. O modelo do projeto já conecta as ferramentas do GitHub ao espaço de trabalho, então não é necessário configurar nada. Na primeira vez que o Copilot usar uma delas, o VS Code talvez solicite autenticação no GitHub. Siga as instruções para autorizá-lo. Em um exercício posterior, você verá como essa conexão funciona com o Model Context Protocol.

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a exibição Copilot Chat e confirme que **Agent** está selecionado no seletor de agentes.
3. Peça ao Copilot que encontre a issue relacionada à atualização dos arquivos de instruções e crie um PR a partir da branch atual, descrevendo as atualizações das instruções e a nova base do helper de editoras:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. O Copilot começará a procurar a issue e criar o PR.
5. Quando o Copilot pedir permissão para executar ações do GitHub em seu nome, revise o comando e selecione **Allow**, conforme apropriado.
6. Quando o PR for criado, peça ao Copilot que o mescle e volte sua branch local para `main`:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Quando o Copilot pedir permissão para executar ações do GitHub e comandos do shell em seu nome, revise os comandos e selecione **Allow**, conforme apropriado.

Você criou e mesclou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximas etapas

Você explorou como o Copilot obtém contexto dos arquivos de instruções deste projeto e usou o Copilot Chat no VS Code para:

- explorar como o contexto do projeto, as diretrizes de codificação e os padrões de documentação chegam ao Copilot por meio das instruções personalizadas do repositório e de arquivos associados a caminhos;
- gerar a primeira parte de dados para a filtragem (um helper de editoras) usando as instruções *existentes*;
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`;
- enviar novamente o mesmo prompt e observar o código proposto adotar o novo padrão;
- fazer commit da atualização das instruções e da base do helper em `main` para que o próximo exercício possa usá-las.

Em seguida, use essas instruções no [modo de agente][next-lesson] para adicionar uma funcionalidade em várias partes da base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções personalizadas pessoais para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
