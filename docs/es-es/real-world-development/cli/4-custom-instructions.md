---
title: "Lección 4 - Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental al trabajar con inteligencia artificial (IA) generativa. Si una tarea debe realizarse de una forma concreta, conviene proporcionar esa orientación a Copilot. Los [archivos de instrucciones][instruction-files] describen tanto *qué* código quieres como *cómo* debe estructurarse. Ahora que has desarrollado el filtrado, explorarás las instrucciones utilizadas por Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas para determinadas rutas.
- actualizarás el archivo de instrucciones para asegurar que se siguen los estándares de codificación.
- observarás el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys tiene pautas y requisitos para las prácticas de desarrollo:

- Los comentarios deben explicar la intención y las decisiones que no resulten evidentes, no repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos con comentarios de documentación para TypeScript (TSDoc) y JavaScript (JSDoc); también deben documentar el argumento `db` inyectable cuando exista.
- Los componentes reutilizables de Astro deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Se deben conservar las pautas de formato y lint existentes.

Utilizarás archivos de instrucciones para proporcionar a Copilot la información necesaria y ayudarle a seguir estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo y requisitos de codificación. Permiten orientar las sugerencias y los fragmentos de código. Puedes especificar las convenciones de codificación, las bibliotecas y los comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto que sea pertinente para la mayoría de las solicitudes.
- Los archivos `.github/instructions/*.instructions.md`, que proporcionan pautas para determinados lenguajes, tipos de archivos o tareas.

> [!NOTE]
> Los formatos de instrucciones adicionales y su compatibilidad varían según el entorno. Consulta la [referencia de compatibilidad de las instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones personalizadas del proyecto

El proyecto de ejemplo ya incluye algunos archivos de instrucciones. Revísalos antes de hacer cambios para observar su efecto.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Lee la breve descripción del proyecto y las pautas de codificación. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Explora los archivos de `.github/instructions`. Hay instrucciones para archivos de Astro, la capa de datos de Drizzle, las pruebas y otros temas.
5. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior, que define un patrón glob para determinar a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y observa los enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos reutilizables más pequeños y señalar ejemplos que Copilot debe seguir.

## Actualiza los archivos de instrucciones según las pautas del equipo

Los archivos existentes son un buen punto de partida, pero falta una pauta. Modificarás el archivo principal `copilot-instructions.md` para pedir que se añadan comentarios TSDoc al TypeScript nuevo.

1. En `.github/copilot-instructions.md`, localiza la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade este elemento al final de la sección:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplica las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar las instrucciones, sal de la conversación de filtrado y reanúdala para que Copilot cargue las pautas nuevas sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las pautas actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los comentarios TSDoc nuevos y confirma que describen el código con precisión.

## Resumen y pasos siguientes

Has explorado cómo Copilot utiliza el contexto de los archivos de instrucciones y has aplicado un estándar nuevo a la funcionalidad. En esta lección:

- has explorado cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas para determinadas rutas.
- has actualizado el archivo de instrucciones para asegurar que se siguen los estándares de codificación.
- has observado el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para comprobar que las pruebas y lint se ejecutan de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas a Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
