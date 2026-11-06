---
title: "Lição 1 - Instalar o aplicativo GitHub Copilot"
description: "Instale o aplicativo GitHub Copilot, conecte o repositório criado a partir do modelo, conheça o espaço de trabalho e experimente um chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O **[aplicativo GitHub Copilot][about-copilot-app]** é um aplicativo para desktop voltado ao desenvolvimento orientado por agentes. Ele é baseado no GitHub Copilot CLI e se integra nativamente ao GitHub, para que repositórios, branches e pipelines de integração contínua (CI) funcionem sem configuração adicional. O aplicativo foi desenvolvido para fluxos de trabalho em que você orienta vários agentes em paralelo, cada um no próprio espaço de trabalho isolado, e automatiza tarefas repetitivas. Com o Node.js instalado e sua cópia do projeto pronta, instale o aplicativo e conecte o repositório.

Nesta lição, você vai:

- instalar o aplicativo GitHub Copilot e entrar na sua conta.
- adicionar seu projeto ao aplicativo pelo repositório do GitHub.
- conhecer o espaço de trabalho e o backlog criado pelo modelo.
- experimentar um chat rápido para saber mais sobre o aplicativo.

## Cenário

Sua equipe está adotando agentes de IA para trabalhar em um backlog crescente. O aplicativo Copilot oferece um único lugar para orientar esse trabalho: selecionar issues, executar agentes, revisar alterações e mesclar pull requests. Nesta lição, você instalará e conectará o aplicativo e aprenderá a iniciar uma conversa sobre o projeto.

> [!NOTE]
> É necessário ter um plano elegível do Copilot: Copilot Student ou qualquer plano pago (Pro, Pro+, Business ou Enterprise). Se você usa o Copilot Business ou o Copilot Enterprise, o administrador precisa manter habilitada a política **GitHub Copilot app**.

## Instalar e configurar o aplicativo GitHub Copilot

O aplicativo tem versões para Windows, macOS e Linux. Instale-o, autentique-se e adicione o repositório Tailspin Toys.

1. Em um navegador, abra a [página do aplicativo GitHub Copilot][download-app].
2. Baixe e instale a versão correspondente à sua plataforma, seguindo as instruções da página.
3. Abra o aplicativo.
4. Selecione **Sign in to GitHub** e siga as instruções para autenticar sua conta. Se você usa o GitHub Enterprise Cloud com residência de dados, selecione **Use GitHub Enterprise** e informe seu nome de host `*.ghe.com` quando solicitado. O GitHub Enterprise Server não é compatível.
5. Se o processo inicial oferecer a seleção de repositórios, selecione o repositório Tailspin Toys criado a partir do modelo. Você também pode ignorar essa etapa e adicionar o projeto depois.
6. Escolha um tema quando solicitado e conclua a configuração inicial.
7. Procure o repositório em **Sessions**, na barra lateral. Se ele já estiver listado, não é necessário adicioná-lo novamente.
8. Se o repositório ainda não estiver conectado, selecione **+** ao lado de **Sessions** para criar uma sessão e, em seguida, selecione **Add GitHub repository**. Use o proprietário e o nome do repositório anotados na lição de pré-requisitos.
9. Confirme se o repositório aparece na barra lateral. No layout usado aqui, pastas de repositório, como `tailspin-toys`, ficam agrupadas em **Sessions**, com as sessões individuais abaixo de cada repositório.

## Conhecer o espaço de trabalho

Com o projeto conectado, conheça as áreas da barra lateral:

- **New**: inicie uma nova sessão de chat com o Copilot.
- **Pull requests**: navegue e filtre pull requests, verifique o status da CI e revise PRs com a integração nativa do GitHub, sem sair do aplicativo.
- **Issues**: navegue e filtre issues e inicie uma sessão a partir de uma issue.
- **Automations**: tarefas de agente salvas que são executadas em uma agenda ou sob demanda. Elas ajudam a gerenciar listas de tarefas, manutenção regular do projeto e outras atividades repetitivas. O resumo apresenta esse recurso como próximo passo, não como outro exercício do workshop.
- **Customize**: adicione recursos ao aplicativo, como servidores MCP, plugins e skills. Você usará esta área para configurar o Playwright MCP.
- **Chats**: conversas leves para perguntas e brainstorming que não precisam de branch ou espaço de trabalho próprios. A seção aparece depois do primeiro chat. Se não estiver visível, selecione **+** ao lado de **Sessions** para iniciar um chat.
- **Sessions**: agrupa as pastas dos repositórios e as sessões de agente. Selecione o nome de um repositório, como `tailspin-toys`, para abrir a visualização do repositório ou selecione uma sessão para voltar à conversa. Cada sessão usa um espaço de trabalho isolado, permitindo executar várias ao mesmo tempo sem conflito. Você iniciará sua primeira sessão ao adicionar avaliações por estrelas.

Ao longo do workshop, você explorará o espaço de trabalho.

> [!TIP]
> Na dúvida, pergunte ao Copilot. Se não souber como fazer algo ou se algo é possível, ele poderá orientar você.

### Localizar o backlog criado pelo modelo

Explore o backlog criado quando você gerou sua cópia do modelo.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys, não uma sessão individual. Depois, na navegação superior do repositório, selecione **Issues**.
2. Localize estas issues pelo título, sem presumir seus números:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecione uma issue para ler seus detalhes. Cada issue também pode iniciar uma sessão de agente. Você começará pela issue de filtragem depois de fazer uma primeira alteração rápida.

> [!NOTE]
> As listas de **Issues** e **Pull requests** mostram automaticamente somente itens dos repositórios adicionados ao aplicativo Copilot. Para ver itens de outros repositórios, adicione-os ao aplicativo.

## Experimentar um chat rápido

Um chat rápido ajuda você a conhecer o aplicativo sem criar uma branch nem um worktree. Use-o para fazer uma pergunta ou trocar ideias sem iniciar uma sessão.

1. Na barra lateral, selecione **+** ao lado de **Chats** para abrir um chat. Se **Chats** ainda não estiver visível, selecione **+** ao lado de **Sessions** para iniciar seu primeiro chat; **Chats** aparecerá depois.
2. Pergunte como o aplicativo usa worktrees:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Leia a resposta na visualização da conversa. Cada sessão é executada em seu próprio worktree Git isolado, o que permite trabalhar com vários agentes em paralelo sem conflito. Você pode continuar a conversa ou iniciar outro chat a qualquer momento.

## Resumo e próximos passos

Parabéns! Você instalou o aplicativo GitHub Copilot, conectou o projeto e explorou o espaço de trabalho. Você aprendeu a:

- instalar o aplicativo e entrar no GitHub.
- adicionar um projeto pelo repositório do GitHub.
- conhecer o espaço de trabalho e localizar o backlog em **Issues**.
- usar um chat rápido para fazer uma pergunta breve e descartável.

Em seguida, você [iniciará sua primeira sessão de agente][next-lesson] para exibir uma avaliação por estrelas nos cartões dos jogos.

## Recursos

- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Introdução ao aplicativo GitHub Copilot][getting-started]
- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
