---
title: "Lección 10 - Repaso y próximos pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI en un flujo de trabajo continuo para Tailspin Toys. En concreto:

- preparaste un Codespace, instalaste Copilot CLI, exploraste el proyecto y localizaste la incidencia de filtrado del backlog.
- añadiste las valoraciones con estrellas, revisaste el resultado en un navegador reenviado y combinaste manualmente tu primera solicitud de incorporación de cambios (PR).
- partiste de la incidencia de filtrado, definiste un enfoque en modo Plan, lo implementaste con Autopilot y lo revisaste en modo Interactive.
- orientaste al agente con instrucciones personalizadas, personalizaste la habilidad `quality-checks` y la usaste para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadiste el servidor Model Context Protocol (MCP) de Playwright y lo usaste para explorar el filtrado en un navegador real.
- creaste y seleccionaste un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas del navegador.
- revisaste todos los cambios del filtrado y autorizaste Agent Merge para combinar la PR.
- exploraste comandos de barra para gestionar el contexto y los modelos, compartir sesiones y delegar trabajo opcionalmente en la nube.

## Lo que has entregado

El taller incluye dos hitos de PR, cada uno en una rama creada a partir de `main` actualizada:

1. **Valoraciones con estrellas:** muestra en las tarjetas de juegos `starRating` y un estado explícito para los juegos sin valorar.
2. **Filtrado y flujo de trabajo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la creación de su PR, usaste la misma conversación y rama. Agrupamos ese trabajo en una PR para simplificar el taller.

## Diferentes tipos de validación

Comprobaste el código de varias formas: con pruebas automatizadas, examinando el sitio en el navegador y explorándolo con el navegador de Copilot a través de MCP. La habilidad `quality-checks` ejecutó las pruebas unitarias, el lint y las comprobaciones de tipos, y presentó los resultados con el nuevo formato. Antes de la PR, QA reunió esos resultados con la revisión de los requisitos y la cobertura de las pruebas.

Las pruebas nuevas deben cerrar carencias reales; puede ser correcto que una ejecución de QA no requiera pruebas adicionales. Revisa el código y las evidencias antes de autorizar la combinación, y actualiza las evidencias afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller actualizaste instrucciones, personalizaste una habilidad, creaste un perfil de QA y configuraste un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y adáptalas a las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para pensar el enfoque antes de implementarlo, **Interactive** para colaborar en cambios acotados y **Autopilot** para tareas bien definidas. Elige un modelo más rápido para ediciones rutinarias y uno más avanzado para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad **qué** quieres desarrollar, **por qué** y **cómo** influye de forma significativa en el resultado.

## Más posibilidades

Has completado el flujo de trabajo principal. Estas son algunas funciones más de la CLI que puedes probar:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar otra tarea.
- `/delegate` para enviar una tarea al agente de Copilot en la nube.

## Próximos pasos

La mejor forma de mejorar con una herramienta es seguir usándola: en código de producción, en proyectos personales o en esa pequeña aplicación que llevas años queriendo crear. Comparte lo que aprendas con el equipo y aprovecha lo que aprendan los demás. Y, como siempre, sigue explorando la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la [serie opcional Backer Concierge con Foundry][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
