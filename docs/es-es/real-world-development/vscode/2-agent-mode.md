---
title: "Ejercicio 2 - Añade funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios sencillos suelen requerir modificar varios archivos y ejecutar tareas como las pruebas. El flujo de trabajo habitual consiste en localizar los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta y volver a modificarlo. Y así sucesivamente.

El modo agente de Copilot puede ayudarte con ese proceso.

El modo agente trabaja con más autonomía en el IDE: explora la estructura del proyecto, realiza los cambios necesarios y ejecuta tareas como las pruebas. También puede corregir los errores que detecte. Vamos a usarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en tu IDE y permite iterar rápidamente. El agente en la nube trabaja de forma asíncrona como otro miembro del equipo, y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- hacer que el modo agente de Copilot explore el proyecto, identifique archivos y coordine los cambios.
- usar el modo agente de GitHub Copilot para implementar funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados por Copilot antes de combinarlos en el código.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior añadiste un asistente para editoriales; ahora completarás el trabajo restante en la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecuta el sitio de Tailspin Toys

Antes de hacer cambios, explora el sitio para entender su funcionalidad actual.

El sitio de financiación colectiva de juegos de mesa con temática de desarrollo permite consultar listas de juegos y ver sus detalles. Es una única aplicación de Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM desde su frontmatter; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los asistentes reutilizables para acceder a los datos están en `src/lib/`. La interactividad se añade mediante un pequeño `<script>` de Astro con las API estándar del DOM.

### Inicia el sitio

La tarea `npm run dev` inicia el único servidor de desarrollo de Astro. Ejecútalo en tu Codespace.

1. Vuelve a tu Codespace. Al terminar el ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en el Codespace con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando se inicie el servidor de desarrollo de Astro, verás un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un Codespace, al seleccionar el enlace localhost en el terminal se te redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado a tu Codespace, que ahora aloja el servidor web.

### Explora el sitio

Con el sitio en marcha, explora sus funciones principales:

- **Página de inicio**: muestra la lista de juegos de mesa, con títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección de modo y modelo varía en cada ejercicio; aquí se indica cuando corresponda.

El sitio inicial funciona, pero queremos añadir capacidades. Empieza por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó un backlog de incidencias. Pide a GitHub Copilot que te muestre esas tareas.

1. En el menú desplegable de agentes de Chat, selecciona **Agent**. El agente planifica e implementa cambios en varios archivos, ejecuta comandos de terminal y usa herramientas.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no expresan preferencia por un modelo concreto. Durante su desarrollo usamos Claude Sonnet 4.5 y lo indicamos para que las sugerencias sean relativamente coherentes y la experiencia sea más uniforme. Como los modelos de lenguaje grandes (LLM) son probabilísticos, es normal que las sugerencias varíen.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede usar un comando MCP distinto, pero debería poder completar la tarea.

3. Envía este prompt a Copilot para consultar el backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que se enumeren todas las incidencias.
5. Revisa la lista de incidencias generada.

Observa cómo Copilot también ha priorizado las tareas según su criterio sobre cuáles serán más útiles para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que le dará contexto. Usarás el archivo de instrucciones de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidad al sitio.

1. En tu Codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales para añadir funcionalidad, entre ellas:
   - una descripción de la arquitectura.
   - principios de diseño de componentes, pruebas y accesibilidad.
   - enlaces a archivos de instrucciones para distintos tipos de archivos, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así puedes reutilizar directrices, dividir conjuntos complejos en partes más manejables y señalar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, hay que hacer al menos tres cambios:

- añadir o ajustar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de filtrado.
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios. El modo agente de Copilot puede hacer este trabajo.

1. Continúa en la conversación actual de Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el menú desplegable de agentes.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad descrita en la incidencia del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos de la funcionalidad, incluida la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y del código. Mientras trabaja, pueden aparecer **Keep** y **Undo** en el editor. No tienes que seleccionarlos mientras Copilot trabaja; al terminar podrás aceptar o deshacer todos los cambios.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede haber pausas e incluso algunas pruebas pueden fallar durante el proceso. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Sesión completada de Copilot Chat en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Revisa el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot y otras herramientas de IA.

10. Vuelve al navegador con el sitio en marcha y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y próximos pasos

Has usado el modo agente de GitHub Copilot para añadir funcionalidad al sitio de Tailspin Toys. Has aprendido a:

- implementar funcionalidades en la interfaz y la capa de datos con el modo agente.
- hacer que Copilot explore el proyecto, identifique archivos y coordine cambios.
- revisar los cambios y las pruebas generados por Copilot antes de combinarlos en el código.

A continuación, [probarás la funcionalidad con el servidor de Model Context Protocol (MCP) Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio extra: implementa la paginación

A medida que crezca la lista de juegos, hará falta paginar los resultados. Usa lo que has aprendido para pedir a Copilot que añada paginación. Al preparar el prompt:

- pide que siga las prácticas recomendadas y use los archivos de instrucciones existentes.
- decide si los usuarios podrán elegir el tamaño de página o si será fijo.
- proporciona a Copilot las directrices necesarias para implementar la paginación como prefieras.
- itera con Copilot, pidiendo cambios y añadiendo contexto. Es el flujo normal de trabajo.

## Recursos

- [Copilot ask, edit y agent modes: qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
