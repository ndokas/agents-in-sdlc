---
title: "Lección 7 - Crear y usar un agente de control de calidad"
description: "Crea un agente personalizado de control de calidad que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar el filtrado en un navegador. Ahora combinarás estas capacidades en un agente personalizado con un proceso de control de calidad bien definido.

En esta lección, aprenderás a:

- explorar cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- crear y revisar un perfil reutilizable de control de calidad.
- seleccionar el agente de control de calidad y revisar sus conclusiones en relación con la incidencia de filtrado.

## Escenario

Antes de abrir una solicitud de cambios (PR), Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de las pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar este proceso y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil Markdown. El perfil describe la finalidad, las instrucciones y las herramientas disponibles para el agente. En este taller definirás un rol de control de calidad en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has usado cumplen funciones distintas. Las instrucciones del repositorio describen los estándares del equipo. La habilidad `quality-checks` agrupa comprobaciones repetibles. Playwright MCP proporciona herramientas para el navegador. El perfil de control de calidad indica a Copilot cómo usar estas capacidades para evaluar los requisitos e informar de los resultados. No sustituye a las demás personalizaciones ni requiere otra conversación.

## Crear el perfil de control de calidad

Antes de abrir la PR de la funcionalidad, pide a Copilot que cree un perfil de control de calidad reutilizable. El perfil definirá las comprobaciones y los límites que debe respetar.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Revisar el perfil

Antes de usar el nuevo agente, revisa su perfil para confirmar que Copilot ha incluido el proceso y los límites previstos. Así evitarás que un agente incompleto o demasiado amplio modifique una funcionalidad que solo quieres verificar.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, aunque proporciona un nombre visible claro.
3. Lee las instrucciones del perfil y confirma que el agente parte de los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y usa Playwright MCP.
4. Confirma que informa de las pruebas con evidencias, pide permiso antes de cambiar código de implementación y no confirma cambios ni abre solicitudes de cambios.
5. Si falta alguna responsabilidad o límite, pide al agente predeterminado que revise el perfil antes de continuar.

## Ejecutar el control de calidad para la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Retoma la misma conversación de filtrado después de crear el perfil y selecciona el agente de control de calidad para que pueda usar la incidencia y las decisiones de planificación que ya están en el contexto.

1. Activa el agente con el siguiente comando:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, quizá no aparezca en la lista de agentes disponibles. El comando anterior lo activará.

2. Pide al agente que revise la funcionalidad:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. Espera a que el agente de control de calidad termine su trabajo.
4. Lee el informe completo.

## Resumen y siguientes pasos

Has añadido un rol especializado reutilizable y revisado su trabajo. En esta lección, has:

- explorado cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- creado y revisado un perfil reutilizable de control de calidad.
- seleccionado el agente y revisado sus conclusiones en relación con la incidencia de filtrado.

Ya tienes la implementación, la actualización de la habilidad, el perfil de control de calidad, las pruebas y el informe de verificación listos para revisar. A continuación, los reunirás en una [PR de la funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
