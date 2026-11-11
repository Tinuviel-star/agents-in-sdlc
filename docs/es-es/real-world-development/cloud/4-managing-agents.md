---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los ejercicios anteriores, pediste al agente de GitHub Copilot en la nube que se encargara de tres tareas para mejorar la experiencia y añadir funcionalidades. Aunque trabaja de forma autónoma y asíncrona, es importante supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente, entre ellas la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver las tareas con solicitudes de incorporación de cambios (PR) abiertas, examinar las operaciones realizadas y orientar las sesiones en curso.

En este ejercicio:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente el modo de alto contraste, el equipo decidió que también sería conveniente añadir un modo claro. Como ya se están actualizando los estilos y añadiendo controles de cambio de modo, pedirás al agente que incluya también el modo claro.

## Revisa las tareas del agente de Copilot en la nube

Consulta el estado actual de las tareas que has asignado al agente de GitHub Copilot en la nube.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa las tareas del panel principal y del panel izquierdo. Deberías ver las tareas asignadas a Copilot, entre ellas:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio.
3. Selecciona una tarea en curso y revisa las operaciones realizadas por Copilot. Pueden incluir:
   - descargar el código del repositorio.
   - preparar el entorno para Copilot.
   - configurar servidores MCP.
   - realizar varias operaciones para completar la tarea.

> [!NOTE]
> Los pasos exactos varían según el estado de la tarea y el enfoque que haya seguido Copilot.

4. Revisa también el panel de PR, que aparece a la derecha y permite consultar la solicitud y los archivos modificados.

## Orienta al agente en la nube

Ahora que has visto las tareas activas, pedirás a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión correspondiente al modo de alto contraste. El título exacto dependerá del nombre que haya utilizado Copilot y del estado de la tarea.

   ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos hasta que la sesión indique que ha terminado de preparar el entorno y ha empezado a trabajar. Lo notarás cuando aparezcan mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Comprueba que Copilot lo recibe y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot incorporará la nueva solicitud al flujo cuando termine el paso en el que estaba trabajando. La tarea tardará unos minutos; puedes aprovechar ese tiempo para repasar lo que has aprendido.

## Resumen y pasos siguientes

Has explorado la página de agentes de Copilot, el centro de control para supervisar y orientar las tareas del agente de GitHub Copilot en la nube. Puedes realizar un seguimiento de las tareas activas y completadas, revisar el trabajo y reorientar las sesiones en curso.

En este ejercicio:

- has explorado el centro de control y la página de agentes para supervisar las tareas del agente en la nube.
- has reorientado una sesión en curso para solicitar una funcionalidad adicional.

Mientras Copilot termina el trabajo de accesibilidad, [revisarás e iterarás sobre las PR que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Información sobre los agentes personalizados][custom-agents]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Ejercicio siguiente: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
