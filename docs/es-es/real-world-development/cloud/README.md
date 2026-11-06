---
slug: es-es/real-world-development/cloud
title: "Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[El agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** permite que GitHub Copilot trabaje de forma asíncrona en la nube. Le asignas tareas en GitHub y el agente las realiza en segundo plano: explora el repositorio, hace cambios y abre una solicitud de incorporación de cambios mientras tú te ocupas de otras tareas.

En estos ejercicios añadirás instrucciones personalizadas que seguirá el agente y, después, le asignarás una incidencia para que implemente el trabajo. Revisarás y usarás agentes personalizados, supervisarás y dirigirás las sesiones desde el panel de agentes y, por último, revisarás las solicitudes de incorporación de cambios e iterarás sobre los resultados.

## Ejercicios

| Ejercicio | Tema | Descripción |
|----------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Crea el repositorio y un Codespace |
| [1. Instrucciones personalizadas][ex1] | Contexto | Añade instrucciones personalizadas que seguirá el agente en la nube |
| [2. Agente en la nube][ex2] | Agente asíncrono | Asigna incidencias al agente de GitHub Copilot en la nube |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revisa y usa agentes personalizados |
| [4. Gestiona agentes][ex4] | Supervisión | Supervisa y dirige las sesiones de agente |
| [5. Itera][ex5] | Revisión | Revisa las PR, itera sobre el trabajo de Copilot y elige los siguientes pasos |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] El agente de Copilot en la nube habilitado para tu cuenta u organización

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis mediante [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye las funciones de agente, MCP, revisión de código y Copilot CLI que se usan en este taller, así que puedes completar todos los itinerarios.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> El agente en la nube está disponible con **Copilot Student, Pro, Pro+, Business y Enterprise**, es decir, en todos los planes excepto Copilot Free. En Copilot Business o Enterprise, un administrador debe habilitarlo para la organización.

> [!NOTE]
> Este itinerario no incluye Model Context Protocol (MCP). Para aprender a usar servidores MCP con Copilot, consulta el [itinerario de CLI](../cli/) o el [itinerario de VS Code](../vscode/).

## Empieza

**[Empieza por los requisitos previos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
