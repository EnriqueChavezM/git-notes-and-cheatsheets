# 02. Estrategias de Ramificación (Branching Strategies)

Una estrategia de ramificación define un conjunto de reglas para estructurar y gestionar la creación, fusión y mantenimiento de ramas dentro de un equipo de desarrollo. Su objetivo es minimizar conflictos, organizar despliegues y mantener la estabilidad del código base.

---

## 1. GitHub Flow

Es un flujo de trabajo ligero, ágil y centrado en despliegues continuos (*Continuous Deployment*). Es ideal para proyectos donde la rama principal (`main`) se encuentra en un estado constantemente desplegable.

### Principios Básicos:
* **Rama `main` estable:** Todo el código presente en `main` debe ser listo para producción.
* **Ramas descriptivas:** Cada nueva funcionalidad o corrección se crea a partir de `main` con un nombre representativo (ej. `feature/login`, `fix/sensor-read`).
* **Pull Requests (PR):** Permiten solicitar retroalimentación y realizar revisiones de código antes de la fusión.
* **Fusión a `main`:** Tras la revisión y la aprobación de pruebas automáticas, la rama se fusiona con `main` y se despliega inmediatamente.

---

## 2. Git Flow

Es un modelo altamente estructurado diseñado para proyectos con ciclos de entrega (*release*) programados y controlados. Utiliza ramas permanentes y de apoyo con propósitos estrictos.

### Ramas Principales (Permanentes):
* **`main`:** Contiene únicamente código de producción con etiquetas (*tags*) de versión.
* **`develop`:** Funciona como rama de integración para todas las nuevas características desarrolladas.

### Ramas de Apoyo (Temporales):
* **`feature/*`:** Nacen de `develop` y se fusionan de vuelta en `develop` cuando finaliza una funcionalidad.
* **`release/*`:** Nacen de `develop` para preparar una nueva versión para producción (pruebas finales, corrección de errores menores). Al finalizar, se fusionan tanto en `main` como en `develop`.
* **`hotfix/*`:** Nacen de `main` para corregir errores críticos en producción de forma urgente. Al finalizar, se fusionan en `main` y `develop`.

---

## 3. Trunk-Based Development

Es un enfoque enfocado en el desarrollo iterativo rápido donde todos los desarrolladores integran sus cambios en una sola rama principal (`trunk` o `main`) con mucha frecuencia (varias veces al día).

### Características Clave:
* **Ramas de corta duración:** Si se crean ramas secundarias, deben durar unas pocas horas o un par de días como máximo.
* **Integración Continua (CI):** Requiere pruebas automáticas rigurosas para validar cada commit integrado en la rama principal.
* **Feature Flags (Interruptores de función):** Permiten desplegar código incompleto u ocultar funcionalidades en desarrollo sin afectar a los usuarios en producción.

---

## 4. Cuadro Comparativo de Estrategias

| Criterio | GitHub Flow | Git Flow | Trunk-Based |
| :--- | :--- | :--- | :--- |
| **Complejidad** | Baja | Alta | Media |
| **Velocidad de Despliegue** | Muy alta | Lenta (programada) | Extrema (diaria/continua) |
| **Ramas Permanentes** | 1 (`main`) | 2 (`main`, `develop`) | 1 (`main`) |
| **Ideal para** | Web apps, microservicios | Software embebido, versiones estables | Equipos experimentados, CI/CD |