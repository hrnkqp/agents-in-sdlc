---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido que documente el código, cree una sección de juegos relacionados para que el equipo de diseño pueda probarla e implemente funciones de accesibilidad, incluidos los modos de contraste alto y claro. Revisemos los cambios y, si es necesario, proporcionemos comentarios a Copilot para mejorarlos.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Revisaremos las sugerencias de GitHub Copilot para la documentación, los juegos relacionados y la accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y el agente en la nube de GitHub Copilot

Como el agente en la nube trabaja de forma asíncrona y sin supervisión, hay restricciones para garantizar que todo siga siendo seguro:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para sus cambios.
- El agente en la nube se ejecuta en GitHub Actions, en un entorno efímero independiente.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de ejecutarlo.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por la primera solicitud de incorporación de cambios (PR) del agente en la nube: añadir documentación al código. Utilizarás la interfaz de PR habitual de GitHub.com.

> [!NOTE]
> Es posible que aparezca una advertencia que indique que el firewall bloqueó a GitHub Copilot. Es **normal**: Copilot tiene acceso limitado a recursos externos, incluidas las llamadas a servidores MCP. Si quieres, puedes [configurar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la PR titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, el título de la PR incluirá **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluirá comentarios TSDoc y otra documentación. Los cambios concretos pueden variar.

   Busca comentarios TSDoc, convenciones de TypeScript y cabeceras de archivo. Proceden de los archivos de instrucciones que revisaste al principio del Ejercicio 2.

6. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en cola en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. Puede tardar varios minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot en una PR no es unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios en línea del código. Copilot los verá e iniciará otra sesión para resolverlos. Como los resultados son no deterministas, no existe una indicación única. Por ejemplo, puedes pedirle:

- que añada al principio de cada archivo de código una cabecera que describa brevemente su contenido.
- que añada comentarios TSDoc a archivos TypeScript y Astro.
- que cree un README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario que solicite un cambio en la documentación generada y menciona a **@copilot** como harías con cualquier otro usuario. Puedes utilizar una idea de la lista u otra sugerencia.
2. Selecciona **View Session** para observar cómo Copilot inicia una nueva sesión y realiza los cambios.
3. Selecciona **Back to pull request** para volver a la PR.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, debería aparecer una confirmación nueva en la PR.
5. Selecciona **Files changed** y revisa los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y combinarla.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Vuelve a la PR que Copilot creó para mostrar juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR titulada, por ejemplo, **Show related games on the game details page**.
4. Selecciona **Files changed** y revisa el código generado.
5. Cuando hayas terminado, vuelve a **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán.
9. **Opcional:** puedes cambiar al codespace a esta rama para probar manualmente la funcionalidad. Abre un terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama de Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Puedes iterar y pedir ajustes, por ejemplo, cuántos juegos se muestran o que se añadan cabeceras y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Añade comentarios en la pestaña **Conversation** para que Copilot los procese en una sesión nueva.

## Revisa las funciones de accesibilidad

Por último, revisa las funciones implementadas con el agente personalizado de accesibilidad. La PR debería incluir el modo de contraste alto del Ejercicio 3 y el modo claro solicitado en mission control durante el Ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR titulada, por ejemplo, **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot aún está trabajando, el título de la PR incluirá **[WIP]**. Espera unos minutos a que termine.

4. Selecciona **Files changed** y revisa el código generado.
5. Revisa la implementación, en especial:
   - los controles para cambiar de modo.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios CSS o de estilo para los modos de contraste alto y claro.
   - los atributos de accesibilidad, como etiquetas ARIA y navegación con teclado.
   - el código JavaScript/TypeScript que gestiona los modos.

6. Cuando hayas terminado, vuelve a **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán.
10. **Opcional:** puedes cambiar al codespace a esta rama para probar manualmente las funciones de accesibilidad. Abre un terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los modos de contraste alto y claro en el navegador. Comprueba que funcionan y que la configuración persiste al recargar la página.

Observa cómo el agente personalizado de accesibilidad orientó a Copilot para implementar estas funciones según las prácticas recomendadas de accesibilidad. Si detectas problemas o mejoras posibles, menciona a **@copilot** en un comentario y solicita cambios como hiciste con las otras PR.

## Ejercicio opcional: sigue delegando

El agente en la nube es útil para tareas reales de la lista de trabajo pendiente mientras tú haces otra cosa. Para practicar, registra varias incidencias y asígnaselas a Copilot. Algunas ideas:

- crear un formulario de interés de patrocinadores en la página de detalles del juego.
- añadir paginación a la lista de juegos.
- añadir validación de entradas y gestión de errores a las funciones de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. Durante estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizados del repositorio** para entender su efecto en el trabajo del agente.
- **Has asignado incidencias al agente en la nube de Copilot** y has observado cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para implementar funciones de accesibilidad, incluidos los modos de contraste alto y claro.
- **Has utilizado la página de agentes de Copilot como mission control** para supervisar y orientar la sesión de accesibilidad.
- **Has revisado e iterado sobre las PR del agente en la nube**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Repaso y próximos pasos

Has completado el itinerario del agente en la nube. Para seguir explorando, puedes completar otros itinerarios:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración MCP desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados con los modos Plan y Autopilot, habilidades de agente, un agente QA personalizado, MCP de Playwright, Agent Merge y la opción `/delegate` para volver a delegar en el agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
