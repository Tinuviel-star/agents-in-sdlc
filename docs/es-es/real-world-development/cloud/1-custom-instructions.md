---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Requisitos previos][previous-lesson] · [Ejercicio siguiente: Agente de Copilot en la nube →][next-lesson]

El contexto es fundamental al trabajar con inteligencia artificial (IA) generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información previa, asegúrate de que pueda acceder a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan orientación para que Copilot entienda tanto *qué* debe hacer como *cómo* debe hacerlo.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las pautas de codificación y los estándares de documentación mediante las instrucciones del repositorio y las instrucciones específicas para determinadas rutas.
- añadirás un estándar general al archivo `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los entornos de VS Code y de la interfaz de línea de comandos (CLI), aquí no compararás los resultados de un prompt antes y después del cambio. El agente de GitHub Copilot en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil demostrar su efecto en tiempo real. Verás cómo influyen las instrucciones cuando revises las solicitudes de incorporación de cambios (PR) que genere el agente en ejercicios posteriores.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene pautas y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz debe utilizar un tema oscuro y un diseño moderno.
- El código debe incluir comentarios de documentación de TypeScript (TSDoc).
- Cada archivo debe empezar con un bloque de comentarios que explique su función.

Utilizarás archivos de instrucciones para proporcionar a Copilot la información necesaria y ayudarle a seguir estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de codificación y tus requisitos. Puedes especificar convenciones, bibliotecas y comentarios preferidos, tanto para todo el repositorio como para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto pertinente para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción del proyecto, prácticas recomendadas y otras pautas generales.
- `.github/instructions/*.instructions.md`, archivos que ofrecen pautas para lenguajes, tipos de archivos o tareas específicos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En un entorno de desarrollo integrado (IDE), los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para las finalizaciones de código ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que tienen frontmatter `applyTo`.
>
> Copilot [admite otros formatos de archivos de instrucciones][custom-instructions-support], incluidos AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

El proyecto de ejemplo muestra una forma representativa de organizar las instrucciones:

- Mantén `copilot-instructions.md` centrado en pautas generales, como la descripción y estructura del proyecto y los estándares de codificación globales.
- Utiliza archivos `*.instructions.md` para tipos de archivo, como pruebas unitarias, componentes de Astro y la capa de datos, o para tareas concretas.
- Escribe en lenguaje natural. Expresa las pautas con claridad e incluye ejemplos de código correctos e incorrectos.

No hay una única forma de crear archivos de instrucciones ni de utilizar la IA. La experimentación te ayudará a encontrar la opción adecuada para tu proyecto.

> [!TIP]
> Los proyectos con GitHub Copilot deben contar con un conjunto sólido de archivos de instrucciones. En este proyecto encontrarás archivos para tareas como [actualizar la interfaz][ui-instructions] y trabajar con [Astro][astro-instructions].
>
> Copilot también puede generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El ejercicio correspondiente explicará cómo utilizarla.
>
> Para obtener plantillas o un punto de partida, explora [Awesome Copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Lee los archivos de instrucciones que incluye el repositorio: un `copilot-instructions.md` principal y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Lee la breve descripción del proyecto y las secciones **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las pautas anidadas de **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Explora la carpeta `.github/instructions`. Hay instrucciones para archivos de Astro, la capa de datos de Drizzle, pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo`, que define un patrón glob relativo a la raíz del repositorio. En este caso, se aplica a archivos de pruebas TypeScript como `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares del proyecto, pero todavía no exige documentación en el código. En los pasos siguientes añadirás las pautas para los comentarios TSDoc y los encabezados de comentario en los archivos.

## Añade un estándar para todo el repositorio

Añadirás una pauta general para incluir comentarios de documentación TSDoc y un encabezado de comentario en cada archivo. El agente en la nube aplicará esta pauta a las incidencias que le asignes más adelante.

1. En el terminal de tu Codespace, crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

El archivo `.github/copilot-instructions.md` proporciona pautas generales del proyecto. Actualizarás los estándares para mejorar las sugerencias de código.

2. Vuelve a abrir `.github/copilot-instructions.md`.
3. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que todavía no exige documentación en el código.
4. Debajo de los estándares existentes, añade estas líneas:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Guarda `copilot-instructions.md`.

> [!TIP]
> Puedes añadir instrucciones generales al repositorio en `.github/copilot-instructions.md` o instrucciones para lenguajes, tipos de archivo y tareas concretos en `*.instructions.md`. El archivo del repositorio es el lugar adecuado para un estándar general como la documentación en comentarios.

## Confirma y combina la actualización de instrucciones

El agente en la nube lee las instrucciones de la rama a la que apunta la incidencia. En el siguiente ejercicio, partirá de `main`; por eso, debes combinar los cambios en `main` para que pueda aplicarlos.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios (PR) desde `custom-instructions-cloud` a `main` y combínala. Así, el agente en la nube podrá utilizar estas instrucciones cuando le asignes incidencias en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante el taller, puedes omitir los pasos de la rama y confirmar los cambios directamente en `main`. La rama sirve para practicar un flujo de trabajo habitual en proyectos reales.

## Resumen y pasos siguientes

Has explorado cómo utiliza Copilot el contexto de los archivos de instrucciones y has añadido un estándar general al repositorio en `.github/copilot-instructions.md`. Verás cómo se aplica ese estándar en las PR que genere el agente en la nube durante el resto del taller.

A continuación, [asignarás tu primera incidencia al agente de Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
