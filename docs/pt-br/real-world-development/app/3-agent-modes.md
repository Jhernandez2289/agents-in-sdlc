---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Explore os modos de agente: use Plan para definir uma abordagem, Autopilot para implementar a filtragem a partir de uma issue e Interactive para revisar e verificar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Começamos adicionando um pequeno recurso ao projeto. Alterações maiores exigem um processo mais robusto. O aplicativo GitHub Copilot foi desenvolvido para trabalhar com o fluxo existente de uma organização e ajudar a criar as soluções certas da maneira adequada. Nesta lição, você iniciará um processo de desenvolvimento orientado por agentes a partir de uma issue, verificará o código e o comportamento do recurso e o preparará para a integração ao projeto.

> [!NOTE]
> Você usará a mesma sessão durante todo o fluxo do recurso. Normalmente, as equipes usam sessões ou pull requests diferentes para tipos distintos de alteração. Neste workshop, o processo foi simplificado para manter o foco nos conceitos principais.

Nesta lição, você vai:

- iniciar uma sessão de agente a partir de uma issue do GitHub.
- definir os requisitos no modo **Plan**.
- implementar o recurso usando o modo **Autopilot**.
- revisar o código.
- validar manualmente o recurso em um canvas de navegador.

Ao continuar o trabalho no recurso, você atualizará as instruções do repositório, personalizará a skill `quality-checks`, adicionará validação com o Model Context Protocol (MCP), criará um agente de garantia de qualidade (QA) e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve o recurso, mas detalhes como a combinação de categorias precisam ser definidos antes de começar a programação. Você usará o modo Plan para decidir esses pontos e, em seguida, autorizará uma implementação com escopo definido no Autopilot.

## Contexto

A introdução de agentes de programação de IA no fluxo de desenvolvimento não muda os fundamentos — na verdade, torna-os ainda mais importantes. A maioria das pessoas desenvolvedoras segue um processo semelhante:

1. Abrir uma issue registrada com os detalhes do trabalho necessário.
2. Criar um plano do que será desenvolvido.
3. Implementar e revisar o código.
4. Executar testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois da revisão do código e da aprovação do processo de integração contínua, fazer o merge.

> [!NOTE]
> Os detalhes variam de acordo com a equipe e a organização, mas a maioria dos processos segue esse modelo.

Seguir esse processo ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo controle que o código escrito manualmente.

## Modos de sessão

O **modo de sessão** controla o nível de autonomia do agente. Você pode defini-lo no menu suspenso abaixo do campo de prompt e alterá-lo a qualquer momento:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria primeiro um plano. Você o revisa e aprova antes da execução.
- **Autopilot**: o agente trabalha de forma autônoma, escrevendo código, executando testes e iterando sem aguardar sua orientação.

Comece no modo Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Iniciar uma sessão a partir da issue

Antes de começar, confirme que o PR das avaliações por estrelas foi mesclado e que sua branch `main` local está atualizada.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys e, na navegação superior do repositório, selecione **Issues**. Abra **Allow users to filter games by category and publisher**. Essa issue já foi criada pelo modelo; não crie outra.
2. Selecione **New session** e escolha uma **new working tree** baseada na `main` atualizada.

   ![Visualização da issue no aplicativo GitHub Copilot com uma seta apontando para o botão New session](../../../_images/app-new-session-from-issue.png)

3. Confirme que a issue foi anexada à sessão e selecione **Plan** no seletor de modo.

> [!NOTE]
> Se o Node.js ou o npm não forem encontrados, peça ao agente que execute `node --version` e `npm --version`. Se os comandos funcionarem no terminal, mas não na sessão, verifique o `PATH` do aplicativo ou a configuração do gerenciador de versões e feche e reabra o aplicativo. Repita a operação e as verificações que falharam. Não reinstale o npm apenas porque o agente não o encontrou.

## Planejar o recurso de filtragem

