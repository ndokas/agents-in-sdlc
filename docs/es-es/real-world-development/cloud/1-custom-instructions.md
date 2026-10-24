---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Requisitos previos][previous-lesson] · [Siguiente ejercicio: Agente de GitHub Copilot en la nube →][next-lesson]

El contexto es fundamental cuando se trabaja con inteligencia artificial generativa. Si una tarea debe hacerse de una manera concreta —o hay información que Copilot debería conocer—, conviene asegurarse de que pueda acceder a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo* quieres que lo haga.

En este ejercicio, aprenderás a:

- explorar cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones con ámbito de ruta.
- añadir un nuevo estándar general del repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los itinerarios de VS Code y CLI, aquí no ejecutarás un prompt antes y después del cambio. El agente en la nube trabaja de forma asíncrona en las incidencias de GitHub, así que es más difícil mostrar su efecto en tiempo real. Verás la influencia del archivo de instrucciones más adelante, al revisar las solicitudes de cambios que genere el agente.

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

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe incluir información general del proyecto relevante para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción del proyecto, buenas prácticas y directrices globales.
- Los archivos `.github/instructions/*.instructions.md`, que puedes crear para tareas o tipos de archivo concretos. Sirven para proporcionar directrices sobre lenguajes como TypeScript o Astro, o tareas como crear un componente de interfaz o pruebas unitarias.

> [!NOTE]
> En un entorno de desarrollo integrado (IDE), los archivos de instrucciones solo se usan para generar código en Copilot Chat; no se aplican a las finalizaciones de código ni a las sugerencias de la siguiente edición.
>
> Copilot Chat, Copilot CLI (la interfaz de línea de comandos) y el agente de Copilot en la nube usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Copilot también [admite archivos de instrucciones basados en otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

El taller no aborda en detalle cómo crear archivos de instrucciones, pero los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en directrices del proyecto, como una descripción de lo que se está creando, la estructura del proyecto y los estándares generales de código.
- Usa archivos `*.instructions.md` para instrucciones específicas de tipos de archivo —pruebas unitarias, componentes Astro o la capa de datos— o de tareas.
- Escribe en lenguaje natural y con claridad. Incluye ejemplos del aspecto que debe tener el código y del que no debe tener.

No hay una única forma correcta de crear archivos de instrucciones ni de usar la inteligencia artificial (IA). La experimentación te permitirá descubrir qué funciona mejor para el proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener una colección sólida de archivos de instrucciones. Al explorar los de este proyecto, puede que veas instrucciones para muchos tipos de tareas, incluidas las [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece una opción distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El ejercicio correspondiente lo indicará cuando sea relevante.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas del proyecto

Lee los archivos de instrucciones incluidos en este repositorio: un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en tu editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la descripción breve del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora su contenido. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: establece un glob relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones. Por ejemplo, coincide con los archivos de prueba TypeScript que cumplan `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones —como `unit-tests.instructions.md`— y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables y mostrar a Copilot ejemplos que puede seguir al generar código. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de código del proyecto, pero todavía no exige documentación en el código. En los pasos siguientes añadirás reglas para los comentarios de documentación TSDoc y los encabezados de comentario.

## Añadir un nuevo estándar del repositorio

Este es el único cambio que harás en este ejercicio: añadir una regla general para incluir comentarios de documentación TSDoc y un encabezado de comentario en cada archivo. El agente en la nube la aplicará a las incidencias que le asignes más adelante.

Antes de editar, crea una rama. El agente en la nube leerá las instrucciones de la rama a la que se asigne la incidencia; aun así, confirmarás los cambios de forma ordenada.

1. Desde el terminal de Codespaces, crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

El archivo `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añade los estándares de código del repositorio para orientar mejor las sugerencias.

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

## Confirmar, enviar y combinar la actualización de instrucciones

El agente en la nube lee las instrucciones de la rama de destino de la incidencia. Cuando le asignes una incidencia en el siguiente ejercicio, partirá de `main`; por tanto, los cambios de instrucciones deben llegar a `main` para que pueda usarlos.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de cambios desde `custom-instructions-cloud` hacia `main` y combínala. El agente en la nube leerá estas instrucciones al trabajar en las incidencias del siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir la rama y confirmar el cambio directamente en `main`. El paso de la rama refleja una práctica habitual en proyectos reales.

## Resumen y siguientes pasos

Has explorado cómo obtiene contexto Copilot de los archivos de instrucciones del proyecto y has añadido un estándar general del repositorio a `.github/copilot-instructions.md`. Verás cómo se aplica este estándar en las solicitudes de cambios que genere el agente en la nube durante el resto del itinerario.

A continuación, [asignarás tu primera incidencia al agente de GitHub Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
