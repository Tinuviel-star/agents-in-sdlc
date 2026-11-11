---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Requisitos previos][previous-lesson] · [Ejercicio siguiente: Modo agente →][next-lesson]

El contexto es fundamental al trabajar con inteligencia artificial (IA) generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información adicional, debes proporcionarle ese contexto. Los [archivos de instrucciones][instruction-files] permiten orientar la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las pautas de codificación y los estándares de documentación mediante las instrucciones del repositorio y las instrucciones específicas para determinadas rutas.
- generarás la primera parte de los datos para el filtrado (un helper de editores) con las instrucciones actuales.
- añadirás un estándar general a `.github/copilot-instructions.md`.
- repetirás el prompt y observarás cómo el código generado sigue el estándar nuevo.
- confirmarás los cambios de instrucciones y filtrado en `main` para que Copilot pueda utilizar las instrucciones actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares: Copilot no es determinista. El objetivo es observar la tendencia tras actualizar las instrucciones, no obtener siempre un resultado idéntico.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene pautas y requisitos para el desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz debe utilizar un tema oscuro y un diseño moderno.
- El código debe incluir comentarios de documentación de TypeScript (TSDoc).
- Cada archivo debe empezar con un bloque de comentarios que explique su función.

Utilizarás archivos de instrucciones para proporcionar a Copilot la información necesaria y ayudarle a seguir estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor el estilo y los requisitos de codificación. Puedes especificar convenciones, bibliotecas y comentarios preferidos, tanto para todo el repositorio como para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe incluir información general del proyecto, como la pila tecnológica, una descripción del trabajo, prácticas recomendadas y otras pautas globales.
- `.github/instructions/*.instructions.md`, archivos que ofrecen pautas para lenguajes, tipos de archivos o tareas específicos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En un IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para las finalizaciones de código ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que tienen frontmatter `applyTo`.
>
> Copilot [admite otros formatos de instrucciones][custom-instructions-support], incluidos AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

El proyecto de ejemplo muestra una forma representativa de organizar las instrucciones:

- Mantén `copilot-instructions.md` centrado en pautas generales, como la descripción y estructura del proyecto y los estándares de codificación globales.
- Utiliza archivos `*.instructions.md` para tipos de archivo, como pruebas unitarias, componentes de Astro y la capa de datos, o para tareas concretas.
- Escribe en lenguaje natural. Expresa las pautas con claridad e incluye ejemplos de código correctos e incorrectos.

No hay una única forma de crear archivos de instrucciones ni de utilizar la IA. La experimentación te ayudará a encontrar la opción adecuada para el proyecto.

> [!TIP]
> Los proyectos con GitHub Copilot deben tener un conjunto sólido de instrucciones. En este proyecto encontrarás archivos para tareas como [actualizar la interfaz][ui-instructions] y trabajar con [Astro][astro-instructions].
>
> Copilot también puede generar archivos de instrucciones. Por ejemplo, VS Code ofrece **Configure Chat → Generate Agent Instructions** y Copilot CLI ofrece `/init`. El ejercicio correspondiente explicará cómo utilizar esta función en cada entorno.
>
> Para obtener plantillas o un punto de partida, explora [Awesome Copilot][awesome-copilot], un repositorio con instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizados del proyecto

Lee los archivos incluidos en el repositorio: el archivo principal `copilot-instructions.md` y varios `*.instructions.md` para tareas diferentes. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Lee la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las pautas anidadas de **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Explora la carpeta `.github/instructions`. Hay instrucciones para Astro, la capa de datos de Drizzle, pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa `applyTo`, que define un patrón glob relativo a la raíz del repositorio. En este caso, se aplica a archivos de pruebas TypeScript como `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones y a archivos existentes. Las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares del proyecto, pero todavía no exige documentación en el código. En los pasos siguientes añadirás pautas para comentarios TSDoc y encabezados de comentario.

## Crea una rama para los cambios

Crea una rama para trabajar con prácticas de control de versiones habituales.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Utiliza Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve a tu Codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; los pasos correspondientes indicarán qué seleccionar.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un prompt con las instrucciones actuales. Más adelante, actualizarás las instrucciones y enviarás el mismo prompt para comparar el resultado.

1. Cierra las pestañas abiertas del editor para limitar el contexto que recibe Copilot.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe guardarse el helper.
3. En la vista Chat, selecciona **Agent** en el menú de agentes para que Copilot pueda aplicar cambios.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará los cambios, normalmente al archivo del helper y sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editores. Esto sigue las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que el código propuesto **no incluye** comentarios TSDoc ni un encabezado de comentario para el archivo.

> [!CAUTION]
> Como Copilot no es determinista, puede añadir comentarios aunque no se lo pidas. No pasa nada; el objetivo es observar una mejora de coherencia después de actualizar las instrucciones.

## Añade un estándar para todo el repositorio

El archivo `.github/copilot-instructions.md` proporciona pautas generales a Copilot. Actualiza los estándares de codificación para orientar mejor las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que todavía no exige documentación en el código.
3. Debajo de los estándares existentes, añade:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Las instrucciones generales del proyecto se guardan en `.github/copilot-instructions.md`; las pautas para lenguajes, tipos de archivos y tareas concretos se guardan en archivos `*.instructions.md`. El archivo general es el lugar adecuado para estándares como los comentarios de documentación.

## Repite el prompt y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para limpiar el contexto.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía el **mismo prompt**:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentario parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta incluye ahora un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo y lo aplique al código que utilizarás en el ejercicio siguiente.

## Confirma los cambios y publica la rama

Los archivos de instrucciones, como cualquier otro archivo, se gestionan mediante el control de código fuente. Confirma los cambios y publícalos en el repositorio.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Confirma que las instrucciones y los cambios del helper están presentes:

   ```bash
   git status
   ```

3. Prepara y confirma las instrucciones y todos los archivos modificados por Copilot para crear el helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Con la rama publicada, crea una PR asociada a la incidencia del backlog sobre la actualización de las instrucciones. Copilot puede localizar la incidencia y crear la PR.

1. Abre Copilot Chat en tu Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que encuentre la incidencia y cree una PR desde la rama actual, indicando que incluye las instrucciones y el helper de editores:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot te pida permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** si estás de acuerdo.
6. Cuando se cree la PR, pídele que la combine y vuelva a `main`:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando Copilot te pida permiso para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** si estás de acuerdo.

Has creado y combinado una PR con ayuda de GitHub Copilot.

## Resumen y pasos siguientes

Has explorado cómo Copilot utiliza el contexto de los archivos de instrucciones y, después, has utilizado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones existentes.
- añadir un estándar general a `.github/copilot-instructions.md`.
- repetir el prompt y observar cómo el código propuesto sigue el estándar nuevo.
- confirmar las instrucciones y el helper en `main` para que el siguiente ejercicio pueda aprovecharlos.

A continuación, aplicarás las instrucciones en [modo agente][next-lesson] mientras Copilot desarrolla una funcionalidad en varias partes del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
