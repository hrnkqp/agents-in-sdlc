---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores asignaste al agente en la nube de Copilot tres tareas distintas relacionadas con mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, es importante supervisar las tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, entre ellas la [página de agentes][agents-page] de GitHub.com. Desde este centro puedes ver las tareas de agentes con solicitudes de incorporación de cambios abiertas, consultar las operaciones realizadas y orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de pedir al agente que cree un modo de contraste alto, el equipo decidió que sería buen momento para añadir también un modo claro. Como ya se están actualizando los estilos y añadiendo controles de modo, pedirás al agente que incluya ambas opciones.

## Revisa las tareas del agente en la nube de Copilot

Veamos el estado de las tareas asignadas al agente en la nube.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa las tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado, entre ellas:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de contraste alto al sitio.
3. Selecciona una tarea en curso y revisa las operaciones realizadas por Copilot, como:
   - clonar el código del repositorio.
   - preparar el entorno de trabajo.
   - configurar servidores MCP.
   - realizar pasos para completar la tarea.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque utilizado.

4. Observa también el panel de PR a la derecha, donde puedes consultar la solicitud y los archivos modificados.

## Orienta al agente en la nube

Ya has visto las tareas activas. Ahora pide a Copilot que añada el modo claro mientras trabaja en el modo de contraste alto.

1. Selecciona la sesión relacionada con el modo de contraste alto. El título exacto depende del nombre que haya elegido Copilot y del estado de la tarea.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos a que termine la configuración y empiece el trabajo. Lo sabrás cuando aparezcan mensajes parecidos a los siguientes.
3. En el cuadro **Steer active session while Copilot is working**, escribe esta indicación:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes con el cuadro para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar la indicación.
5. Observa cómo Copilot la confirma y la añade al flujo de trabajo.

## Deja que Copilot trabaje

Copilot incorporará la nueva solicitud al trabajo cuando termine el paso que estaba realizando al recibirla.

El proceso puede tardar varios minutos. Es un buen momento para repasar lo aprendido.

## Resumen y próximos pasos

Has explorado la página de agentes de Copilot, el centro para supervisar y orientar las tareas del agente en la nube. Permite seguir las tareas activas y completadas, revisar el trabajo y cambiar la dirección de las sesiones en curso.

Has aprendido a:

- utilizar mission control y la página de agentes para supervisar tareas.
- redirigir una sesión en curso y pedir más funcionalidad.

Mientras Copilot termina el trabajo de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
