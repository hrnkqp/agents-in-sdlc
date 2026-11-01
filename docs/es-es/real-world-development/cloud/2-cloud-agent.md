---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan problemas de deuda técnica: incidencias de seguridad pendientes, código heredado que necesita actualizarse o solicitudes de funcionalidad que llevan tiempo en la lista de trabajo. El agente en la nube de GitHub Copilot puede actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así puedes delegar tareas tediosas y centrarte en trabajos de mayor alcance.

Con el agente en la nube de Copilot explorarás:

- cómo personalizar el entorno de generación de código.
- cómo garantizar que las operaciones se realicen de forma segura.
- por qué es importante delimitar bien las incidencias.
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica pendiente. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: de hecho, no hay documentación. El primer paso es añadir comentarios TSDoc a todas las funciones exportadas.

Además, el equipo de diseño quiere mejorar la búsqueda de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para que los visitantes sigan explorando. No necesita una implementación refinada todavía; quiere algo que pueda utilizar para probar y aceptar la experiencia de usuario. Aunque esta tarea bloquea otras, hay incidencias de mayor prioridad.

Son tareas que suelen posponerse y que conviene asignar al agente en la nube de Copilot. Mientras trabaja de forma asíncrona, el desarrollador puede ocuparse de otras tareas y volver después para revisar el resultado.

## Introducción al agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma similar a un desarrollador. Por ejemplo, puedes [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se le asigna, Copilot crea una PR en borrador para hacer un seguimiento, configura un entorno y empieza a trabajar. Puedes examinar la sesión mientras sigue activa o después de que termine. Cuando la solución esté lista para revisión, te mencionará en la PR.

## La importancia de unas instrucciones bien delimitadas

GitHub Copilot no hace magia: no elimina la necesidad de pensar. Incluso las tareas aparentemente sencillas pueden ser complejas; por eso conviene [delimitar bien las tareas para el agente en la nube][cloud-agent-best-practices]. Trátalo como un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el proceso. Los fundamentos del desarrollo de software no cambian al añadir IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has conocido las instrucciones personalizadas y cómo orientan a Copilot. Si ya completaste el ejercicio práctico de instrucciones personalizadas, habrás visto funcionar `.github/copilot-instructions.md`; si no, aprovecha para echarle un vistazo. Antes de asignar trabajo al agente en la nube, revisa los archivos de instrucciones del repositorio para poder observar más adelante su efecto.

Abre estos archivos en la interfaz web de GitHub o en un codespace:

- `.github/copilot-instructions.md`: revisa **Code standards**, en especial las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente la incidencia `Code lacks documentation`, fíjate en los comentarios TSDoc, las convenciones TypeScript y las cabeceras de archivo de la PR resultante. Proceden de estos archivos de instrucciones. Volveremos a señalarlo al revisar las PR en un ejercicio posterior.

## Configura el entorno de desarrollo del agente en la nube

El trabajo de programación suele requerir un entorno específico y scripts de configuración para garantizar que todo esté preparado. Lo mismo ocurre cuando asignas tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] para configurar el entorno. Puedes personalizarlo con un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. Así puedes instalar las herramientas y dependencias necesarias. El taller ya incluye la configuración necesaria: Node.js, dependencias del proyecto, binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Define los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de la configuración, por ejemplo, para instalar dependencias, necesitas el permiso `contents: read`. Si no lo clonas en estos pasos, Copilot lo hará automáticamente al terminar.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de frontend y aplicación - Node.js (la aplicación completa utiliza Astro + Drizzle/libSQL)
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

      # Migra y carga la base de datos SQLite local para que las compilaciones y pruebas dispongan de datos.
      - name: Set up the database
        run: npm run db:setup
```

Es parecido a otros flujos de GitHub Actions, pero tiene algunos aspectos importantes:

- Tiene un único trabajo llamado `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El activador `workflow_dispatch` permite ejecutarlo manualmente desde la pestaña **Actions** del repositorio. Resulta útil para comprobar que funciona sin esperar a que Copilot lo inicie.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Es un tipo de deuda técnica que ralentiza el trabajo y dificulta el mantenimiento del código o la incorporación de nuevos desarrolladores. Copilot puede ayudar a crear documentación; así que esta es una buena tarea para asignarle. Trabajará en segundo plano y revisarás sus cambios en un ejercicio posterior.

1. Abre tu repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

  ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la PR que acaba de generar Copilot. Su título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer. Copilot está preparando su entorno —ejecutando el flujo de trabajo anterior—, analizando el proyecto y determinando cómo abordar la tarea.

13. Revisa las tareas que realizará.
14. Desplázate por la cronología de la PR. Deberías ver que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en directo: Copilot explora el código y su estado, planifica el enfoque y crea código.

El proceso puede tardar varios minutos. Una de las principales ventajas del agente en la nube de Copilot es que trabaja de forma asíncrona y te permite ocuparte de otras tareas. Aprovecharemos esa característica asignándole otra tarea y, mientras trabaja, añadiremos funcionalidades a la aplicación.

## Añade una sección de juegos relacionados

Otra ventaja del agente en la nube es la posibilidad de dividir el trabajo. Aunque añadir una sección de juegos relacionados quizá no lleve mucho tiempo, puedes dedicarlo a otras tareas mientras Copilot trabaja. Asígnale la tarea:

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

  ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

  ![Detalles de la asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

Poco después, aparecerá un conjunto de 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando en ella.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda tarea. Como funciona de forma parecida a un ingeniero de software, no tienes que supervisarlo constantemente; revisa el resultado cuando termine. Mientras tanto, puedes crear y utilizar agentes personalizados.

## Resumen y próximos pasos

Este ejercicio ha presentado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona en deuda técnica, nuevas funcionalidades o migraciones entre frameworks.

Has explorado:

- cómo personalizar el entorno de generación de código.
- cómo garantizar que las operaciones se realicen de forma segura.
- por qué es importante delimitar bien las incidencias.
- cómo asignar incidencias a Copilot.

Mientras Copilot trabaja en segundo plano, puedes aprender a crear y utilizar agentes personalizados. [El agente en la nube de Copilot también admite servidores MCP][cloud-agent-mcp] e instrucciones personalizadas, que ya hemos explorado en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
