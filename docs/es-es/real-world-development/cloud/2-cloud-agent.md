---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que pocas organizaciones estén libres de deuda técnica. Puede tratarse de problemas de seguridad pendientes, código heredado que necesita actualizarse o funcionalidades que llevan tiempo en el backlog porque no ha habido tiempo para implementarlas. El agente en la nube de GitHub Copilot está diseñado para tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en cuestiones más amplias.

Con el agente en la nube de Copilot explorarás cómo:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realicen de forma segura.
- redactar incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere abordar. Los contratistas que crearon la primera versión del sitio no documentaron el código. Como primer paso, el equipo quiere añadir comentarios de documentación de TypeScript (TSDoc) a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para que los usuarios puedan seguir explorando. No necesita una implementación pulida todavía; solo algo que permita probar la experiencia de usuario (UX). Aunque este trabajo está bloqueado, hay otras incidencias de mayor prioridad.

Estas tareas suelen quedar relegadas, por lo que son adecuadas para asignarlas al agente en la nube de Copilot. Este puede trabajar de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, revisar el trabajo y comprobar que todo esté bien.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot][cloud-agent-overview] puede realizar tareas en segundo plano de forma parecida a un desarrollador. Por ejemplo, puedes [asignar una incidencia de GitHub a Copilot][assign-issue]. Una vez asignada, Copilot crea una PR en borrador para seguir el progreso, prepara un entorno y empieza a trabajar. Puedes consultar la sesión mientras está en curso o cuando haya terminado. Cuando la solución esté lista para revisión, Copilot te mencionará en la PR.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no es magia: no elimina la necesidad de pensar. Incluso las tareas que parecen sencillas pueden tener complejidades ocultas, así que [delimita cuidadosamente las tareas para el agente en la nube][cloud-agent-best-practices]. Trátalo como un compañero de programación con inteligencia artificial (IA): trabaja por etapas, aprende, experimenta y adapta el enfoque. La IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas en este repositorio

En ejercicios anteriores aprendiste cómo las instrucciones personalizadas orientan a Copilot. Si ya has completado el ejercicio práctico sobre instrucciones, habrás visto `.github/copilot-instructions.md` en acción; si no, aprovecha para echarle un vistazo. Antes de asignar trabajo al agente en la nube, inspecciona en modo de solo lectura los archivos de instrucciones incluidos para que después puedas reconocer su efecto.

Abre estos archivos en la interfaz web de GitHub o en un codespace que ya tengas abierto:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa los metadatos `applyTo`, que aplican estas instrucciones a archivos `**/*.test.ts`.

Cuando asignes la incidencia `Code lacks documentation` al agente en la nube en la siguiente sección, observa si la PR resultante incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas prácticas proceden de los archivos de instrucciones. Volverás a revisarlas en una lección posterior.

## Configurar el entorno de desarrollo del agente en la nube

Crear código suele requerir un entorno concreto y la ejecución de scripts de configuración para dejarlo todo preparado. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma similar a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] para preparar su entorno. Puedes personalizarlo con un [flujo de trabajo de configuración][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece el trabajo. Así se instalan las herramientas y dependencias necesarias. El archivo ya está preparado para agilizar el taller y que puedas explorarlo. Da acceso a Node.js, las dependencias del proyecto, los binarios de navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y con datos iniciales de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Usa los permisos mínimos necesarios para estos pasos. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Para clonar el repositorio en los pasos de configuración, por ejemplo para instalar dependencias, se necesita el permiso `contents: read`. Si no se clona el repositorio, Copilot lo hará automáticamente cuando terminen los pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la aplicación: Node.js (la aplicación completa ahora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga la base de datos SQLite local con datos iniciales para las compilaciones y las pruebas.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunas características importantes:

- Contiene un único trabajo, `copilot-setup-steps`, que GitHub Actions ejecuta antes de que Copilot empiece a trabajar en la PR.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Es útil para comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añadir documentación

Aunque todos reconocen la importancia de la documentación, muchos proyectos tienen información desactualizada o carecen de ella. Esta deuda técnica ralentiza el trabajo y dificulta mantener el código o incorporar nuevos desarrolladores. Copilot puede ayudar a crear documentación, por lo que esta es una incidencia adecuada para asignar al agente en la nube. Trabajará en segundo plano; revisarás el resultado en un ejercicio posterior.

1. Abre tu repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. En **Description**, escribe:

    ```plaintext
    Nuestra organización exige que las funciones y los métodos incluyan comentarios de documentación TSDoc cuando sean útiles. Sin embargo, las últimas actualizaciones no han seguido este estándar. Hay que actualizar el código existente para incluir comentarios de documentación cuando ayuden a aclarar el comportamiento.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el panel de la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

    ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva solicitud de incorporación de cambios (PR), cuyo título será parecido a `[WIP]: Code lacks documentation` (trabajo en curso). Si no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en la PR. Copilot está preparando el entorno, ejecutando el flujo de trabajo descrito antes, analizando el proyecto y determinando el mejor enfoque.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la PR. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

    ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot resuelve la tarea: explora el código, analiza el estado, se detiene para planificar y, después, crea código.

El proceso puede tardar varios minutos. Una de las ventajas principales del agente en la nube de Copilot es que puede trabajar de forma asíncrona y dejarte tiempo para otras tareas. Aprovecharás esta función asignándole otra tarea y, mientras trabaja, añadirás funcionalidad a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Una de las ventajas del agente en la nube de GitHub Copilot es que permite repartir el trabajo: tú puedes centrarte en una tarea mientras el agente se ocupa de otra. Aunque añadir una sección de juegos relacionados quizá no lleve mucho tiempo, ese tiempo también se puede dedicar a otras tareas. Asigna esta tarea al agente en la nube.

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue**.
5. Escribe `Show related games on the game details page` en el campo **Title**.
6. En **Description**, escribe:

    ```markdown
    Queremos ayudar a los usuarios a descubrir más juegos mostrando juegos relacionados en la página de detalles de cada juego. El equipo de diseño quiere explorar la experiencia de usuario y hacer pruebas de aceptación. Estos son los requisitos:

   - Añadir en `src/lib/` un helper de acceso a datos que devuelva otros juegos de la misma categoría que un juego determinado, excluyendo ese juego
   - Mostrar una sección "Related games" en la página de detalles del juego que use el helper
   - Gestionar el caso en que un juego no tenga juegos relacionados
   - Crear pruebas unitarias para el nuevo helper
   - Asegurarse de que todas las pruebas pasan antes de crear la PR
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el panel de la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

    ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, deberías ver el emoji 👀 en el primer comentario de la incidencia, que indica que Copilot está trabajando.

![El emoji de ojos indica que Copilot está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente en la nube funciona de forma parecida a un ingeniero de software: no necesitas supervisarlo continuamente, sino revisar el resultado cuando termine. Ahora explorarás cómo crear y usar agentes personalizados.

## Resumen y siguientes pasos

Este ejercicio te ha permitido explorar el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona y usarlo para abordar deuda técnica, crear funcionalidades o ayudar a migrar código entre frameworks.

Has explorado estos conceptos:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realicen de forma segura.
- redactar incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, crearás y usarás agentes personalizados. El [agente en la nube también puede usar servidores MCP][cloud-agent-mcp] y tiene acceso a las instrucciones personalizadas que exploraste en ejercicios anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente ejercicio: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
