# 03. Plantillas de Issues (Issue Templates)

Las plantillas de incidencias (*Issue Templates*) en GitHub ayudan a estandarizar la información que los colaboradores y usuarios proporcionan cuando reportan un bug o solicitan una nueva función. Se ubican en el directorio `.github/ISSUE_TEMPLATE/`.

---

## 1. Configuración de Plantillas en GitHub

Las plantillas se definen mediante archivos Markdown o YAML con un encabezado en formato **YAML Front Matter**:

```text
.github/
└── ISSUE_TEMPLATE/
    ├── bug_report.md
    └── feature_request.md
```

---

## 2. Ejemplo 1: Plantilla de Reporte de Bug (`bug_report.md`)

```markdown
---
name: Reporte de Bug
about: Crea un informe para ayudarnos a corregir un fallo
title: '[BUG] '
labels: bug
assignees: ''
---

## Descripción del Problema
Una descripción clara y concisa de lo que sucede.

## Pasos para Reproducir
1. Ir a '...'
2. Hacer clic en '....'
3. Ver el error '...'

## Comportamiento Esperado
Una descripción clara de lo que esperabas que sucediera.

## Capturas de Pantalla o Logs
Si aplica, agrega capturas de pantalla o registros de la consola/terminal.

## Entorno de Ejecución
* **Sistema Operativo:** [ej. Ubuntu 24.04, Windows 11]
* **Versión de Software/Herramienta:** [ej. v2.1.0]
* **Navegador/Compilador:** [ej. GCC 13.2, Chrome 120]
```

---

## 3. Ejemplo 2: Plantilla de Solicitud de Funcionalidad (`feature_request.md`)

```markdown
---
name: Solicitud de Funcionalidad
about: Sugiere una nueva idea o mejora para este proyecto
title: '[FEAT] '
labels: enhancement
assignees: ''
---

## ¿Tu solicitud está relacionada con un problema?
Una descripción clara de cuál es el inconveniente o limitación actual. Ejemplo: "Me resulta molesto tener que..."

## Solución Propuesta
Una descripción clara y concisa de lo que quieres que suceda o se implemente.

## Alternativas Consideradas
Una descripción de cualquier solución o característica alternativa que hayas considerado.

## Contexto Adicional
Agrega cualquier otra información o capturas de pantalla sobre la solicitud de la función aquí.
```

---

## 4. Formularios Interactivos en YAML (`form.yml`)

GitHub permite crear formularios interactivos con campos de entrada mediante archivos `.yml`:

```yaml
name: Reporte de Error
description: Reporta un problema técnico en el código.
title: "[BUG]: "
labels: ["bug", "triage"]
body:
  - type: textarea
    id: description
    attributes:
      label: Descripción del fallo
      description: Explica qué ocurrió de forma detallada.
    validations:
      required: true
  - type: dropdown
    id: priority
    attributes:
      label: Prioridad estimada
      options:
        - Baja
        - Media
        - Crítica
    validations:
      required: true
```