---
slug: es-es/real-world-development/cli
title: "GitHub Copilot CLI"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)** incorpora GitHub Copilot al terminal como asistente de programación con agentes. Explora código, genera código, ejecuta comandos y se conecta a herramientas externas, todo desde la línea de comandos para que puedas mantener el flujo de trabajo sin cambiar a un editor gráfico.

En estos ejercicios instalarás y autenticarás Copilot CLI y, después, proporcionarás contexto del proyecto con instrucciones personalizadas antes de planificar e implementar una funcionalidad. Conectarás el servidor MCP de Playwright para probarla en un navegador real y, a continuación, ampliarás Copilot con habilidades de agente reutilizables y agentes personalizados. Por último, explorarás comandos de barra para gestionar el contexto, los modelos y el uso compartido. También puedes seguir una [serie opcional de tres módulos con GitHub Copilot CLI y Foundry][foundry] para preparar un modelo, crear y desplegar un agente hospedado e integrarlo en el sitio.

## Ejercicios

| Ejercicio | Tema | Descripción |
|----------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Crea tu repositorio y tu codespace |
| [1. Instalar Copilot CLI][ex1] | Instalación | Instala y autentica Copilot CLI |
| [2. Añadir valoraciones por estrellas][ex2] | Primer cambio | Muestra las valoraciones existentes y combina tu primera solicitud de incorporación de cambios |
| [3. Modos de agente][ex3] | Planificación e implementación | Usa Plan y Autopilot para crear una funcionalidad a partir de una incidencia |
| [4. Instrucciones personalizadas][ex4] | Contexto | Explora y actualiza las instrucciones del proyecto |
| [5. Habilidades de agente][ex5] | Habilidades | Personaliza y utiliza la habilidad de comprobaciones de calidad |
| [6. Validar con Playwright MCP][ex6] | Herramientas externas | Añade Playwright MCP y prueba la funcionalidad en un navegador |
| [7. Agente de QA][ex7] | Agentes personalizados | Crea y utiliza un agente de QA para revisar requisitos y pruebas |
| [8. Crear y combinar la solicitud de incorporación de cambios][ex8] | Revisión y combinación | Revisa el cambio completo y utiliza Agent Merge |
| [9. Herramientas prácticas de CLI][ex9] | Comandos de CLI | Gestiona el contexto, los modelos y las sesiones, y explora la delegación opcional |
| [10. Repaso][ex10] | Resumen | Repasa el flujo de trabajo y los próximos pasos |
| [Opcional: Incorpora Foundry][foundry] | Agentes hospedados | Prepara un modelo, crea y despliega un agente y conéctalo al sitio |
| [Opcional: incorpora Foundry][foundry] | Agentes hospedados | Prepara un modelo, crea y despliega el concierge y conéctalo al sitio web en tres módulos |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo de **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Conocimientos básicos del terminal o la línea de comandos
- [ ] Git instalado y configurado

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis a través de [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye las funciones de agente, MCP, revisión de código y Copilot CLI que utiliza este taller, así que puedes completar todos los itinerarios con él.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Si usas Copilot Business o Copilot Enterprise, asegúrate de que tu administrador haya habilitado Copilot CLI.

## Empezar

**[Empieza con el ejercicio 0: Requisitos previos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
