---
title: "Ejercicio 4 - Supervisa y gestiona agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de Copilot en la nube que realice tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque trabaja de forma autónoma y asíncrona, es importante supervisar estas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente, entre ellas la [página de agentes][agents-page] de GitHub.com. Desde este panel de control puedes consultar las tareas con solicitudes de incorporación de cambios abiertas, revisar las operaciones realizadas e incluso dirigir una sesión que sigue en curso.

En este ejercicio:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- dirigirás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de asignar al agente el modo de alto contraste, el equipo decidió que también sería un buen momento para añadir un modo claro. Como ya se están actualizando los estilos del sitio y añadiendo controles de selección de modo, pedirás al agente que incluya el modo claro además del modo de alto contraste.

## Revisa las tareas del agente de Copilot en la nube

Consulta el estado de todas las tareas asignadas al agente.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en los paneles principal e izquierdo. Deberías ver las tareas asignadas a Copilot, entre ellas:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio.
3. Selecciona una de las tareas en curso y revisa las operaciones realizadas por Copilot. Pueden incluir:
   - obtener el código del repositorio.
   - crear el entorno de trabajo de Copilot.
   - configurar servidores de Model Context Protocol (MCP).
   - realizar distintas acciones para completar la tarea.

> [!NOTE]
> Las acciones concretas varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios (PR) de la derecha. Desde allí puedes consultar la PR y los archivos modificados para supervisar el trabajo.

## Dirige al agente en la nube

Ya has revisado las tareas activas. Ahora pide a Copilot que añada el control de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto depende del nombre que Copilot haya elegido y del estado del trabajo.

   ![Sesión de accesibilidad en el panel de control](../../../_images/ex6-accessibility-session.png)

2. Supervisa la sesión hasta que indique que ha terminado de configurarse y ha empezado a trabajar. Verás mensajes que indican ese progreso.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Captura de la tarea del agente en la nube en la página de agentes con el cuadro de diálogo para dirigir la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo confirma y lo incorpora a su trabajo.

## Deja que Copilot trabaje

Copilot continuará con la tarea actualizada. Incorporará la nueva solicitud cuando termine el paso que estaba realizando al recibirla.

El proceso tardará varios minutos; puedes aprovechar ese tiempo para repasar lo que has aprendido.

## Resumen y próximos pasos

En este ejercicio has explorado la página de agentes de Copilot, el panel central para supervisar y dirigir las tareas del agente en la nube. Desde allí puedes hacer seguimiento de las tareas activas y completadas, revisar el trabajo y redirigir una sesión en curso para cambiar su alcance o añadir indicaciones.

Has explorado estos conceptos:

- el panel de control y la página de agentes para supervisar tareas.
- la redirección de una sesión en curso para pedir una funcionalidad adicional.

Mientras Copilot completa las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
