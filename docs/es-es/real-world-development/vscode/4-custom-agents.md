---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes de inteligencia artificial (IA) especializados en tareas o ámbitos concretos del desarrollo. Sus archivos Markdown, guardados en `.github/agents`, proporcionan instrucciones, prácticas recomendadas, patrones de codificación y conocimientos específicos. Los equipos pueden convertir su experiencia en agentes reutilizables, como un agente de accesibilidad que aplique las [Pautas de Accesibilidad para el Contenido Web (WCAG)][wcag], uno de seguridad o uno que mantenga patrones de pruebas coherentes.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` o, para uso global, de `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML con, al menos, los campos `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones.

### Diferencias entre los agentes personalizados y las habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se definen principalmente mediante archivos Markdown y explican a la IA cómo realizar tareas. Una forma sencilla de distinguirlos: el **agente personalizado** es quien realiza el trabajo y las **habilidades** son sus herramientas.

Los agentes tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes. En este taller, el agente de accesibilidad revisará y actualizará el sitio según las pautas de accesibilidad; para ello, podría utilizar habilidades de flujo de PR o de ejecución y gestión de pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Prueba e itera para encontrar la que mejor se adapte a tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas. Los comentarios de usuarios indican que algunas personas tienen dificultades para leer el tema oscuro debido al bajo contraste entre el texto y el fondo. El equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar. Utilizarás un agente personalizado para implementar esta función.

En este ejercicio:

- revisarás el agente personalizado de accesibilidad existente.
- utilizarás el agente de accesibilidad en Copilot Chat para implementar el modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya hay un agente personalizado de accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

1. Vuelve a tu Codespace y abre un terminal.
2. Cambia a una rama nueva basada en `main` para mantener separada la solicitud de incorporación de cambios (PR) de filtrado del ejercicio 3:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

3. Abre `.github/agents/accessibility.md`.
4. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

5. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores habituales que conviene evitar.

## Utiliza el agente personalizado en Copilot Chat

VS Code muestra los agentes personalizados definidos en `.github/agents` en el menú de agentes de Copilot Chat. Puedes seleccionar un agente para aplicar sus instrucciones y herramientas a la sesión.

> [!TIP]
> **Abre Copilot Chat**
>
> Vuelve a tu Codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; los pasos correspondientes indicarán qué seleccionar.

1. En el menú de agentes de la vista Chat, confirma que **Agent** está seleccionado.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Abre el menú de agentes de la parte inferior de la vista Chat. De forma predeterminada, muestra el agente activo, **default**.
3. Selecciona **Accessibility agent** en la lista.
4. Envía este prompt:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y las diferencias para que puedas revisarla.
6. Observa cómo se actualiza el indicador **Files changed** con los archivos modificados por el agente.

> [!NOTE]
> El proceso puede tardar varios minutos. Copilot editará archivos existentes, como componentes Astro, CSS y pruebas relacionadas.

Revisarás y orientarás este trabajo en curso en el siguiente ejercicio.

## Resumen y pasos siguientes

Has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes especializados en tareas y ámbitos concretos. Permiten convertir la experiencia y los estándares del equipo en agentes reutilizables.

En este ejercicio:

- has revisado cómo se definen los agentes personalizados.
- has utilizado un agente personalizado en el modo agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson], revisarás los cambios mientras se realizan y añadirás un control para el modo claro a la misma sesión.

## Recursos

- [Información sobre los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el entorno de desarrollo integrado (IDE)][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados de Awesome Copilot][awesome-copilot-agents]

---

| [← Ejercicio anterior: Probar la funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Ejercicio siguiente: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
