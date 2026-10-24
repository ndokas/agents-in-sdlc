---
title: "Lección 5 - Personalizar y usar la habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste solo en escribir código. Ya has comprobado manualmente que funciona y has usado archivos de instrucciones para garantizar que siga los estándares del proyecto. Pero ¿qué ocurre con las pruebas, el lint y el resto de las tareas de integración continua (CI)?

Las **habilidades de agente** son adecuadas para este tipo de tareas. Ayudan a Copilot a entender cómo ejecutar operaciones como estas.

En esta lección, aprenderás a:

- explorar la habilidad existente `quality-checks`.
- personalizar el formato de los resultados.
- volver a cargar y ejecutar la habilidad.

## Escenario

Tailspin Toys usa la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos complementarios que el agente carga cuando los necesita. En esencia, son carpetas que incluyen un archivo Markdown llamado `SKILL.md`. Este archivo contiene frontmatter con el nombre y la descripción de la habilidad, además de una descripción general y las indicaciones sobre cuándo debe usarse. La carpeta también puede incluir subcarpetas con scripts y otros recursos.

> [!NOTE]
> Una habilidad no necesita archivos ni carpetas adicionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md`, porque usa los comandos que ya existen en el proyecto.

Las habilidades pueden estar en la carpeta `.github/skills` del proyecto para que el equipo las comparta y reutilice, o en la carpeta personal de habilidades `~/.copilot/skills`.

## Explorar la habilidad

Explora la habilidad `quality-checks` que ha creado el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` del principio. La descripción ayuda a Copilot a decidir cuándo debe usar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot por el proceso de pruebas y lint.
4. Fíjate en que la habilidad todavía no incluye la sección **Results output formatting**.

## Ejecutar la habilidad antes de modificarla

Puedes llamar a las habilidades directamente desde Copilot CLI o pedirlas en lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observa el informe final.

## Personalizar el informe

El informe debería indicar qué se ejecutó, si se completó correctamente y qué resultados comunicaron las herramientas. Actualiza la habilidad para que genere ese tipo de informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guardará automáticamente.

## Ejecutar la habilidad actualizada

Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación. Comprueba el resultado de la modificación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía el mismo prompt de antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observa el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y usado una habilidad de agente existente. En esta lección, has:

- explorado la habilidad `quality-checks` para las pruebas unitarias, el lint y las comprobaciones de tipos.
- personalizado el formato de los resultados.
- vuelto a cargar y ejecutado la habilidad.

Ese cambio acompañará al filtrado en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio mediante el [servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad sirven como referencia, no como tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
