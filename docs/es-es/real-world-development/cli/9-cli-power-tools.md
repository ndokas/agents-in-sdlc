---
title: "Lección 9 - Comandos de barra de GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar trabajo opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye comandos de barra para interactuar con ella. Estos comandos ofrecen funciones avanzadas, información interna y opciones de configuración adicionales. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Ahora explorarás otros comandos útiles.

En esta lección, aprenderás a:

- usar `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- usar `/model` para explorar los modelos disponibles.
- descubrir cómo `/share` permite exportar o compartir una sesión.
- explorar comandos opcionales para el trabajo en paralelo, los árboles de trabajo y la delegación al agente en la nube.

## Escenario

Has completado el flujo de trabajo principal de CLI. Ahora explorarás otras funciones: gestión del contexto, cambio de modelo, uso compartido de sesiones y delegación opcional al [agente de GitHub Copilot en la nube][about-cloud-agent].

## Explorar el contexto de Copilot CLI

Al trabajar en tareas grandes o complejas, puedes alcanzar el límite de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando es necesario; también puedes inspeccionar o compactar el contexto con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Si la conversación ya es breve, quizá apenas cambie.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto cuando se llena la ventana. Usa `/compact` para elegir el momento. Usa `/clear` o `/new` cuando empieces una tarea distinta y quieras una conversación nueva.

## Elegir un modelo

Los modelos tienen distintas capacidades y cada desarrollador puede tener sus propias preferencias. Copilot CLI permite ver y seleccionar el modelo que quieres usar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Conserva el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Compartir una sesión

Compartir el trabajo y lo aprendido ayuda a todo el equipo a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartirla o publicar un gist de GitHub.

1. Escribe `/help` y consulta las opciones de `/share` de la versión instalada.
2. Si quieres compartir la sesión, elige un destino adecuado, como `/share file` para exportarla localmente a Markdown.
3. Revisa el contenido exportado antes de enviarlo a alguien o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o gist es opcional. No publiques contenido del repositorio o de la conversación que el equipo no quiera compartir.

## Opcional: ampliar o delegar el trabajo

Has completado el taller principal. Copilot CLI también incluye comandos para tareas más amplias:

- `/fleet` puede repartir subtareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para otra tarea.
- `/delegate` puede enviar una tarea al agente de GitHub Copilot en la nube, que trabaja de forma asíncrona y puede abrir una solicitud de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo o tareas remotas adicionales. Antes de probarlos, empieza una tarea nueva y bien delimitada, y revisa el resultado con el flujo habitual. Si quieres profundizar en el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI te permiten configurarlo, compartir sesiones y consultar lo que ocurre en segundo plano. En esta lección, has:

- usado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- usado `/model` para explorar los modelos disponibles.
- descubierto cómo `/share` permite exportar o compartir una sesión.
- explorado comandos opcionales para el trabajo en paralelo, los árboles de trabajo y la delegación al agente en la nube.

Copilot CLI incluye más comandos de barra y funciones por explorar. Para terminar, [repasarás lo aprendido][next-lesson] y los próximos pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente de GitHub Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
