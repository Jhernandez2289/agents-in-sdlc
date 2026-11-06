---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Ao longo deste workshop, você trabalhou com o GitHub Copilot em tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse o recurso de jogos relacionados para a equipe de design e implementasse recursos de acessibilidade, incluindo alternâncias para os modos de alto contraste e claro. Agora, explore as alterações e, se necessário, dê feedback ao Copilot para aprimorá-las.

### Cenário

Os fundamentos do design de software e do DevOps não mudam com a inclusão da inteligência artificial (IA) generativa. Ainda é necessário revisar o código gerado e seguir o processo de DevOps habitual. Revise as sugestões do GitHub Copilot para a documentação, os jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, há restrições de segurança para proteger o ambiente:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O agente de nuvem é executado no GitHub Actions, em um ambiente separado e efêmero.
- Uma pessoa precisa aprovar a execução de qualquer fluxo de trabalho do GitHub Actions.
- [O acesso a recursos externos é limitado por padrão][agent-firewall], inclusive a servidores do Model Context Protocol (MCP).

## Revisar a documentação gerada

Comece pelo primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot: o PR que adiciona documentação ao código. Use a interface padrão de PR do GitHub.com.

> [!NOTE]
> Ao abrir o PR, você talvez veja um aviso de que o Copilot está bloqueado por um firewall. Isso é **esperado**: por padrão, o acesso do Copilot a recursos externos, incluindo servidores MCP, é limitado. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de PRs.
3. Abra o PR com um título semelhante a **Add missing documentation**.
4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui comentários TSDoc e outra documentação. As alterações exatas variam. Procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário, orientados pelos arquivos de instruções personalizados vistos no início do Exercício 2.
6. Depois da revisão, volte à guia **Conversation** e role a página.
7. Verifique se há um indicador de que alguns fluxos de trabalho aguardam aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho devem ser adicionados à seção de verificações do PR. Aguarde as verificações do projeto para o aplicativo Astro serem aprovadas; isso pode levar alguns minutos.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

Você também pode mencionar o Copilot em comentários do PR ou em comentários de linha no código. Ele usará esses comentários para iniciar outra sessão e atender às solicitações. Como os resultados são não determinísticos, não há um texto único a recomendar. Você pode pedir, por exemplo, para:

- adicionar no início de cada arquivo de código um cabeçalho que descreva sua finalidade.
- adicionar comentários TSDoc aos arquivos TypeScript e Astro.
- criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração à documentação gerada e mencione **@copilot**, como faria com qualquer pessoa da equipe. Use uma das ideias acima ou outra solicitação de documentação.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao PR.

   ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, confirme que um novo commit foi adicionado ao PR.
5. Selecione **Files changed** e revise as alterações.

Continue iterando até ficar satisfeito. Depois, você pode marcar o PR como pronto para revisão e mesclá-lo à branch principal.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

Agora, revise o PR criado pelo Copilot para a issue que solicita jogos relacionados na página de detalhes de cada jogo.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione **Files changed** e revise o código gerado.
5. Depois da revisão, volte à guia **Conversation** e role a página.
6. Verifique se há um indicador de que alguns fluxos de trabalho aguardam aprovação.
7. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

8. Na seção de verificações do PR, confirme que os fluxos de trabalho foram enfileirados e aguarde a aprovação das verificações do aplicativo Astro. Isso pode levar alguns minutos.
9. **Opcional:** para testar manualmente o recurso, mude para a branch no Codespace. Abra o terminal e execute estes comandos, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, como `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot implementou o recurso de jogos relacionados. Você pode continuar iterando com o agente de nuvem para pedir alterações, como ajustar o número de jogos exibidos ou adicionar cabeçalhos de comentário e comentários TSDoc. Lembre-se de que o trabalho foi atribuído **antes** da atualização das instruções personalizadas. Para solicitar mudanças, adicione um comentário na guia **Conversation**; o Copilot iniciará outra sessão.

## Revisar os recursos de acessibilidade

Por fim, revise os recursos de acessibilidade implementados pelo agente personalizado. O PR deve incluir o modo de alto contraste solicitado no Exercício 3 e o modo claro solicitado no painel central do Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Add high contrast mode to website**.
4. Selecione **Files changed** e revise o código gerado.
5. Examine a implementação, principalmente:
   - os controles de alternância entre modos.
   - o uso do armazenamento local para manter as preferências.
   - as alterações de folhas de estilos em cascata (CSS) ou estilo dos modos de alto contraste e claro.
   - os atributos de acessibilidade, como rótulos Accessible Rich Internet Applications (ARIA) e navegação pelo teclado.
   - o código JavaScript/TypeScript que controla a troca de modos.
6. Depois da revisão, volte à guia **Conversation** e role a página.
7. Verifique se há um indicador de que alguns fluxos de trabalho aguardam aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Na seção de verificações do PR, confirme que os fluxos de trabalho foram enfileirados e aguarde a aprovação das verificações do aplicativo Astro. Isso pode levar alguns minutos.
10. **Opcional:** para testar manualmente os recursos de acessibilidade, mude para a branch no Codespace. Abra o terminal e execute os comandos abaixo, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Inicie o aplicativo e teste os controles dos modos de alto contraste e claro no navegador. Confirme se funcionam e se as preferências persistem após recarregar a página.

O agente personalizado de acessibilidade orientou o Copilot a implementar esses recursos seguindo práticas recomendadas. Se encontrar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário e solicite alterações, como fez com os outros PRs.

## Exercício opcional — continuar delegando

O agente de nuvem é mais útil quando recebe itens reais do backlog para que você possa se dedicar a outras tarefas. Para desenvolver esse hábito, crie algumas issues no repositório e atribua-as ao Copilot. Por exemplo:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na lista de jogos.
- adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Nestas lições, você:

- **inspecionou os arquivos de instruções personalizados do repositório** para ver seus efeitos no trabalho do agente de nuvem.
- **atribuiu issues ao agente de nuvem do Copilot** e acompanhou a preparação do ambiente, o planejamento e a execução assíncrona.
- **criou e usou um agente personalizado** de acessibilidade para adicionar alternâncias dos modos de alto contraste e claro.
- **usou a página de agentes do Copilot como painel central** para monitorar e direcionar a sessão de acessibilidade em andamento.
- **revisou e refinou os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando fluxos de trabalho.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Para continuar explorando, os outros percursos complementam o que você praticou:

- **[Percurso do VS Code](../../vscode/)** — explore o modo agente do Copilot Chat e a integração com MCP diretamente no IDE.
- **[Percurso do CLI](../../cli/)** — entregue duas alterações revisadas no terminal com os modos Plan e Autopilot, skills de agente, um agente de QA personalizado, Playwright MCP, Agent Merge e uma ponte `/delegate` opcional para o agente de nuvem.

No seu repositório, experimente também:

- atribuir ao agente de nuvem uma issue de refatoração ou cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
