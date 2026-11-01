---
title: "Ejercicio 2 - Añadir funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. El flujo habitual de un desarrollador consiste en localizar los archivos necesarios, hacer los cambios, ejecutar pruebas, depurar, averiguar qué archivo falta, volver a cambiarlo... y así sucesivamente.

El modo agente de Copilot ayuda con este proceso.

El modo agente está diseñado para trabajar con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como pruebas y corrige automáticamente los errores que encuentra. Veamos cómo utilizarlo para añadir una funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite ciclos de respuesta rápidos e interacción directa. El agente en la nube trabaja de forma asíncrona como un compañero del equipo e interactúa contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot puede explorar el proyecto, identificar archivos pertinentes y coordinar cambios.
- el modo agente de GitHub Copilot puede implementar funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados por el modo agente de Copilot antes de combinar el código.

## Escenario

A medida que crece la lista de juegos, quieres permitir el filtrado por categoría y editor. En el ejercicio anterior añadiste una función auxiliar para editores. Ahora completarás con el modo agente de Copilot el resto del trabajo de datos, interfaz y pruebas.

## Ejecuta el sitio de Tailspin Toys

Antes de cambiar nada, explora el sitio para entender sus funciones actuales.

El sitio es una plataforma de financiación colectiva de juegos de mesa con una temática para desarrolladores. Permite mostrar los juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz en el cliente. Las funciones reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se añade mediante un pequeño `<script>` de Astro con API estándar del DOM.

### Inicia el sitio

La tarea `npm run dev` inicia el único servidor de desarrollo de Astro.

1. Vuelve a tu codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea y selecciona una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir un navegador en `http://localhost:4321`, ciérralo seleccionando **x**.

5. Para abrir el sitio, mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el vínculo de localhost en el terminal se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado a tu codespace, donde se está ejecutando el servidor web.

### Explora el sitio

Las funciones principales incluyen:

- **Página principal**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, muestra más información, como el título, la descripción, el editor y la categoría.

## Explora la lista de trabajo pendiente con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección de modo y modelo varía según el ejercicio; cada paso la indicará cuando sea relevante.

La implementación inicial del sitio funciona, pero queremos ampliarla. Empecemos por revisar la lista de trabajo pendiente. La plantilla creó incidencias automáticamente; pide a GitHub Copilot que te las muestre.

1. Selecciona **Agent** en el selector de agentes de la vista Chat. El agente puede planificar e implementar cambios en varios archivos, ejecutar comandos de terminal y utilizar herramientas.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no recomiendan un modelo concreto. Durante su creación se utilizó Claude Sonnet 4.5 para que las sugerencias fueran relativamente coherentes, pero las respuestas pueden variar por el carácter probabilístico de los modelos de lenguaje. Es normal.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje, Copilot podría utilizar un comando MCP distinto y completar igualmente la tarea.

3. Pide a Copilot que muestre las incidencias:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Copilot también habrá priorizado los elementos según lo útiles que podrían ser para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En tu codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales, entre ellas:
   - una descripción de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - vínculos a instrucciones para distintos tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar tanto a otros archivos de instrucciones como a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así se pueden reutilizar ejemplos y dividir conjuntos de instrucciones grandes en partes más manejables.

## Implementa el filtrado

Para completar el filtrado, hay que realizar al menos tres cambios independientes:

- añadir o ajustar la lógica de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento del filtrado.
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse correctamente antes de combinar los cambios. El modo agente de Copilot puede hacerlo.

1. Continúa la conversación actual o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de interfaz.](../../../_images/ex3-select-instructions-file.png)

3. Confirma que está seleccionado **Agent**.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que sigue seleccionado el modelo **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto, localiza los archivos pertinentes de datos, interfaz y pruebas, modifica los archivos y ejecuta las pruebas.

   ![Captura de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot explora, modifica archivos y ejecuta pruebas. El proceso puede tardar varios minutos. Mientras trabaja, quizá veas botones **Keep** y **Undo** en el editor. No tienes que seleccionarlos hasta que Copilot termine.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede haber pausas y algunos fallos de prueba. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea sin detectar errores.

   ![Captura de una sesión completa de Copilot Chat en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Inspecciona el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o por cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y próximos pasos

En este ejercicio has aprendido a utilizar el modo agente de GitHub Copilot para añadir funciones al sitio:

- el modo agente puede implementar funcionalidades en la interfaz y la capa de datos.
- puede explorar el proyecto, identificar archivos pertinentes y coordinar cambios.
- puedes revisar los cambios y las pruebas antes de combinar el código.

Ahora [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crece la lista de juegos, será necesario paginarla. Utiliza lo aprendido para pedir a Copilot que añada paginación. Por ejemplo:

- sigue las prácticas recomendadas y utiliza los archivos de instrucciones existentes.
- decide cómo se implementará la paginación y si el usuario podrá elegir el tamaño de página.
- proporciona a Copilot la orientación necesaria en la indicación.
- itera con Copilot y añade contexto o solicita cambios cuando sea necesario. Es lo habitual al trabajar con Copilot.

## Recursos

- [Copilot ask, edit y agent: qué hacen y cuándo utilizarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
