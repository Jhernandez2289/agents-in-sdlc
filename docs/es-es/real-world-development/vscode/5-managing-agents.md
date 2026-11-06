---
title: "Ejercicio 5 - Supervisa y gestiona agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas el modo agente de GitHub Copilot, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tu equipo y en tiempo real, Copilot Chat en VS Code muestra cada llamada a una herramienta y cada edición. Puedes revisar las diferencias en línea, aceptar o rechazar cambios y orientar la conversación con prompts de seguimiento, sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones de archivos propuestas.
- orientarás al agente durante la sesión para que añada un control de modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que usaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha pedido también un control para el modo claro, pensado para quienes prefieren una interfaz menos oscura. En lugar de empezar una conversación nueva, ampliarás el trabajo de la sesión actual.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar las llamadas a herramientas, las lecturas y ediciones de archivos y los comandos de terminal que solicita ejecutar.
3. Cuando el agente proponga una edición, aparecerá una vista de diferencias en el chat:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta requiere permiso, por ejemplo, para ejecutar un comando de terminal, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final que resuma los cambios.

## Pide al agente que añada el modo claro

Cuando el modo de alto contraste esté listo, amplía la misma conversación para añadir un control para el modo claro. Al continuar en el mismo chat, el agente conservará el contexto de su trabajo: los componentes modificados, el enfoque de estilos y el uso del almacenamiento local.

1. Envía este prompt de seguimiento desde el campo de texto de Copilot Chat:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente continuará desde donde se quedó, explorará los cambios anteriores y propondrá los cambios necesarios para el modo claro.
3. Revisa cada edición nueva cuando aparezca, igual que con el modo de alto contraste. Usa **Keep** o **Undo** para cada diferencia.
4. Si el agente toma una dirección que no quieres —por ejemplo, intenta reorganizar los controles de una forma que no te convence—, deténlo con el botón **Stop** junto al campo de texto y envía otro prompt que aclare el enfoque.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Usa prompts de seguimiento para solicitar ajustes («simplifica el CSS: usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa todos los archivos modificados por el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que se aplican los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación por teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y próximos pasos

Has usado el modo agente de Copilot Chat para supervisar y dirigir una sesión en curso: has revisado las ediciones a medida que aparecían, las has aceptado o rechazado y has enviado prompts de seguimiento para ampliar el trabajo sin perder contexto.

En este ejercicio:

- supervisaste en tiempo real el trabajo de un agente.
- aceptaste y rechazaste ediciones de archivos.
- orientaste una sesión en curso con prompts de seguimiento.

A continuación, en la [siguiente lección][next-lesson], publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios que ha realizado el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
