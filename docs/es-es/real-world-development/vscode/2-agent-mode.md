---
title: "Ejercicio 2 - Añadir funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso una actualización sencilla de una aplicación suele requerir modificar varios archivos y ejecutar operaciones, como las pruebas. El flujo de trabajo habitual incluye localizar los archivos necesarios, hacer cambios, ejecutar pruebas, depurar, detectar qué archivo falta y volver a modificarlo. La lista continúa.

El modo agente de Copilot ayuda a abordar este trabajo. Funciona de forma más autónoma en el IDE: explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los problemas detectados. En este ejercicio lo usarás para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en tu IDE y permite ciclos rápidos de comentarios y cambios. El agente en la nube trabaja de forma asíncrona, como otro miembro del equipo, y se comunica contigo mediante incidencias y solicitudes de cambios.

En este ejercicio, aprenderás a:

- usar el modo agente de Copilot para explorar el proyecto, identificar archivos pertinentes y coordinar cambios.
- implementar funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados por Copilot antes de combinarlos en el código base.

## Escenario

El catálogo de juegos está creciendo y quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior añadiste un helper de editoriales; ahora terminarás el trabajo restante en la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio de Tailspin Toys para conocer su funcionamiento actual.

El sitio es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite consultar juegos y ver sus detalles. Es una única aplicación Astro que genera páginas en lenguaje de marcado de hipertexto (HTML) estáticas en tiempo de compilación. Las páginas consultan directamente una base de datos SQLite local desde el frontmatter mediante el mapeador objeto-relacional (ORM) Drizzle; no hay una interfaz de programación de aplicaciones (API) de backend independiente ni un framework de interfaz del lado del cliente. Los helpers de acceso a datos reutilizables están en `src/lib/`, y la interactividad se añade con pequeños elementos `<script>` de Astro que usan las API estándar del Modelo de objetos del documento (DOM).

### Iniciar el sitio web

El comando `npm run dev` inicia el único servidor de desarrollo Astro. Ejecútalo en tu GitHub Codespace:

1. Vuelve a tu Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal nuevo en el Codespace con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, debería aparecer un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un diálogo que te pregunta si quieres abrir una ventana del navegador para `http://localhost:4321`, ciérralo con **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro, `http://localhost:4321`, en el terminal.

> [!NOTE]
> En un Codespace, al seleccionar el enlace de localhost en el terminal, se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado al servidor web que se ejecuta en el Codespace.

### Explorar el sitio web

Con el sitio en ejecución, explora las funciones actuales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, aparece una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

La versión inicial del sitio funciona, pero queremos añadir funciones. Empieza por revisar el backlog. La plantilla crea incidencias automáticamente; pide a GitHub Copilot que te las muestre.

> [!TIP]
> **Abrir Copilot Chat**
>
> Inicia una conversación limpia para que Copilot use el contexto pertinente para estos pasos.
>
> 1. Antes de continuar, vuelve a tu Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para empezar una conversación limpia.
>
> El modo y el modelo varían según el ejercicio; cada paso los indicará cuando sea necesario.

1. Selecciona **Agent** en la lista desplegable de agentes de la vista Chat. El agente planifica e implementa cambios en varios archivos, ejecuta comandos de terminal e invoca herramientas.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no expresan preferencia por un modelo. Al crear el taller usamos Claude Sonnet 4.5 y lo incluimos para que las sugerencias sean relativamente coherentes y la experiencia sea adecuada. Como los modelos de lenguaje grandes (LLM) son probabilísticos, las sugerencias pueden ser distintas de las que se describen. Es normal.

> [!NOTE]
> Por su naturaleza probabilística, Copilot puede invocar un comando de Model Context Protocol (MCP) distinto y, aun así, completar la tarea.

3. Envía este prompt para preguntar a Copilot por el backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista de incidencias generada.

Observa que Copilot también ha priorizado las incidencias según lo que considera más útil para el usuario.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidad al sitio.

1. En el Codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales para añadir funcionalidad, que incluyen:
   - una descripción de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a instrucciones específicas para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así puedes reutilizar contenido, dividir instrucciones complejas en partes manejables y proporcionar ejemplos y plantillas.

## Implementar el filtrado

Para completar el filtrado, hay que hacer al menos tres cambios en la aplicación:

- añadir o completar la lógica restante en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del filtrado.
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

También hay que ejecutar y superar las pruebas antes de combinar los cambios. El modo agente de Copilot puede realizar estas tareas.

1. Continúa en la conversación actual de Copilot o inicia otra con **New Chat**.
2. Selecciona **Add Context**, **Instructions** y `ui` como archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de la interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en la lista desplegable de agentes de la vista Chat.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes para la funcionalidad, incluidos los helpers de la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot explorará el proyecto, modificará archivos y ejecutará pruebas. El proceso puede tardar varios minutos. Mientras trabaja, puede que aparezcan los botones **Keep** y **Undo** en el editor. Al terminar, podrás aceptar o deshacer cada cambio; no hace falta que los selecciones mientras Copilot trabaja.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Diálogo de Copilot Chat que pide confirmar la ejecución de las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas y que algunas pruebas fallen durante el proceso. Copilot alternará entre generar código y ejecutar pruebas hasta terminar y no detectar errores.

   ![Sesión de Copilot Agent Mode completada](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Revisa el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot y otras herramientas de inteligencia artificial (IA).

10. Vuelve al navegador donde se ejecuta el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

Has explorado cómo usar el modo agente de GitHub Copilot para añadir funciones al sitio web de Tailspin Toys. En este ejercicio, has aprendido a:

- usar el modo agente de Copilot para implementar funcionalidades en la interfaz y la capa de datos.
- explorar el proyecto, identificar archivos pertinentes y coordinar cambios.
- revisar los cambios y las pruebas generados antes de combinarlos en el código base.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de cambios.

### Ejercicio adicional: implementar la paginación

Cuando aumente la lista de juegos, será necesario habilitar la paginación. Usa lo aprendido para pedir a Copilot que añada paginación al sitio. Ten en cuenta estas consideraciones:

- sigue las buenas prácticas existentes, incluidos los archivos de instrucciones.
- decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo.
- proporciona a Copilot la orientación necesaria para implementar la paginación que quieres.
- puede que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Es normal al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
