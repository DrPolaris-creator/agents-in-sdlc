---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente en la nube de Copilot que se ocupe de tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidad. Aunque el agente trabaja de forma autónoma y asíncrona, sigue siendo importante supervisar las tareas.

Hay varias herramientas para gestionar tareas asignadas al agente en la nube, entre ellas la [página de agentes][agents-page] de GitHub. Desde este panel puedes consultar las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas. También puedes explorar las operaciones realizadas y orientar una sesión en curso.

En este ejercicio:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para pedir funcionalidad adicional.

## Escenario

Después de asignar al agente el modo de alto contraste, el equipo decidió que también sería un buen momento para añadir un modo claro. Como ya se estaban actualizando los estilos del sitio y añadiendo controles, parecía lógico incorporar esa función. Orientarás el trabajo del agente para que añada tanto el modo claro como el de alto contraste.

## Revisar las tareas del agente en la nube de Copilot

Consulta el estado de todas las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa las tareas del panel principal y del panel izquierdo. Deberías encontrar las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código.
   - generar interfaces de programación de aplicaciones (API) para modificar juegos.
   - añadir un modo de alto contraste al sitio.
3. Selecciona una tarea en curso y revisa las operaciones realizadas por Copilot, que pueden incluir:
   - descargar el código del repositorio.
   - preparar el entorno de trabajo.
   - configurar servidores de Model Context Protocol (MCP).
   - realizar distintas tareas para completar la solicitud.

> [!NOTE]
> Los pasos exactos varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios (PR) de la derecha, donde puedes consultar la solicitud y los archivos modificados.

## Orientar al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada un control de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión de la tarea del modo de alto contraste. El título exacto dependerá del nombre que Copilot haya elegido y del estado del trabajo.

    ![Sesión de accesibilidad en el panel de agentes](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que haya terminado la configuración y empiece a trabajar. Verás mensajes que indiquen que ha comenzado la tarea.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, añade esta indicación:

    ```plaintext
    Ya que estamos trabajando en un modo de alto contraste, añadamos también un modo claro. Debe haber un control para este modo que permita a los usuarios elegir el modo de visualización que prefieren.
    ```

    ![Tarea del agente en la nube con el cuadro para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar la indicación.
5. Observa cómo Copilot confirma que la ha recibido y la incorpora al flujo de trabajo.

## Dejar que Copilot trabaje

Copilot continuará con la tarea actualizada. Incorporará la nueva solicitud cuando termine el paso que estaba realizando en el momento de recibir el mensaje.

El proceso tardará varios minutos. Puedes aprovechar la pausa para repasar lo aprendido.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el centro de supervisión y orientación de las tareas del agente en la nube. Permite seguir las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para cambiar su alcance o darles más orientación.

Has explorado estos conceptos:

- exploraste el panel y la página de agentes para supervisar las tareas del agente en la nube.
- redirigiste una sesión en curso para pedir funcionalidad adicional.

Mientras Copilot completa las funciones de accesibilidad, pasarás a [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

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
