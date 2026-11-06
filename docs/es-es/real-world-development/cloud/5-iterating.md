---
title: "Ejercicio 5: Itera sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le pediste que documentara el código, desarrollara una función de juegos relacionados para que el equipo de diseño pudiera explorarla e implementara mejoras de accesibilidad, incluidos controles para los modos de alto contraste y claro. Vamos a revisar los cambios y, si hace falta, pedir a Copilot que los mejore.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian al añadir inteligencia artificial (IA) generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de desarrollo. Revisa las propuestas de Copilot para la documentación, los juegos relacionados y las funciones de accesibilidad antes de permitir que el resto del equipo las revise.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente de Copilot en la nube trabaja de forma asíncrona y sin supervisión, se han establecido medidas de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usa para sus cambios.
- El agente se ejecuta en GitHub Actions, dentro de un entorno independiente y temporal.
- Un desarrollador debe aprobar los flujos de trabajo de GitHub Actions antes de que puedan ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores de Model Context Protocol (MCP).

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) generada por el agente: la documentación del código. La revisarás con la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Es posible que veas una advertencia que indica que el cortafuegos ha bloqueado a GitHub Copilot. Es normal: el acceso del agente a recursos externos, incluidos los servidores MCP externos, está limitado de forma predeterminada. Si quieres, puedes [personalizar o desactivar el cortafuegos del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la solicitud cuyo título sea parecido a **Add missing documentation** o que describa de forma más completa la tarea.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Examina el código actualizado, que incluye los nuevos comentarios TSDoc y otra documentación. Los cambios concretos pueden variar. Comprueba los comentarios TSDoc, las convenciones de TypeScript y los comentarios de cabecera. Proceden de los archivos de instrucciones que revisaste al principio del ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Busca el indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos aparecen en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

Puedes mencionar a Copilot en comentarios de la PR, igual que a otras personas del equipo, o añadir comentarios en línea al código. Copilot los recibe e inicia otra sesión para resolverlos. Como los resultados son probabilísticos, no hay un prompt único que debas usar. Puedes pedirle, por ejemplo, que:

- añada al principio de cada archivo de código un comentario breve que explique su propósito.
- añada comentarios TSDoc a los archivos TypeScript y Astro.
- cree un archivo README que describa la estructura de la aplicación Astro.

1. Añade un comentario en la PR con `@copilot` y pide una mejora para la documentación. Puedes usar una de las sugerencias anteriores o formular otra petición.
2. Selecciona **View Session** para observar cómo Copilot inicia una sesión nueva y aplica los cambios.
3. Selecciona **Back to pull request** para volver a la solicitud.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, busca el nuevo commit en la PR.
5. Selecciona la pestaña **Files changed** y revisa los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Ahora revisarás la PR que Copilot creó para mostrar juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR cuyo título sea parecido a **Show related games on the game details page** o que describa de forma más completa la tarea.
4. Selecciona **Files changed** y revisa el código generado.
5. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Busca el indicador de que hay flujos de trabajo pendientes de aprobación.
7. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Comprueba que los flujos aparecen en la sección de comprobaciones de la PR. Si todo va bien, deberían superarse las comprobaciones del proyecto para la aplicación Astro. El proceso puede tardar unos minutos.
9. **Opcional:** cambia a esta rama en tu Codespace para probar manualmente la funcionalidad de juegos relacionados. Ve al Codespace, abre un terminal y ejecuta estos comandos. Sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes pedir cambios mediante comentarios en la pestaña **Conversation**; Copilot los verá e iniciará otra sesión. Por ejemplo, puedes cambiar el número de juegos que se muestran o pedir que añada comentarios de cabecera y comentarios TSDoc. Recuerda que esta tarea se asignó antes de actualizar las instrucciones personalizadas.

## Revisa las funciones de accesibilidad

Por último, revisarás las funciones de accesibilidad implementadas con el agente personalizado. La PR debería incluir el modo de alto contraste que asignaste en el ejercicio 3 y el modo claro que pediste en el panel de control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR cuyo título sea parecido a **Add high contrast mode to website** o que describa de forma más completa la tarea.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** y revisa el código generado.
5. Examina la implementación y presta especial atención a:
   - los componentes de interfaz para alternar entre modos.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios de hojas de estilo en cascada (CSS) para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como las etiquetas de las aplicaciones de Internet enriquecidas y accesibles (ARIA) y la navegación por teclado.
   - el código JavaScript/TypeScript que gestiona los modos.

6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Busca el indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos aparecen en la sección de comprobaciones de la PR y que se superan las comprobaciones del proyecto para la aplicación Astro.
10. **Opcional:** cambia a esta rama en tu Codespace para probar las funciones de accesibilidad. Abre un terminal y ejecuta estos comandos. Sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles de los modos de alto contraste y claro en el navegador. Confirma que funcionan y que la preferencia se conserva al volver a cargar la página.

El agente personalizado de accesibilidad ha orientado a Copilot para que aplique prácticas recomendadas. Si detectas problemas o mejoras posibles, puedes mencionar a `@copilot` en un comentario de la PR, como hiciste con las solicitudes anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube es útil para delegar tareas reales del backlog y ocuparte de otros asuntos mientras trabaja. Para practicar, crea algunas incidencias en tu repositorio y asígnaselas a Copilot. Por ejemplo:

- crear un formulario para mostrar interés por un juego en la página de detalles.
- implementar la paginación en la página de la lista de juegos.
- añadir validación de entradas y gestión de errores a las funciones de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. En estos ejercicios:

- **Revisaste los archivos de instrucciones personalizados del repositorio** para reconocer su efecto en el trabajo del agente.
- **Asignaste incidencias al agente de Copilot en la nube** y observaste cómo configura el entorno, planifica y trabaja de forma asíncrona.
- **Creaste y usaste un agente personalizado** para implementar modos de alto contraste y claro.
- **Usaste la página de agentes de Copilot como panel de control** para supervisar y dirigir la sesión de accesibilidad.
- **Revisaste las solicitudes del agente e iteraste sobre ellas**, mencionando a `@copilot` para pedir cambios y aprobando flujos de trabajo.

## Repaso y próximos pasos

Has completado el itinerario del agente en la nube. Si quieres seguir explorando, los otros itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración con MCP directamente desde el entorno de desarrollo integrado (IDE).
- 💻 **[Itinerario de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y el puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente una incidencia de refactorización o de cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente de Copilot en la nube][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
