---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Muchas organizaciones tienen deuda técnica: problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidad relegadas en el backlog por falta de tiempo. El agente de GitHub Copilot en la nube puede actualizar código y añadir funcionalidades de forma autónoma. Al terminar, crea una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así puedes delegar tareas tediosas y centrarte en el trabajo de mayor alcance.

Explorarás cómo utilizar el agente en la nube para:

- personalizar el entorno de generación de código.
- asegurarte de que las operaciones se realicen de forma segura.
- comprender la importancia de delimitar bien las incidencias.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica. El equipo quiere añadir comentarios de documentación de TypeScript (TSDoc) a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para poder evaluar la experiencia de usuario (UX). No hace falta una implementación definitiva, pero el trabajo está bloqueado y hay otras incidencias con mayor prioridad.

Son tareas que suelen quedar relegadas y que se pueden asignar al agente de Copilot en la nube. El agente trabaja de forma asíncrona mientras tú te ocupas de otras tareas; después puedes revisar el resultado y comprobar que cumple los requisitos.

## Introducción al agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Puedes, por ejemplo, [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). El agente crea una PR en borrador para mostrar su progreso, prepara el entorno y empieza a trabajar. Puedes consultar la sesión mientras se ejecuta o después de que termine. Cuando esté lista la solución propuesta, te mencionará en la PR para que la revises.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no elimina la necesidad de pensar. Incluso las tareas aparentemente sencillas pueden ser complejas; por eso, [delimita con cuidado las tareas para el agente en la nube][cloud-agent-best-practices]. Trátalo como un compañero de programación con inteligencia artificial (IA): trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al incorporar IA generativa.

## Instrucciones personalizadas del repositorio

Los ejercicios anteriores explicaron cómo las instrucciones personalizadas orientan a Copilot. Si todavía no has trabajado con ellas, revisa los archivos incluidos en el repositorio antes de asignar tareas al agente. Así podrás observar su efecto en las PR.

Abre estos archivos en la interfaz web de GitHub o en un Codespace:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica las instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente la incidencia `Code lacks documentation`, fíjate en los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentario de la PR. Estas pautas proceden de los archivos de instrucciones que has revisado.

## Configura el entorno de desarrollo del agente

Para desarrollar código, normalmente se necesita un entorno específico y scripts de configuración. Lo mismo ocurre con las tareas que asignas a Copilot: trabaja de forma parecida a un ingeniero de software.

El agente utiliza [GitHub Actions][github-actions] como entorno. Puedes personalizarlo con un [flujo de trabajo de configuración][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece el trabajo. Este flujo de trabajo ya está configurado para instalar Node.js y las dependencias del proyecto, los navegadores de Playwright para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Define los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para las operaciones que realice.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no clonas el repositorio en esos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la interfaz y la aplicación: Node.js (toda la aplicación utiliza Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos iniciales en SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero hay dos aspectos importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`, que GitHub Actions ejecuta antes de que Copilot empiece a trabajar en la PR.
- El activador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añade documentación

La documentación desactualizada o inexistente es una forma de deuda técnica que ralentiza el trabajo y dificulta el mantenimiento y la incorporación de desarrolladores. Copilot puede ayudar a crear documentación; esta es una buena tarea para asignar al agente en la nube y revisar más adelante.

1. En una nueva pestaña del navegador, abre tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en **Title**.
6. En **Description**, escribe:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecciona **Create**.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

   ![Detalles de la asignación de Copilot a una incidencia](../../../_images/ex4-assign-copilot-details.png)
10. Selecciona la pestaña **Pull Requests**.
11. Abre la PR nueva, que tendrá un título parecido a `[WIP]: Code lacks documentation`. Si todavía no aparece, espera un momento y actualiza la página.
12. Espera a que Copilot cree una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer mientras Copilot prepara el entorno, ejecuta el flujo de trabajo descrito y analiza el proyecto.

13. Revisa la lista y las tareas previstas.
14. Desplázate por la cronología de la PR y comprueba que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que debas actualizar la ventana para ver el indicador actualizado.

16. Recorre la sesión en directo para observar cómo Copilot explora el código, analiza su estado, elabora un plan y crea código.

El trabajo puede tardar varios minutos. El agente en la nube está diseñado para trabajar de forma asíncrona, así que mientras se ocupa de la tarea de documentación, le asignarás otra tarea y podrás centrarte en otras actividades.

## Añade una sección de juegos relacionados

Mientras el agente trabaja, asígnale otra tarea: añadir una sección de juegos relacionados a la página de detalles de cada juego para que el equipo de diseño pueda evaluar la experiencia de usuario.

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** y, después, **Blank issue**.
4. Escribe `Show related games on the game details page` en **Title**.
5. En **Description**, escribe:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

6. Selecciona **Create**.
7. A la derecha, selecciona **Assign to Copilot**.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

8. Selecciona **Assign**.

Poco después, aparecerá 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando en ella.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

El agente ya trabaja en la segunda solicitud. No es necesario supervisar continuamente su trabajo; puedes revisar el resultado cuando termine. Mientras tanto, continuarás con los agentes personalizados.

## Resumen y pasos siguientes

Has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona y se ocupe de deuda técnica, desarrolle funcionalidades o ayude a migrar código entre frameworks.

En este ejercicio:

- has personalizado el entorno de generación de código.
- has comprobado cómo se protegen las operaciones.
- has visto la importancia de delimitar claramente las incidencias.
- has asignado incidencias a Copilot.

Mientras el agente trabaja, crearás y utilizarás agentes personalizados. El agente en la nube también puede utilizar servidores Model Context Protocol (MCP) y acceder a las instrucciones personalizadas que has explorado.

## Recursos

- [Información sobre el agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Ejercicio siguiente: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
