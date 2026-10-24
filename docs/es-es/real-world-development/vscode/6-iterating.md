---
title: "Ejercicio 6 - Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← Ejercicio anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

A lo largo de este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario. Has usado el modo agente para añadir filtrado en el cliente y el servidor, el servidor MCP de Playwright para probarlo manualmente en un navegador y un agente personalizado para implementar funciones de accesibilidad —modos de alto contraste y claro—. También has dirigido la sesión mientras estaba en curso para ampliar el trabajo. Ahora publicarás esos cambios locales y los revisarás como lo haría tu equipo.

### Escenario

La incorporación de la inteligencia artificial (IA) generativa no cambia los fundamentos del diseño de software ni del desarrollo y las operaciones (DevOps). Sigue siendo necesario revisar todo lo que produce Copilot. Publicarás los cambios de accesibilidad desde tu Codespace, abrirás una solicitud de cambios y revisarás la diferencia antes de compartirla con el resto del equipo.

## Publicar las funciones de accesibilidad

Los controles de alto contraste y modo claro que implementaste con el agente personalizado de accesibilidad en el [ejercicio 4][exercise-4] y el [ejercicio 5][exercise-5] están confirmados mediante commits en tu Codespace. Enviarás los cambios a una rama y abrirás una solicitud de cambios para que el equipo pueda revisarlos.

1. Vuelve a tu Codespace.
2. Abre la vista **Source Control** en VS Code.
3. Confirma que los cambios de accesibilidad están confirmados. Si tienes cambios sin confirmar del ejercicio 5, añádelos al área de preparación y crea un commit ahora con un mensaje descriptivo, por ejemplo, `Add high-contrast and light-mode toggles`.
4. Publica la rama seleccionando **Publish Branch** o usa el menú **...** y selecciona **Push**.
5. VS Code te ofrecerá abrir la rama nueva en github.com. Acepta la propuesta o ve manualmente al repositorio y selecciona **Compare & pull request** en el aviso de la rama.
6. Escribe un título claro —por ejemplo, `Add high-contrast and light-mode toggles`— y una descripción breve que resuma los cambios y su finalidad.
7. Selecciona **Create pull request**.
8. Una vez abierta la solicitud de cambios (PR), selecciona la pestaña **Files changed** para revisar el trabajo completo. Presta especial atención a:
   - los componentes de la interfaz que alternan entre modos.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios de hojas de estilo en cascada (CSS) o estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como las etiquetas de aplicaciones de Internet enriquecidas accesibles (ARIA) y la navegación con teclado.
   - el código JavaScript o TypeScript que gestiona el cambio de modo.
9. Vuelve a la pestaña **Conversation**.
10. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

    ![Aprobar y ejecutar flujos de trabajo con Approve and run workflows](../../../_images/shared-approve-workflows.png)

11. Espera a que terminen los flujos de trabajo. Si todo va bien, deberían superarse.

> [!TIP]
> ¿Quieres otra opinión sobre el trabajo de accesibilidad? Menciona a `@copilot` en un comentario de la PR con una petición como «revisa esta PR para encontrar otros problemas de las Pautas de Accesibilidad para el Contenido Web (WCAG)» o «sugiere mejoras en la navegación con teclado». Copilot iniciará una sesión nueva para atender el comentario.

## Ejercicio opcional: seguir explorando en local

Trabajar de forma iterativa con un agente en el entorno de desarrollo integrado (IDE) es una habilidad que se desarrolla con la práctica. Estas son algunas ideas para sesiones de seguimiento en VS Code:

- añadir un formulario de interés para patrocinadores en la página de detalles de un juego.
- implementar paginación en la lista de juegos.
- añadir validación de entradas y gestión de errores a los helpers de acceso a datos en `src/lib/`.
- ampliar el alcance del agente de accesibilidad; por ejemplo, auditar el orden del foco del teclado en todo el sitio.

## Resumen

¡Enhorabuena! Has completado el itinerario de VS Code. En este laboratorio, has:

- **Usado Playwright MCP para probar manualmente la funcionalidad.** Has añadido el servidor MCP de Playwright y has dejado que Copilot controle un navegador para verificar el filtrado antes de abrir una solicitud de cambios.
- **Dirigido el modo agente para coordinar cambios en varias capas.** Has añadido una función de filtrado que afecta al cliente, al servidor y a las pruebas en una sola sesión.
- **Usado un agente personalizado.** Has seleccionado el agente de accesibilidad en el selector de agentes y has observado cómo implementaba el modo de alto contraste en el repositorio.
- **Gestionado y dirigido una sesión de agente.** Has revisado los cambios propuestos en línea, aceptado los que querías y ampliado la sesión con una petición posterior para el modo claro.
- **Cerrado el ciclo con una solicitud de cambios.** Has publicado el trabajo local y lo has revisado de principio a fin, como lo haría tu equipo.

## Revisión y siguientes pasos

Este es el final del itinerario obligatorio de VS Code. Puedes dar el taller por terminado aquí.

Si quieres ampliar tu conocimiento de las funciones de agente de Copilot, los otros itinerarios cubren escenarios relacionados desde distintas interfaces:

- 💻 **[Itinerario de la interfaz de línea de comandos (CLI)](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades, un agente personalizado de control de calidad, Playwright MCP, Agent Merge y transferencias que usan el contexto.
- ☁️ **[Itinerario del agente en la nube](../../cloud/)**: asigna incidencias al agente en la nube, supervisa las sesiones desde la página de agentes e itera de forma asíncrona sobre las solicitudes de cambios.

También puedes seguir desarrollando lo que has empezado. [awesome-copilot][awesome-copilot] es una buena fuente de archivos de instrucciones, agentes personalizados y habilidades que puedes adaptar a tus proyectos.

Como ampliación opcional, [Opcional: Incorporar Foundry][exercise-7] usa VS Code y Microsoft Foundry Toolkit para preparar un modelo, desplegar un Backer Concierge y conectarlo al sitio.

## Recursos

- [GitHub Copilot][github-copilot]
- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo agente][agent-mode]

---

| [← Ejercicio anterior: Gestionar agentes][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
