---
title: "Lección 7 - Planificar e implementar incidencias"
description: "Utiliza las herramientas de GitHub para crear incidencias desde Copilot Chat e implementa una en una nueva sesión de chat."
authors:
  - jamesmontemagno
lastUpdated: 2026-09-28
---

Con el servidor de Model Context Protocol (MCP) de GitHub habilitado, Copilot puede crear y leer incidencias directamente desde el chat.

En esta lección:

- crearás tres incidencias de GitHub bien delimitadas desde Copilot Chat.
- iniciarás una nueva sesión de chat para una incidencia.
- implementarás y comprobarás la incidencia.

## Crea incidencias

Pide a Copilot que proponga ideas concretas y las registre como incidencias, sin implementar todavía los cambios.

Envía el siguiente prompt:

```plaintext
Review the space quiz and suggest three focused feature ideas. Use the GitHub tools to create a separate issue for each with a clear title, user-focused description, and acceptance criteria. Do not implement them yet.
```

## Implementa una incidencia

Después, iniciarás una sesión nueva a partir de una incidencia y comprobarás el cambio antes de abrir una solicitud de incorporación de cambios.

1. Abre la vista **GitHub** en la barra de actividad e inspecciona las nuevas incidencias.
2. Elige una incidencia y selecciona el botón **+** en Copilot Chat para iniciar una nueva sesión para ella.
3. Utiliza el modo Agent para implementar la incidencia mientras la sesión original sigue disponible para comparar.
4. Ejecuta la prueba de humo del explorador integrado de la [lección 3][lesson-3] antes de crear una solicitud de incorporación de cambios.

## Resumen y pasos siguientes

Has creado incidencias e implementado una en su propia sesión. Continúa con la [lección 8: Revisar y combinar][next-lesson].

[lesson-3]: ../3-inspect-and-test/
[next-lesson]: ../8-review-and-merge/
