---
title: "Lección 10 - Repaso y siguientes pasos"
description: "Repasa el flujo de trabajo con Copilot CLI, las dos solicitudes de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI durante un flujo de trabajo continuo en Tailspin Toys. Has:

- preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado del backlog.
- añadido las valoraciones con estrellas, revisado el resultado en un navegador reenviado y combinado manualmente tu primera solicitud de cambios (PR).
- partido de la incidencia de filtrado, definido el enfoque en el modo Plan, desarrollado la funcionalidad en Autopilot y revisado el resultado en Interactive.
- guiado al agente con instrucciones personalizadas, personalizado la habilidad existente `quality-checks` y usado la habilidad para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadido el servidor Model Context Protocol (MCP) de Playwright y lo usado para explorar el filtrado en un navegador real.
- creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las evidencias del navegador.
- revisado todos los cambios del filtrado y autorizado Agent Merge para combinar la PR.
- explorado comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Lo que has entregado

El taller incluye dos hitos de PR, cada uno en una rama propia creada a partir de `main` actualizado:

1. **Valoraciones con estrellas:** mostrar en las tarjetas de juegos el valor existente de `starRating` y un estado explícito para los juegos sin valoración.
2. **Filtrado y flujo de calidad:** implementar el filtrado, actualizar las instrucciones y aplicarlas a la funcionalidad, personalizar el informe de `quality-checks`, crear un perfil de control de calidad e incluir las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su PR, has usado la misma conversación y rama. Hemos combinado todo en una sola PR para agilizar el taller.

## Diferentes tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, inspeccionando el navegador y usando Copilot para explorar el sitio mediante MCP. La habilidad `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos, y comunicó los resultados con el nuevo formato. El agente de control de calidad reunió esos resultados con la revisión de requisitos y la cobertura antes de abrir la PR.

Las pruebas nuevas deben cubrir carencias reales; también puede ser correcto que una ejecución de control de calidad no necesite ninguna. Revisa el código y las evidencias antes de autorizar la combinación, y actualiza las evidencias afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller, has actualizado instrucciones, personalizado una habilidad, creado un perfil de control de calidad y configurado un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y adáptalas a las necesidades de tu equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados reales de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** según la tarea. Usa **Plan** para definir un enfoque antes de empezar, **Interactive** para participar en cambios concretos y **Autopilot** para tareas bien delimitadas. Elige un modelo más rápido para cambios rutinarios y otro más capaz para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye de forma significativa en el resultado.

## Más funciones para explorar

Has cubierto el flujo de trabajo principal. También puedes probar estas funciones de CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar subtareas independientes en paralelo.
- `/worktree` para aislar otra tarea.
- `/delegate` para enviar una tarea al agente de GitHub Copilot en la nube.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir usándola. Úsala en código de producción, en proyectos personales o en esa pequeña aplicación que llevas años queriendo crear. Comparte lo que aprendas con tu equipo y aprende de sus experiencias. Y, como siempre, consulta la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Backer Concierge con Foundry][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de GitHub Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
