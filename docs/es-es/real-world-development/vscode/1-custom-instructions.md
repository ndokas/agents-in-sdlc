---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Requisitos previos][previous-lesson] · [Siguiente ejercicio: Modo agente →][next-lesson]

El contexto es fundamental cuando se trabaja con inteligencia artificial (IA) generativa. Si una tarea debe hacerse de una manera concreta —o hay información que Copilot debería conocer—, conviene asegurarse de que pueda acceder a ese contexto. Hay varias maneras de proporcionárselo; una de las principales son los [archivos de instrucciones][instruction-files], que orientan la generación de código.

En este ejercicio, aprenderás a:

- explorar cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones con ámbito de ruta.
- generar el primer conjunto de datos para el filtrado —un helper de editoriales— con las instrucciones *actuales*.
- añadir un nuevo estándar general del repositorio a `.github/copilot-instructions.md`.
- repetir el mismo prompt y observar cómo el código generado adopta el nuevo estándar.
- confirmar en `main` la actualización de instrucciones y la base del filtrado para que Copilot pueda usar las instrucciones actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no cumplir todos los estándares que establezcas. Copilot no es determinista. El objetivo es observar la *tendencia* del cambio de comportamiento después de actualizar las instrucciones, no obtener una respuesta idéntica carácter por carácter.

## Archivos de instrucciones

### Escenario

Tailspin Toys, como cualquier buen equipo de desarrollo, tiene directrices y requisitos para las prácticas de desarrollo. Entre ellos se incluyen:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe tener un tema oscuro y un aspecto moderno.
- El código debe incluir comentarios de documentación de TypeScript (TSDoc).
- Cada archivo debe empezar con un bloque de comentarios que describa su finalidad.

