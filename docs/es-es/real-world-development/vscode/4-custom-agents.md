---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de inteligencia artificial (IA) especializados en tareas o ámbitos concretos del flujo de desarrollo. Al definirlos mediante archivos Markdown en `.github/agents` del repositorio, puedes proporcionar a Copilot instrucciones específicas, buenas prácticas, patrones de código y conocimientos del dominio para que realice ciertos tipos de trabajo con mayor eficacia. Los equipos pueden plasmar su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las [Pautas de Accesibilidad para el Contenido Web (WCAG)][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen mediante archivos Markdown en `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo incluye frontmatter YAML con, al menos, `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se solapan en algunos aspectos. Ambos se definen principalmente mediante archivos Markdown que indican a la IA cómo realizar operaciones. La diferencia más sencilla es que un **agente personalizado** es el trabajador y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes. En este laboratorio, el agente de accesibilidad revisa y actualiza el sitio según las directrices de accesibilidad; para ello, podría usar habilidades como una que gestione solicitudes de cambios o pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como ocurre con otras funciones de IA, prueba y ajusta el agente hasta que se adapte a tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades o preferencias visuales. Algunos usuarios han indicado que les cuesta leer el tema oscuro actual por la falta de contraste entre el texto y el fondo. Para abordar este problema, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es importante, así que usarás un agente personalizado para generar esta funcionalidad.

En este ejercicio, aprenderás a:

- revisar un agente personalizado de accesibilidad existente.
- usar el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado para las tareas de accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

Vuelve a tu Codespace y cambia a una rama nueva, creada desde `main`, para separar el trabajo de accesibilidad de la solicitud de cambios (PR) de filtrado del ejercicio 3:

1. Abre un terminal y crea una rama nueva a partir de `main`:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

1. Abre `.github/agents/accessibility.md`.
2. Fíjate en los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` son obligatorios en el frontmatter de los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las buenas prácticas de accesibilidad.
   - ejemplos de código en lenguaje de marcado de hipertexto (HTML), hojas de estilo en cascada (CSS) y JavaScript.
   - errores y problemas frecuentes.

## Usar el agente personalizado en Copilot Chat

VS Code muestra en la lista desplegable de agentes de Copilot Chat todos los agentes personalizados definidos en `.github/agents`. Puedes seleccionar uno para aplicar sus instrucciones y herramientas a la sesión de chat.

> [!TIP]
> **Abrir Copilot Chat**
>
> Inicia una conversación limpia para que Copilot use el contexto pertinente para estos pasos.
>
> 1. Antes de continuar, vuelve a tu Codespace, abre el panel de Copilot Chat y selecciona **New Chat** para empezar una conversación limpia.
>
> El modo y el modelo varían según el ejercicio; cada paso los indicará cuando sea necesario.

1. Si aún no está seleccionado, elige **Agent** en la lista desplegable de agentes de la vista Chat.

   ![Selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona la lista desplegable de agentes de la parte inferior de la vista Chat. Muestra el agente activo; de forma predeterminada, es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía este prompt al agente de accesibilidad:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat explorará el código base, propondrá cambios y editará archivos del proyecto. Cada edición aparecerá en línea en el chat con la ruta del archivo y una diferencia para revisar.
6. A medida que se apliquen cambios, el indicador **Files changed** mostrará los archivos que el agente ha modificado.

> [!NOTE]
> El proceso puede tardar unos minutos. Copilot está haciendo cambios reales en el repositorio: puede modificar componentes Astro, CSS y pruebas relacionadas.

En el siguiente ejercicio revisarás y dirigirás el trabajo mientras sigue en curso.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents], asistentes de IA especializados en tareas y ámbitos concretos. Permiten plasmar la experiencia y los estándares del equipo en agentes reutilizables que orientan el trabajo de Copilot.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo usar un agente personalizado en el modo agente de Copilot Chat.

A continuación, [supervisarás y dirigirás el trabajo del agente][next-lesson], revisarás los cambios a medida que se realizan y añadirás un control para el modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el entorno de desarrollo integrado (IDE)][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Ejercicio anterior: Probar la funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Siguiente ejercicio: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
