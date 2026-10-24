---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una funcionalidad pequeña al proyecto, pero los cambios más amplios requieren un proceso más sólido. GitHub Copilot CLI está diseñado para trabajar con el flujo existente de una organización y ayudar a crear lo adecuado de la manera correcta. En esta lección iniciarás un flujo de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, validarás el código y su comportamiento y, finalmente, lo combinarás en el proyecto.

> [!NOTE]
> Usarás la misma conversación y rama durante el flujo de trabajo de esta funcionalidad. Normalmente se crearían ramas o solicitudes de cambios (PR) distintas para los diferentes tipos de archivos, pero aquí usaremos un atajo para centrarnos en los conceptos principales.

En esta lección, aprenderás a:

- iniciar una conversación de Copilot desde una incidencia de GitHub.
- definir requisitos en el modo Plan.
- implementar la funcionalidad con el modo Autopilot.
- revisar el código.
- validar manualmente la funcionalidad en un navegador reenviado.

Mientras avanzas con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación con MCP, crearás un agente de control de calidad y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero hay detalles —como la combinación de categorías— que deben acordarse antes de programar. Usarás el modo Plan para resolverlos y autorizarás después una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia sus fundamentos; de hecho, los hace aún más importantes. La mayoría de los desarrolladores sigue un proceso parecido a este:

1. Partir de una incidencia registrada que describa el trabajo necesario.
2. Crear un plan de implementación.
3. Crear y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de cambios (PR).
7. Combinar el código cuando se haya revisado y se hayan superado las comprobaciones de integración continua (CI).

> [!NOTE]
> Los detalles concretos dependen del equipo y la organización, pero la mayoría de los procesos se parece al anterior.

Si sigues este enfoque habitual, comprobarás que el código generado por IA cumple los requisitos y pasa por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** determina el grado de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: tú y el agente trabajáis en equipo. El agente propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma, escribe código, ejecuta pruebas e itera hasta que considera que la tarea está completa.

Empieza en el modo Plan, revisa el plan y, a continuación, usa Autopilot para implementarlo.

## Empezar desde la incidencia

Antes de empezar el filtrado, vuelve a tu Codespace y comprueba que el repositorio y el terminal están preparados.

1. Vuelve a tu Codespace. Si está detenido, reinícialo.
2. Confirma que se ha combinado la PR de las valoraciones con estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de flecha para resaltar la incidencia **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt ahora empieza por `#7` (o un número parecido). El prefijo `#` permite incluir en el contexto una incidencia o PR de GitHub.

## Planificar la funcionalidad de filtrado

La planificación permite acordar cómo implementar una funcionalidad o completar una tarea antes de asignársela a Copilot. Conviene dedicar tiempo a planificar las tareas complejas. Cambia al modo Plan y pide a Copilot que cree un plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador situado debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia añadida en el paso anterior, escribe este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot explorará el proyecto y determinará el mejor enfoque para desarrollar el plan.

3. Copilot puede hacer preguntas sobre cómo debe funcionar el filtrado. Respóndelas según tus preferencias; no hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Desplázate por el plan y revísalo.
6. Pide a Copilot que modifique cualquier parte que no coincida con tus decisiones.

## Aprobar Autopilot

Con el plan redactado y revisado, ya puedes implementarlo. Autopilot permite que Copilot itere en la tarea hasta que considere que está completa.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador de modo situado debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación inmediatamente, así que revisa primero el plan. Si Copilot indica que faltan dependencias o que hay un conflicto de puertos, resuelve el problema de configuración antes de dar por completadas las comprobaciones.

## Revisar y verificar la implementación

Revisa el código generado antes de combinarlo, como harías con cualquier otro código. Inspecciona también el sitio para comprobar que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones que tomaste al planificar.
4. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija los fallos.

## Explorar la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio mediante el puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, por editorial y las combinaciones acordadas en el plan.
4. Confirma que el restablecimiento y los resultados vacíos se comportan según la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para desarrollar y revisar una funcionalidad. En esta lección, has:

- iniciado una conversación de Copilot desde una incidencia de GitHub.
- definido requisitos en el modo Plan.
- implementado la funcionalidad con el modo Autopilot.
- revisado el código.
- validado manualmente la funcionalidad en un navegador reenviado.

A continuación, profundizarás en cómo se genera el código y cómo garantizar que siga las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
