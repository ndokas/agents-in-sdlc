---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Cuando usas GitHub Copilot en modo agente, trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tiempo real en tu ordenador, Copilot Chat en VS Code muestra las llamadas a herramientas y las ediciones de archivos a medida que ocurren. Puedes revisar cada diferencia en línea, aceptar o rechazar cambios individuales y dirigir la conversación con prompts de seguimiento sin salir del editor.

En este ejercicio, aprenderás a:

- supervisar en tiempo real el trabajo del agente de accesibilidad.
- revisar y aceptar los cambios propuestos en los archivos.
- dirigir al agente durante la sesión para añadir un control del modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha pedido también un control para el modo claro, pensado para quienes consideran demasiado oscuro el tema predeterminado. En lugar de iniciar otra conversación, dirigirás la sesión actual para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en línea en el chat con una diferencia y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver las llamadas a herramientas. Aparecerán lecturas y ediciones de archivos, así como comandos de terminal que requieren tu autorización.
3. Cuando el agente proponga una edición, aparecerá la diferencia en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y permitir que el agente lo intente de nuevo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados en esta sesión.

> [!NOTE]
> Si una llamada a una herramienta requiere permiso —por ejemplo, para ejecutar un comando de terminal—, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba únicamente los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final con un resumen de los cambios.

## Dirigir al agente para añadir el modo claro

Con el modo de alto contraste implementado, amplía la misma conversación para añadir un control del modo claro. Como continúas en la sesión actual, el agente conserva el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilos y el uso del almacenamiento local.

1. En el campo de Copilot Chat, envía este prompt de seguimiento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente retomará el trabajo, explorará los cambios existentes y propondrá actualizaciones para el modo claro.
3. Revisa cada edición nueva cuando aparezca, como hiciste con el modo de alto contraste. Usa **Keep** o **Undo** para cada diferencia.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, refactoriza el panel de controles de una forma que no te gusta—, deténlo con el botón **Stop** situado junto al campo de chat y envía una aclaración.

> [!TIP]
> Dirigir una sesión en curso es más rápido que empezar de nuevo. Envía prompts de seguimiento para pedir ajustes —por ejemplo, «simplifica el CSS y usa variables CSS en lugar de duplicar selectores»—, corregir el enfoque —«no añadas un tercer control; integra el modo claro en el panel de configuración existente»— o ampliar el alcance —«añade también atajos de teclado para ambos controles»—.

## Revisar los cambios

Antes de confirmar el trabajo, revisa todo lo que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa todos los archivos modificados. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que el agente ha aplicado los patrones de accesibilidad: atributos de aplicaciones de Internet enriquecidas accesibles (ARIA), navegación con teclado, lenguaje de marcado de hipertexto (HTML) semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en el [siguiente ejercicio][next-lesson].

## Resumen y siguientes pasos

Has usado el modo agente de Copilot Chat para supervisar y dirigir una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En este ejercicio, has aprendido a:

- supervisar el trabajo de un agente en tiempo real.
- aceptar o rechazar ediciones individuales.
- dirigir una sesión en curso con prompts de seguimiento.

En el [siguiente ejercicio][next-lesson], publicarás el trabajo de accesibilidad como solicitud de cambios y revisarás los cambios que ha producido el agente personalizado local durante el laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo agente][agent-mode]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Siguiente ejercicio: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
