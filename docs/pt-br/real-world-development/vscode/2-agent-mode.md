---
title: "Exercício 2 - Adicionar funcionalidades com o modo Agent do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até uma pequena atualização de aplicativo pode exigir alterações em vários arquivos e operações como executar testes. Em geral, pessoas desenvolvedoras precisam localizar os arquivos, fazer as alterações, executar testes, depurar e descobrir o que faltou. O modo Agent do Copilot pode explorar a estrutura do projeto, fazer alterações coordenadas, executar tarefas como testes e corrigir problemas encontrados. Veja como usá-lo para adicionar funcionalidade ao site.

> [!NOTE]
> Apesar dos nomes semelhantes, o modo agente e o agente de nuvem oferecem experiências diferentes. O modo agente trabalha no ambiente de desenvolvimento integrado (IDE), com ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, usando issues e pull requests para interagir com você.

Neste exercício, você vai:

- explorar como o modo Agent do Copilot localiza arquivos e faz alterações coordenadas.
- usar o modo Agent do GitHub Copilot para implementar recursos na interface e na camada de dados.
- revisar alterações e testes gerados pelo Copilot antes de mesclá-los à base de código.

## Cenário

Com o crescimento da lista de jogos, você quer permitir filtros por categoria e editora. Você já criou um auxiliar de editoras no exercício anterior. Agora, concluirá as alterações restantes na camada de dados, na interface e nos testes com o modo Agent do Copilot.

## Executar o site Tailspin Toys

Antes de alterar o código, explore o site para conhecer o comportamento atual.

O site Tailspin Toys é uma plataforma de financiamento coletivo para jogos de tabuleiro com um tema voltado a desenvolvedores. Ele lista jogos e exibe seus detalhes. O site é um único aplicativo Astro, que gera páginas de linguagem de marcação de hipertexto (HTML) estáticas durante a compilação. As páginas consultam diretamente um banco de dados SQLite local usando Drizzle ORM; não há uma interface de programação de aplicações (API) de backend separada nem um framework de interface executado no cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade usa pequenos elementos `<script>` do Astro com APIs padrão do DOM.

### Iniciar o site

Uma tarefa `npm run dev` inicia o servidor de desenvolvimento Astro. Use-a no Codespace para executar o site.

1. Volte ao Codespace. Você deve estar na branch `main` depois do exercício anterior.
2. Abra um terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, será exibido um aviso semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo pedir para abrir `http://localhost:4321` em um navegador, feche-a selecionando **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço `http://localhost:4321` no terminal.

> [!NOTE]
> No Codespace, selecionar o link localhost no terminal redireciona para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o servidor em execução no Codespace.

### Explorar o site

Com o site em execução, explore suas principais funcionalidades:

- **Página inicial**: mostra uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá informações como título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa informa o que selecionar.

O site já funciona, mas você vai adicionar novos recursos. Comece revisando o backlog de issues criado automaticamente quando o projeto foi configurado a partir do modelo.

1. Na lista de agentes da exibição Chat, selecione **Agent**. O agente planeja e implementa alterações em vários arquivos, executa comandos do terminal e chama ferramentas.

   ![Seletor de agentes na exibição Chat](../../../_images/shared-chat-mode-selector.png)

2. Na lista de modelos disponíveis, selecione **Claude Sonnet 4.5**.

> [!CAUTION]
> Os autores deste workshop não recomendam um modelo específico. Claude Sonnet 4.5 foi usado para criar o workshop para que as sugestões fossem relativamente consistentes. Como a inteligência artificial (IA) e os modelos de linguagem grandes (LLMs) são probabilísticos, suas sugestões podem ser diferentes; isso é normal.

> [!NOTE]
> Devido à natureza probabilística dos modelos de linguagem grandes (LLMs), o Copilot pode usar um comando MCP diferente, mas ainda deve conseguir concluir a tarefa.

3. Envie este prompt para perguntar sobre o backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para permitir que o comando liste todas as issues.
5. Revise a lista gerada.

Observe como o Copilot prioriza os itens que considera mais úteis para as pessoas usuárias.

## Revisar os arquivos de instruções

Antes de pedir ao agente para gerar código, revise o arquivo de instruções que fornecerá contexto. O arquivo [UI][ui-instructions] contém orientações para adicionar funcionalidade ao site.

1. No Codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para instruções específicas, incluindo Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Isso facilita a reutilização, a divisão de orientações complexas e o uso de exemplos e modelos.

## Implementar a filtragem

Para concluir a filtragem, atualize a lógica da camada de dados em `src/lib/`, os testes e a página de listagem de jogos com a interface de filtragem. Os testes devem passar antes da mesclagem. Peça ao modo Agent do Copilot que execute essas tarefas.

1. Continue a conversa atual ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** para adicionar o arquivo de instruções.

   ![Exemplo de seleção do arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado na lista de agentes.
4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente o recurso com base na issue relacionada do backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto, localizar os arquivos relacionados à funcionalidade, aos auxiliares da camada de dados, à interface e aos testes, fazer alterações e executar testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, alterar arquivos e executar testes. Isso pode levar alguns minutos. Durante o trabalho, os botões **Keep** e **Undo** podem aparecer no editor. Aguarde a conclusão antes de aceitar ou desfazer as alterações.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Caixa de diálogo do Copilot Chat solicitando autorização para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas ou falhas em alguns testes durante o processo. O Copilot alterna entre gerar código e executar testes até concluir a tarefa e não detectar erros.

   ![Sessão concluída do modo Agent do Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado e procure problemas.

> [!CAUTION]
> Revise sempre o código gerado pelo Copilot ou por outras ferramentas de IA.

10. Volte ao navegador com o site em execução e explore o recurso.
11. Depois de confirmar que funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Neste exercício, você usou o modo Agent do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Você aprendeu a:

- usar o modo Agent para implementar recursos na interface e na camada de dados.
- permitir que o modo Agent explore o projeto, localize arquivos relevantes e faça alterações coordenadas.
- revisar as alterações e os testes gerados antes da mesclagem.

Em seguida, [teste o recurso com o servidor MCP do Playwright][next-lesson] e abra um pull request.

### Exercício bônus de exploração — implementar paginação

Com o crescimento da lista de jogos, será necessário adicionar paginação. Use o que aprendeu neste exercício e peça ao Copilot que implemente esse recurso. Considere:

- seguir as práticas recomendadas existentes, inclusive as instruções do projeto.
- decidir se a pessoa usuária poderá escolher o tamanho da página ou se ele será fixo.
- fornecer contexto e orientações suficientes no prompt.
- iterar e fornecer informações adicionais ao Copilot, se necessário. Essa é uma parte normal do trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
