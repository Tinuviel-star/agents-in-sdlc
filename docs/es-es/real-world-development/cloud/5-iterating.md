---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Has pedido a GitHub Copilot que documente el código, añada una sección de juegos relacionados y desarrolle funciones de accesibilidad con modos de alto contraste y claro. Ahora revisarás los cambios y, si hace falta, pedirás al agente que los mejore.

### Escenario

La inteligencia artificial (IA) generativa no cambia los fundamentos del diseño de software y DevOps. Hay que revisar el código generado y seguir el proceso habitual. Revisarás las sugerencias de Copilot para la documentación, los juegos relacionados y las funciones de accesibilidad antes de activar la revisión del equipo.

## Seguridad y agente de GitHub Copilot en la nube

El agente trabaja de forma asíncrona y sin supervisión, por lo que se aplican restricciones de seguridad:

- Copilot tiene acceso de lectura al repositorio y acceso de escritura **solo** a la rama que utiliza para el código.
- El agente se ejecuta en GitHub Actions, dentro de un entorno temporal e independiente.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de ejecutarlos.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empezarás por la solicitud de incorporación de cambios (PR) de Copilot que añade documentación al código. La revisarás desde la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Es posible que la PR muestre un aviso de que un firewall bloquea a Copilot. Es normal: el acceso a recursos externos, incluidas las llamadas a servidores MCP externos, está limitado de forma predeterminada. Si quieres, puedes [configurar o desactivar el firewall del agente en la nube][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la PR titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot todavía está trabajando, la PR tendrá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la PR esté lista, selecciona **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Examina el código actualizado, que incluirá comentarios de documentación TSDoc. Los cambios concretos pueden variar. Comprueba los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentario: proceden de los archivos de instrucciones que revisaste en el ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos de trabajo aparezcan en las comprobaciones de la PR y que las comprobaciones del proyecto para la aplicación Astro finalicen correctamente. Esto puede tardar unos minutos.

## Solicita cambios a GitHub Copilot

Puedes mencionar a Copilot en comentarios de la PR o en comentarios insertados en el código para pedirle cambios. Copilot iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no hay un prompt único. Por ejemplo, puedes pedirle que:

- añada encabezados de comentario al principio de cada archivo de código.
- añada comentarios de documentación TSDoc a los archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario que solicite un cambio en la documentación generada y menciona a **@copilot** como harías con otra persona. Utiliza una de las ideas anteriores u otra petición relacionada con la documentación.
2. Selecciona **View Session** para observar cómo Copilot inicia una nueva sesión y realiza los cambios.
3. Para volver a la PR, puedes seleccionar **Back to pull request**.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, comprueba que se haya añadido un commit a la PR.
5. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Vuelve a la PR que Copilot creó para mostrar juegos relacionados en la página de detalles.

1. Abre tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** y revisa el código generado.
5. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Comprueba si hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Comprueba que los flujos de trabajo aparezcan en las comprobaciones de la PR y que las comprobaciones del proyecto para la aplicación Astro finalicen correctamente.
9. **Opcional:** cambia a la rama de esta PR en tu Codespace para probar manualmente la funcionalidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la funcionalidad de juegos relacionados. Puedes seguir pidiéndole cambios mediante comentarios en la pestaña **Conversation**. Por ejemplo, puedes ajustar la cantidad de juegos relacionados o solicitar encabezados de comentario y comentarios TSDoc. Recuerda que esta incidencia se asignó antes de actualizar las instrucciones personalizadas.

## Revisa las funciones de accesibilidad

Por último, revisarás la PR con las funciones de accesibilidad implementadas por el agente personalizado. Debe incluir tanto el modo de alto contraste del ejercicio 3 como el modo claro que solicitaste en el centro de control del ejercicio 4.

1. Abre tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot todavía está trabajando, la PR tendrá la etiqueta **[WIP]**. Espera unos minutos a que termine.

4. Selecciona **Files changed** y revisa el código generado.
5. Examina la implementación y presta atención a:
   - los controles para cambiar de modo.
   - el uso del almacenamiento local para guardar las preferencias.
   - los cambios de hojas de estilo en cascada (CSS) para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como las etiquetas Accessible Rich Internet Applications (ARIA) y la navegación con teclado.
   - el código JavaScript/TypeScript que gestiona los cambios de modo.
6. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos de trabajo aparezcan en las comprobaciones de la PR y que las comprobaciones del proyecto para la aplicación Astro finalicen correctamente.
10. **Opcional:** cambia a la rama de la PR en tu Codespace y prueba manualmente las funciones de accesibilidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Después, inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador. Comprueba que funcionen y que las preferencias se mantengan al volver a cargar la página.

El agente personalizado de accesibilidad ayudó a orientar a Copilot para que implementara estas funciones siguiendo las prácticas recomendadas. Si encuentras algún problema o mejora de accesibilidad, menciona a **@copilot** en un comentario para solicitar cambios.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil para delegar tareas reales del backlog y ocuparte de otras cosas mientras trabaja. Para practicar:

- crea un formulario de interés para patrocinadores en la página de detalles de un juego.
- implementa la paginación en la página de la lista de juegos.
- añade validación de entradas y gestión de errores a los métodos de acceso a datos.

## Resumen

Has completado el entorno del agente en la nube. En estos ejercicios:

- has inspeccionado las instrucciones personalizadas del repositorio para ver cómo influyen en el trabajo del agente.
- has asignado incidencias al agente de Copilot en la nube y observado cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- has creado y utilizado un agente personalizado de accesibilidad para añadir los modos de alto contraste y claro.
- has utilizado la página de agentes de Copilot para supervisar y orientar la sesión de accesibilidad.
- has revisado e iterado sobre las PR del agente y has aprobado los flujos de trabajo.

## Revisión y pasos siguientes

Has completado el entorno del agente en la nube. Para seguir explorando, puedes probar otros entornos:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración MCP en el IDE.
- 💻 **[Entorno de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con Plan y Autopilot, habilidades de agente, un agente QA personalizado, Playwright MCP, Agent Merge y la opción de delegar al agente en la nube con `/delegate`.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Información sobre los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente de Copilot en la nube][agent-firewall]

---

| [← Ejercicio anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
