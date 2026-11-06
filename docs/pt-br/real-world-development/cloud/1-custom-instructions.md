---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com inteligência artificial (IA) generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou há informações de contexto que o Copilot deve conhecer — disponibilize essas informações. Os [arquivos de instruções][instruction-files] fornecem essa orientação para que o Copilot entenda não apenas *o que* fazer, mas também *como* fazer.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções aplicados a caminhos específicos.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos percursos do VS Code e do CLI, você não executará um prompt antes e depois para comparar os resultados. O agente de nuvem do Copilot trabalha de forma assíncrona em issues do GitHub, então é mais difícil demonstrar o efeito lado a lado em tempo real. Você verá a influência do arquivo de instruções ao revisar os pull requests gerados pelo agente nas próximas lições.

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

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter contexto do projeto relevante para a maioria das solicitações, como a pilha de tecnologias, uma visão geral do produto, práticas recomendadas e outras orientações globais.
- `.github/instructions/*.instructions.md`: arquivos que você pode criar para tarefas ou tipos de arquivo específicos, como TypeScript, Astro, componentes de interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para sugestões de conclusão de código ou de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam as instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a outros formatos de arquivos de instruções][custom-instructions-support], como `AGENTS.md` e `CLAUDE.md`.

### Práticas recomendadas para gerenciar arquivos de instruções

Os exemplos do projeto mostram uma abordagem possível para organizar arquivos de instruções:

- Mantenha `copilot-instructions.md` focado em orientações do projeto, como uma descrição do que está sendo desenvolvido, a estrutura do projeto e os padrões globais de código.
- Use arquivos `*.instructions.md` para instruções específicas de tipos de arquivo (testes unitários, componentes Astro, camada de dados) ou tarefas.
- Use linguagem natural e mantenha as orientações claras. Inclua exemplos de código adequado e inadequado.

Não há uma única maneira correta de criar arquivos de instruções ou de usar IA. Experimente para descobrir o que funciona no seu projeto.

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

## Explorar os arquivos de instruções personalizados do projeto

Leia os arquivos de instruções incluídos no repositório: o arquivo principal `copilot-instructions.md` e os arquivos `*.instructions.md` para diferentes tarefas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam a todas as interações com o Copilot.
3. Abra a pasta `.github/instructions` e examine os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo` no frontmatter. Ele define um padrão global relativo à raiz do repositório. Neste caso, ele corresponde a arquivos de teste TypeScript, por exemplo, os que seguem `**/*.test.ts`.
5. Leia as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e vá até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos do projeto. Assim, você pode dividir conjuntos de instruções maiores em arquivos menores e reutilizáveis e indicar exemplos ao Copilot. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de arquivo.

## Adicionar um padrão para todo o repositório

Adicione uma orientação global que exija comentários TSDoc e um cabeçalho de comentário para cada arquivo. O agente de nuvem usará essa orientação ao trabalhar nas issues atribuídas nas próximas lições.

1. No terminal do Codespace, crie e selecione uma branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Abra `.github/copilot-instructions.md` novamente.
3. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe que ela ainda não exige documentação no código.
4. Logo abaixo dos padrões existentes, adicione estas linhas Markdown:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Salve `copilot-instructions.md`.

> [!TIP]
> Arquivos de instruções podem ser criados na raiz do repositório (`.github/copilot-instructions.md`) para orientações globais ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o local adequado para padrões gerais da equipe, como a regra de comentários de documentação adicionada aqui.

## Fazer commit, enviar e mesclar a atualização das instruções

O agente de nuvem lê os arquivos de instruções da branch de destino da issue. Na próxima lição, quando você atribuir uma issue ao Copilot, ele criará uma branch a partir de `main`. Portanto, mescle as instruções em `main` para que o agente possa usá-las.

1. Prepare e faça commit da alteração:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request de `custom-instructions-cloud` para `main` e mescle-o. Assim, o agente de nuvem poderá ler as instruções ao trabalhar nas issues da próxima lição.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, ignore a criação da branch e faça commit diretamente em `main`. A etapa da branch demonstra o fluxo usado em um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções e adicionou um padrão para todo o repositório em `.github/copilot-instructions.md`. Você verá esse padrão nos pull requests que o agente de nuvem gerar nas próximas lições.

Em seguida, [atribua sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
