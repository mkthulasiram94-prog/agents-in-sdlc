---
title: "Lección 1 - Instalar la aplicación GitHub Copilot"
description: "Instala la aplicación GitHub Copilot, conecta el repositorio creado a partir de la plantilla, explora el espacio de trabajo y prueba un chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La **[aplicación GitHub Copilot][about-copilot-app]** es una aplicación de escritorio para el desarrollo dirigido por agentes. Se basa en GitHub Copilot CLI y se integra de forma nativa con GitHub, por lo que los repositorios, las ramas y las canalizaciones de integración continua funcionan desde el primer momento. Está diseñada para flujos de trabajo en los que diriges varios agentes en paralelo, cada uno en su propio espacio de trabajo aislado, y automatizas tareas repetitivas en lugar de hacerlo todo por tu cuenta. Con Node.js instalado y una copia del proyecto preparada, el siguiente paso es instalar la aplicación y conectar ese repositorio.

En esta lección, aprenderás a:

- instalar la aplicación GitHub Copilot e iniciar sesión.
- añadir el proyecto a la aplicación desde su repositorio de GitHub.
- explorar el espacio de trabajo y el backlog que ha creado la plantilla.
- probar un chat rápido para conocer la aplicación.

## Escenario

Tu equipo está empezando a utilizar agentes de inteligencia artificial (IA) para abordar un backlog cada vez mayor. La aplicación Copilot ofrece un único lugar desde el que dirigir ese trabajo: seleccionar incidencias, ejecutar agentes, revisar cambios y combinar solicitudes de incorporación de cambios. En esta lección instalarás y conectarás la aplicación, y aprenderás a iniciar una conversación sobre el proyecto.

> [!NOTE]
> Se requiere un plan de Copilot válido: Copilot Free, Copilot Student o cualquier plan de pago (Pro, Pro+, Business o Enterprise). Si utilizas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**.

## Instala y configura la aplicación GitHub Copilot

El primer paso para utilizar la aplicación GitHub Copilot es instalarla. Hay versiones para Windows, macOS y Linux. Instala la aplicación, autentícate y añade el repositorio Tailspin Toys.

1. En un navegador, abre la [página de inicio de la aplicación GitHub Copilot][download-app].
2. Descarga la aplicación para tu plataforma e instálala siguiendo las instrucciones de la página.
3. Abre la aplicación después de instalarla.
4. Selecciona **Sign in to GitHub** y sigue las indicaciones para iniciar sesión con una cuenta de GitHub.com o GHE.com. Si utilizas GitHub Enterprise Cloud con residencia de datos, elige **GitHub Enterprise Cloud** e introduce el nombre de host `*.ghe.com` cuando se te solicite.

   Si utilizas GitHub Enterprise Server, completa primero la incorporación con tu cuenta en la nube. Después, abre **Settings**, selecciona **Accounts**, selecciona **Add account** y elige **GitHub Enterprise Server**. Sigue las indicaciones para autorizar la aplicación en el navegador, como se describe en la [guía de inicio de sesión en Enterprise Server][ghes-sign-in].

5. Si durante la incorporación se te ofrece seleccionar un repositorio, elige el repositorio Tailspin Toys que creaste a partir de la plantilla. También puedes omitir la selección y añadir el proyecto después.
6. Cuando se te solicite, elige un tema y termina el proceso de incorporación.
7. Busca el repositorio en **Sessions**, en la barra lateral. Si ya aparece, no tienes que volver a añadirlo.
8. Si todavía no está conectado, selecciona **+** junto a **Sessions** para crear una sesión y, después, selecciona **Add GitHub repository**. Introduce el propietario y el nombre del repositorio que anotaste en los requisitos previos.
9. Confirma que el repositorio aparece en la barra lateral. En la disposición de este ejercicio, las carpetas de repositorio, como `tailspin-toys`, están agrupadas en **Sessions**, con las sesiones individuales debajo de cada repositorio.

## Familiarízate con el espacio de trabajo

Con el proyecto conectado, dedica un momento a conocer la aplicación. La barra lateral organiza el espacio de trabajo en varias áreas:

