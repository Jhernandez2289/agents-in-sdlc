---
title: "Ejercicio 3 - Prueba tu funcionalidad con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear el filtrado con el modo agente. Antes de abrir una solicitud de incorporación de cambios, comprueba que funciona en el navegador. En vez de recorrer la aplicación manualmente, conectarás el servidor Playwright MCP y dejarás que Copilot controle un navegador real para probar la funcionalidad. Después publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot.
- añadirás el servidor Playwright MCP al espacio de trabajo.
- pedirás a Copilot que lo use para probar manualmente la funcionalidad en un navegador.
- publicarás la rama y abrirás una solicitud de incorporación de cambios para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente gana muchas capacidades cuando puede acceder a recursos externos. Model Context Protocol (MCP) es un estándar que permite a Copilot comunicarse con herramientas y servicios externos.

![Diagrama del modo agente y su interacción con el contexto, el modelo de lenguaje y las herramientas, incluidas las que proporcionan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Los agentes acceden a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente y los servicios externos, como las interfaces de programación de aplicaciones (API) existentes o herramientas locales en paquetes npm. Cada servidor proporciona un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona API para gestionar repositorios de GitHub, crear y actualizar repositorios, y administrar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: permite automatizar el navegador con Playwright, por ejemplo, navegar por páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP con distintas herramientas y recursos. GitHub mantiene un [registro de MCP](https://github.com/mcp) para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y valora las implicaciones de seguridad. Usa solo servidores de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu Codespace.
2. Comprueba que ya hay un servidor `github` configurado:

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

La plantilla del proyecto incluye la entrada `github`, por eso Copilot pudo leer el backlog de incidencias en el ejercicio anterior. Usa el [servidor MCP remoto de GitHub][remote-github-mcp-server]. No hace falta instalar nada localmente: VS Code se conecta por HTTP y te autenticas con GitHub la primera vez que Copilot usa una de sus herramientas.

## Añade el servidor Playwright MCP

Añadirás un segundo servidor. [Playwright MCP Server][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

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

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es local. VS Code lo inicia ejecutando `npx @playwright/mcp@latest`. La opción `--headless` hace que Playwright use un navegador sin ventana visible, necesario en un Codespace sin escritorio.

> [!NOTE]
> El proyecto Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, así que normalmente el navegador ya está instalado. Si Copilot indica que falta, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Inicia el servidor y confirma que es de confianza

VS Code inicia los servidores MCP cuando los necesita. La primera vez que Copilot necesita las herramientas de un servidor, VS Code lo inicia y te pregunta si confías en él.

1. En `.vscode/mcp.json`, selecciona la acción **Start** que aparece encima de la entrada `playwright` para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él para iniciarlo.

Iniciarlo manualmente es opcional. Si omites estos pasos, VS Code lo iniciará y te pedirá que confirmes que confías en él cuando Copilot necesite las herramientas.

## Prueba la funcionalidad de filtrado

El servidor Playwright MCP proporciona a Copilot un navegador real. El agente puede abrir una página, aplicar filtros y leer los resultados, y después resumir lo que ha observado. Así puedes comprobar rápidamente el comportamiento sin salir de la conversación.

El servidor Playwright MCP usa el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. El agente trabaja con elementos estructurados y etiquetados —botones, enlaces y elementos de lista— de una forma parecida a las tecnologías de asistencia. Por eso, una comprobación funcional también sirve como revisión básica de accesibilidad.

Con el servidor conectado y la aplicación en marcha, pide a Copilot que pruebe el filtrado:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador mediante Playwright MCP, seguirá los pasos y te informará de los resultados. Compara el informe con los criterios de aceptación de la incidencia. Si algo no funciona como esperabas, pide más detalles o solicita cambios antes de abrir la PR.

> [!NOTE]
> Para esta prueba, la aplicación debe estar en marcha en `http://localhost:4321`. Si detuviste el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot use Playwright MCP quizá tenga que descargar un navegador; si indica que falta, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de incorporación de cambios

Una vez que hayas confirmado que la funcionalidad funciona, publica la rama `filtering-vscode` para que el equipo pueda revisarla.

1. Abre el panel **Source Control** en el Codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de commit con el botón **Sparkle**.

   ![Panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Hay varias formas de crear una solicitud de incorporación de cambios (PR), por ejemplo, en github.com o con la interfaz de línea de comandos de GitHub (CLI). Como ya trabajas con GitHub Copilot, pídele que la cree y la vincule a la incidencia correcta.

1. Ve al panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Confirma que está seleccionado **Agent** para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Si es necesario, selecciona **Continue** para permitir que Copilot recopile información y realice las operaciones. La primera vez que use una herramienta de GitHub, quizá debas autenticarte. Sigue las instrucciones para permitir el acceso.
5. Observa cómo Copilot busca la incidencia correcta y crea la PR.
6. Selecciona el enlace que genera Copilot y revisa la solicitud, pero **no la combines todavía**.

## Resumen y próximos pasos

En este ejercicio:

- aprendiste qué es Model Context Protocol (MCP) y cómo amplía las herramientas de Copilot.
- añadiste el servidor Playwright MCP al espacio de trabajo.
- lo usaste para probar manualmente la funcionalidad de filtrado en un navegador antes de publicarla.
- publicaste la rama y abriste una solicitud de incorporación de cambios para el filtrado.

Has usado un servidor MCP para probar una funcionalidad, pero MCP es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué habla todo el mundo de ello][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro de GitHub MCP][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
