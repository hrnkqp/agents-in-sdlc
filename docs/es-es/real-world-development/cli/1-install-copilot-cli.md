---
title: "Lección 1 - Instalar GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en tu Codespace, familiarízate con el entorno y encuentra la incidencia de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un potente asistente de programación con agentes que se ejecuta en el terminal y permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas desde la línea de comandos. Puedes delegar tareas, solicitar cambios y mantener la concentración. El primer paso es instalar la herramienta, algo que puedes hacer con herramientas que ya conoces.

En esta lección:

- instalarás GitHub Copilot CLI con npm.
- te autenticarás con tu cuenta de GitHub.
- confiarás en el repositorio del taller y probarás una conversación breve.
- encontrarás la incidencia de filtrado con el servidor MCP integrado de GitHub.

## Escenario

Tu equipo empieza a utilizar agentes de IA para gestionar una lista de trabajo pendiente cada vez mayor. Copilot CLI lleva esa capacidad al terminal, donde ya trabajan habitualmente muchos desarrolladores. En esta lección instalarás y autenticarás Copilot CLI para utilizarlo durante el resto del taller.

## Instala Copilot CLI

Puedes instalar Copilot CLI mediante [npm][install-cli], WinGet o Homebrew. Como GitHub Codespaces incluye Node.js, utilizarás npm.

1. Vuelve a tu Codespace y abre un terminal.
2. Comprueba que Node.js está instalado y cumple el requisito de versión:

   ```bash
   node --version
   ```

   Deberías ver la versión 24 o posterior.

3. Instala Copilot CLI de forma global:

   ```bash
   npm install -g @github/copilot
   ```

4. Verifica la instalación:

   ```bash
   copilot --version
   ```

   Debería aparecer el número de versión.

## Autentícate con GitHub

La primera vez que se inicia, Copilot CLI te pide que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones del código de dispositivo para autenticarte y autorizar Copilot CLI.
3. Copilot CLI muestra esta solicitud:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Confirma que la ruta corresponde al repositorio de Tailspin Toys y responde que sí seleccionando **Yes, and remember this folder for future sessions**.

> [!NOTE]
> En un Codespace quizá ya estés autenticado mediante tu sesión de GitHub. Si Copilot CLI se inicia sin pedir autenticación, ya está listo.

## Familiarízate con el entorno

Los comandos que escribes en el prompt habitual del shell se ejecutan directamente en tu Codespace. Cuando se inicia Copilot CLI, el lenguaje natural se envía al agente y los comandos de barra controlan la conversación.

1. Escribe `/model`, utiliza las flechas para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Escribe `/help` para ver los comandos de tu versión y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz a Copilot una pregunta sencilla para comprobar que funciona:

   ```plaintext
   What are the key files in this project?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de responder.
5. Escribe `/mcp list` y confirma que está disponible el servidor MCP integrado de GitHub.
6. Pide a Copilot que encuentre la incidencia de filtrado:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abre la URL y lee la incidencia. La utilizarás después de hacer un primer cambio sencillo.

> [!TIP]
> Una sesión normal de Copilot CLI trabaja en la rama seleccionada en el terminal; no crea automáticamente un árbol de trabajo. Crearás una rama de funcionalidad antes de cada cambio.

## Utiliza el acceso directo del taller

Normalmente, Copilot CLI pide permiso antes de utilizar herramientas fuera de los permisos establecidos. Para este taller, lo iniciarás con `--yolo`, un acceso directo aprobado por el usuario que elimina esas solicitudes de aprobación en el Codespace para que puedas centrarte en los ejercicios.

> [!CAUTION]
> `--yolo` habilita permisos automáticos completos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado, como un Codespace o una máquina virtual, y nunca lo configures como alias predeterminado para el desarrollo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning] para obtener más información.

Durante este taller, `--enable-all-github-mcp-tools` habilita las herramientas de lectura y escritura de GitHub MCP que se utilizan en lecciones posteriores para trabajar con incidencias y solicitudes de incorporación de cambios. El Codespace limita el acceso a tu equipo local, pero los recursos de GitHub autenticados son reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Vuelve a iniciarlo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta breve sobre el proyecto para comprobar que funciona y sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, cuando cambies las instrucciones o añadas un agente, utilizarás `copilot --resume` para volver a la misma conversación y rama de la funcionalidad.

## Resumen y próximos pasos

¡Enhorabuena! Has instalado y autenticado GitHub Copilot CLI. Has aprendido a:

- instalar Copilot CLI con npm.
- autenticarte con tu cuenta de GitHub.
- confiar en el repositorio del taller y probar una conversación breve.
- encontrar la incidencia de filtrado con el servidor MCP integrado de GitHub.

A continuación, [harás tu primer cambio concreto][next-lesson] y utilizarás Copilot CLI para mostrar las valoraciones por estrellas en las tarjetas de juegos.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
