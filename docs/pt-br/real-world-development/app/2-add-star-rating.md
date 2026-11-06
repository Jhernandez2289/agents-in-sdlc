---
title: "Lição 2 - Adicionar avaliações por estrelas: uma vitória rápida"
description: "Inicie sua primeira sessão de agente no aplicativo GitHub Copilot, faça uma pequena alteração nos cartões dos jogos e mescle-a como seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Agora que você explorou o espaço de trabalho e usou um chat rápido, inicie uma **sessão de agente** para fazer sua primeira alteração no projeto. Ela será pequena: os dados dos jogos já têm uma avaliação por estrelas, mas os cartões da página inicial ainda não a mostram. Peça ao agente que a exiba, revise a alteração e mescle-a como seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma sessão de agente e conhecer sua estrutura.
- pedir ao agente que faça uma pequena alteração focada no projeto.
- revisar a alteração na visualização de diferenças do espaço de trabalho.
- executar o aplicativo localmente e confirmar a alteração no navegador.
- abrir e mesclar seu primeiro pull request.

## Cenário

Cada jogo no Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes. Os cartões da página inicial, porém, mostram apenas o título, a categoria, a editora e a descrição. Como aquecimento, peça ao agente para exibir a avaliação existente em cada cartão: uma alteração pequena e independente, ideal para sua primeira sessão.

## Estrutura de uma sessão

Uma **sessão** é uma conversa com um agente executada em um espaço de trabalho isolado. Cada sessão tem seu próprio **git worktree e branch**, o que permite trabalhar com várias sessões ao mesmo tempo — uma para adicionar um recurso e outra para corrigir um bug — sem que as alterações entrem em conflito. As sessões aparecem na barra lateral, agrupadas por repositório. Selecione uma sessão para alternar para ela.

Em uma sessão, você verá a **conversa** com o agente, a **atividade das ferramentas** enquanto ele explora e edita arquivos e a lista de **arquivos alterados** com suas diferenças.

## Iniciar uma sessão e solicitar a alteração

Inicie uma sessão para explorar o projeto e implementar o recurso. Durante a [configuração do aplicativo][prior-lesson], você adicionou o projeto pelo repositório do GitHub. Agora, crie uma sessão para esse repositório e solicite a alteração.

1. Volte ao aplicativo GitHub Copilot ou abra-o.
2. Na parte superior da barra lateral, selecione **+** para iniciar uma sessão.
3. Selecione `tailspin-toys` como repositório.
4. Escolha uma **new working tree** e o modo **Interactive** abaixo da caixa de prompt. Use este prompt:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

5. Pressione <kbd>Enter</kbd> para enviar o prompt.

O aplicativo Copilot começa criando um worktree, uma cópia isolada do projeto. Em seguida, explora o projeto, localiza os arquivos necessários e cria o código do recurso.

## Revisar as diferenças

Revise todas as alterações geradas por IA antes de mesclá-las, mesmo quando forem pequenas.

1. No canto superior direito do aplicativo, selecione **Toggle review panel**. No painel que abrir, selecione **Changes** para ver os arquivos alterados e suas diferenças.

   ![Barra de ferramentas superior do aplicativo GitHub Copilot com uma seta apontando para o botão Toggle review panel, à direita de Create PR](../../../_images/app-2-review-panel.png)

2. Confirme se `GameCard.astro`, o arquivo principal que exibe os detalhes do jogo, contém código semelhante ao exemplo a seguir: um bloco que mostra a avaliação quando disponível e usa `No rating yet` quando `starRating` é `null`.

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, não determinístico. O código exato pode variar, mas deve ser relativamente semelhante.

## Verificar as alterações

Revise os resultados das verificações automatizadas antes de abrir o navegador. Confirme que os testes cobrem um valor numérico de `starRating` e a alternativa para `null`. Um pré-requisito ausente ou uma verificação ignorada não conta como aprovação; revise qualquer solicitação de instalação antes de autorizá-la.

Peça ao Copilot para iniciar o site e abri-lo em um canvas de navegador para que você examine a interface do usuário (UI) atualizada.

> [!TIP]
> Um canvas é um widget interativo disponível dentro do aplicativo Copilot. Mais adiante, você explorará canvases personalizados e criará um; por enquanto, use o canvas de navegador integrado.

1. Use este prompt para pedir ao Copilot que inicie o aplicativo e abra a página no canvas:

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. Aguarde o início do aplicativo e a abertura de uma janela do navegador dentro do Copilot.
3. Confirme que os cartões avaliados mostram a nota em uma escala de cinco.
4. Quando terminar, peça ao Copilot que pare o servidor de desenvolvimento e feche o canvas:

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## Abrir e mesclar seu primeiro pull request

Agora que você criou o recurso, abra um pull request (PR) para integrá-lo à base de código existente.

1. No canto superior direito, selecione **Create PR**.
2. Se solicitado, selecione **Sign in with your browser** e siga as instruções para autenticar sua conta.
3. Se o Copilot explicar que a política do repositório exige uma issue correspondente antes da criação do PR, confirme que ele deve seguir esse processo.
4. Aguarde o Copilot criar o PR.
5. Selecione o indicador **PR** acima do chat para abrir o pull request no painel de revisão e examiná-lo.
6. Quando estiver tudo pronto, selecione **Ready to merge**.
7. Na nova janela, selecione **Merge pull request** para mesclar o PR.

## Resumo e próximos passos

Parabéns! Você entregou sua primeira alteração usando o aplicativo GitHub Copilot. Nesta lição, você:

- iniciou uma sessão de agente e conheceu sua estrutura.
- orientou o agente a fazer uma alteração pequena e focada nos cartões dos jogos.
- revisou a alteração na visualização de diferenças do espaço de trabalho.
- executou o aplicativo localmente e confirmou a avaliação por estrelas no navegador.
- abriu o PR 1, revisou as verificações e fez a mesclagem explicitamente.

Em seguida, você [começará pela issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para desenvolver um recurso maior.

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#instalar-e-configurar-o-aplicativo-github-copilot
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
