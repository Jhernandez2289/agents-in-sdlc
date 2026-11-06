---
title: "Exercício 0: Pré-requisitos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Prepare o ambiente antes de começar os exercícios do VS Code. Você criará uma cópia própria do repositório Tailspin Toys, iniciará um [Codespace][codespaces] e confirmará que o GitHub Copilot Chat está funcionando no editor.

## Configurar o repositório do workshop

Crie uma cópia do repositório a partir do [modelo][template-repository]. A cópia conterá os arquivos necessários para os exercícios.

1. Em uma nova janela do navegador, acesse o [modelo do Tailspin Toys][tailspin-template].
2. Crie sua cópia selecionando **Use this template** na página do repositório e, em seguida, **Create a new repository**.

   ![Botão Use this template](../../../_images/ex0-use-template.png)

3. Se você estiver participando de um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções das pessoas mentoras. Caso contrário, crie o repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

   ![Configurações do repositório criado a partir do modelo](../../../_images/ex0-repository-settings.png)

4. Anote o caminho do repositório criado (**organization-or-user-name/repository-name**), pois você precisará dele mais adiante.

> [!NOTE]
> **O backlog está pronto**
>
> Ao criar o repositório a partir do modelo, um backlog de issues do GitHub será criado automaticamente. Você usará essas issues durante o workshop; não precisa criar issues manualmente.

## Criar um Codespace

Use um Codespace para concluir os exercícios.

[GitHub Codespaces][codespaces] é um ambiente de desenvolvimento na nuvem que permite escrever, executar e depurar código diretamente no navegador. Ele oferece um ambiente de desenvolvimento integrado (IDE) completo, com suporte a várias linguagens, extensões e ferramentas.

1. Acesse o repositório recém-criado.
2. Selecione o botão verde **Code**.

   ![Botão Code](../../../_images/ex0-code-button.png)

3. Selecione a guia **Codespaces** e o botão **+** para criar um Codespace.

   ![Criar um Codespace](../../../_images/ex0-create-codespace.png)

A criação do Codespace pode levar alguns minutos, mas ainda é mais rápida do que instalar manualmente todos os serviços. Enquanto aguarda, explore outros recursos do GitHub Copilot.

> [!CAUTION]
> Você voltará ao Codespace em um exercício futuro. Por enquanto, deixe-o aberto em uma guia do navegador.

> [!NOTE]
> Este workshop foi preparado para ser executado em um Codespace ou em um [contêiner de desenvolvimento][dev-containers] local. Ambos fornecem os pré-requisitos necessários. Para trabalhar localmente, abra o repositório clonado no VS Code e selecione **Reopen in Container** quando solicitado. O VS Code criará o mesmo contêiner usado pelo Codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usar o GitHub Copilot Chat e o modo agente

Para acessar o modo agente do GitHub Copilot Chat, instale a extensão GitHub Copilot Chat no IDE. Ela já deve estar disponível se você estiver usando um Codespace.

> [!TIP]
> Se a extensão GitHub Copilot Chat não estiver instalada, [instale-a pelo Marketplace do Visual Studio Code][copilot-chat-extension]. Você também pode abrir a exibição **Extensions** no Visual Studio Code, pesquisar **GitHub Copilot Chat** e selecionar **Install**.

Depois da instalação, talvez seja necessário autenticar sua conta do GitHub para habilitar a extensão.

1. Volte ao Codespace.
2. Se o Copilot Chat não estiver visível à direita do editor, selecione o ícone **Copilot Chat** na parte superior do Codespace.
3. Digite uma mensagem, como `Hello world`, na janela do Copilot Chat e pressione <kbd>Enter</kbd>. Isso deve ativar o Copilot Chat.
4. Se você não estiver autenticado, siga as instruções para entrar na conta do GitHub.

   ![Prompt de autenticação do Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Depois da autenticação, a janela do Copilot Chat será exibida.

## Resumo

Parabéns! Você criou uma cópia do repositório do workshop e iniciou a criação de um Codespace para começar a desenvolver.

## Próximo passo

Comece a trabalhar com o Copilot. Continue para [Exercício 1: Instruções personalizadas][next-lesson], no qual você apresentará as convenções do projeto ao Copilot.

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um modelo][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
