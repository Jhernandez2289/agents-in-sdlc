---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É comum que organizações enfrentem dívida técnica: problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos que permanecem no backlog por falta de tempo. O agente de nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Ao concluir o trabalho, ele cria um pull request de rascunho para revisão por uma pessoa desenvolvedora. Assim, você pode delegar tarefas repetitivas e se dedicar a assuntos mais amplos.

Neste exercício, você vai:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- entender a importância de issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys quer lidar com parte da dívida técnica. A documentação deixada pelas pessoas contratadas para criar a primeira versão do site está em um estado inadequado — na verdade, ela está totalmente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc às funções exportadas do aplicativo.

Além disso, a equipe de design quer facilitar a descoberta de jogos. Cada página de detalhes deve mostrar jogos relacionados — outros títulos da mesma categoria — para que as pessoas possam continuar explorando. A equipe ainda não precisa de uma implementação refinada; quer algo que possa usar em testes de aceitação da experiência do usuário (UX). Esse trabalho está bloqueado por outras issues de prioridade mais alta.

Esses são exemplos de tarefas que podem perder prioridade e que são adequadas para o agente de nuvem. Ele pode trabalhar nelas de forma assíncrona enquanto você se dedica a outras atividades; depois, você volta para revisar o trabalho.

## Conhecer o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot][copilot-agents] executa tarefas em segundo plano de forma semelhante a uma pessoa desenvolvedora. Por exemplo, você pode [atribuir uma issue do GitHub ao Copilot][assign-issue]. Depois da atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, prepara o ambiente e começa a trabalhar. Você pode examinar a sessão enquanto o trabalho está em andamento ou depois de concluído. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções com escopo claro

O GitHub Copilot não elimina a necessidade de pensar. Mesmo operações que parecem simples podem ser complexas. Por isso, [defina com cuidado o escopo das tarefas para o agente de nuvem][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com inteligência artificial (IA): trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram instruções personalizadas e como elas orientam o Copilot. Se você já concluiu o exercício prático de instruções, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para examiná-lo. Antes de atribuir trabalho ao agente de nuvem, leia os arquivos de instruções já incluídos no repositório, sem alterá-los, para depois identificar seus efeitos.

Abra estes arquivos na interface Web do GitHub ou em um Codespace já iniciado:

- `.github/copilot-instructions.md`: examine a seção **Code standards**, principalmente as convenções de TypeScript e os comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica as instruções a arquivos `**/*.test.ts`.

Na próxima seção, quando você atribuir ao agente a issue `Code lacks documentation`, observe no pull request os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções; voltaremos a esse ponto ao revisar os PRs.

## Configurar o ambiente de desenvolvimento do agente de nuvem

A criação de código normalmente exige um ambiente específico e scripts de configuração para garantir que tudo esteja pronto. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de forma semelhante a uma pessoa engenheira de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo com um [fluxo de configuração especial][setup-workflow] no arquivo `.github/workflows/copilot-setup-steps.yml`, executado antes do início do trabalho. Isso disponibiliza ferramentas e dependências necessárias. O fluxo já está configurado para o workshop: instala o Node.js e as dependências do projeto, os navegadores do Playwright para testes de ponta a ponta e configura o banco de dados SQLite migrado e preenchido do aplicativo Astro.

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para estas etapas. O Copilot recebe um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório durante a configuração, por exemplo, para instalar dependências, você precisa da permissão `contents: read`. Se não clonar o repositório nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend e do aplicativo: Node.js (o aplicativo agora é Astro + Drizzle/libSQL).
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e preenche o banco SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

Esse arquivo se parece com outros fluxos de trabalho do GitHub, mas tem alguns pontos importantes:

- Há um único trabalho chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o fluxo manualmente na guia **Actions** do repositório. Isso é útil para testar a configuração sem esperar que o Copilot a execute.

## Adicionar documentação

Muitos projetos têm informações desatualizadas ou nenhuma documentação, o que pode reduzir a produtividade e dificultar a manutenção ou a integração de novas pessoas na equipe. O Copilot pode ajudar a criar essa documentação, então esta é uma issue adequada para atribuir ao agente de nuvem. Em um exercício futuro, você voltará para revisar o resultado.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de criação de issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Code lacks documentation`.
6. No campo **Description**, digite:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.

   ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-criado, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer, aguarde um ou dois minutos e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está preparando o ambiente (executando o fluxo descrito anteriormente), analisando o projeto e definindo uma abordagem.

13. Revise a lista e as tarefas planejadas.
14. Role a linha do tempo do PR para encontrar uma atualização indicando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

   ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a página para ver o indicador atualizado.

16. Percorra a sessão ao vivo e observe como o Copilot resolve o problema: explora o código, entende o estado do projeto, planeja a abordagem e cria código.

O trabalho pode levar vários minutos. Um dos objetivos do agente de nuvem é executar tarefas de forma assíncrona para que você possa se dedicar a outras atividades. Aproveite esse recurso atribuindo outra tarefa ao agente e, em seguida, trabalhe em uma funcionalidade do aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes

Uma vantagem do agente de nuvem é dividir o trabalho: você pode se concentrar em uma tarefa enquanto o agente cuida de outra. Atribua ao Copilot o pedido da equipe de design para mostrar jogos relacionados.

1. Volte ao repositório no github.com.
2. Selecione **Issues** e, em seguida, **New issue**.
3. Selecione **Blank issue**.
4. No campo **Title**, digite `Show related games on the game details page`.
5. No campo **Description**, digite:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

6. Selecione **Create** para criar a issue.
7. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.

   ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

8. Selecione **Assign**.

Pouco depois, um símbolo 👀 deverá aparecer no primeiro comentário da issue, indicando que o Copilot começou o trabalho.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot está trabalhando no segundo pedido. Como o agente de nuvem funciona de forma semelhante a uma pessoa engenheira de software, não é necessário monitorá-lo continuamente; revise o resultado quando estiver pronto. Enquanto isso, conheça os agentes personalizados.

## Resumo e próximos passos

Neste exercício, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para execução assíncrona e trabalhar em dívida técnica, criar recursos ou migrar código entre frameworks.

Você explorou estes conceitos:

- personalização do ambiente de geração de código.
- execução segura de operações.
- importância de issues com escopo claro.
- atribuição de issues ao Copilot.

Enquanto o agente trabalha, você pode criar e usar agentes personalizados. O [agente de nuvem também pode usar servidores MCP][cloud-agent-mcp] e instruções personalizadas, exploradas em módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
