---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de inteligência artificial (IA) especializados em tarefas ou domínios específicos do fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você fornece instruções focadas, práticas recomendadas, padrões de código e conhecimento de domínio para orientar o Copilot. As equipes podem codificar sua experiência em agentes reutilizáveis — por exemplo, um agente de acessibilidade que aplica as Diretrizes de Acessibilidade para Conteúdo Web (WCAG), um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm algumas semelhanças: ambos são definidos principalmente em arquivos Markdown e orientam uma IA a executar tarefas. Uma distinção útil é que o **agente personalizado** é quem executa o trabalho, enquanto as **skills** são ferramentas.

Agentes personalizados têm sua própria janela de contexto e podem orquestrar skills — e até outros agentes — durante o trabalho. Neste exercício, o agente de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade. Para isso, ele poderia chamar uma skill de fluxo de pull request ou outra que execute e gerencie testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente personalizado. Como em outros usos de IA, teste e itere para encontrar o que funciona no seu ambiente e cenário.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Neste exercício, você vai:

- entender como agentes personalizados são definidos.
- atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys quer tornar sua plataforma de financiamento coletivo acessível a todas as pessoas, independentemente de suas capacidades ou preferências visuais. Comentários recentes indicam que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre o texto e o fundo. Para resolver essa questão, a equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é importante, você usará um agente personalizado para implementar rapidamente essa funcionalidade.

## Revisar o agente personalizado de acessibilidade

O agente personalizado de acessibilidade já foi criado. Revise seu conteúdo para entender como ele orientará o Copilot.

Volte ao Codespace e examine o arquivo do agente personalizado:

1. Abra `.github/agents/accessibility.md`.
2. Observe os campos `name` e `description` no frontmatter YAML.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Examine as seções seguintes, que apresentam:
   - responsabilidades principais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - armadilhas e erros comuns.

## Criar e atribuir uma issue

A página de agentes é o local central para trabalhar com agentes no ambiente. Você pode atribuir tarefas ao agente de nuvem do Copilot, monitorar o trabalho e redirecionar uma sessão ou fornecer orientações adicionais. Atribua ao Copilot a tarefa de criar o modo de alto contraste.

1. Acesse seu repositório.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de criação de issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Add high contrast mode to website`.
6. No campo **Description**, digite:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.
9. Na lista de agentes personalizados, selecione **Accessibility agent**.

   ![Caixa de atribuição do agente de nuvem com o agente personalizado Accessibility selecionado](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. O Copilot começará a trabalhar em segundo plano.

## Resumo e próximos passos

Neste exercício, você explorou [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados em tarefas e domínios específicos. Com eles, você pode transformar a experiência e os padrões da equipe em agentes reutilizáveis que orientam o trabalho do Copilot.

Você explorou estes conceitos:

- como agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot implementa o modo de alto contraste, você pode [monitorar e direcionar a sessão do agente][next-lesson] na página de agentes.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar sua organização para usar agentes personalizados][org-custom-agents]
- [Preparar sua empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
