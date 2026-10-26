---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas GitHub Copilot en modo de agente, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tiempo real en tu equipo, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición. Puedes revisar los diffs en línea, aceptar o rechazar cambios individuales y orientar la conversación con nuevas indicaciones sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones de archivos propuestas.
- orientarás la sesión en curso para añadir un control de modo claro además del modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un control para el modo claro, para los usuarios a quienes les resulta demasiado oscuro el tema predeterminado. En lugar de iniciar una conversación nueva, ampliarás el trabajo de la sesión existente.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar cada llamada a herramientas: aparecerán lecturas de archivos, ediciones y comandos del terminal que el agente quiera ejecutar.
3. Cuando el agente proponga una edición, aparecerá un diff en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y permitir que el agente lo intente de nuevo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramientas necesita permiso —por ejemplo, para ejecutar un comando del terminal—, el agente se detendrá y mostrará la opción **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final que resuma los cambios.

## Orientar al agente para añadir el modo claro

Ahora que ya está implementado el modo de alto contraste, ampliarás la misma conversación para añadir un control del modo claro. Como continúas en la misma sesión, el agente conserva el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilos y el patrón de almacenamiento local.

1. En el campo de Copilot Chat, envía esta indicación de seguimiento:

    ```plaintext
    Buen trabajo. Añade ahora un control para el modo claro junto al control de alto contraste. El modo claro debe seguir el mismo patrón: guardar la preferencia en el almacenamiento local y estar disponible en la misma zona de la interfaz. El usuario debe poder activar el alto contraste y el modo claro de forma independiente.
    ```

2. El agente retomará el trabajo, explorará los cambios existentes y propondrá modificaciones para el modo claro.
3. Revisa cada nueva edición cuando aparezca, igual que hiciste con el modo de alto contraste. Usa **Keep** o **Undo** en cada diff.
4. Si el agente toma una dirección que no quieres —por ejemplo, intenta reorganizar el panel de controles de una forma que no te convence—, detenlo con el botón **Stop** junto al campo de chat y envía una aclaración.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de cero. Puedes pedir ajustes («simplifica las hojas de estilo en cascada (CSS) y usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también atajos de teclado para ambos controles»).

## Revisar los cambios

Antes de confirmar los cambios, revisa rápidamente todos los archivos modificados por el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos modificados y revisa las diferencias. Confirma que se aplican los patrones de accesibilidad del agente personalizado, como los atributos Accessible Rich Internet Applications (ARIA), la navegación con teclado, el lenguaje de marcado de hipertexto (HTML) semántico y la persistencia en el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [el siguiente ejercicio][next-lesson].

## Resumen y siguientes pasos

Has usado el modo de agente de Copilot Chat para supervisar y orientar una sesión en curso: revisaste cada edición, aceptaste o rechazaste cambios y enviaste indicaciones de seguimiento para ampliar el trabajo sin perder el contexto.

En este ejercicio:

- supervisaste en tiempo real el trabajo del agente.
- aceptaste y rechazaste ediciones de archivos individuales.
- orientaste una sesión en curso con indicaciones de seguimiento.

En el [siguiente ejercicio][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás las modificaciones generadas por el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Siguiente ejercicio: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
