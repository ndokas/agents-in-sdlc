---
title: "Ejercicio 3 - Probar la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Modo agente][previous-lesson] · [Siguiente ejercicio: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo agente. Antes de abrir una solicitud de cambios (PR), confirma que funciona en el navegador. En lugar de probar la aplicación manualmente, conectarás el **servidor de Model Context Protocol (MCP) de Playwright** y dejarás que Copilot controle un navegador real. Después, publicarás la rama y abrirás la PR.

En este ejercicio, aprenderás a:

- conocer Model Context Protocol y cómo los servidores MCP amplían las herramientas de Copilot.
- añadir el servidor MCP de Playwright al espacio de trabajo.
- pedir a Copilot que lo use para probar manualmente el filtrado en un navegador.
- publicar la rama y abrir una solicitud de cambios para el filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente es más útil cuando puede acceder a recursos externos al editor. Model Context Protocol (MCP) proporciona esa conexión: es un estándar de comunicación entre agentes y herramientas o servicios externos.

![Diagrama del funcionamiento interno del modo agente y su interacción con el contexto, el modelo de lenguaje grande (LLM) y las herramientas, incluidas las aportadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de inteligencia artificial (IA) se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Los agentes acceden a las herramientas y los recursos a través de un servidor MCP, que actúa de puente con los servicios externos, como las interfaces de programación de aplicaciones (API) existentes o las herramientas locales de paquetes npm. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos son:

- **[Servidor MCP de GitHub](https://github.com/github/github-mcp-server)**: proporciona acceso a interfaces de programación de aplicaciones (API) para gestionar repositorios, crear repositorios y administrar incidencias y solicitudes de cambios de GitHub.
- **[Servidor MCP de Playwright](https://github.com/microsoft/playwright-mcp)**: ofrece automatización de navegadores mediante Playwright, como abrir páginas, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y ten en cuenta las implicaciones de seguridad. Usa solo servidores de confianza y sé prudente al conceder acceso a recursos u operaciones sensibles.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu Codespace.
2. Confirma que ya está configurado un servidor `github`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla incluye la entrada `github`; por eso Copilot pudo leer el backlog en el ejercicio anterior. Usa el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no tienes que instalar nada en local: VS Code se conecta mediante el protocolo de transferencia de hipertexto (HTTP) y te pide autenticarte en GitHub la primera vez que Copilot usa una de sus herramientas.

## Añadir el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador controlable para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en tu ordenador mediante `npx @playwright/mcp@latest`. La opción `--headless` ejecuta el navegador sin mostrar una ventana, algo necesario en Codespaces porque no hay un escritorio donde mostrarla.

> [!NOTE]
> El proyecto Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, por lo que normalmente el navegador ya está instalado. Si Copilot indica que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Iniciar el servidor y confiar en él

VS Code inicia los servidores MCP **cuando se necesitan**: la primera vez que Copilot solicita sus herramientas, VS Code inicia el servidor y te pide confirmar que confías en él.

1. En `.vscode/mcp.json`, aparecerá una acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code pregunta si confías en el servidor, revisa la configuración y confirma la confianza para iniciarlo.

El inicio manual es opcional. Si no lo haces, VS Code iniciará el servidor y te preguntará si confías en él cuando Copilot necesite sus herramientas en el siguiente paso.

## Probar la funcionalidad de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador real. El agente puede abrir una página, navegar, aplicar filtros y comunicarte los resultados; después, resumirá lo que ha observado. Es una forma rápida de comprobar el comportamiento sin salir de la conversación.

El servidor MCP de Playwright accede al árbol de [accesibilidad][playwright-mcp-server] de la página, no a capturas de pantalla. El agente razona sobre elementos estructurados y etiquetados —como botones, enlaces y elementos de lista—, igual que la tecnología de asistencia. Así, una comprobación funcional rápida también sirve como revisión básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe el filtrado:

1. Envía a Copilot este prompt para que verifique la nueva funcionalidad de filtrado:

   ```text
   Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

   1. Go to the games page and note how many games are listed.
   2. Apply a category filter and confirm the list updates to only show games in that category.
   3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
   4. Combine a category and a publisher filter and confirm the results respect both.

   Report what you observe at each step, and call out anything that does not behave as expected.
   ```

Copilot iniciará un navegador mediante el servidor MCP de Playwright, seguirá los pasos y te informará de lo observado. Compara el resumen con los criterios de aceptación de la incidencia. Si algo no funciona como se espera, pide más información o solicita que corrija el código antes de abrir una PR.

> [!NOTE]
> Para esta prueba, la aplicación debe estar disponible en `http://localhost:4321`. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot use el servidor MCP de Playwright quizá tenga que descargar un navegador.

Si la aplicación o el navegador no están listos, sigue la recuperación correspondiente:

1. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt.
2. Si Copilot indica que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar la rama y crear una solicitud de cambios

Después de confirmar que la funcionalidad funciona, publica la rama `filtering-vscode` para que el equipo pueda revisar el cambio.

1. Abre el panel **Source Control** en el Codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de commit con el botón **Sparkle**.

   ![Panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crear la solicitud de cambios

Hay varias formas de crear una solicitud de cambios, por ejemplo, desde github.com o mediante la interfaz de línea de comandos (CLI) de GitHub. Como ya trabajas con GitHub Copilot, pídele que encuentre la incidencia correspondiente y cree una PR asociada.

1. Abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Confirma que **Agent** está seleccionado en la lista de agentes para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree la PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Si es necesario, selecciona **Continue** para permitir que Copilot recopile información y realice las operaciones. La primera vez que use una herramienta de GitHub, puede que tengas que autenticarte. Sigue las indicaciones para autorizarlo.
5. Observa cómo Copilot busca entre las incidencias, encuentra la adecuada y crea la PR.
6. Selecciona el enlace de Copilot para revisar la solicitud, pero **no la combines todavía**.

## Resumen y siguientes pasos

En este ejercicio, has:

- aprendido qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot.
- añadido el servidor MCP de Playwright al espacio de trabajo.
- usado el servidor para probar manualmente el filtrado en un navegador antes de publicar el cambio.
- publicado la rama y abierto una solicitud de cambios para el filtrado.

Has usado un servidor MCP para *probar* una funcionalidad, pero MCP también permite ampliar Copilot de otras formas. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
