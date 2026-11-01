---
title: "Lección 0 - Requisitos previos"
description: "Crea una copia de Tailspin Toys y prepara un GitHub Codespace para el taller de Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Antes de empezar las lecciones de Copilot CLI, prepara todo lo necesario. Crearás una copia propia del repositorio Tailspin Toys e iniciarás un [codespace][codespaces]. En la siguiente lección utilizarás su terminal integrado para instalar y ejecutar Copilot CLI.

En esta lección:

- crearás una copia propia del proyecto Tailspin Toys a partir de la plantilla.
- crearás un Codespace y confirmarás que el proyecto está listo.

## Configura el repositorio del taller

Trabajarás con una copia propia del proyecto Tailspin Toys. Créala a partir del [repositorio de plantilla][tailspin-template]. El nuevo repositorio incluye todos los archivos necesarios para el taller.

1. En una ventana nueva del navegador, abre la [plantilla de Tailspin Toys][tailspin-template].
2. Crea una copia del repositorio seleccionando **Use this template** y, después, **Create a new repository**.
3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. En caso contrario, crea el repositorio en una organización donde tengas acceso a GitHub Copilot.
4. Anota la ruta del repositorio que has creado (`organization-or-user-name/repository-name`); la consultarás más adelante.

> [!NOTE]
> Al crear el repositorio a partir de la plantilla, se crea automáticamente una lista de incidencias de GitHub. Trabajarás con ellas durante el taller; no tienes que crear ninguna.

Utiliza una copia nueva de la plantilla del taller. Incluye instrucciones del repositorio, el código de la aplicación, pruebas, la habilidad `quality-checks` y las incidencias del taller. Si utilizas una copia más antigua, consulta con el instructor para confirmar que contiene los archivos necesarios.

## Crea un Codespace

A continuación, utilizarás un Codespace para completar el taller.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Ofrece un editor completo con compatibilidad con varios lenguajes de programación, extensiones y herramientas.

1. Abre el repositorio que acabas de crear.
2. Selecciona **Code**.
3. Selecciona la pestaña **Codespaces** y, después, **Create codespace on main**.
4. Espera a que termine la configuración. La plantilla instalará las dependencias del proyecto y Playwright Chromium, y preparará la base de datos local.
5. Si aparece la pregunta **Do you trust the authors of the files in this folder?**, selecciona **Trust Folder & Continue**.
6. Abre un terminal en la raíz del repositorio e inicia la aplicación:

   ```bash
   npm run dev
   ```

7. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser** y confirma que se carga Tailspin Toys.
8. Vuelve al terminal y detén el servidor de desarrollo con <kbd>Ctrl</kbd>+<kbd>C</kbd>.

## Resumen y próximos pasos

Ya tienes todo preparado. En esta lección:

- has creado una copia propia del proyecto Tailspin Toys a partir de la plantilla.
- has creado un Codespace y confirmado que el proyecto está listo.

A continuación, [instalarás GitHub Copilot CLI][next-lesson] en tu Codespace y te autenticarás con tu cuenta de GitHub.

## Recursos

- [Introducción a GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Introducción a Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
