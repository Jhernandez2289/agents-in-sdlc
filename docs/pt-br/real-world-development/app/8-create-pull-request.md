---
title: "Lição 8 - Criar e mesclar o PR do recurso"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

A implementação da filtragem, as atualizações das instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora, revise tudo em conjunto e abra um pull request. Você mesclou o pull request (PR) de avaliações por estrelas manualmente; desta vez, deixe o **Agent Merge** gerenciar o processo.

> [!NOTE]
> Em geral, dividiríamos o recurso, as atualizações das instruções, a atualização da skill e o agente de QA em alguns PRs. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma sessão e branch, e incluiremos esse trabalho em um único PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionar o PR completo do recurso e as evidências de verificação.
- autorizar o Agent Merge somente depois da revisão e confirmar a mesclagem do PR.

## Cenário

Durante o trabalho de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização de mesclagem sob o controle da pessoa desenvolvedora.

## Conhecer o Agent Merge

O **Agent Merge** automatiza o trabalho restante para integrar um pull request no aplicativo GitHub Copilot. Quando você o habilita, a sessão do aplicativo lê o PR e resolve o que estiver bloqueando a integração — corrige verificações de integração contínua (CI) com falha, responde a comentários de revisão e faz rebase quando necessário — e faz a mesclagem assim que o GitHub permite. O Agent Merge funciona em segundo plano, continua após reinicializações do aplicativo e é desativado quando o PR é mesclado.

Até aqui, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade, mas a edição de código e a mesclagem ainda exigem sua autorização explícita. Revise as ações permitidas e o trabalho antes de conceder permissão para mesclar.

## Usar o Agent Merge para gerenciar o PR

Com todo o código criado e revisado, permita que o Agent Merge gerencie o processo do PR.

1. Use o seletor de agentes para selecionar **Default agent**.
2. Selecione o menu suspenso ao lado de **Create PR**.
3. Selecione **Agent Merge**. O botão mudará para **Agent Merge**.
4. Selecione o botão **Agent Merge** para iniciar o processo. Selecionar a opção no menu suspenso, por si só, não inicia a operação.

O Agent Merge iniciará o processo e poderá:

- criar o pull request com título e descrição.
- mencionar a issue relacionada no corpo da descrição se a sessão tiver começado a partir de uma issue.
- fazer rebase ou lidar com conflitos de mesclagem com a branch de destino.
- monitorar a integração contínua para confirmar que todas as verificações passaram.
- monitorar comentários de outras pessoas desenvolvedoras e da revisão de código do Copilot e atualizar o PR para responder a eles.
- fazer a mesclagem automática do PR depois que todas as etapas forem concluídas, se você autorizar.

Peça ao Agent Merge que também faça a mesclagem depois que todas as verificações forem aprovadas.

5. Revise as diferenças do PR e as evidências de verificação antes de conceder permissão para mesclar.
6. Selecione o menu suspenso ao lado de **Agent Merge**.
7. Confirme se há uma marca ao lado de **Merge pull request**.
8. Peça explicitamente ao agente que faça a mesclagem:

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. Acompanhe o processo e resolva os bloqueios informados. Antes de continuar, confirme que o status do PR é **Merged**. A permissão habilitada ou um agente em execução não confirma que a mesclagem foi concluída.

> [!IMPORTANT]
> O Agent Merge não ignora as proteções do repositório nem as permissões ausentes. Resolva esses bloqueios antes de continuar.

## Resumo e próximos passos

Você automatizou várias etapas do processo de desenvolvimento, incluindo geração de código, testes, validação e o fluxo do pull request. Nesta lição, você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionou o PR completo do recurso e as evidências de verificação.
- autorizou o Agent Merge somente depois da revisão e confirmou a mesclagem do PR.

Em seguida, você [usará um canvas existente e criará um canvas de triagem][next-lesson] para explorar outras maneiras de inspecionar, planejar e visualizar o trabalho com o agente.

## Recursos

- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
