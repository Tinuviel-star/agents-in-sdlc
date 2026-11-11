---
title: "Ejercicio 2 - Añadir funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios sencillos suelen implicar editar varios archivos y ejecutar pruebas. Como desarrollador, normalmente tienes que localizar los archivos, hacer los cambios, probarlos, depurar y comprobar si falta algo. El modo agente de Copilot puede explorar el proyecto, actualizar varios archivos, ejecutar tareas como las pruebas y corregir errores que detecte. Utilízalo para añadir una funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite ciclos de respuesta rápidos. El agente en la nube trabaja de forma asíncrona con incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- utilizar el modo agente de Copilot para explorar el proyecto, localizar archivos pertinentes y coordinar cambios.
- implementar funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados antes de combinarlos.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editor. En el ejercicio anterior ya añadiste un helper de editores. Ahora completarás con el modo agente de Copilot el resto del trabajo de datos, interfaz y pruebas.

## Ejecuta el sitio de Tailspin Toys

Antes de hacer cambios, explora el sitio para conocer sus funciones actuales.

Tailspin Toys es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite mostrar listas de juegos y sus detalles. El sitio es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM; no hay una interfaz de programación de aplicaciones (API) de backend independiente ni un framework de interfaz del lado del cliente. Los helpers reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se implementa con elementos `<script>` de Astro acotados que utilizan las interfaces estándar del Modelo de Objetos del Documento (DOM).

### Inicia el sitio

El proyecto incluye la tarea `npm run dev`, que inicia el único servidor de desarrollo Astro.

1. Vuelve a tu Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en Codespaces con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor Astro esté listo, aparecerá un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Para abrir el sitio, mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar el enlace localhost desde el terminal se abrirá `https://<your-codespace-name>-4321.app.github.dev/`, un túnel privado a tu Codespace, que ahora aloja el servidor web.

### Explora el sitio

Las funciones principales son:

- **Home Page**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Game Details Page**: al seleccionar un juego, abre su página de detalles con información como el título, la descripción, el editor y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve a tu Codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; los pasos correspondientes indicarán qué seleccionar.

El sitio ya funciona, pero quieres añadir más funciones. Revisa el backlog de incidencias que se creó automáticamente a partir de la plantilla.

1. En el menú de agentes de la vista Chat, selecciona **Agent**. El agente puede planificar e implementar cambios en varios archivos, ejecutar comandos del terminal e invocar herramientas.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> La selección del modelo no expresa una preferencia de los autores. Se utilizó Claude Sonnet 4.5 al crear el taller para ofrecer resultados relativamente coherentes, pero las sugerencias pueden variar porque los modelos de lenguaje grandes (LLM) son probabilísticos.

> [!NOTE]
> Copilot puede utilizar un comando distinto del Model Context Protocol (MCP), pero debería completar la tarea.

3. Envía este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que Copilot enumere las incidencias.
5. Revisa la lista generada.

Copilot también puede priorizar las incidencias según la utilidad que considere que tienen para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones de interfaz de usuario (UI), que proporciona contexto sobre cómo añadir funciones al sitio.

1. En tu Codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las pautas, que incluyen:
   - una descripción de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a instrucciones para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones, lo que permite reutilizar pautas y ejemplos.

## Implementa el filtrado

Para completar el filtrado, tendrás que actualizar la lógica de datos en `src/lib/`, añadir o ajustar las pruebas y actualizar la página de lista de juegos para incluir la interfaz de filtrado. Copilot puede encargarse de estas tareas; ejecutará las pruebas antes de que combines los cambios.

1. Continúa la conversación actual o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Selección del archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que sigue seleccionado el modelo **Claude Sonnet 4.5**.
5. Envía este prompt para implementar los requisitos de la incidencia relacionada:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto, localiza los helpers de datos, la interfaz y las pruebas, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot puede tardar varios minutos en explorar, editar y ejecutar pruebas. Mientras trabaja, es posible que aparezcan los botones **Keep** y **Undo**. Cuando termine, aparecerá una opción para conservar o deshacer cada cambio; no hace falta seleccionar nada mientras el agente sigue trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat para confirmar la ejecución de pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas o que fallen algunas pruebas mientras Copilot itera entre la generación de código y las pruebas.
9. Examina el código generado para detectar posibles problemas.

   ![Sesión completada de Copilot en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de inteligencia artificial (IA).

10. Vuelve al navegador donde se ejecuta el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y pasos siguientes

Has utilizado el modo agente de GitHub Copilot para añadir funciones al sitio de Tailspin Toys. En este ejercicio has aprendido a:

- utilizar el modo agente para explorar el proyecto, localizar archivos pertinentes y coordinar cambios.
- implementar funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados antes de combinarlos.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios (PR).

### Ejercicio de exploración adicional: implementar la paginación

La lista de juegos puede crecer y necesitar paginación. Utiliza lo aprendido para pedirle a Copilot que la implemente:

- sigue las prácticas recomendadas y utiliza los archivos de instrucciones existentes.
- decide si el usuario podrá elegir el tamaño de página o si será fijo.
- incluye en el prompt las pautas necesarias para la implementación deseada.
- itera con Copilot y proporciona más contexto cuando haga falta; es parte del flujo de trabajo habitual.

## Recursos

- [Modos Ask, Edit y Agent de Copilot: usos recomendados][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
