---
title: "Lección 4 - Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental cuando se trabaja con inteligencia artificial generativa. Si una tarea debe hacerse de una manera concreta, conviene proporcionar esa orientación a Copilot. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones que ha seguido Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección, aprenderás a:

- explorar cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones con ámbito de ruta.
- actualizar el archivo de instrucciones para garantizar que se sigan los estándares de código.
- observar el impacto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys, como cualquier buen equipo de desarrollo, tiene directrices y requisitos para las prácticas de desarrollo. Entre ellos se incluyen:

- Los comentarios deben explicar la intención y las decisiones que no resultan evidentes, en lugar de repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su finalidad, parámetros y valores devueltos con comentarios de documentación de TypeScript (TSDoc) y JavaScript (JSDoc), incluido el argumento `db` inyectable cuando corresponda.
- Los componentes Astro reutilizables deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Deben conservarse las indicaciones existentes de formato y lint.

Con los archivos de instrucciones, te asegurarás de que Copilot tenga la información necesaria para seguir estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Permiten orientar a Copilot para obtener sugerencias y fragmentos de código más adecuados. Puedes especificar convenciones de código, bibliotecas e incluso los tipos de comentarios que quieres incluir. También puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto relevante para la mayoría de las solicitudes.
- Los archivos `.github/instructions/*.instructions.md`, que proporcionan directrices para determinados lenguajes, tipos de archivo o tareas.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones depende del entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de confiar en un formato concreto.

## Explorar los archivos de instrucciones personalizadas del proyecto

El proyecto inicial ya incluye varios archivos de instrucciones. Explóralos antes de hacer un cambio para observar su efecto.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Explora el archivo y fíjate en la descripción breve del proyecto y las directrices de código. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora los archivos. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y otros temas.
5. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: establece el glob que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y observa sus enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables y señalar a Copilot ejemplos que puede seguir.

## Actualizar los archivos de instrucciones para cumplir las directrices del equipo

Los archivos existentes son un buen punto de partida, pero todavía falta algo. Modifica el archivo principal `copilot-instructions.md` para que el nuevo código TypeScript incluya comentarios TSDoc.

1. En `.github/copilot-instructions.md`, localiza la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade el siguiente elemento al final de la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplicar las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardarlas, sal de la conversación de filtrado y retómala para que Copilot use las nuevas indicaciones sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda automáticamente las conversaciones.
2. Desde la raíz del repositorio en la rama `game-filters-cli`, retoma la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está activa la rama `game-filters-cli`.
3. Pide a Copilot que aplique las instrucciones actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Fíjate en los nuevos comentarios TSDoc y confirma que describen el código correctamente.

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones y has aplicado un nuevo estándar a la funcionalidad. En concreto, has:

- explorado cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones con ámbito de ruta.
- actualizado el archivo de instrucciones para garantizar que se sigan los estándares de código.
- observado el impacto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para garantizar que el lint y las pruebas se ejecuten de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