- **New**: permite iniciar una nueva sesión de chat con Copilot.
- **Pull requests**: muestra las solicitudes de incorporación de cambios mediante la integración nativa con GitHub. Puedes explorarlas y filtrarlas, comprobar el estado de integración continua (CI) y revisarlas sin salir de la aplicación.
- **Issues**: muestra las incidencias. Puedes explorarlas y filtrarlas, e iniciar una sesión a partir de una incidencia.
- **Automations**: contiene tareas de agente guardadas que se ejecutan según una programación o bajo demanda. Son útiles para gestionar listas de tareas, realizar tareas periódicas de mantenimiento del proyecto o delegar otras tareas tediosas. En el repaso se mencionan como un siguiente paso, no como otro ejercicio del taller.
- **Customize**: permite añadir funciones a la aplicación Copilot mediante servidores de Model Context Protocol (MCP), complementos, habilidades y otros componentes. La utilizarás para configurar MCP de Playwright.
- **Chats**: contiene conversaciones breves para preguntas y lluvias de ideas que no necesitan una rama ni un espacio de trabajo propios. Esta sección aparece después de iniciar el primer chat. Si no está visible, selecciona **+** junto a **Sessions** para iniciar un chat.
- **Sessions**: agrupa las carpetas de repositorio y las sesiones de agente. Selecciona un repositorio, como `tailspin-toys`, para abrir su vista, o selecciona una sesión debajo para volver a la conversación. Cada sesión se ejecuta en su propio espacio de trabajo aislado, así que puedes ejecutar varias a la vez sin que sus cambios entren en conflicto. Iniciarás tu primera sesión al añadir las valoraciones por estrellas.

Mientras avanzas por el taller, explorarás el espacio de trabajo.

> [!TIP]
> Si tienes dudas, pregunta a Copilot. Si no sabes cómo hacer algo o si es posible, Copilot puede orientarte.

### Busca el backlog creado para ti

Prácticamente todos los proyectos tienen un backlog, y Tailspin Toys no es una excepción. Explora las incidencias que se crearon al crear el repositorio a partir de la plantilla.

1. En **Sessions**, en la barra lateral, selecciona la carpeta del repositorio Tailspin Toys, no una sesión individual. Después, selecciona **Issues** en la navegación superior del repositorio.
2. Busca estas incidencias por el título, sin dar por hecho cuál es su número:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecciona una incidencia para leer los detalles. Cada incidencia también permite iniciar una sesión de agente. Partirás de la incidencia de filtrado después de realizar un primer cambio rápido.

> [!NOTE]
> Las listas de **Issues** y **Pull requests** se filtran automáticamente para mostrar solo los elementos de los repositorios que has añadido a la aplicación Copilot. Para ver elementos de otros repositorios, añade esos repositorios a la aplicación.

## Prueba un chat rápido

Una buena forma de familiarizarte con la aplicación es utilizarla para conocer la propia aplicación. Un **chat rápido** te permite hacer una pregunta o plantear ideas sin crear una rama ni un árbol de trabajo, por lo que es ideal para consultas breves que no necesitan una sesión.

1. En la barra lateral, selecciona **+** junto a **Chats** para abrir un chat nuevo. Si **Chats** todavía no está visible, selecciona **+** junto a **Sessions** para iniciar el primer chat; después aparecerá **Chats**.
2. Pregunta cómo funcionan las sesiones de la aplicación:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Lee la respuesta en la vista de conversación. Verás que cada sesión se ejecuta en su propio árbol de trabajo de Git aislado, lo que permite ejecutar varios agentes en paralelo sin que sus cambios entren en conflicto. Puedes continuar la conversación o iniciar un chat nuevo en cualquier momento.

## Resumen y pasos siguientes

Has instalado la aplicación GitHub Copilot, conectado el proyecto y explorado el espacio de trabajo. En esta lección, has:

- instalado la aplicación e iniciado sesión en GitHub.
- añadido el proyecto desde su repositorio de GitHub.
- explorado el espacio de trabajo y localizado el backlog creado para ti en **Issues**.
- utilizado un chat rápido para plantear una pregunta breve.

A continuación, [iniciarás tu primera sesión de agente][next-lesson] y utilizarás la aplicación para mostrar la valoración por estrellas en las tarjetas de juegos.

## Recursos

- [Información sobre la aplicación GitHub Copilot][about-copilot-app]
- [Primeros pasos con la aplicación GitHub Copilot][getting-started]
- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
