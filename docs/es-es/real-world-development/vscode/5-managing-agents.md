---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando GitHub Copilot trabaja en modo agente, explora el código, propone cambios, edita archivos y ejecuta comandos de forma autónoma. Como el trabajo se realiza en tu equipo y en tiempo real, Copilot Chat de VS Code muestra cada llamada a herramientas y cada edición. Puedes revisar las diferencias, aceptar o rechazar cambios y orientar la conversación con nuevas indicaciones sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos.
- orientarás al agente durante la sesión para añadir un modo claro junto al modo de contraste alto.

## Escenario

El agente personalizado de accesibilidad del [Ejercicio 4][previous-lesson] está implementando el modo de contraste alto. Mientras trabaja, el equipo de diseño solicita también un modo claro para quienes consideran demasiado oscuro el tema predeterminado. En vez de iniciar una conversación nueva, orientarás la sesión existente para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: archivos que explora, comandos que ejecuta y cambios que propone. Las ediciones aparecen en el chat con controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramienta, lectura o edición de archivos y comando de terminal que solicita ejecutar.
3. Cuando proponga una edición, aparecerá una diferencia en el chat:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y pedir al agente que vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramienta pide permiso, por ejemplo, para ejecutar un comando de terminal, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de contraste alto. Debería mostrar un mensaje con un resumen de los cambios.

## Pide al agente que añada el modo claro

Una vez implementado el modo de contraste alto, amplía la misma conversación con una opción de modo claro. Como continuarás en la misma sesión, el agente conserva el contexto de los componentes, los estilos y el patrón de almacenamiento local que utilizó.

1. Envía esta indicación de seguimiento en Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente revisará los cambios anteriores y propondrá los cambios para el modo claro.
3. Revisa cada edición y utiliza **Keep** o **Undo**, como hiciste con el modo de contraste alto.
4. Si el agente toma una dirección que no quieres —por ejemplo, intenta reorganizar la interfaz de los botones—, detenlo con el botón **Stop** junto al campo de chat y envíale una indicación aclaratoria.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Envía indicaciones de seguimiento para solicitar ajustes («simplifica el CSS y utiliza variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer botón; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos modos»).

## Revisa el conjunto de cambios

Antes de confirmar el trabajo, revisa los archivos que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Debería incluir componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que se aplican los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y próximos pasos

Has utilizado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado las ediciones, aceptado o rechazado cambios y enviado indicaciones de seguimiento para ampliar el trabajo sin perder contexto.

En esta lección has aprendido a:

- supervisar el trabajo de un agente en tiempo real.
- aceptar y rechazar ediciones individuales.
- orientar una sesión en curso con indicaciones de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios del agente local personalizado producidos durante este taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
