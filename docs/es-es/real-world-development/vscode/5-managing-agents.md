---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] |
|:--|

En el modo agente, GitHub Copilot explora el código, propone cambios, edita archivos y ejecuta comandos de forma autónoma. Como trabaja en tu equipo en tiempo real, Copilot Chat en VS Code muestra las llamadas a herramientas y las ediciones. Puedes revisar las diferencias, aceptar o rechazar cambios y orientar la conversación con prompts de seguimiento.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos.
- orientarás la sesión para añadir un control del modo claro además del modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño también solicita un modo claro. En lugar de iniciar otra conversación, orientarás la sesión existente para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra los pasos del agente: archivos que explora, comandos que ejecuta y cambios que propone. Cada edición aparece en el chat con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver las llamadas a herramientas, lecturas y ediciones de archivos y comandos del terminal que solicita ejecutar.
3. Cuando proponga una edición, revisa las diferencias:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. Abre el indicador **Files changed** de la parte superior del chat para consultar todos los archivos modificados en la sesión.

> [!NOTE]
> Si una llamada a una herramienta necesita permiso, por ejemplo, para ejecutar un comando del terminal, el agente se detendrá y mostrará **Allow**. Aprueba solo los comandos que hayas revisado y aceptes.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final que resuma los cambios.

## Pide al agente que añada el modo claro

Ahora ampliarás la misma conversación para añadir un control del modo claro. El agente conservará el contexto de su trabajo anterior, incluidos los componentes y estilos que modificó y el patrón de almacenamiento local utilizado.

1. Envía este prompt de seguimiento en Copilot Chat:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente revisará los cambios anteriores y propondrá los cambios necesarios para el modo claro.
3. Revisa cada edición y selecciona **Keep** o **Undo**, como hiciste con el modo de alto contraste.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar los controles de una forma que no te convence—, deténlo con el botón **Stop** junto al campo de chat y envía un mensaje para aclarar lo que necesitas.

> [!TIP]
> Es más rápido orientar una sesión en curso que empezar de nuevo. Utiliza prompts de seguimiento para pedir ajustes, corregir el enfoque o ampliar el alcance; por ejemplo, solicitar que se utilicen variables de hojas de estilo en cascada (CSS), integrar el modo claro en el panel existente o añadir atajos de teclado.

## Revisa los cambios

Antes de confirmar el trabajo, revisa todos los archivos modificados por el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Debería incluir componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que se siguen las pautas de accesibilidad del agente personalizado: atributos Accessible Rich Internet Applications (ARIA), navegación con teclado, lenguaje de marcado de hipertexto (HTML) semántico y persistencia en almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [el siguiente ejercicio][next-lesson].

## Resumen y pasos siguientes

Has utilizado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder contexto.

En este ejercicio:

- has supervisado el trabajo de un agente en tiempo real.
- has aceptado y rechazado ediciones de archivos.
- has orientado una sesión en curso con prompts de seguimiento.

A continuación, publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios (PR) y revisarás los cambios del agente local personalizado durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Utilizar el modo agente][agent-mode]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Ejercicio siguiente: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
