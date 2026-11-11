---
title: "Lección 6 - Validar la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y explora la funcionalidad de filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos señalado, programar implica mucho más que escribir código. También hay que trabajar con datos y servicios externos, y permitir que Copilot utilice otras automatizaciones. Para ello sirven los servidores MCP, que amplían Copilot CLI con herramientas y servicios adicionales.

En esta lección:

- aprenderás qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI.
- añadirás el servidor MCP de Playwright si todavía no está disponible.
- pedirás al agente que controle un navegador para explorar la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz hay que interactuar con ella. Permitirás que Copilot utilice el sitio como lo haría una persona y automatice más comprobaciones para aumentar la confianza en el resultado.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] ofrece a los agentes de inteligencia artificial (IA) una forma de comunicarse con herramientas y servicios externos. Así pueden acceder a información actualizada y realizar acciones en tu nombre.

Estas herramientas y recursos se ofrecen mediante servidores MCP, que conectan el agente de IA con las herramientas y los servicios externos. Cada servidor proporciona un conjunto distinto de herramientas y recursos al agente.

Algunos servidores MCP conocidos son:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a las API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: proporciona funciones de automatización del navegador mediante Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP][mcp-registry] para facilitar la búsqueda y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizar uno, revisa su código fuente, comprueba quién lo publica y considera las implicaciones de seguridad.

## Añade el servidor MCP de Playwright

Si el servidor MCP de Playwright todavía no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría un usuario.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve a tu Codespace.
2. En Copilot CLI, abre el cuadro de diálogo para añadir un servidor MCP con este comando:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** con <kbd>Enter</kbd> y, después, pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo MCP.

## Pide a Copilot que explore la funcionalidad con Playwright

Antes comprobaste manualmente el comportamiento de la funcionalidad. Ahora utilizarás el servidor MCP de Playwright disponible en Copilot para repetir la comprobación.

1. Envía este prompt para pedir a Copilot que utilice el servidor MCP de Playwright:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> No es imprescindible pedirle explícitamente a Copilot que utilice el servidor MCP; normalmente lo detecta por sí solo. Indicar la herramienta deseada puede ayudar a que los resultados sean más coherentes y ahorrar tokens.

2. Observa los pasos que ejecuta Copilot en el navegador para comprobar que la funcionalidad funciona.
3. Lee el informe y confirma que todo se comporta como esperas.

Copilot iniciará el servidor, utilizará Playwright para interactuar con el sitio, detendrá el servidor y te entregará un informe.

## Resumen y pasos siguientes

Has utilizado el servidor MCP de Playwright para explorar la funcionalidad en un navegador real desde Copilot CLI. En esta lección:

- has aprendido qué es MCP y cómo lo utiliza Copilot CLI.
- has añadido el servidor MCP de Playwright si todavía no estaba disponible.
- has pedido al agente que controle un navegador para explorar la funcionalidad de filtrado.

A continuación, [crearás un agente personalizado de control de calidad (QA)][next-lesson] que combinará la habilidad y las herramientas del navegador en una función especializada.

## Recursos

- [Qué es MCP y por qué se habla tanto de él][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
