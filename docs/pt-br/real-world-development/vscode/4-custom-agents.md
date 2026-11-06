---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados em tarefas ou domínios específicos do fluxo de desenvolvimento. Ao defini-los em arquivos Markdown na pasta `.github/agents` do repositório, você fornece instruções focadas, práticas recomendadas, padrões de código e conhecimento de domínio. As equipes podem codificar sua experiência em agentes reutilizáveis, como um agente de acessibilidade que aplica as diretrizes [WCAG][wcag], um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm algumas semelhanças: ambos são definidos principalmente em arquivos Markdown e orientam uma IA a executar tarefas. Uma distinção útil é que o **agente personalizado** é quem executa o trabalho, enquanto as **skills** são ferramentas.

Agentes personalizados têm sua própria janela de contexto e podem orquestrar skills — e até outros agentes — durante o trabalho. Neste exercício, o agente de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade. Para isso, ele poderia chamar uma skill de fluxo de pull request ou outra que execute e gerencie testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente personalizado. Como em outros usos de IA, teste e itere para encontrar o que funciona no seu ambiente e cenário.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer tornar sua plataforma de financiamento coletivo acessível a todas as pessoas, independentemente de suas capacidades ou preferências visuais. Comentários recentes indicam que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre o texto e o fundo. A equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar o modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado. Primeiro, crie uma branch nova a partir de `main` para manter o trabalho de acessibilidade separado do PR de filtragem do Exercício 3.

1. Volte ao Codespace, abra um terminal e mude para `main` atualizado:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. Abra `.github/agents/accessibility.md`.
3. Observe os campos `name` e `description` no frontmatter YAML.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

4. Examine as seções seguintes, que apresentam:
   - responsabilidades principais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - armadilhas e erros comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code mostra os agentes personalizados definidos em `.github/agents` na lista de agentes na parte inferior da exibição Copilot Chat. Selecione um agente para aplicar as instruções e as ferramentas correspondentes à conversa.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa informa o que selecionar.

1. Na lista de agentes da exibição Chat, selecione **Agent** se ainda não estiver selecionado.

   ![Seletor de agentes na exibição Chat](../../../_images/shared-chat-mode-selector.png)

2. Abra a lista de agentes na parte inferior da exibição Chat. Ela mostra o agente ativo, que por padrão é **default**.
3. Selecione **Accessibility agent**.
4. Envie este prompt ao agente:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. O Copilot Chat explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e diferenças para revisão.
6. Observe o indicador **Files changed**, que é atualizado à medida que o agente modifica arquivos.

> [!NOTE]
> O processo pode levar alguns minutos. O Copilot fará alterações reais no repositório, como editar componentes Astro, CSS e testes relacionados.

Você revisará e orientará o trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Neste exercício, você explorou [agentes personalizados][custom-agents], assistentes de IA especializados em tarefas e domínios específicos. Com eles, é possível transformar a experiência e os padrões da equipe em agentes reutilizáveis que orientam o trabalho do Copilot.

Você explorou estes conceitos:

- como agentes personalizados são definidos.
- como usar um agente personalizado no modo agente do Copilot Chat.

Em seguida, [monitore e direcione o trabalho do agente][next-lesson], revise as alterações enquanto são feitas e solicite um controle para o modo claro na mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar seu recurso com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
