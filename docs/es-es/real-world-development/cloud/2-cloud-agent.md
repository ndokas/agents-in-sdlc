---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que haya pocas organizaciones —si es que hay alguna— que no tengan problemas de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o funcionalidades que llevan tiempo en el backlog porque no había tiempo para implementarlas. El agente de GitHub Copilot en la nube está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea una solicitud de cambios (PR) en borrador para que la revise un desarrollador. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en asuntos de mayor alcance.

En este ejercicio explorarás cómo usar el agente de GitHub Copilot en la nube para:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realicen de forma segura.
- comprender la importancia de delimitar bien las incidencias.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys quiere resolver parte de su deuda técnica. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado deficiente: observarás que falta por completo. Como primer paso, el equipo quiere añadir comentarios de documentación de TypeScript (TSDoc) a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. Quiere que la página de detalles de cada juego muestre otros títulos de la misma categoría para que los visitantes sigan explorando. Por ahora no necesita una implementación pulida, sino algo que pueda usar para probar la experiencia de usuario (UX). El trabajo está bloqueado, aunque en este momento hay incidencias de mayor prioridad.

Estos son ejemplos de tareas que pueden perder prioridad con facilidad y que se pueden asignar al agente de GitHub Copilot en la nube. Este puede trabajar en ellas de forma asíncrona mientras tú atiendes otras tareas y, después, puedes volver para revisar su trabajo.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Hay varias maneras de asignarle trabajo, por ejemplo, [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se le asigna una incidencia, Copilot crea una solicitud de cambios (PR) en borrador para mostrar el progreso, prepara un entorno y empieza a trabajar. Puedes consultar la sesión mientras está en curso o después de que termine. Cuando la solución esté lista para revisarla, Copilot te mencionará en la PR.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no hace magia: no puedes dejar de pensar en la tarea. Incluso las operaciones aparentemente sencillas tienen cierta complejidad, así que [delimita cuidadosamente el trabajo que asignas al agente de Copilot en la nube][cloud-agent-best-practices]. Trátalo como un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. La inteligencia artificial generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas orientan a Copilot. Si ya has completado un ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente en la nube, inspecciona sin modificar los archivos de instrucciones del repositorio para reconocer su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en Codespaces:

1. Abre `.github/copilot-instructions.md` y revisa la sección **Code standards**, sobre todo las convenciones de TypeScript y los comentarios de documentación de TypeScript (TSDoc).
2. Abre `.github/instructions/unit-tests.instructions.md` y observa el frontmatter `applyTo`, que aplica las instrucciones a los archivos `**/*.test.ts`.

Cuando asignes la incidencia **Code lacks documentation** al agente en la nube en la siguiente sección, fíjate en los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentario de la PR resultante. Proceden de estos archivos de instrucciones. Volveremos a hablar de ellos al revisar las PR en un ejercicio posterior.

## Configurar el entorno de desarrollo del agente de Copilot en la nube

Para crear código, una persona o un agente suele necesitar un entorno y ejecutar scripts de configuración. Esto también se aplica a Copilot, que trabaja de forma similar a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] para preparar su entorno. Puedes personalizarlo con un [flujo de trabajo de configuración][setup-workflow] especial en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece el trabajo. Permite instalar las herramientas y dependencias necesarias. El flujo de trabajo ya está configurado para este laboratorio: proporciona Node.js, las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro única:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio durante la configuración, por ejemplo para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la interfaz y la aplicación: Node.js (ahora toda la aplicación usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrar y cargar datos en la base de datos SQLite local para que las compilaciones y pruebas dispongan de datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos detalles importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la PR.
- El evento `workflow_dispatch` permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añadir documentación

Muchos proyectos tienen información desactualizada o carecen de documentación, aunque esta facilite el mantenimiento del código y la incorporación de desarrolladores. Copilot puede ayudar a crear documentación, así que esta es una incidencia adecuada para asignarle. El agente trabajará en segundo plano y volverás a revisar el resultado en un ejercicio posterior.

1. Abre el repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el diálogo de nueva incidencia.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el diálogo de asignación.

  ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva solicitud de cambios (PR), que tendrá un título parecido a `[WIP]: Code lacks documentation`. Si no aparece, espera un momento y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en la PR. Copilot está preparando el entorno —ejecutando el flujo de trabajo mencionado—, analizando el proyecto y decidiendo cómo abordar la tarea.

13. Revisa la lista y las tareas que Copilot va a realizar.
14. Desplázate por la cronología de la PR y comprueba si Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Desplázate por la sesión en directo y observa cómo Copilot resuelve la tarea: explora el código, analiza el estado, considera un plan y crea código.

La tarea puede tardar varios minutos. El agente en la nube está diseñado para trabajar de forma asíncrona y dejarte tiempo para otras tareas. Aprovecharás esa capacidad asignándole otra incidencia y, después, trabajarás en una funcionalidad de la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Una de las ventajas del agente de GitHub Copilot en la nube es que permite dividir el trabajo: tú puedes centrarte en una tarea mientras el agente aborda otra. Aunque añadir una sección de juegos relacionados quizá no tarde mucho, puedes dedicar ese tiempo a otros asuntos. Asigna la tarea al agente.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el diálogo de nueva incidencia.
4. Selecciona **Blank issue**.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el diálogo de asignación.

  ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, verás unos ojos 👀 en el primer comentario de la incidencia. Indican que Copilot ha empezado a trabajar.

![Copilot usa el emoji de los ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. El agente en la nube trabaja de forma parecida a un ingeniero de software; no hace falta supervisarlo constantemente. Podrás revisar el resultado cuando termine. Mientras tanto, puedes crear y usar agentes personalizados.

## Resumen y siguientes pasos

Has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Permite asignar incidencias a Copilot para que trabaje de forma asíncrona. Puedes usarlo para abordar deuda técnica, crear funcionalidades o ayudar a migrar código de un framework a otro.

Has explorado estos conceptos:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realicen de forma segura.
- comprender la importancia de delimitar bien las incidencias.
- asignar incidencias a Copilot.

Mientras Copilot trabaja en segundo plano, crearás y usarás agentes personalizados. El [agente de Copilot en la nube también puede usar servidores de Model Context Protocol (MCP)][cloud-agent-mcp] y las instrucciones personalizadas que has explorado en módulos anteriores.

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente ejercicio: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
