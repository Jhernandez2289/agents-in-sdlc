---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente trabalhe de forma autônoma e assíncrona, é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel central, você vê as tarefas com pull requests abertos, explora as operações realizadas e direciona uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para monitorar tarefas do agente de nuvem.
- direcionar uma sessão em andamento para solicitar outra funcionalidade.

## Cenário

Depois de atribuir ao agente a implementação do modo de alto contraste, a equipe percebeu que também seria útil adicionar um modo claro. Como o agente já está atualizando o estilo do site e adicionando controles de alternância, você direcionará o trabalho para incluir o modo claro junto com o modo de alto contraste.

## Revisar as tarefas do agente de nuvem do Copilot

Confira o status das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe as tarefas listadas no painel principal e no painel à esquerda. Entre elas, devem estar:
   - atualização da documentação da base de código.
   - geração de APIs para modificar jogos.
   - adição do modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações feitas pelo Copilot. Elas podem incluir:
   - obtenção do código do repositório.
   - preparação do ambiente de trabalho do Copilot.
   - configuração de servidores do Model Context Protocol (MCP).
   - outras etapas para concluir a tarefa.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho e a abordagem adotada pelo Copilot.

4. Observe também o painel do pull request (PR), à direita, que permite monitorar o PR e os arquivos alterados.

## Direcionar o agente de nuvem

Agora que você conhece as tarefas ativas, peça ao Copilot que inclua o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente à implementação do modo de alto contraste. O título exato varia conforme o nome usado pelo Copilot e o estado atual do trabalho.

   ![Sessão de acessibilidade no painel central de agentes](../../../_images/ex6-accessibility-session.png)

2. Aguarde alguns minutos até que a sessão indique que a configuração foi concluída e o trabalho começou. Você verá mensagens semelhantes às apresentadas na interface.
3. Na caixa de diálogo **Steer active session while Copilot is working**, envie este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarefa do agente de nuvem na página de agentes com a caixa de diálogo de direcionamento da sessão destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot reconhece a solicitação e a incorpora ao trabalho.

## Deixar o Copilot trabalhar

O Copilot atualizará o trabalho e incorporará a nova solicitação depois de concluir a etapa em andamento no momento em que você enviou a mensagem.

Isso pode levar alguns minutos. Aproveite para refletir sobre o que você aprendeu e explorou até agora.

## Resumo e próximos passos

Neste exercício, você explorou a página de agentes do Copilot, o painel central para monitorar e orientar tarefas do agente de nuvem do GitHub Copilot. Você pode acompanhar tarefas ativas e concluídas, revisar o trabalho e redirecionar uma sessão em andamento para ajustar o escopo ou dar outras orientações.

Você explorou estes conceitos:

- uso do painel central e da página de agentes para monitorar tarefas do agente de nuvem.
- redirecionamento de uma sessão em andamento para solicitar outra funcionalidade.

Enquanto o Copilot conclui os recursos de acessibilidade, prepare-se para [iterar sobre os pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
