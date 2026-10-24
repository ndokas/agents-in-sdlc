---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de inteligencia artificial (IA) especializados en tareas o ámbitos concretos del flujo de desarrollo. Al definirlos mediante archivos Markdown en `.github/agents`, puedes proporcionar a Copilot instrucciones específicas, buenas prácticas, patrones de código y conocimientos del dominio para que realice ciertos tipos de trabajo con mayor eficacia. Los equipos pueden plasmar su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las [Pautas de Accesibilidad para el Contenido Web (WCAG)][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen mediante archivos Markdown en `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo incluye frontmatter YAML con al menos `name` y `description`, seguido de un prompt en Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se solapan en algunos aspectos. Ambos se definen principalmente mediante archivos Markdown que indican a la IA cómo realizar operaciones. La diferencia más sencilla es que un **agente personalizado** es el trabajador y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y coordinan habilidades e incluso otros agentes. En este laboratorio, el agente de accesibilidad revisa y actualiza el sitio según las directrices de accesibilidad; para ello, podría usar habilidades como una que gestione el flujo de solicitudes de cambios o las pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como ocurre con otras funciones de IA, prueba y ajusta el agente hasta que se adapte a tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Con los agentes personalizados, explorarás:

- cómo se definen los agentes personalizados.
- cómo se asigna una tarea a un agente personalizado.

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades o preferencias visuales. Algunos usuarios han indicado que les cuesta leer el tema oscuro actual por la falta de contraste entre el texto y el fondo. Para abordar este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que puedan activar y desactivar.

La accesibilidad es importante, así que usarás un agente personalizado para generar esta funcionalidad lo antes posible.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado para las tareas de accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

Vuelve a tu Codespace y abre el archivo del agente personalizado de accesibilidad:

1. Abre `.github/agents/accessibility.md`.
2. Fíjate en los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` son obligatorios en el frontmatter de los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las buenas prácticas de accesibilidad.
   - ejemplos de código en lenguaje de marcado de hipertexto (HTML), hojas de estilo en cascada (CSS) y JavaScript.
   - errores y problemas frecuentes.

## Crear y asignar una incidencia

Mission control es el centro para trabajar con los agentes de tu entorno. Permite asignar tareas al agente de Copilot en la nube, supervisarlas y redirigirlas o proporcionar directrices adicionales. Asigna a Copilot la tarea de crear un modo de alto contraste.

1. Ve a tu repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el diálogo de nueva incidencia.
4. Selecciona **Blank issue**.
5. Escribe `Add high contrast mode to website` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el diálogo de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Diálogo de asignación del agente en la nube con el agente personalizado de accesibilidad resaltado](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en la tarea en segundo plano.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents] de GitHub Copilot: asistentes de IA especializados en tareas y ámbitos concretos. Permiten plasmar la experiencia y los estándares del equipo en agentes reutilizables que orientan el trabajo de Copilot.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo asignarles tareas.

Mientras Copilot implementa el modo de alto contraste, puedes [supervisar y dirigir la sesión del agente][next-lesson] desde mission control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Prepararse para usar agentes personalizados en una organización][org-custom-agents]
- [Prepararse para usar agentes personalizados en una empresa][enterprise-custom-agents]

---

| [← Ejercicio anterior: Agente de GitHub Copilot en la nube][previous-lesson] | [Siguiente ejercicio: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
