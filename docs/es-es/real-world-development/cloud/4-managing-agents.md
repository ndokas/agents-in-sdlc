---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de GitHub Copilot en la nube que realice tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma asíncrona y autónoma, sigue siendo importante supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Este centro de control muestra todas las tareas de agentes con solicitudes de cambios (PR) abiertas. Puedes explorar las operaciones realizadas e incluso dirigir una sesión en curso.

En este ejercicio, aprenderás a:

- explorar la página de agentes y supervisar las tareas del agente en la nube.
- dirigir una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo decidió que también sería útil añadir un modo claro. Como el agente ya estaba actualizando los estilos del sitio y añadiendo controles, parecía lógico incluir esta funcionalidad. Dirigirás su trabajo para que añada ambos modos.

## Revisar las tareas del agente de Copilot en la nube

Consulta el estado actual de todas las tareas asignadas al agente de Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa las tareas que aparecen en el panel principal y en el panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código.
   - generar interfaces de programación de aplicaciones (API) para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso y revisa las operaciones que Copilot ha realizado. Pueden incluir:
   - obtener el código del repositorio.
   - crear el entorno de trabajo de Copilot.
   - configurar servidores de Model Context Protocol (MCP).
   - realizar distintos pasos para completar la tarea.

> [!NOTE]
> Los pasos que aparezcan dependen del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de solicitudes de cambios (PR) situado a la derecha. Permite consultar la solicitud y los archivos modificados para supervisar el trabajo.

## Dirigir al agente en la nube

Ya has visto las tareas activas. Ahora pedirás a Copilot que añada el control del modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión que corresponde al modo de alto contraste. El título exacto dependerá del nombre que haya usado Copilot y del estado actual del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de configurar el entorno y ha empezado a trabajar. Lo notarás cuando aparezcan mensajes parecidos a los siguientes.
3. En el diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea del agente en la nube en la página de agentes, con el diálogo Steer active session while Copilot is working resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo confirma y lo incorpora al flujo de trabajo.

## Dejar que Copilot trabaje

Copilot continuará con la tarea actualizada. Incorporará la solicitud nueva cuando termine el paso que estaba realizando al recibir el mensaje.

Como antes, puede tardar varios minutos. Aprovecha el tiempo para repasar lo que has aprendido hasta ahora.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el centro de control para supervisar y dirigir las tareas del agente de GitHub Copilot en la nube. Desde allí puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir una tarea en curso para ajustar su alcance o darle más indicaciones.

Has explorado estos conceptos:

- supervisar las tareas del agente en la nube desde mission control y la página de agentes.
- redirigir una sesión en curso para pedir una funcionalidad adicional.

Mientras Copilot termina las funciones de accesibilidad, puedes [iterar sobre las solicitudes de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Siguiente ejercicio: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
