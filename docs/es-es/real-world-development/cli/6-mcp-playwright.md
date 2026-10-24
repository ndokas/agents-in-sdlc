---
title: "Lección 6 - Validar la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar el filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos señalado, programar no consiste únicamente en escribir código. También debemos trabajar con datos y servicios externos, y permitir que Copilot acceda a otras automatizaciones. Para ello sirven los servidores MCP: permiten que Copilot vaya más allá de las funciones integradas en la CLI y use más herramientas y servicios.

En esta lección, aprenderás a:

- entender qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI.
- añadir el servidor MCP de Playwright si aún no está disponible.
- pedir al agente que controle un navegador y explore la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz hay que interactuar con ella. Permitir que Copilot use el sitio como lo haría un usuario ayuda a automatizar las comprobaciones y aumenta la confianza en que los cambios funcionan como se espera.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos. Así pueden acceder a información actualizada y realizar acciones en tu nombre.

Los agentes acceden a estas herramientas y recursos mediante un servidor MCP, que actúa de puente con los servicios externos. Cada servidor MCP ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos son:

- **[Servidor MCP de GitHub][github-mcp]**: proporciona acceso a las API para gestionar repositorios, incidencias y solicitudes de cambios de GitHub.
- **[Servidor MCP de Playwright][playwright-mcp-server]**: ofrece funciones de automatización de navegadores mediante Playwright.

Hay muchos otros servidores MCP. GitHub aloja un [registro de MCP][mcp-registry] para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y ten en cuenta las implicaciones de seguridad.

## Añadir el servidor MCP de Playwright

Si el servidor MCP de Playwright aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio como un usuario.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos de configuración y continúa con la sección siguiente.

1. Vuelve a tu Codespace.
2. En Copilot CLI, abre el diálogo para añadir un servidor MCP con este comando:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** con <kbd>Enter</kbd> y pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del diálogo MCP.

## Pedir a Copilot que explore la funcionalidad con Playwright

Ya has comprobado manualmente que la funcionalidad se comporta como esperas. Ahora usarás el servidor MCP de Playwright disponible en Copilot para repetir la comprobación.

1. Envía este prompt para pedir a Copilot que use Playwright MCP para validar la funcionalidad:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> No es necesario indicar explícitamente a Copilot que use el servidor MCP; normalmente lo detectará por su cuenta. Sin embargo, mencionarlo ayuda a orientar la tarea y puede hacer que los resultados sean más coherentes.

2. Observa cómo Copilot describe los pasos que realiza en el navegador para comprobar la funcionalidad.
3. Lee el informe y confirma que todo se comporta como se espera.

Copilot iniciará el servidor, interactuará con el sitio mediante Playwright, detendrá el servidor y te proporcionará un informe.

## Resumen y siguientes pasos

Has utilizado el servidor MCP de Playwright para explorar la funcionalidad en un navegador real desde Copilot CLI. En esta lección, has:

- aprendido qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI.
- añadido el servidor MCP de Playwright si aún no estaba disponible.
- pedido al agente que controlara un navegador y explorara la funcionalidad de filtrado.

A continuación, crearás un [agente personalizado de control de calidad][next-lesson] que combine la habilidad y las herramientas del navegador en un rol especializado.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
