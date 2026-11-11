---
title: "Lección 8 - Crear y combinar la solicitud de incorporación de cambios (PR) de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la actualización de la habilidad de control de calidad (QA) y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las instrucciones actualizadas, la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Ahora revisarás todos los cambios y abrirás una solicitud de incorporación de cambios. Combinaste manualmente la PR de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Lo habitual sería separar la funcionalidad, las instrucciones, la actualización de la habilidad y el agente QA en varias PR. Para simplificar el taller, has mantenido todo el trabajo de filtrado y calidad en una misma conversación y rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el proceso de combinación.
- inspeccionarás todos los cambios de la funcionalidad y las pruebas de verificación.
- crearás la PR de filtrado.
- habilitarás Agent Merge solo después de revisar los cambios y confirmarás que se ha combinado la PR.

## Escenario

Durante el trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante necesario para integrar una solicitud de incorporación de cambios. Cuando lo habilitas, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y reorganiza la rama cuando es necesario. Después, habilita la combinación automática de GitHub si la configuración del repositorio lo permite.

Hasta ahora seleccionabas **Merge pull request** personalmente. Agent Merge puede encargarse de esa tarea. Revisa el trabajo y confirma que está listo antes de habilitar Agent Merge.

## Utiliza Agent Merge para gestionar la PR

Con el código terminado, revisarás los cambios, crearás la PR y permitirás que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente con `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo termine correctamente, realizará la combinación.
7. Confirma que Copilot muestre un mensaje parecido a `PR #14 was squash-merged successfully.`

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si el proceso se bloquea, lee el motivo indicado y completa manualmente la combinación revisada cuando la configuración del repositorio lo permita.

## Resumen y pasos siguientes

Has automatizado varias partes del desarrollo: la generación de código, las pruebas y la validación, y ahora el proceso de la PR. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el proceso de combinación.
- has inspeccionado todos los cambios de la funcionalidad y las pruebas de verificación.
- has creado la PR de filtrado.
- has habilitado Agent Merge después de revisar los cambios y has confirmado que se combinó la PR.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Recursos

- [Gestión de solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