O planejamento permite revisar a abordagem antes de o Copilot escrever código. Como você iniciou pela issue, o Copilot já tem a solicitação do recurso como contexto. Envie:

```plaintext
Build this feature.
```

Responda às perguntas do Copilot e compare o plano com os critérios de aceitação da issue. Confirme que ele abrange filtros por categoria e editora, controles acessíveis, alterações no acesso a dados e testes. Discuta comportamentos que não estejam claros, como combinar várias categorias ou o que acontece quando nenhum jogo corresponde.

O plano deve incluir lint, testes unitários, testes de ponta a ponta (E2E) e verificação de tipos com as ferramentas existentes do projeto. Mantenha o escopo na implementação e nos testes da filtragem; você criará o PR depois de concluir o fluxo de qualidade. Peça alterações no plano antes de aprová-lo e guarde o URL da issue e os esclarecimentos acordados para a validação posterior.

## Aprovar explicitamente o Autopilot

Quando estiver satisfeito com o plano, selecione **Approve and implement this plan** ou a opção equivalente na sua versão. Confirme que o indicador de modo ativo mostra **Autopilot**.

O Copilot começará a implementação, seguirá o plano, gerará código e executará testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente; revise o plano antes de aprová-lo. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas. Interrompa apenas os servidores que você iniciou.

## Revisar e verificar a implementação

Revise o código e execute o site antes de mesclar, como faria com qualquer outra alteração.

1. Abra **Changes** e inspecione a implementação da filtragem e os testes.
2. Compare o resultado com a issue e os esclarecimentos aprovados, incluindo combinações de categorias e editoras. Confirme que as alterações seguem as instruções existentes do repositório.
3. Examine os resultados de lint, testes unitários, testes E2E e verificação de tipos. Uma verificação ignorada não conta como aprovada.
4. Resolva as falhas e execute novamente as verificações afetadas antes de aceitar a implementação. A configuração E2E do Playwright cria e disponibiliza uma versão de pré-visualização e pode reutilizar um servidor local. Confirme que o servidor testado pertence a esta worktree, não a uma lição anterior.

## Explorar a nova funcionalidade

Inicie o aplicativo e abra o site em um canvas de navegador para testar o recurso.

1. Peça ao Copilot para iniciar o aplicativo e abrir a página no canvas de navegador:

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. Aguarde o início do aplicativo e a abertura de uma janela do navegador dentro do Copilot.
3. Selecione uma categoria e confirme que os resultados correspondem a ela. Limpe o filtro e selecione uma editora para verificar os resultados.
4. Combine filtros de categoria e editora e compare os resultados com a issue e o plano aprovado. Se houver suporte a várias categorias, verifique o comportamento combinado acordado.
5. Teste uma combinação sem jogos correspondentes e confira o estado vazio. Limpe ou redefina os filtros e confirme que a lista completa voltou.
6. Ao terminar, peça ao Copilot para parar o servidor de desenvolvimento iniciado nesta sessão:

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## Manter esta sessão para as próximas lições

Ainda não crie nem mescle o PR de filtragem. Mantenha esta sessão, worktree e branch para as Lições 4 a 8. Em seguida, aplique instruções personalizadas ao código de filtragem e adicione melhorias à skill e ao agente de QA antes de criar e mesclar o PR combinado na Lição 8. Depois de parar o servidor, deixe a sessão inativa e retome-a na próxima lição.

## Resumo e próximos passos

Você usou diferentes modos de agente para desenvolver e revisar um recurso. Nesta lição, você:

- iniciou uma sessão de agente a partir de uma issue do GitHub.
- definiu os requisitos no modo **Plan**.
- implementou o recurso usando o modo **Autopilot**.
- revisou o código.
- validou manualmente o recurso em um canvas de navegador.

Em seguida, você verá como gerar código que siga práticas documentadas ao [usar instruções personalizadas][next-lesson].

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
