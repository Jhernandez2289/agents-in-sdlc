---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo agente, o GitHub Copilot trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho ocorre na sua máquina em tempo real, o Copilot Chat no VS Code mostra cada chamada de ferramenta e edição de arquivo. Você pode revisar as diferenças, aceitar ou rejeitar alterações e orientar a conversa com prompts complementares, sem sair do editor.

Neste exercício, você vai:

- monitorar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições de arquivos propostas.
- orientar o agente durante a sessão para adicionar um controle do modo claro junto com o modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Durante o trabalho, a equipe de design solicita também um controle do modo claro para pessoas que consideram o tema padrão escuro demais. Em vez de iniciar uma conversa nova, direcione a sessão existente para incluir esse recurso.

## Monitorar o progresso do Copilot

A exibição Copilot Chat mostra as etapas executadas pelo agente: arquivos explorados, comandos executados e alterações propostas. Cada edição aparece na conversa com diferenças e os controles **Keep** e **Undo**.

1. Volte à exibição Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra a conversa e observe as chamadas de ferramentas, leituras e edições de arquivos e comandos do terminal solicitados.
3. Quando o agente propuser uma edição, revise as diferenças:
   - Selecione **Keep** para aceitar a alteração.
   - Selecione **Undo** para desfazê-la e permitir que o agente tente novamente.
4. Abra o indicador **Files changed** na parte superior da conversa para revisar todos os arquivos alterados durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão, por exemplo, para executar um comando do terminal, o agente aguardará sua resposta e mostrará **Allow**. Aprove somente comandos com os quais você esteja de acordo.

5. Aguarde o agente concluir o modo de alto contraste. Ele apresentará uma mensagem final que resume as alterações.

## Orientar o agente para adicionar o modo claro

Com o modo de alto contraste implementado, estenda a conversa para adicionar um controle do modo claro. Como você continuará na mesma sessão, o agente manterá o contexto dos componentes, da abordagem de estilo e do padrão de armazenamento local que usou.

1. Na caixa de entrada do Copilot Chat, envie este prompt:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. O agente continuará o trabalho, explorando as alterações existentes e propondo a implementação do modo claro.
3. Revise cada nova alteração e use **Keep** ou **Undo**, como fez com o modo de alto contraste.
4. Se o agente seguir uma direção indesejada, como refatorar os controles de uma maneira que você não quer, interrompa-o com o botão **Stop** ao lado da caixa de entrada e envie uma orientação complementar.

> [!TIP]
> Direcionar uma sessão em andamento é mais rápido do que recomeçar. Use prompts complementares para pedir ajustes, como simplificar o CSS usando variáveis, corrigir a abordagem, como integrar o modo claro ao painel de configurações existente, ou ampliar o escopo, como adicionar atalhos de teclado aos dois controles.

## Revisar as alterações

Antes de fazer commit, revise o conjunto completo de arquivos alterados pelo agente.

1. Abra a exibição **Source Control** no VS Code.
2. Revise a lista de arquivos alterados. Deve haver atualizações em componentes Astro, estilos e testes relacionados.
3. Abra alguns arquivos alterados e examine as diferenças. Confirme se as práticas de acessibilidade do agente estão presentes: atributos ARIA, navegação pelo teclado, HTML semântico e persistência com armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo agente do Copilot Chat para monitorar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou alterações e enviou prompts complementares para ampliar o trabalho sem perder o contexto.

Neste exercício, você:

- monitorou o trabalho do agente em tempo real.
- aceitou e rejeitou edições individuais.
- direcionou uma sessão em andamento com prompts complementares.

Na [próxima lição][next-lesson], publique seu trabalho de acessibilidade em um pull request e revise as alterações produzidas pelo agente personalizado local durante o workshop.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
