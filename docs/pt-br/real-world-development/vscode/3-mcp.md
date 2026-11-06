---
title: "Exercício 3 - Testar seu recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de implementar a filtragem com o modo agente. Antes de abrir um pull request, confirme se ela funciona no navegador. Em vez de testar o aplicativo manualmente, conecte o **servidor MCP do Playwright** e peça ao Copilot que controle um navegador real. Depois, publique sua branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como servidores MCP ampliam as ferramentas do Copilot.
- adicionar o servidor MCP do Playwright ao workspace.
- pedir ao Copilot que o use para testar manualmente o recurso de filtragem no navegador.
- publicar a branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo agente é mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) é um padrão para o agente se comunicar com ferramentas e serviços externos.

![Diagrama da arquitetura do modo agente e da interação com contexto, LLM e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Assim, eles podem acessar informações atualizadas por meio de recursos e executar ações em seu nome por meio de ferramentas.

Um servidor MCP faz a ponte entre o agente de IA e as ferramentas e os serviços externos, gerenciando a comunicação com APIs existentes ou ferramentas locais, como pacotes npm. Cada servidor oferece um conjunto diferente de ferramentas e recursos.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub][github-mcp-server]**: oferece acesso a APIs para gerenciar repositórios do GitHub, criar repositórios, atualizar os existentes e gerenciar issues e pull requests.
- **[Servidor MCP do Playwright][playwright-mcp-server]**: oferece automação de navegador com Playwright, como navegar em páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP. O GitHub mantém um [registro de MCP][mcp-registry] para facilitar sua descoberta e as contribuições ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise o código-fonte, verifique o publicador e considere as implicações de segurança. Use somente servidores MCP confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no Codespace.
2. Confirme que já há um servidor `github` configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

Essa entrada `github` vem do modelo do projeto. Por isso, o Copilot conseguiu ler o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não é preciso instalar nada localmente. O VS Code se conecta por HTTP e você se autentica no GitHub na primeira vez que o Copilot usa uma ferramenta.

## Adicionar o servidor MCP do Playwright

Adicione um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] oferece ao Copilot um navegador controlável para testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, de modo que o arquivo fique assim:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Salve o arquivo.

Diferentemente do servidor remoto `github`, `playwright` é local: o VS Code o inicia no computador com `npx @playwright/mcp@latest`. O sinalizador `--headless` executa o navegador sem uma janela visível, necessário no Codespace, que não tem uma área de trabalho gráfica.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para testes de ponta a ponta, então o navegador necessário geralmente já está instalado. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP **sob demanda**: quando o Copilot precisa das ferramentas de um servidor pela primeira vez, o VS Code o inicia e pede que você confirme se confia nele.

1. Em `.vscode/mcp.json`, localize a ação **Start** exibida acima da entrada `playwright` e selecione-a para iniciar o servidor e confirmar a configuração.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração e autorize-o.

O início manual é opcional. Se você ignorar essas etapas, o VS Code iniciará o servidor e pedirá sua autorização quando o Copilot precisar das ferramentas no próximo exercício.

## Testar o recurso de filtragem

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste a filtragem:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O servidor MCP do Playwright controla um navegador real. O agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumir o que observou. Compare o relatório com os critérios de aceitação da issue; se algo parecer errado, faça perguntas complementares ou peça ao Copilot que corrija o código antes de abrir o PR.

O servidor usa a [árvore de acessibilidade][playwright-mcp-server] da página, não capturas de tela. Assim, o agente examina elementos estruturados e rotulados, como botões, links e itens de lista, tal como a tecnologia assistiva. Um teste funcional rápido também oferece uma verificação básica de acessibilidade.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321`. Se você tiver parado o servidor de desenvolvimento, inicie-o antes de enviar o prompt. Na primeira utilização do servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se o Copilot informar que ele está ausente, peça que execute `npx playwright install chromium` e tente novamente.

## Publicar a branch e criar um pull request

Depois de confirmar que o recurso funciona, publique a branch `filtering-vscode` para que a equipe possa revisar o pull request (PR).

1. Abra o painel **Source Control** no Codespace e revise as alterações do Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Painel Source Control com as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar o pull request

Há várias maneiras de criar um pull request, inclusive pelo github.com e pela interface de linha de comando (CLI) do GitHub. Peça ao Copilot que encontre a issue relacionada e associe-a ao PR.

1. Abra o painel Copilot Chat e selecione **New Chat** para iniciar uma sessão nova.
2. Na lista de agentes, confirme que **Agent** está selecionado para permitir o uso das ferramentas do GitHub.
3. Peça ao Copilot que crie o PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Selecione **Continue** quando necessário para permitir que o Copilot reúna informações e execute operações. Na primeira utilização de uma ferramenta do GitHub, talvez seja necessário autenticar-se; siga as instruções.
5. Observe o Copilot pesquisar as issues, localizar a correta e criar o PR.
6. Abra o link criado pelo Copilot para revisar o PR, mas **não o mescle ainda**.

## Resumo e próximos passos

Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como servidores MCP ampliam as ferramentas do Copilot.
- adicionou o servidor MCP do Playwright ao workspace.
- usou o servidor para testar o recurso de filtragem no navegador antes de publicá-lo.
- publicou a branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para testar um recurso, mas essa é apenas uma maneira de ampliar o Copilot. Em seguida, [crie um agente personalizado][next-lesson] para agilizar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro de MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