Con los archivos de instrucciones, te asegurarás de que Copilot tenga la información necesaria para seguir estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Permiten orientar a Copilot para obtener sugerencias y fragmentos de código más adecuados. Puedes especificar convenciones de código, bibliotecas e incluso los tipos de comentarios que quieres incluir. También puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto relevante para la mayoría de las solicitudes, como la pila tecnológica, una descripción del proyecto, las buenas prácticas y otras directrices generales.
- Los archivos `.github/instructions/*.instructions.md`, que puedes crear para tareas o tipos de archivo concretos. Sirven para proporcionar directrices sobre lenguajes como TypeScript o Astro, o tareas como crear componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En un entorno de desarrollo integrado (IDE), los archivos de instrucciones solo se usan para generar código en Copilot Chat; no se aplican a las finalizaciones de código ni a las sugerencias de la siguiente edición.
>
> Copilot Chat, la interfaz de línea de comandos (CLI) de Copilot y el agente de Copilot en la nube usan las instrucciones del repositorio y los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Copilot también [admite archivos de instrucciones basados en otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

El taller no aborda en detalle cómo crear archivos de instrucciones, pero los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en directrices del proyecto, como una descripción de lo que se está creando, la estructura del proyecto y los estándares generales de código.
- Usa archivos `*.instructions.md` para instrucciones específicas de tipos de archivo —pruebas unitarias, componentes Astro o la capa de datos— o de tareas.
- Escribe en lenguaje natural y con claridad. Proporciona ejemplos del aspecto que debe tener el código y del que no debe tener.

No hay una única forma de crear archivos de instrucciones ni de usar la IA. La experimentación te permitirá descubrir qué funciona mejor para el proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener una colección sólida de archivos de instrucciones. Al explorar los de este proyecto, puede que veas instrucciones para muchas tareas, incluidas las [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece una opción distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El ejercicio correspondiente lo indicará cuando sea relevante.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas del proyecto

El repositorio incluye un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para tareas concretas. Ábrelos en el editor o en la interfaz web de GitHub para conocer las directrices actuales.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la descripción breve del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora su contenido. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: establece un glob relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones. Por ejemplo, coincide con los archivos de prueba TypeScript que cumplan `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones —como `unit-tests.instructions.md`— y a archivos existentes del proyecto. Así puedes dividir conjuntos grandes en archivos más pequeños y reutilizables y proporcionar a Copilot ejemplos que seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de código del proyecto, pero todavía no exige documentación en el código. En los pasos siguientes añadirás reglas para los comentarios de documentación de TypeScript (TSDoc) y los encabezados de comentario.

## Crear una rama para los cambios

Crea una rama para trabajar en los cambios.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal nuevo con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat *antes* de actualizar las instrucciones

Para ver el efecto de las instrucciones personalizadas, empieza enviando un prompt con el archivo de instrucciones actual. Después lo actualizarás y enviarás el mismo prompt de nuevo.

> [!TIP]
> **Abrir Copilot Chat**
>
> Inicia una conversación limpia para que Copilot use el contexto pertinente para estos pasos.
>
> 1. Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia.
>
> El modo y el modelo dependen del ejercicio; cada paso lo indicará cuando sea necesario.

1. Cierra las pestañas del editor que hayan quedado abiertas de ejercicios anteriores para que Copilot solo reciba el contexto pertinente.
2. Abre `src/lib/publishers.ts` para mostrar a Copilot dónde debe ubicarse el helper.
3. Selecciona **Agent** en la lista desplegable de agentes de la vista Chat para que Copilot pueda modificar archivos.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios, posiblemente tanto al helper como a sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esto sigue las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Fíjate en que al código propuesto **le faltan** los comentarios de documentación TSDoc y el encabezado de comentario a nivel de archivo.

> [!CAUTION]
> Como Copilot no es determinista, puede que añada comentarios de documentación aunque no se lo pidas. No pasa nada: el objetivo sigue siendo observar la mejora de *coherencia* después de actualizar las instrucciones.

## Añadir un nuevo estándar del repositorio

El archivo `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añade los estándares de código del repositorio para orientar mejor sus sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que todavía no incluye una regla de documentación en el código.
3. Añade estas líneas de Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en el ejercicio anterior, las instrucciones pueden estar en el nivel del repositorio (`.github/copilot-instructions.md`) para proporcionar directrices generales o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para los estándares generales, como la regla de comentarios de documentación que acabas de añadir.

## Repetir el prompt y observar el cambio

Vuelve a enviar el prompt después de actualizar las instrucciones para comparar el código generado.

1. Vuelve a Copilot Chat y selecciona **New Chat** para limpiar la conversación.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía **el mismo prompt** de antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has orientado a Copilot para que siga un nuevo estándar y lo aplique a código real que se usará en el siguiente ejercicio.

## Confirmar las instrucciones y enviar la rama

Los archivos de instrucciones, como cualquier otro recurso del repositorio, se gestionan mediante el control de código fuente. Confirma y envía la rama al repositorio.

1. Abre un terminal nuevo con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que aparecen los cambios de instrucciones y del helper:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de instrucciones y todos los archivos modificados por Copilot para el helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y combinar una solicitud de cambios

Con la rama enviada, crea una solicitud de cambios vinculada a la incidencia de estándares de documentación del backlog. Copilot puede encontrar la incidencia y crear y combinar la PR mediante las herramientas de GitHub conectadas al proyecto.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actúa en GitHub en tu nombre. La plantilla del proyecto ya configura las herramientas de GitHub; no tienes que configurar nada. Es posible que VS Code te pida iniciar sesión la primera vez que Copilot use una herramienta de GitHub. Sigue las indicaciones para autorizarlo. En un ejercicio posterior conocerás la conexión basada en Model Context Protocol (MCP).

1. Abre Copilot Chat en el Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista de Copilot Chat y confirma que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia sobre las instrucciones y cree una PR desde la rama actual. Indica que debe mencionar tanto la actualización de instrucciones como la base del helper de editoriales:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot te pida permiso para realizar acciones en GitHub, revisa el comando y selecciona **Allow** si procede.
6. Después de crear la PR, pide a Copilot que la combine y vuelva a `main` local:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando Copilot te pida permiso para realizar acciones en GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** si procede.

¡Has creado y combinado una solicitud de cambios con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo obtiene contexto Copilot de los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*.
- añadir un estándar general del repositorio a `.github/copilot-instructions.md`.
- repetir el prompt y observar cómo el código propuesto adopta el nuevo estándar.
- confirmar en `main` la actualización de instrucciones y la base del helper para que el siguiente ejercicio pueda aprovecharlas.

A continuación, aplicarás esas instrucciones en el [modo agente][next-lesson] para que Copilot añada una funcionalidad en varias partes del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
