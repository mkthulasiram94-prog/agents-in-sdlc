---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: utiliza Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar y verificar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Empezaste por añadir una función pequeña al proyecto, pero los cambios más amplios requieren un proceso más sólido. La aplicación GitHub Copilot está diseñada para adaptarse al flujo de trabajo de una organización y ayudar a desarrollar lo adecuado de la forma correcta. En esta y las siguientes lecciones seguirás un proceso habitual de desarrollo dirigido por agentes: partirás de una incidencia para crear una función, comprobarás que el código es válido y que la función se comporta como esperas y, finalmente, la combinarás correctamente en el proyecto.

> [!NOTE]
> Continuarás con la misma sesión durante todo el flujo de trabajo de la función. Lo habitual sería utilizar sesiones o solicitudes de incorporación de cambios distintas para diferentes tipos de archivo, pero este atajo te permitirá centrarte en los conceptos principales.

En esta lección, aprenderás a:

- iniciar una sesión de agente nueva desde una incidencia de GitHub.
- definir los requisitos en modo **Plan**.
- implementar la función en modo **Autopilot**.
- revisar el código.
- validar manualmente la función en un lienzo de navegador.

Durante el desarrollo de esta función, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks` existente, añadirás validación de Model Context Protocol (MCP), crearás un agente de control de calidad (QA) y abrirás la solicitud de incorporación de cambios de la función.

## Escenario

El catálogo de Tailspin Toys está creciendo y sus visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la función, pero antes de programar hay que acordar detalles como la combinación de categorías. Utilizarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Introducir agentes de programación con inteligencia artificial (IA) en el flujo de desarrollo no cambia los principios fundamentales; de hecho, hace que sean aún más importantes. La mayoría de los desarrolladores siguen un flujo similar:

1. Abrir una incidencia que detalle lo que se debe hacer.
2. Crear un plan de lo que se debe desarrollar.
3. Desarrollar y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la nueva función.
6. Crear una solicitud de incorporación de cambios (PR).
7. Cuando se haya revisado el código y se haya completado correctamente el proceso de integración continua, combinar el cambio.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los flujos de trabajo son variantes del proceso anterior.

Si sigues este enfoque estándar, te aseguras de que el código generado por IA cumpla los requisitos establecidos y pase por el mismo proceso de validación que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** determina el grado de autonomía del agente. Puedes establecerlo en el menú desplegable situado debajo del campo del prompt y cambiarlo en cualquier momento:

- **Interactive**: trabajas junto con el agente. Propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: primero, el agente crea un plan. Lo revisas y apruebas antes de que el agente lo ejecute.
- **Autopilot**: el agente trabaja de forma totalmente autónoma: escribe código, ejecuta pruebas e itera sin esperar indicaciones.

Empieza en modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Inicia una sesión desde la incidencia

Antes de empezar, confirma que la PR de valoraciones por estrellas está combinada y que tu rama local `main` está actualizada.

1. En **Sessions**, en la barra lateral, selecciona la carpeta del repositorio Tailspin Toys y, después, **Issues** en la navegación superior del repositorio. Abre la incidencia **Allow users to filter games by category and publisher**. Es una incidencia incluida en la plantilla; no tienes que crear otra.
2. Selecciona **New session** y elige un **new working tree** basado en la rama `main` actualizada.

    ![Vista de una incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia está adjunta a la sesión y selecciona **Plan** en el selector de modo.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si funcionan en tu terminal, pero no en la sesión, comprueba la variable `PATH` o la configuración del gestor de versiones de la aplicación y ciérrala por completo antes de volver a abrirla. Reintenta la operación y las comprobaciones que hayan fallado; no vuelvas a instalar npm solo porque el agente no lo encuentre.

## Planifica la función de filtrado

La planificación te permite revisar el enfoque antes de que Copilot escriba código. Como has iniciado la sesión desde la incidencia, Copilot ya tiene la solicitud como contexto. Envía:

```plaintext
Build this feature.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluye el filtrado por categoría y editorial, controles accesibles, cambios en la capa de acceso a datos y pruebas. Aclara cualquier comportamiento poco definido, como la combinación de varias categorías o qué ocurre cuando ningún juego coincide.

El plan debe incluir lint, pruebas unitarias, pruebas de extremo a extremo (E2E) y comprobaciones de tipos con las herramientas existentes del proyecto. Céntrate en implementar y probar el filtrado; crearás la PR después de completar el flujo de calidad. Pide los cambios necesarios antes de aprobar el plan y guarda la URL de la incidencia y las aclaraciones acordadas para validarlas más adelante.

## Aprueba Autopilot explícitamente

Cuando estés conforme con el plan, selecciona **Approve and implement this plan** o la opción equivalente en tu versión. Comprueba el modo activo en lugar de dar por hecho que la aprobación lo cambia. Confirma que el indicador muestra **Autopilot**.

Copilot empezará a implementar la función. Verás cómo sigue el plan, genera código e itera, incluso mediante la ejecución de pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa primero el plan. Si Copilot informa de dependencias ausentes o de un conflicto de puerto, resuelve el problema de configuración antes de dar las comprobaciones por completadas. Detén solo los servidores que hayas iniciado.

## Revisa y verifica la implementación

Antes de combinar el código generado, revísalo como cualquier otro cambio y ejecuta el sitio para comprobar que funciona.

1. Abre **Changes** y examina la implementación del filtrado y las pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de categorías y editoriales. Comprueba que los cambios siguen las instrucciones existentes del repositorio.
3. Examina los resultados de lint, las pruebas unitarias, las pruebas E2E y las comprobaciones de tipos. Una comprobación omitida no cuenta como superada.
4. Resuelve los fallos y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa, y puede reutilizar un servidor local; confirma que el servidor probado pertenece a este árbol de trabajo, no a una lección anterior.

## Explora la nueva función

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio en un lienzo de navegador.

1. Envía este prompt para pedir a Copilot que inicie la aplicación y abra la página en el lienzo de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. Espera a que se inicie la aplicación y se abra una ventana del navegador dentro de la aplicación Copilot.
3. Selecciona una categoría y confirma que los resultados coinciden. Borra el filtro, selecciona una editorial y confirma que los resultados corresponden a esa editorial.
4. Combina filtros de categoría y editorial y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, verifica el comportamiento acordado para combinarlas.
5. Prueba una combinación sin juegos coincidentes y comprueba el estado vacío. Borra o restablece los filtros y confirma que vuelve a aparecer la lista completa.
6. Cuando termines, envía este prompt para pedir a Copilot que detenga el servidor de desarrollo que inició en esta sesión:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Conserva esta sesión para las lecciones siguientes

Todavía no crees ni combines la PR de filtrado. Conserva esta sesión, el árbol de trabajo y la rama para las lecciones siguientes. A continuación, aplicarás instrucciones personalizadas al código de filtrado y añadirás las mejoras de la habilidad y del agente de QA antes de crear y combinar la PR conjunta. Puedes dejar la sesión inactiva después de detener su servidor de desarrollo y retomarla en la siguiente lección.

## Resumen y pasos siguientes

Has utilizado distintos modos de agente para desarrollar y revisar una función. En esta lección, has:

- iniciado una sesión de agente nueva desde una incidencia de GitHub.
- definido los requisitos en modo **Plan**.
- implementado la función en modo **Autopilot**.
- revisado el código.
- validado manualmente la función en un lienzo de navegador.

A continuación, profundizarás en cómo se genera el código y cómo seguir las prácticas documentadas mediante el [uso de instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
