---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica ou o Copilot precisa conhecer informações adicionais, disponibilize esse contexto. Os [arquivos de instruções][instruction-files] orientam a geração de código para que o Copilot entenda não apenas *o que* você quer, mas também *como* fazer.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio das instruções do repositório e de arquivos aplicados a caminhos específicos.
- gerar a primeira parte dos dados para filtragem — um auxiliar de editoras — com as instruções atuais.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.
- executar novamente o mesmo prompt e observar o código gerado seguir o novo padrão.
- fazer commit das instruções e da primeira parte da filtragem em `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos. O Copilot é não determinístico. O objetivo é observar a tendência de mudança após atualizar as instruções, não obter resultados idênticos caractere por caractere.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface do usuário (UI) deve usar um tema escuro e ter uma aparência moderna.
- O código deve incluir documentação em comentários TSDoc.
- O início de cada arquivo deve conter um bloco de comentários que descreva sua finalidade.

Os arquivos de instruções fornecem ao Copilot as informações necessárias para seguir essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor o estilo e os requisitos de programação. Elas ajudam a orientar sugestões de código e trechos gerados. Você pode especificar convenções, bibliotecas e os tipos de comentários desejados, tanto para todo o repositório quanto para tipos específicos de arquivos ou tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter contexto do projeto relevante para a maioria das solicitações, como a pilha de tecnologias, uma visão geral do produto, práticas recomendadas e orientações globais.
- `.github/instructions/*.instructions.md`: arquivos para tarefas ou tipos de arquivo específicos, como TypeScript, Astro, componentes de interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para sugestões de conclusão de código ou de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam as instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a outros formatos de arquivos de instruções][custom-instructions-support], como `AGENTS.md` e `CLAUDE.md`.

### Práticas recomendadas para gerenciar arquivos de instruções

Os exemplos do projeto mostram uma abordagem possível:

- Mantenha `copilot-instructions.md` focado em orientações do projeto, como uma descrição do que está sendo desenvolvido, a estrutura do projeto e os padrões globais de código.
- Use arquivos `*.instructions.md` para tipos de arquivo (testes unitários, componentes Astro, camada de dados) ou tarefas específicas.
- Use linguagem natural e orientações claras. Inclua exemplos de código adequado e inadequado.

Não há uma única maneira correta de criar arquivos de instruções ou usar IA. Experimente para descobrir o que funciona no seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Neste projeto, há orientações para várias tarefas, incluindo [atualizações de UI][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada ambiente oferece essa opção de forma diferente, por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. A lição do ambiente em uso apresenta a opção relevante.
>
> Para encontrar modelos ou exemplos, explore o [awesome-copilot][awesome-copilot], um repositório de arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções do projeto

Leia os arquivos de instruções incluídos no repositório: o arquivo principal `copilot-instructions.md` e os arquivos `*.instructions.md` para diferentes tarefas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam a todas as interações com o Copilot.
3. Abra `.github/instructions` e examine os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo` no frontmatter. Ele define um padrão global relativo à raiz do repositório. Neste caso, ele corresponde a arquivos de teste TypeScript, como os que seguem `**/*.test.ts`.
5. Leia as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e vá até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos do projeto. Assim, você pode dividir conjuntos de instruções maiores em arquivos menores e reutilizáveis e indicar exemplos ao Copilot. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de arquivo.

## Criar uma branch para as alterações

Crie uma branch para manter as alterações deste exercício isoladas.

1. Volte ao Codespace usado no exercício anterior.
2. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e selecione uma branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat antes de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> Volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa informa o que selecionar.

Para observar o efeito das instruções personalizadas, envie primeiro um prompt com o arquivo de instruções atual. Depois, atualize-o e envie o mesmo prompt novamente para comparar.

1. Feche as guias de editor abertas em exercícios anteriores para limitar o contexto disponível ao Copilot.
2. Abra `src/lib/publishers.ts` para indicar onde o auxiliar deve ficar.
3. Na lista de agentes da exibição Chat, selecione **Agent** para que o Copilot possa alterar arquivos.

   ![Seletor de agentes na exibição Chat](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará alterações, normalmente ao arquivo do auxiliar e aos testes.
6. Observe que a função gerada é tipada, recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras. Isso segue as convenções da camada de dados em `.github/instructions/drizzle.instructions.md`, aplicadas a `src/lib/*.ts`.
7. Observe se o código proposto não inclui comentários TSDoc nem cabeçalho de comentário do arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele adicione comentários mesmo sem essa orientação. Tudo bem: o objetivo é observar a consistência após a atualização das instruções.

## Adicionar um padrão para todo o repositório

Adicione orientações globais ao `.github/copilot-instructions.md` para melhorar as sugestões de código.

1. Abra `.github/copilot-instructions.md` novamente.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Ela ainda não exige documentação no código, por isso o auxiliar gerado pode não ter comentários.
3. Logo abaixo dos padrões existentes, adicione estas linhas Markdown:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Arquivos de instruções podem ser criados na raiz do repositório (`.github/copilot-instructions.md`) para orientações globais ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o local adequado para padrões gerais da equipe, como a regra de documentação adicionada aqui.

## Executar novamente o prompt e observar a alteração

Agora envie o mesmo prompt em uma conversa limpa para observar como a nova orientação afeta a proposta do Copilot.

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto da conversa.
2. Abra `src/lib/publishers.ts` para direcionar o Copilot ao arquivo correto.
3. Envie o mesmo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe se o arquivo proposto começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe se a função proposta agora inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você orientou o Copilot a seguir um novo padrão do projeto e aplicá-lo a código real, que será usado no próximo exercício.

## Fazer commit das instruções e enviar a branch

Os arquivos de instruções são gerenciados pelo controle do código-fonte como qualquer outro arquivo do repositório. Faça commit e envie a branch.

1. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Confirme se as instruções e as alterações do auxiliar estão presentes:

   ```bash
   git status
   ```

3. Prepare e faça commit das instruções e de todos os arquivos alterados pelo Copilot para o auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras alterações relacionadas, como testes, inclua esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e mesclar um pull request

Depois de enviar a branch, crie um pull request associado à issue de documentação do backlog. O Copilot pode encontrar a issue, criar um PR para fechá-la e mesclar o PR.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot atua no GitHub. O modelo do projeto já conecta as ferramentas do GitHub ao ambiente. Na primeira utilização, o VS Code talvez peça para você entrar no GitHub. Siga os prompts para autorizar. Em outro exercício, você aprenderá como essa conexão funciona com o Model Context Protocol (MCP).

1. Abra o Copilot Chat no Codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a exibição Copilot Chat e confirme que **Agent** está selecionado na lista de agentes.
3. Peça ao Copilot que encontre a issue de atualização das instruções e crie um PR a partir da branch atual, descrevendo também a base do auxiliar de editoras:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. O Copilot procurará a issue e criará o PR.
5. Quando solicitado a permitir operações do GitHub em seu nome, revise o comando e selecione **Allow** se estiver de acordo.
6. Depois da criação do PR, peça ao Copilot para mesclá-lo e atualizar sua branch local:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Quando solicitado a permitir operações do GitHub e comandos do shell, revise cada comando e selecione **Allow** se estiver de acordo.

Você criou e mesclou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar a saída com as instruções existentes.
- adicionar um padrão para todo o repositório em `.github/copilot-instructions.md`.
- enviar novamente o mesmo prompt e observar o código seguir o novo padrão.
- fazer commit das instruções e da base do auxiliar em `main` para que o próximo exercício possa usar esse trabalho.

Em seguida, use as instruções no [modo agente][next-lesson] para adicionar um recurso à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
