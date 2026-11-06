---
slug: pt-br/real-world-development/cloud
title: "Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

O **[agente de nuvem do GitHub Copilot][cloud-agent]** permite que o GitHub Copilot trabalhe de forma assíncrona na nuvem. Você atribui tarefas no GitHub e o agente de nuvem as executa em segundo plano — explora o repositório, faz alterações e abre um pull request — enquanto você pode se dedicar a outras atividades.

Ao longo destes exercícios, você adicionará instruções personalizadas para orientar o agente de nuvem e, depois, atribuirá uma issue do GitHub para que ele implemente o trabalho. Você revisará e usará agentes personalizados para orientar a abordagem, monitorará e direcionará sessões no painel de agentes e, por fim, revisará os pull requests e iterará sobre os resultados.

## Exercícios

| Exercício | Tópico | Descrição |
|----------|-------|-----------|
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e Codespace |
| [1. Instruções personalizadas][ex1] | Contexto | Adicione instruções personalizadas para o agente de nuvem |
| [2. Agente de nuvem][ex2] | Agente assíncrono | Atribua issues ao agente de nuvem do Copilot |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revise e use agentes personalizados |
| [4. Gerenciar agentes][ex4] | Monitoramento | Monitore e direcione sessões de agentes |
| [5. Iterar][ex5] | Revisão | Revise pull requests, itere sobre o trabalho do Copilot e escolha os próximos passos |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] O agente de nuvem do Copilot habilitado para sua conta ou organização

> [!TIP]
> Não tem um plano pago? Estudantes com status verificado podem obter o GitHub Copilot gratuitamente pelo [GitHub Education][student-plan]. O plano **Copilot Student** inclui os recursos de agente, Model Context Protocol (MCP), revisão de código e Copilot CLI usados neste workshop.

[student-plan]: https://github.com/education/students

> [!NOTE]
> O agente de nuvem está disponível nos planos **Copilot Student, Pro, Pro+, Business e Enterprise** — todos, exceto o Copilot Free. No Copilot Business ou Enterprise, um administrador precisa habilitá-lo para a organização.

> [!NOTE]
> Este percurso não aborda o Model Context Protocol (MCP). Para explorar servidores MCP com o Copilot, consulte o [percurso do CLI][cli-workshop] ou o [percurso do VS Code][vscode-workshop].

## Começar

**[Comece pelos pré-requisitos →][ex0]**

[cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cli-workshop]: ../cli/
[vscode-workshop]: ../vscode/
[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
