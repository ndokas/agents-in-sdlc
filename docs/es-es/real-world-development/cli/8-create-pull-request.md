---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad, el perfil de control de calidad y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la habilidad, el perfil de control de calidad (QA) y las pruebas están guardadas en una rama. Ahora revisarás todos los cambios y abrirás una solicitud de cambios (PR). Combinaste manualmente la PR de la valoración con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> En condiciones normales, dividiríamos la funcionalidad, las actualizaciones de instrucciones, la habilidad y el agente de control de calidad en varias PR. Para agilizar el taller, hemos mantenido todo el flujo de filtrado y calidad en una conversación y una rama, y lo incluiremos en una sola PR.

En esta lección, aprenderás a:

- conocer Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- inspeccionar todos los cambios de la funcionalidad y las evidencias de verificación.
- crear la PR de filtrado.
- habilitar Agent Merge solo después de revisar la PR y confirmar que se ha combinado.

## Escenario

Durante el flujo de trabajo de filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo pendiente de la PR, sin quitar al desarrollador el control de la autorización para combinarla.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para integrar una solicitud de cambios. Cuando lo habilitas, Copilot resuelve los bloqueos de la PR —por ejemplo, corrige comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y vuelve a aplicar los cambios sobre la rama base si es necesario— y habilita la combinación automática de GitHub cuando el repositorio lo permite.

Hasta ahora has seleccionado tú mismo **Merge pull request**. Agent Merge puede encargarse de esa responsabilidad. Revisa el trabajo y confirma que está listo antes de habilitar Agent Merge.

## Usar Agent Merge para gestionar la PR

Con el código creado, revisarás los cambios, crearás la PR y dejarás que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el diálogo del agente con `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo se complete correctamente, la combinará.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos necesarios. Si se bloquea, lee el motivo indicado y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del desarrollo: generar código, probarlo y validarlo y, ahora, gestionar la solicitud de cambios. En esta lección, has:

- conocido Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- inspeccionado todos los cambios de la funcionalidad y las evidencias de verificación.
- creado la PR de filtrado.
- habilitado Agent Merge solo después de revisar la PR y confirmado que se ha combinado.

A continuación, explorarás otros [comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, las sesiones compartidas y la delegación opcional en la nube.

## Recursos

- [Gestionar solicitudes de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
